MS Windows Command Prompt (`cmd.exe`) is the default command-line interpreter of Windows.

## Basic commands

Here are a few commands and their usage:

To check you current path, use `set`.

To check OS version, use `ver`. To list more information, we can use `systeminfo`.

If the output is too long, we can pipe the output with `more`

Help can be provided on a command by using `help <command>`.

To clear the command prompt screen, we can use `screen`.

## Networking

We can check our internet connection using `ipconfig`, which will output our IP address, the subnet mask and the default gateway.

To have more information, we can do `ipconfig /all`.

To check if we can access a specific server, we can type `ping target_name`, which works in the same way as [[Ping|ping]] on Linux.

The tool `traceroute` on Linux actually corresponds to `tracert target_name` and behaves in the same way.

To check the ip address of a domain name, we can use `nslookup domain_name` which looks up a domain name and returns its IP address. To specify the server we want to use to check the DNS record, we can use `nslookup domain_name server_name`.

To display the current network connections we can use `netstat` with no arguments. Some options are:
- -a: displays all established connections and listening ports
- -b: shows as well the program associated with each listening port and established connection
- -o: reveals the process ID associated with the connection
- -n: uses numerical form for the addresses and port numbers

## File and Disk Management

To travel between folders: `cd`.

To display child directories: `dir`. With some options:
- `dir /a`: displays hidden and system files as well
- `dir /s`: displays files in current directory and all subdirectories

To have a visual display of the child directories: `tree`.

To create a directory: `mkdir directory_name`.

To delete a directory: `mrdir`

To display the type of a file: `file file_name`.

If the file is too long, display its content using `more file_name`.

To copy files, use `copy file_name copy_destination`.

They can be moved as well using: `move current_location new_location`.

To delete, `erease file_name` or `del file_name` can be used.

## Task and Process Management

List the running processes using `tasklist`. To really be useful, filtering should be applied, whose usage can be found doing `tasklist /?` to find more info on the possible flags.

An example of filtering :

```shell-session
tasklist /FI "imagename eq sshd.exe"
```

Here `/FI` is used to set the filter "image name = sshd.exe".

With a given PID, we can kill a process using `taskkill /PID PID_value`
