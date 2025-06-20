
## Instant Loading States

- `loading.js` used to render element while loading
- can pre-render skeletons, spinners, cover-photo, title, etc.
- `loading.js` content nested inside `layout.js`
- wraps Page children inside a `<Suspense>` 

Ex:

```typescript file:app/dashboard/loading.tsx
export default function Loading() {
  // You can add any UI inside Loading, including a Skeleton.
  return <LoadingSkeleton />
}
```

## Streaming with Suspense

### Server-Side Rendering of page

![[Pasted image 20250220151157.png]]
![[Pasted image 20250220151231.png]]
### With streaming

![[Pasted image 20250220151250.png]]
![[Pasted image 20250220151302.png]]

## SEO

- waits for fetches inside generateMetadata to finish before streaming, so that includes `<head>`
- streaming not impact SEO because server-rendered
