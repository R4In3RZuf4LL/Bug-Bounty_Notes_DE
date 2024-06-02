
## Recon

### Combine Tools

>[[Amass]] + [[Subfinder]] + [[RegEx (Reguläre Ausdrücke)]] + [[Grep]] + [[PureDNS]]
```
#!/bin/bash

# Schritt 1: Amass für DNS-Rekursion und Brute-Force
amass enum -d example.com -o amass_enum.txt -nocolor -v &&

# Schritt 2: Extrahieren von Subdomains aus der Amass-Ausgabe
grep -Eo '([a-zA-Z0-9.-]+\.)*example\.com' amass.txt > amass-subs.txt &&

# Schritt 3: Subfinder für zusätzliche Subdomains
subfinder -dL wildcards.txt -all -recursive -o subfinder.txt &&

# Schritt 4: Gau für zusätliche Subdomains
echo "https://example.com" | gau --subs > gau.txt &&

# Schritt 5: PureDNS für Brute-Force mit einer Wortliste
puredns bruteforce /opt/wordlist/SecLists/Discovery/DNS/dns-Jhaddix.txt example.com --resolvers /opt/puredns/resolvers.txt > puredns.txt &&

# Schritt 6: Zusammenführen und Sortieren der Ergebnisse
cat puredns.txt amass-subs.txt subfinder.txt gau.txt | sort -u > sorted.txt &&

# Schritt 7: Gotator für permutierte Subdomains
gotator -sub sorted.txt -perm /opt/wordlist/SecLists/Discovery/Web-Content/raft-small-directories.txt -depth 3 -numbers 10 -md | uniq > perm-subs.txt &&

# Schritt 8: HTTPX für lebende Subdomains
cat perm-subs.txt | httpx -ports 80,443,8000,8080,8888 -title -o subdomains-alive.txt &&

# Schritt 9: Katana für aktive Endpoints
katana -u subdomains_alive.txt -d 5 -ps -pss waybackarchive,commoncrawl,alienvault -kf -jc -fx -ef woff,css,png,svg,jpg,woff2,jpeg,gif,svg -o allurls.txt &&
```

`nuclei -list subdomains_alive.txt -as info -tags cve`

### Filter Subdomains

>Filter Subdomains with [[RegEx (Reguläre Ausdrücke)]]
`cat amass.txt | grep -Eo '([a-zA-Z0-9.-]+\.)*example\.com > amass-subs.txt'`

>Sort two files together
`cat amass-subs subfinder.txt | sort -u > sorted.txt`

### Subdomain Enumeration with Permutation

>[[Gotator]] + [[Uniq]]
`gotator -sub subdomains.txt -perm /opt/wordlist/SecLists/Discovery/Web-Content/raft-small-directories.txt -depth 3 -numbers 10 -md | uniq > perm-subs.txt`

### Port-Scanning

>[[Nmap]]
`nmap -sV -sC -p- example.com`

>[[Naabu]]
`naabu -list subdomains.txt -c 50 -nmap-cli '-sV -sC' -o naabu.txt`

#### Finding [[Origin-IP]]

>[[CloudFlair]]
`python3 cloudflair.py example.com`

>[[SecurityTrails]]
>We can use the https://securitytrails.com/ Dashboard to find[[ Origin-IP]]s

## Content-Discovery

### Directory Fuzzing

>[[DirSearch]]
 `dirsearch -u https://example.com -i 200,204,403,404 -x 500,502,501,429,503 -R 5 --random-agent -t 50 -F -o directory.txt -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt --recursion`



