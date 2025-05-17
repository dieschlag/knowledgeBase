- custom request handlers for a given route (specific endpoints)
- defined in `route.js` inside
- can be nested like page.js and layout.js but cannot be at the same level than a page.js
- supported methods: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `HEAD`, `OPTIONS` | something else = 405
- NextRequest/NextResponse extends Request and Response APIs
- by default: no cach, but can be added, ex: add `export const dynamic = 'force-static'` in file
- special handlers remain static unless using Dynamic APIs: `sitemap.ts`, `opengraph-image.tsx`, `icon.tsx` other metadata files
- can set/read cookies 
```typescript file:app/api/route.ts 
import { cookies } from 'next/headers'
 
export async function GET(request: Request) {
  const cookieStore = await cookies()
  const token = cookieStore.get('token')
 
  return new Response('Hello, Next.js!', {
    status: 200,
    headers: { 'Set-Cookie': `token=${token.value}` },
  })
}
```

```typescript file:app/api/route.ts
import { type NextRequest } from 'next/server'
 
export async function GET(request: NextRequest) {
  const token = request.cookies.get('token')
}
```

- set/read headers:
```typescript file:app/api/route.ts
import { headers } from 'next/headers'
 
export async function GET(request: Request) {
  const headersList = await headers()
  const referer = headersList.get('referer')
 
  return new Response('Hello, Next.js!', {
    status: 200,
    headers: { referer: referer },
  })
}
```

```typecript file:app/api/route.ts
import { type NextRequest } from 'next/server'
 
export async function GET(request: NextRequest) {
  const requestHeaders = new Headers(request.headers)
}
```

- [[Redirecting (TODO)]]

```typescript file:app/api/route.ts
import { redirect } from 'next/navigation'
 
export async function GET(request: Request) {
  redirect('https://nextjs.org/')
}
```

- [[Dynamic Routes]]:

```typescript file:app/items/[slug]/route.ts
export async function GET(
  request: Request,
  { params }: { params: Promise<{ slug: string }> }
) {
  const slug = (await params).slug // 'a', 'b', or 'c'
}
```

