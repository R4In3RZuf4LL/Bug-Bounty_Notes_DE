
## Website

https://github.com/incogbyte/quickpress/tree/master

## Description

Scannen Sie URLs oder eine einzelne [[URL (Uniform Resource Locator)]] auf XMLRPC-WordPress-Probleme.

## Usage

- List of URLS

```bash
# urls without / at the end of URL!
cat urls.txt | quickpress -server http://burpcollaborator.net
```


- Single [[URL (Uniform Resource Locator)]]

```bash
quickpress -target https://target.com -server http://burpcollaborator.net
```

