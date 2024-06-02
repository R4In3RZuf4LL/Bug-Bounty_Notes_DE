
## Website

https://github.com/0xInfection/XSRFProbe?tab=readme-ov-file

## Description

XSRFProbe ist ein erweitertes Cross-Site-Request-Forgery-(CSRF/XSRF)-Audit- und Exploitation-Toolkit. Ausgestattet mit einer leistungsstarken Crawling-Engine und zahlreichen systematischen Prüfungen ist es in der Lage, die meisten Fälle von CSRF-Schwachstellen und die damit verbundenen Umgehungen zu erkennen und für jede gefundene Schwachstelle einen (böswillig) ausnutzbaren Proof of Concepts zu generieren. Weitere Informationen zur Funktionsweise von XSRFProbe finden Sie unter XSRFProbe Internals im Wiki.

## Usage

```
usage: python3 xsrfprobe.py [-h] [-u URL] [-c COOKIE] [-o OUTPUT] [-d DELAY]
                            [-q] [-v] [--user-agent USER_AGENT]
                            [--headers HEADERS] [--exclude EXCLUDE]
                            [--timeout TIMEOUT] [--max-chars MAXCHARS]
                            [--crawl] [--no-analysis] [--malicious]
                            [--skip-poc] [--display] [--update]
                            [--random-agent] [--version]
Required Arguments:
  -u URL, --url URL     Main URL to test
Optional Arguments:
  -c COOKIE, --cookie COOKIE
                        Cookie value to be requested with each successive
                        request. If there are multiple cookies, separate them
                        with commas. For example: `-c PHPSESSID=i837c5n83u4,
                        _gid=jdhfbuysf`.
  -o OUTPUT, --output OUTPUT
                        Output directory where files to be stored. Default is
                        the`files` folder where all files generated will be
                        stored.
  -d DELAY, --delay DELAY
                        Time delay between requests in seconds. Default is
                        zero.
  -q, --quiet           Set the DEBUG mode to quiet. Report only when
                        vulnerabilities are found. Minimal output will be
                        printed on screen.
  -v, --verbose         Increase the verbosity of the output (e.g., -vv is
                        more than -v).
  --user-agent USER_AGENT
                        Custom user-agent to be used. Only one user-agent can
                        be specified.
  --headers HEADERS     Comma separated list of custom headers you'd want to
                        use. For example: ``--headers "Accept=text/php,
                        X-Requested-With=Dumb"``.
  --exclude EXCLUDE     Comma separated list of paths or directories to be
                        excluded which are not in scope. These paths/dirs
                        won't be scanned. For example: `--exclude somepage/,
                        sensitive-dir/, pleasedontscan/`
  --timeout TIMEOUT     HTTP request timeout value in seconds. The entered
                        value may be either in floating point decimal or an
                        integer. Example: ``--timeout 10.0``
  --max-chars MAXCHARS  Maximum allowed character length for the custom token
                        value to be generated. For example: `--max-chars 5`.
                        Default value is 6.
  --crawl               Crawl the whole site and simultaneously test all
                        discovered endpoints for CSRF.
  --no-analysis         Skip the Post-Scan Analysis of Tokens which were
                        gathered during requests
  --malicious           Generate a malicious CSRF Form which can be used in
                        real-world exploits.
  --skip-poc            Skip the PoC Form Generation of POST-Based Cross Site
                        Request Forgeries.
  --display             Print out response headers of requests while making
                        requests.
  --update              Update XSRFProbe to latest version on GitHub via git.
  --random-agent        Use random user-agents for making requests.
  --version             Display the version of XSRFProbe and exit.
```

