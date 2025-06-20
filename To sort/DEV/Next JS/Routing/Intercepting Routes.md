- load route from other part of application within current layout, no need to switch context
	- ex: from /feed, clicks on photo, intercepts /photo/id route, masks the URL and overlays it over /feed => display in a modal
		- but when accessing /photo/id directly, picture does not appear in a modal
![[Pasted image 20250221160054.png]]

- (.) to match segment at the same level
- (..) to match one level above
- (..)(..) to match two level above
- ( ... ) to match from root
![[Pasted image 20250221160227.png]]
- used with [[Parallel Routes]] to create modals
![[Pasted image 20250221160320.png]]
