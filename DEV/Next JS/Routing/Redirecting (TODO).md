## redirect function

- Used for Server Components
- returns 307 by default, 303 if used in server action
- internally throws an error, not to use inside `try/catch`
- can be called in client components during the rendering process, but not in event handlers => use `useRouter`
- accepts URLs to redirect to external links 

Ex: 

```typescript file:app/team/[id]/page.tsx
import { redirect } from 'next/navigation'
 
async function fetchTeam(id: string) {
  const res = await fetch('https://...')
  if (!res.ok) return undefined
  return res.json()
}
 
export default async function Profile({
  params,
}: {
  params: Promise<{ id: string }>
}) {
  const id = (await params).id
  if (!id) {
    redirect('/login')
  }
 
  const team = await fetchTeam(id)
  if (!team) {
    redirect('/join')
  }
 
  // ...
}
```
## permanent_redirect function

- premanently redirects user
- used after mutation of data (ex: username change)
- returns 308
- accepts URL and external links

Ex:

```typescript file:app/actions.ts hl:14
'use server'
 
import { permanentRedirect } from 'next/navigation'
import { revalidateTag } from 'next/cache'
 
export async function updateUsername(username: string, formData: FormData) {
  try {
    // Call database
  } catch (error) {
    // Handle errors
  }
 
  revalidateTag('username') // Update all references to the username
  permanentRedirect(`/profile/${username}`) // Navigate to the new user profile
}
```
## useRouter()

Ex:

```typescript file:app/page.tsx
'use client'
 
import { useRouter } from 'next/navigation'
 
export default function Page() {
  const router = useRouter()
 
  return (
    <button type="button" onClick={() => router.push('/dashboard')}>
      Dashboard
    </button>
  )
}
```

- prefer to use `<Link>` unless specific need
- use inside event handler

## redirects in next.config.json

- redirect incoming request to different path
- supports header, cookies, paths, query matching
- returns a 307 or 308 + permanent option
- may have limit on plateforms (Vercel)
- if limited, other solution: use middleware
- runs before middleware


TODO


