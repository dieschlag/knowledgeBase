https://nextjs.org/docs/app/building-your-application/data-fetching/incremental-static-regeneration#on-demand-revalidation-with-revalidatetag

Data fetching: 

with fetch if no dynamic API => renders as static page on build
using fetch, by default no cache, can be triggered by usign 'force-cache'

```typescript
const res = await fetch(`https://api.vercel.app/blog/${id}`, {
    cache: 'force-cache',
  })
```

if using ORM, wrap in cache :

```typescript

export const getPost = cache(async (id) => {
  const post = await db.query.posts.findFirst({
    where: eq(posts.id, parseInt(id)),
  })
 
  if (!post)  founotFound()
  return post
})

```

Only query made is route called several times.