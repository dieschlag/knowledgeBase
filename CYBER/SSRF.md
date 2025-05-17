- SSRF: Server-Side Request Forgery

Occurs when remote urls can be called via parameters

To find SSRF: check for suspicious parameters (server, dst, etc.) 

Can be regular: attacker receives info on screen
Can be blind: attacker receives no info

Can be used for:
- access unauthorized areas
- access to customer data
- ability to scale to internal networks
- reveal authentication tokens/credentials

It uses a parameter in urls to define remote server which is fetched

Photo above: to get API key

Usage:
- make api calls to remote server:
![[Pasted image 20250404000846.png]]
- Retreive API keys:
![[Pasted image 20250311033913.png]]

Solutions:
- Deny List: accept all requests except from certain ressources
Can be resolved by using other localhost references like:
`0, 0.0.0.0, 0000, 127.1, 127.*.*.*, 2130706433, 017700000001`

- Allow List: all requests get denied unless they appear on a list or match a pattern
Last one can be be circumvented by creating a subdomain matching the pattern

- Open Redirect: open redirect endpoint: open endpoint on the server to automatically redirect users

Ex: the link https://website.thm/link?url=https://tryhackme.com, created to count the number of visitors. Possible SSRF (TO CHECK) 

One IP in cloud environment to try to reach via ssrf: contains meta data for the deployed server