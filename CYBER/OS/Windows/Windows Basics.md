
Windows dates back to 1985 and is nowadays the most dominant operating system for home and corporate use.

Many versions of Windows have been released:
- Windows XP: a popular version and had a long-running
- Windows Vista: a complete overhaul of the OS which had many issues and wasn't received well by users
- Windows 7: first version to be marked with an end of support date at the same time it was launched
- Windows 8.x: short-lives
- Windows 10
- Windows 11: comes in two flavors (Home and Pro)
- for servers, current version: Windows Server 2025

When Microsoft announced the end-of-life date for Windows XP, it caused global panick and was a nightmare for companies who had to update and for providers which might lose client if they didn't make the migration to the new version.

That's why the end-of-life date for Windows 7 was given at the same time as it was released, to avoid this sudden turmoil in the community.

## File System

The current file system used by windows is New Technology File System (NTFS). Before that FAT16/FAT32 was used and HFPS (High Performance File System). 

Some FAT partitions can still be seen nowadays, for example in USB devices, MicroSD cards, etc.

NTFS is known as a journaling system. In case of a failure, a file or folder can be restored using the information found in a specific log file. In addition, it solved the issues of previous file systems:
- it accepts files of size > 4Go
- it allows to set specific permissions on files
- allow compression and encryption

Encryption is done with Encryption File System (EFS)

Here is a list of permissions that can be added on NTFS volumes:
- Full control
- Modify
- Read and Execute
- List folder contents
- Read
- Write

As well, here is a list of permissions that can be applied to a file or a folder:
- Read
- Write
- Read & Execute
- List Folder Contents
- Modify
- Full Control

Another feature of NTFS is Alternate Data Streams.  Each file has at least one stream of data, and ADS allows a file to have several. Such streams are not displayed by the Windows Explorer, requiring to use 3rd-party tools.

ADS can be used to hide malware inside files but it is not used only in a malicious way. For example when downloading a file from the internet, a specific data stream is added to the file, so that the system knows this file has been downloaded from the internet.

## Windows folder

The `C:\Windows` is the folder containing Windows and does not necessarily reside in the C directory. To keep track of the position of this folder, an environment variable is associated to it: `%windir%`.

One folder of `\Windows` is `System32` which holds the critical files for the OS. So caution is required when interacting with it.

## User accounts

User accounts can be two things: 
- Administrator
- Standard User

The users are located under the folder `C:\Users` and each user will have the same default folders:
- Desktop
- Documents
- Downloads
- Music
- Pictures

Another way to access user information is to access `Local User and Group Management`.

Since a user does not need admin privileges for most of his actions, to protect user with local admin rights, the User Account Control was introduced: the session of the user runs by default without its admin rights, when an admin operation is required, a prompt will ask for user confirmation that the action should be executed.

UAC acts as a form of protection because a malware executed on a machine will only run with the standard user permissions and not admin ones.
















- system environment variable for `/Windows` is `%windir%`
- \Windows\System32 folder: files critical for OS
- Admin: do everything
- Standard User: cannot install programs, only modify folders
- change user type in Administrator & Standard User settings
- C:\Users: location for all Users
- User Account Control: by default user does not run with admin rights, prompts to the user for confirmation before installing

- Windows passwords: use NTLM, variant of MD4, identical to MD4/MD5 hashes
	- stored in the SAM (Security Accounts Manager)

To manage many windows systems: [[Active Directory]].

Some basic command lines used inside the default cli of windows can be found here: [[MS Windows Command Prompt Basic Commands]]