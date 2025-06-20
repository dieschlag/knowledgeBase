- Broken Access Control: 
	- Failing principle of the [[Least Privilege|least privilege]]
	- View/Modify account by using id
	- Browse page that requires authentication
- Other BAC
	- when checking for pathnames, like checking if path starts by "/admin", must be careful as the type of verification.
		- ex: if using `===` in TS or PHP, "/ADmin" will not be verified, and the page could be displayed
- Also wrong access when checking the data given by a POST form, using curl, we can specify a URL containing parameters, but also can specify data using -d when content is set to `application/x-www-form-urlencoded`. If same name is used to get both the query string, and POST data, app logic can favour POST data fields. It allows to control, for example in password reset fields what the user email is:
Ex: 
```shell-session
curl 'http://MACHINE_IP/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email=attacker@hacker.com'
```
=> email is sent to attacker address

- Cookies can also be leveraged, when they are stored in plain text, with explicit names
	- ex: `logged_in = true`
		- can set cookies to manipulate frontend behavior
	- 

