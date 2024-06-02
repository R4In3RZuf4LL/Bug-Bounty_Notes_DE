## Website

https://github.com/owasp-amass/amass

## Description

Das OWASP Amass Project führt eine Netzwerkkartierung von Angriffsflächen und die Erkennung externer Assets durch, indem es Open-Source-Informationserfassung und aktive Aufklärungstechniken nutzt.

## Usage

>Find ASN numbers
`amass intel -org 'CompanyName' -v`

>Subdomain brutforcing
`amass enum -d example.com -brute -w /wordlist.txt -v`

