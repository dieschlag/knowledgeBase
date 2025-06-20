nPOP3 stands for Post Office Protocol Version 3 and is designed to allow the client to communicate with a mail server.

So a client relies on SMTP to send its messages and retrieves them using POP3.

Some common POP3 commands are:
- USER \<username>: identifies the USER
- PASS \<password: provides the user's password
- STAT: requests the number of messages and total size
- LIST: list all messages and their sizes
- RETR \<message_number>: retrieves the specified message
- DELE \<message_number>: maks a message for deletion
- QUIT: ends the POP3 session and apply changes such as deletions

POP3 listens on the TCP port 110 by default