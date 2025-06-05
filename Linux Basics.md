Linux is an umbrella term to designate multiple OS that are based on UNIX, another operating system.
## Basic commands

The interaction with a Linux environment is usually done via a terminal. Here can be found a list of basic commands usable: [[Linux Terminal Basic Commands]]
## Permissions

Files have three possible permissions:
- Read
- Write
- Execute

When displaying a file using `ls -l`, we have the following output:

```shell-session
-rw-r--r-- 1 cmnatic cmnatic 0 Feb 19 10:37 file1
```

The column on the left displays the permission given to the file as `r`, `w` and `x`. The very first is a `-` for a file and a `d` for a folder.

After that, we can divide the letters in three columns. The first three letters will be the user permission (the we are logged in as), the three next ones are for the group our user belongs to, the last ones are for the other groups/users.
## Common directories

### /etc

This directory is the most important one because it is a commonplace to store files used by the OS. 

You have in particular the sudoers file, containing the users and groups defined on the OS.

As well you have the `passwd` and the `shadow` files they are used to store the passwords of the users.

### /var

This folder stores data that is frequently accessed by the system or written by services. One example is `var/log` which is where running services tend to write their logs.

### /root

It corresponds to the home folder of the `root` user. Compared to the other users, the `root` folder is not contained under the `/home` folder

### /tmp

Tmp is short for temporary and by definition the /tmp folder is volatile and is used for data that needs to be accessed once or twice. When the computer restarts the contents of that folder are reseted as well.

By default any user can write to this folder by default and when pentesting, it can be used to store enumeration scripts.

