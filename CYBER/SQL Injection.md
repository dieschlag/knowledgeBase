## Origin

Happens when user-provided data is included in the SQL query. SQL operators and special characters can then be used to make specific query.

Example of an SQL injection:
```
SELECT * FROM users WHERE username = 'John' AND password = 'abc' OR 1=1;-- -';
```
comes from login with:
`Username: John`
`Password: abc' OR 1=1;-- -`

1=1 always verified so prints what we want, is a way to skip the AND, checking the password
## Types of SQL Injections

Three types:
- [[#In-Band SQLi]]: output is displayed directly on the browser
- [[#Blind SQLi]]: no output shown on the website, but different versions and methods possible, depending on the situation 
- Out-of-Band SQLi: least common since requires server to have specific config. The payload added to input will force the app's logic to make an external network call towards a machine controlled by the hacker with targeted information 

## Attack Steps

1) Find proof of sqli: in-band or blind 
2) determine database used: find a way to know the results of the `database()` query
3) determine tables: find all possibilities in `information_schema.tables` table where `table_schema` is set as the result of previous step
4) describe target tables: find all possibilities in `information_schema.columns` where `table_schma` is set as result of (1) and `table_name` is set as result of (2)
5) get elements inside of target table: use description found in (3) to query elements

## In-Band SQLi

Easiest of the two types, the proof of the SQLi will be directly visible in the browser by displaying unexpected elements according to your test of SQLi. Then simply add correct payload, bypassing the validation to make the steps above.
## Blind SQLi

Close to In-Band, but more difficult because result and proof of SQLi is different.

### Proof of SQLi

Instead of visual elements, we need to rely on unexpected app behavior to make sure that our payload is valid.
#### Boolean based

Is used when the route returns true of false, yes or no, etc. based on the query success, which can be indicators of a SQLi success when a positive is returned when it shouldn't.

Ex: Authentication Bypass: if the app only checks that user query with given auth parameters returns one element, then bypassing authentication by forcing the query to return one element is a proof of SQLi.
#### Time based

Is used when boolean based is not possible: app behavior does not change whether the query returns an element or not. Then we can try to delay the query using a specific payload. If the query is delayed when it shouldn't, then we have proof of SQLi.

Ex: adding `UNION SELECT SLEEP(5),1,2,3,4` with the right amount of numbers matching the table column count.

### Exploiting the SQLi

With blind attacks, making direct calls is not possible, since the output is not returned by the route. 

Then we can make matching tests in our queries using `LIKE` operator and progressive patterns, with `a%`, `b%`, etc. to find the first letter of an element (database, table, column, column element), then the second, etc.

To make sure that the name is correct use `= element_name` instead of `LIKE` to check validity.
## Remediation

Several things to do:
- have input validation
- have prepared queries: the developer writes the query, then adds user inputs as parameters
- escape user inputs: add `\` to special characters so they are parsed as regular string and not special characters

## Automation

Automation can be done using [[SQLMap]]