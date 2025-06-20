Incremental Static Regeneration: 

- update static content without rebuilding site
- prerendering
- cache-control headers added to pages
- 
```typescript file:app/blog/[id]/page.tsx
export const revalidate = 60

export const dynamicParams = true // or false, to 404 on unknown paths
 
export async function generateStaticParams() {
  const posts: Post[] = await fetch('https://api.vercel.app/blog').then((res) =>
    res.json()
  )
  return posts.map((post) => ({
    id: String(post.id),
  }))
}

export default async function Page({
  params,
}: {
  params: Promise<{ id: string }>
}) {
  const { id } = await params
  const post = await getPost(id)
 
  return (
    <article>
      <h1>{post.title}</h1>
      <p>{post.content}</p>
    </article>
  )
}

```

- generateStaticParams: defines static params fetched on build time (prerendering)
- revalidate: defines time after which page is prerendered again (updates cache)
- dynamicParams: defines behavior for not prerendered pages:
	- true: generate and cache page on demand (for example if one of the /page/\[id] was not prerendered because not present on last page prerendering)
	- false: error 404

## On demand revalidation

### with `revalidatePath`

```typescript file:/app/actions.ts
'use server'
 
import { revalidatePath } from 'next/cache'
 
export async function createPost() {
  // Invalidate the /posts route in the cache
  revalidatePath('/posts')
}
```

post route called => revalidate cache for /posts pages

### with `revalidateTag`

Goal: revalidate specific fetch, not entire paths
Prefer revalidate entire paths

#### with fetch and `tags`:

```typescript file:/app/blog/page.tsx hl:3
export default async function Page() {
  const data = await fetch('https://api.vercel.app/blog', {
    next: { tags: ['posts'] },
  })
  const posts = await data.json()
  // ...
}
```

#### with ORM and `unstable_cache`:

```typecript file:app/blog/page.tsx
import { unstable_cache } from 'next/cache'
import { db, posts } from '@/lib/db'
 
const getCachedPosts = unstable_cache(
  async () => {
    return await db.select().from(posts)
  },
  ['posts'],
  { revalidate: 3600, tags: ['posts'] }
)
 
export default async function Page() {
  const posts = getCachedPosts()
  // ...
}
```

#### with Server Actions:

```typescript file:/app/actions.ts
'use server'
 
import { revalidateTag } from 'next/cache'
 
export async function createPost() {
  // Invalidate all data tagged with 'posts' in the cache
  revalidateTag('posts')
}
```