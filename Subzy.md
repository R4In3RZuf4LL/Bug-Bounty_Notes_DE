## Website

https://github.com/PentestPad/subzy

## Description

Subdomain takeover tool which works based on matching response fingerprints from [can-i-take-over-xyz](https://github.com/EdOverflow/can-i-take-over-xyz/blob/master/README.md)

## Examples

`subzy run --targets list.txt`

## Usage

```
$ subzy --help
Subdomain takeover tool

Usage:
  subzy [command]

Available Commands:
  help        Help about any command
  run         Run subzy
  update      Update local fingerprints.json file
  version     Print subzy version

Flags:
  -h, --help   help for subzy

Use "subzy [command] --help" for more information about a command.
```

If you get an error `exec format error: ./subzy`, you need to [install Golang](https://golang.org/doc/install) for your OS and compile the program by running `go build -o subzy main.go` which will generate new `subzy` binary file

### Options

[](https://github.com/PentestPad/subzy#options)

Only required flag for `run` subcommand(`r` short version) is either `--target` or `--targets`

`--target` (string) - Set single or multiple (comma separated) target subdomain/s  
`--targets` (string) - File name/path to list of subdomains  
`--concurrency` (integer) - Number of concurrent checks (default 10)  
`--hide_fails` (boolean) - Hide failed checks and invulnerable subdomains (default false)  
`--https` (boolean) - Use HTTPS by default if protocol not defined on targeted subdomain (default false)  
`--timeout` (integer) - HTTP request timeout in seconds (default 10)  
`--verify_ssl` (boolean) - If set to true, it won't check site with invalid SSL

Target subdomain can have protocol defined, if not `http://` will be used by default if `--https` not specifically set to true.

- List of subdomains
    
    - `./subzy run --targets list.txt`
- Single or multiple targets
    
    - `./subzy run --target test.google.com`
    - `./subzy run --target test.google.com,https://test.yahoo.com`

### Command aliases

[](https://github.com/PentestPad/subzy#command-aliases)

Each `subzy` subcommand has its own short version. Running `subzy version` or `subzy v` is the same.

- run - r
- update - u
- version - v