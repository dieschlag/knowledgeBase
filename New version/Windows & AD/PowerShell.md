PowerShell is a Windows tool designed for task automation and configuration management. It combines a CLI and a scripting language built on the .NET framework.

PowerShell is now available of MacOS and Linux.

It was originally created because the existing CLi tools of Windows were too limited (cmd.exe). A new tool was need to better automate and to interact with modern Windows APIs. 

The power on PowerShell resides in its object-oriented usage.

## Objects

An object in Powershell is the same as in the rest of programming, having properties and methods linked to it. They encapsulate data and functionality.

A method in Powershell would correspond to a cmdlet (to pronounce command-let) returning an object with its own properties and methods again, which allows for more flexible data manipulation because it is not required to parse the text output (as it is when using cmd.exe).

## The shell

When presented with a Powershell terminal, there is a PS at the beginning of the line on the terminal.

The synthax of the cmdlets respect a Verb-Nous form. Some basic utility cmdlets are:
- Get-Command: retrieves a list of cmdlets, it is also possible to filter them using: 
	`-CommandType Function"`
- Antoher useful tool is `Get-Help element_name` which provides detailed information on cmdlets. In particular, the `-examples` flag can be given with this cmdlet to display a list of common ways to use the cmdlet

Aliases exist in Powershell and some are included by default for admin who are used to other CLI. As an example, `dir` is an alias for `Get-ChildItem` and `cd` is an alias for 
`Set-Location`.

It is also possible to add cmdlets by downloading them from other repositories. To do so, you can search for modules using `Find-Module`. Filtering can be applied by adding the flags `-Name searched_name`, and a more complete search with patterns can be done using 
`-Property "pattern*"`. Once we have found a specific module, it can be downloaded using `Install-Module module_name`.

## Filtering and Piping

Piping is done in the same way as in Linux using `|`. The difference is as we said above, objects are transmitted through the pipe. Here is an example:

```powershell
Get-ChildItem | Sort-Object Length
```

The `Where-Object` is commonly used to filter an output based on the properties of the output:

```powershell
Get-ChildItem | Where-Object -Property "Extension" -eq ".txt" 
```

In addition of the -eq flag, other can be given:
- `-ne`: not equal
- `-gt`: greater than
- `-ge`: greater than or equal to
- `-lt`: less than
- `-lt`: les
- s than or equal to

To filter with patterns, `-like` can be used:

```powershell
Get-ChildItem | Where-Object -Property "Name" -like "ship*"  
```
To retrieve specific properties of a returned object, we can pipe with `Select-Object property1,property2,...`:

```powershell
Get-ChildItem | Select-Object Name,Length 
```

To search for specific texte elements inside of text files, similar to `grep`, we can use `Select-String`:

```powershell
Select-String -Path ".\captain-hat.txt" -Pattern "hat" 
```
## System and Network Information

Useful cmdlets are:
- `Get-ComputerInfo`: retrieves comprehensive system information (hardware spec, BIOS, etc.)
- `Get-LocalUser`: lists all local users on the system
- `Get-NetIPConfiguration`: similar to ipconfig, returns info on network interfaces, IP addresses, DNS servers, gateway config, etc.
- `Get-NetIPAddress`: shows details for all IP addresses configured on the system, including those not active
## Real-time analysis

List of useful cmdlets:
- `Get-Process`: detailed view of all currently running processes, CPU memory usage, etc.
- `Get-Service`: retrieval of info about status of a machine, services running, stopped, paused, etc. Used for troubleshooting, and to hunt for anomalous services installed on the system
- `Get-NetTCPConnection`: displays current TCP connection, give insights on local and remote endpoints. Useful for incident response, can uncover hidden backdoors or established connections
- `Get-FileHash`: to generate file hash. Useful in incident response and malware analysis, helps verify integrity and detect potential tampering

## Scripting

Scripting refers to the process of writing and executing a series of commands contained in a text file to automate tasks we would usually perform by hand in a shell.

To execute a script, we use `Invoke-Command -FilePath path -ComputerName computer_name `, which we can use on remote computers as well as our local one.
