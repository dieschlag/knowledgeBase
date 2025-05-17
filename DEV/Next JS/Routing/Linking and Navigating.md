

## Link Component

- replaces `<a>`
- provide prefetching
- recommended way to navigate between routes
- import from `next/link
- add `href`

Ex: 

```typescript file:app/page.tsx
import Link from 'next/link'
 
export default function Page() {
  return <Link href="/dashboard">Dashboard</Link>
}
```

## useRouter()

Ex:

```typeecript file:app/page.tsx
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

- prefer to use `<Link>` unless specific need (like programmatic navigate user)

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

## Native History API


### window.history.pushState

- adds new entry to browser history stack

Ex:

```typescript hl:11
'use client'
 
import { useSearchParams } from 'next/navigation'
 
export default function SortProducts() {
  const searchParams = useSearchParams()
 
  function updateSorting(sortOrder: string) {
    const params = new URLSearchParams(searchParams.toString())
    params.set('sort', sortOrder)
    window.history.pushState(null, '', `?${params.toString()}`)
  }
 
  return (
    <>
      <button onClick={() => updateSorting('asc')}>Sort Ascending</button>
      <button onClick={() => updateSorting('desc')}>Sort Descending</button>
    </>
  )
}
```

### window.history.replaceState

- to replace the current state of the browser's history stack, user not able to navigate to previous state

Ex:

```typescript hl:11
'use client'
 
import { usePathname } from 'next/navigation'
 
export function LocaleSwitcher() {
  const pathname = usePathname()
 
  function switchLocale(locale: string) {
    // e.g. '/en/about' or '/fr/contact'
    const newPath = `/${locale}${pathname}`
    window.history.replaceState(null, '', newPath)
  }
 
  return (
    <>
      <button onClick={() => switchLocale('en')}>English</button>
      <button onClick={() => switchLocale('fr')}>French</button>
    </>
  )
}
```