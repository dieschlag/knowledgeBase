
- \_folder: private folder (for organisation)
- (folder): to organise routes by subcategories
- link between pages: use `<Link>` component + href component
```typescript
<Link href={`/blog/${post.slug}`}>{post.title}</Link>
```
- image: use `<Image>` component
- import and apply font:
``` typescript
import { Geist } from 'next/font/google'
 
const geist = Geist({
  subsets: ['latin'],
})
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en" className={geist.className}>
      <body>{children}</body>
    </html>
  )
}
```

- if not possible to use entire font, specify weight:
```typescript
import { Roboto } from 'next/font/google'
 
const roboto = Roboto({
  weight: '400',
  subsets: ['latin'],
})
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en" className={roboto.className}>
      <body>{children}</body>
    </html>
  )
}
```

- import local font in `public` folder 
``` typescript
import localFont from 'next/font/local'
 
const myFont = localFont({
  src: './my-font.woff2',
})
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en" className={myFont.className}>
      <body>{children}</body>
    </html>
  )
}
```

- stream data thanks to`Suspense`
	- Promise from server component is passed to client component
	- can be used to have fallback when fetching data
```typescript
import Posts from '@/app/ui/posts
import { Suspense } from 'react'
 
export default function Page() {
  // Don't await the data fetching function
  const posts = getPosts()
 
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Posts posts={posts} />
    </Suspense>
  )
}
```

```typescript
'use client'
import { use } from 'react'
 
export default function Posts({
  posts,
}: {
  posts: Promise<{ id: string; title: string }[]>
}) {
  const allPosts = use(posts)
 
  return (
    <ul>
      {allPosts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  )
}
```

- stream entire page: add loading.js file in the page folder

 ```typescript file:app/blog/loading.tsx
export default function Loading() {
  // Define the Loading UI here
  return <div>Loading...</div>
}
```

- update data with server actions, "use server", should be written in a separate file
	- can be called inside client components
		- inside form actions/handler inside forms
		- in event handlers (onClick, etc.
		- use useActionState, with prevState taken as new argument in server action function to show pending state
		- state can be used to display a message

```typescript file:app/ui/signup.tsx
  const [state, formAction, pending] = useActionState(createUser, initialState)
```

