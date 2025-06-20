- IMAP: Internet Message Access Protocol
- allows synchronization read/move/delete of messages
- useful when using several clients
- listens on TCP 143 by default
- Commands:
	- LOGIN \<usename> \<password>
	- SELECT \<mailbox>
	- FETCH <mail_number> <data_item_name> 
		- ex: fetch 3 body\[]
	- MOVE <sequence_set> \<mailbox>
	- COPY <sequence_set> <data_item_name>
	- LOGOUT

IMAP stands for Internet Message Access Protocol. It acts as a complementary protocol with POP3. 

POP3 is enough when working from one device but things get more complex when we are accessing our email from different devices. We need a protocol that allows synchronization.

IMAP uses more storage than POP3 as the email is kept on the server to be synchronized accross the email clients.

Some usual commands are:
- LOGIN \<username> \<password>: authenticates the user
- SELECT \<mailbox>: selects the mailbox to work with
- FECTCH \<mail_number> \<data_item_name>: to fetch information. Example: `FETCH 3 body[]` to fetch message number 3 with header and bodyù
- MOVE <sequence_set> \<mailbox>: moves the specified message to another mailbox 
- COPY <sequence_set> <data_item_name>: copies the specified messages to another mailbox
- LOGOUT: logs out

IMAP server listens generally on TCP port 143