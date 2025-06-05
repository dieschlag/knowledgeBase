In order to simplify the management of many computers based on Windows, we can use a Windows domain, which is a group of users and computers under the administration of a given business.

The idea is to centralise the administration of a Windows computer network in a single repository, called the Active Directory. 

The server that runs the AD is called the Domain Controller.

The advantages of using AD are:

- Centralised identity management: all users are configured from the AD
- Managing security policies: apply policies to all user/computers on the entire network directly

The core of the Windows Domain is Active Directory Domain Service, which acts as a catalogue that holds all the information of all the "objects" that exist in our network.

Here are some of the objects:

- users: most common object, known as security principals since they can be authenticated and assigned privileges over ressources. Two types:
	- people: persons in the organisation
	- services: users are defined to be used by services like [[IIS]] or [[MSSQL]]

- machines: a machine is created for every computer on the domain, also considered as security principals and assigned an account like a regular user with limited rights on the domain itself. They act as local administrators on the assigned computer and are not supposed to be accessed by anyone but the computer. Machine pwd are generally around 120 characters and are automatically rotated out. The account is generally the  machine name `D01` followed by $ `DO1$`

- security groups: group of privileges to which we can assign users that will inherit all privileges of the group. Considered as security principals as well since it can have privileges over the network.

Some default security groups:

- Domain Admins: admin rights over the entire network
- Server Operators: can administer Domain Controllers but cannot change administartive group memberships
- Backup Operators: allowed to access any files. cannot change administrative group memberships
- Account Operators: can create or modify other accounts in the domain
- Domain Users: all existing users in the domain
- Domain computers: same for computers
- Domain Controllers: all existing Domain Controllers in the domain

Objects like users, computers, groups that exist in the domain as gathered into Organizational Units (OUs). These OUs are generally used to defined a set of users and machines with similar policing requirements. Ex: people in the Sales Department are likely to be groupes inside an OU.

There are default containers in the AD:
- Builtin: contains default groups available to any windows host
- Computers: any machine joining the network is added here and can be moved according to the situation
- Domain Controllers: Default OU that contains all DCs in the network
- Users: default users and groups that apply to a domain-wide context
- Managed Service Accounts: accounts used by services in the WD

Note: 
- OUs are used to apply policies to a group of users/computers, a member can only be a member of one OU at a time
- Security Groups: to grant permissions over ressources, a user can be part of many groups at once

By default OU are protected against deletion, it is required to access the advanced features in the view of the AD to do so.

Then right click on the OU > Object > Uncheck "protection against accidental deletion"

## Delegation

It is possible to grant user control over the OU. This process is known as delegation where we grant users the possibility to do advanced tasks without needing a domain admin. Ex: granting the IT Support the possibility to reset other lower-privilege users.

To do so: 

Right click > Click "Delegate Control..."

Then register the users and objects that will receive these privileges, and then select which specific tasks they will be able to perform.

## Devices

Devices can be divided into three categories:

- workstations: most common devices in the AD, users in the domain generally are connected to a workstation and will use this device to do their work/browsing. They would never have a privileged user signed to them
- servers: second most common device, used to provide services to other users or servers
- domain controllers: third most common device, allow to manage the AD, most sensitive as they contains the hashed passwords of the users

## Group Policy Objects

The goal of OU is to apply a specific policy for each OU. These policies are managed through Group Policy Objects (GPO) which are a set of settings applied to OU, aimed either to machines or users.

To use GPO, we first need to create one under "Group Policy Objects" and apply them by putting these objects among the OU we want to affect (like putting a file inside a folder).

GPO apply to an OU and all sub-OU, which allows to define default policies on the entire domain and more specific ones for the subdomains.

Security Filtering can be applied to GPO so that it only applies to specific users. For example, a GPO could be applied only to authenticated users.

GPO are distributed using the network `SYSVOL`, stored in the DC. All users have access to that network and sync their GPO periodically and elements are shared inside the `C:\Windows\SYSVOL\sysvol\` folder on each DC.

It can take up to 2 hours for a GPO change to be applied on the network, but a computer can be forced to synch with the command:

```shell
PS C:\> gpupdate /force
```

## Authentication

With WD, all credentials are stored in the DC. When a user tries to authenticate, the service ask the DC if the credentials are correct. Two protocols are used:
- Kerberos: used by recent versions
- NetNTLM: legacy protocol, kept for compatibility purposes

### Kerberos

Users authenticating using Kerberos are assigned a ticket, which acts as a proof of authentication and allows an authenticated to prove they are already authenticated int the network.

**First step of the process:**

![[Pasted image 20250605140705.png]]

The user send its username and a timestamp encrypted using a key (based on the user password hash) to the Key Distribution Center (which stores as well the password hash so has access to the key), installed on the DC and in charge of creating the tickets. 

After sending it, the KDC answers with a Ticket Granting Ticket (TGT), which allows the user to request other tickets to access other services, without having to send again the credentials. It send as well a Session Key. The TGT is encrypted using krbtgt account's password hash. The session key is also included inside the TGT, so the KDC does not have to store a copy of the session key, since it can access it by decrypting the TGT (if needed).

**To connect to services:**

![[Pasted image 20250605141235.png]]

A user will use its obtained TGT to ask the KDC for a Ticket Granting Service (TGS). A TGS allows connection only for the service they were created for. In addition of the TGT, the client username username and timestamp are sent, but this time encrypted using the session key. The Service Principal Name is also sent, to specify the service and server we are trying to access.

As a response, the KDC sends along the TGS, a Service session key is sent, which is encrypted using the service owner password hash. As for the TGT, the TGS contains as well a copy of the TGS, so that the KDC does not have to store it on its side, and allowing the Service Owner to decrypt it if needed.

**To access the service:**

Finally, the TGS is sent to the desired service and authenticate. The service will use its stored owner's password hash to decrypt it.

![[Pasted image 20250605143052.png]]

### NetNTLM Authentication

The process is the following:

![[Pasted image 20250605143126.png]]

**Steps**:

1) client sends an authentication request
2) the server generates a random number and sends it to the client as a challenge
3) the client combines the challenge and the NTLM password hash to send back a Response
4) The server sends the challenge and the response to the DC
5) The DC uses the challenge and the locally stored NTLM hash to generate the response again and compare it to the response. The two are compared to validate the authentication. This validation is sent back to the server which will foward the answer to the client

## Trees

Imagine a company expands to another country, having a single huge domain and AD can be hard to manage.

To compensate, AD allows to integrate several domains, so that you can partition your network into units that can be managed independently. Each unit is its own domain with its own AD. These domains will be branches of a common domain, forming overall a tree:

![[Pasted image 20250605151108.png]]

Another advantage is that people in one subdomain does not have access to another domain. 

A new security group is added when using Trees, which is the Enterprise Admins group, granting a user admin rights over the entire enterprise domain.

## Forests

When having different domains on the same network, we call this set of domains a tree.

![[Pasted image 20250605151610.png]]

Now the issue with such a structure is to know how to do if someone from one tree needs to access data from another tree.

To do that, domains can have trust relationship. The simplest one is the one-way trust relationship:

![[Pasted image 20250605151818.png]]

Here if doain AAA trusts domain BBB, then users in BBB can access data in AAA.

By default, joining two trees into a forest will create a two-way trust relationship.

When creating a trust relationship, we need to specify what users from another domain can actually access.



