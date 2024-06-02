
## Website

https://github.com/d3mondev/puredns

## Description

PureDNS ist ein schnelles Domain-Resolver- und Subdomain-Bruteforcing-Tool, das Wildcard-Subdomains und DNS-vergiftete Einträge präzise herausfiltern kann.  
  
Es verwendet Massdns, einen leistungsstarken Stub-DNS-Resolver, um Massensuchen durchzuführen. Mit der richtigen Bandbreite und einer guten Liste öffentlicher Resolver können Millionen von Abfragen in nur wenigen Minuten gelöst werden. Leider sind die Ergebnisse von massdns nur so gut wie die Antworten der öffentlichen Resolver. Die Ergebnisse werden oft durch falsche DNS-Antworten und falsch positive Ergebnisse von Wildcard-Subdomains verfälscht.  
  
PureDNS löst dieses Problem mit seinem Wildcard-Erkennungsalgorithmus. Es kann Platzhalter basierend auf den DNS-Antworten herausfiltern, die von einer Reihe vertrauenswürdiger Resolver erhalten wurden. Außerdem wird versucht, DNS-Poisoning zu umgehen, indem die mithilfe dieser vertrauenswürdigen Resolver erhaltenen Antworten validiert werden.  
  
Halten Sie das für nützlich? ⭐ Markieren Sie uns auf GitHub – es hilft!

## Usage

>Subdomain Brute-Forcing
`puredns bruteforce /opt/wordlist/SecLists/Discovery/DNS/ example.com --resolvers /opt/puredns/resolvers.txt > puredns.txt`