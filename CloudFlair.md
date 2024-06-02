
## Website

https://github.com/christophetd/CloudFlair

## Description

CloudFlair ist ein Tool zum Auffinden von Ursprungsservern von Websites, die durch CloudFlare (oder CloudFront) geschützt sind, die öffentlich zugänglich sind und den Netzwerkzugriff nicht angemessen auf die relevanten CDN-IP-Bereiche beschränken.  
  
Das Tool verwendet internetweite Scandaten von Censys, um exponierte IPv4-Hosts zu finden, die ein SSL-Zertifikat vorlegen, das mit dem Domänennamen des Ziels verknüpft ist. API-Schlüssel sind erforderlich und können von Ihrem Censys-Konto abgerufen werden.  
  
Weitere Einzelheiten zu dieser häufigen Fehlkonfiguration und zur Funktionsweise von CloudFlair finden Sie im begleitenden Blogbeitrag unter https://blog.christophetd.fr/bypassing-cloudflare-using-internet-wide-scan-data/.  
  
So sieht CloudFlair in Aktion aus.

```python
$ python cloudflair.py myvulnerable.site

[*] The target appears to be behind CloudFlare.
[*] Looking for certificates matching "myvulnerable.site" using Censys
[*] 75 certificates matching "myvulnerable.site" found.
[*] Looking for IPv4 hosts presenting these certificates...
[*] 10 IPv4 hosts presenting a certificate issued to "myvulnerable.site" were found.
  - 51.194.77.1
  - 223.172.21.75
  - 18.136.111.24
  - 127.200.220.231
  - 177.67.208.72
  - 137.67.239.174
  - 182.102.141.194
  - 8.154.231.164
  - 37.184.84.44
  - 78.25.205.83

[*] Retrieving target homepage at https://myvulnerable.site

[*] Testing candidate origin servers
  - 51.194.77.1
  - 223.172.21.75
  - 18.136.111.24
        responded with an unexpected HTTP status code 404
  - 127.200.220.231
        timed out after 3 seconds
  - 177.67.208.72
  - 137.67.239.174
  - 182.102.141.194
  - 8.154.231.164
  - 37.184.84.44
  - 78.25.205.83

[*] Found 2 likely origin servers of myvulnerable.site!
  - 177.67.208.72 (HTML content identical to myvulnerable.site)
  - 182.102.141.194 (HTML content identical to myvulnerable.site)
```

## Usage

```python
$ python cloudflair.py --help

usage: cloudflair.py [-h] [-o OUTPUT_FILE] [--censys-api-id CENSYS_API_ID] [--censys-api-secret CENSYS_API_SECRET] [--cloudfront] domain

positional arguments:
  domain                The domain to scan

options:
  -h, --help            show this help message and exit
  -o OUTPUT_FILE, --output OUTPUT_FILE
                        A file to output likely origin servers to (default: None)
  --censys-api-id CENSYS_API_ID
                        Censys API ID. Can also be defined using the CENSYS_API_ID environment variable (default: None)
  --censys-api-secret CENSYS_API_SECRET
                        Censys API secret. Can also be defined using the CENSYS_API_SECRET environment variable (default: None)
  --cloudfront          Check Cloudfront instead of CloudFlare. (default: False)
```


## Usage

```python
python3 cloudflair.py example.com
```