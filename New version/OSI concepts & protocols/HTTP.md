- HTTP: Hyper Text Transfer Protocol
- HTTP: Hypet Text Transfer Protocol Secure
- URL: instruction on how to access a resource on the Internet
![[Pasted image 20250228165331.png]]
- Make requests:
	- most basic: `GET/HTTP/1.1`
	- then headers
	- then blank space
	- then body
- Methods
	- GET
	- POST
	- PUT
	- DELETE
- Status Codes:
	- Groups
		- 100-199 - Information Response
		- 200-299 - Success
		- 300-399 - Redirection
		- 400-499 - Client Errors
		- 500-599 - Server Errors
	- Most common:
		- 200 - OK
		- 201 - Created
		- 301 - Moved Permanently: redirect to new page
		- 302 - Found: same as 301 but for temporary change
		- 400 - Bad Request
		- 401 - Not Authorised: require auth
		- 403 - Forbidden: not allowed whether logged in or not
		- 404 - Page Not Found
		- 405 - Method Not Allowed
		- 500 - Internal Service Error
		- 503 - Service Unavailable: server overloaded/maintenance
- Headers: 
	- Host: to specify which website it comes from when server hosts several websites
	- User-Agent: browser software/version number to format website properly
	- Content-Length: how much data to expect in the request
	- Accept-Encoding: tells server what type of compression methods the browser accepts (to transmit less data)
	- Cookie: sent to the server to persist data
	- Set-Cookie: to store which cookie gets sent back to server on each request
	- Cache-Control: how long to store the response in browser chache before request again
	- Content-Type: tells client what type of data is being returned, so that browser knows how to process data
	- Content-Encoding: method used to compress data
- Cookies:
![[Pasted image 20250228170917.png]]

HTTP stands for Hypertext Transfer Protocol and defines how a browser client communicates with remote servers. The secured version of this protocol is HTTPS, where the S simply stands for Secure.

Some methods are very common when dealing with this protocol:
- GET: to retrieve data from a server
- POST: to submit new data to the server
- PUT: to create new/update/overwrite resource on the server
- DELETE: to delete data on the server

HTTP will commonly use the port 80.
HTTPS commonly uses port 443.

