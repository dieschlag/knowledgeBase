- allows to perform dictionnary attack on hashes
- Automatic cracking:
```shell
john --wordlist=[path to wordlist] [path to file]
```
- Format specific cracking:
```shell
john --format=[format] --wordlist=[path to wordlist] [path to file]
```
 - generally if dealing with standard hash types, needs to add `raw-` when specifying format
 - To crack [[NTLM]], format flag =  `nt`
 - Crack /etc/shadow passwords
	 - need to combine /etc/passwd, need to use unshadow (included in John The Ripper suite):
```shell
unshadow [path to passwd] [path to shadow] > unshadowed.txt

john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt unshadowed.txt`
```
- Single Crack mode: takes info in username and work out possible passwords by changing letters
	- will also use info in [[GECOS]]
```shell
john --single --format=[format] [path to file]
```
- Custom rules
	- defined in `john.conf`, in `/opt/john/john.conf` or in `/etc/john/john.conf`
	- first line: `[List.Rules:RuleName]` to define rule name
	- use regex/signs to edit password:
		- Actions:
			-  `Az`: Takes the word and appends it with the characters you define
			- `A0`: Takes the word and prepends it with the characters you define
			- `c`: Capitalises the character positionally
		- Elements that can be added specified in `[]
			- `[0-9]`: Will include numbers 0-9  
			- `[0]`: Will include only the number 0
			- `[A-z]`: Will include both upper and lowercase  
			- `[A-Z]`: Will include only uppercase letters
			- `[a-z]`: Will include only lowercase letters
	- Ex: 
```
[List.Rules:PoloPassword]

cAz"[0-9] [!£$%@]"
```

```shell
john --wordlist=[path to wordlist] --rule=<RuleName> [path to file]
```

- Cracking password protected zip files:
	- need to use zip2john
```shell
zip2john zipfile.zip > zip_hash.txt

john --wordlist=/usr/share/wordlists/rockyou.txt output.txt
```
-  Cracking password protected RAR files
```
rar2john [rar file] > [output file]

`john --wordlist=/usr/share/wordlists/rockyou.txt output.txt`
```

- Cracking SSH Keys
```shell
/opt/john/ssh2john.py id_rsa > id_rsa_hash.txt

john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash.txt

```