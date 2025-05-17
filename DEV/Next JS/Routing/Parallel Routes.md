- allows to render several routes at once
- requires slots, made with @folder
	- do not affect route segment
	- rendered in the same Page component
	- streamed indepently
		- can have own loading.js and error.js
- children = implicit slot

	
Ex: 

![[Pasted image 20250221153022.png]]

```typescript file:app/layout.tsx
export default function Layout({
  children,
  team,
  analytics,
}: {
  children: React.ReactNode
  analytics: React.ReactNode
  team: React.ReactNode
}) {
  return (
    <>
      {children}
      {team}
      {analytics}
    </>
  )
}

```

- soft navigation: partial render, changes subpage within slot + maintain other slot subpages
- hard navigation: cannot determine active state for slots unmatching url => 404 or renders `default.js`
	- need default.js for children if next.js cannot recover active state of parent page
- useSelectedLayoutSegment() => accepts `parallelRoutesKey`: read active route segment within slot
	- navigates to `app/@auth/login` =>`loginSegment = loging`

Ex: 

```typescript file:app/layout.tsx 
'use client'
 
import { useSelectedLayoutSegment } from 'next/navigation'
 
export default function Layout({ auth }: { auth: React.ReactNode }) {
  const loginSegment = useSelectedLayoutSegment('auth')
  // ...
}
```

- can create a layout within slots to create tab navigation
- can be used to create conditional route rendering depending on user rights
- create modals with [[Intercepting Routes]]
	- content shareable via URL

Ex: Login Modal:

![[Pasted image 20250221161908.png]]

- Add default.js that returns null so that nothing rendered when not active

```typescript file:app/@auth/default.tsx
export default function Default() {
  return null
}
```

- intercept by changing `/login` to `(.)/login` in @auth
- add Page.tsx in (.)/login

```typescript file:app/@auth/(.)login/page.tsx
import { Modal } from '@/app/ui/modal'
import { Login } from '@/app/ui/login'
 
export default function Page() {
  return (
    <Modal>
      <Login />
    </Modal>
  )
}
```

