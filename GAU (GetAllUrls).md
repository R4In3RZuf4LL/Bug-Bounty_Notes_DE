## Website

https://github.com/lc/gau

## Description

Getallurls (GAU) ruft bekannte URLs von AlienVaults Open Threat Exchange, der Wayback Machine, Common Crawl und URLScan für jede bestimmte Domain ab. Inspiriert von Tomnomnoms Wayback-URLs.

## Usage

```
$ printf example.com | gau
$ cat domains.txt | gau --threads 5
$ gau example.com google.com
$ gau --o example-urls.txt example.com
$ gau --blacklist png,jpg,gif example.com
```