Refers to the injection of commands on a server, is a type of [[Remote Code Execution]]
## Origin of vulnerability

Happens when user input not controled and directly given to commands making command calls on the machine.

Ex with PHP:

![[Pasted image 20250420112540.png]]

## Detect Command Injections

#### Blind Command Injections

Occurs when there is command injection but no output is visible. Need to use payloads causing some delays (ex: by using `ping` or `sleep`) or forcing output (ex: by using `>` + `cat` on output).

#### Verbose Command Injection

Easiest, output of command is displayed right in the app, only need to test commands with clear outputs.

## Remediate Command Injection

Simple: add input sanitation.

Ex in php:

Here: restricts to only numbers
![[Pasted image 20250420113551.png]]

Here: add filtering on input before executing rest of function code
![[Pasted image 20250420113640.png]]

## Bypassing filters

Many things possible, generally consists in abusing the filtering logic, for example if app removes specific characters, can use their hexadecimal formats, which will be understood when command is executed.

For a list of Windows and Linux payloads to try to bypass filters: https://github.com/payloadbox/command-injection-payload-list.

