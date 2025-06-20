
https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions-and-mutations
## Resume

- asynchronous functions
- can be called from Server or Client components
- handle form submissions and data mutations
- add "use server" directive at top of async function  to define server action
- can be called in `action` attribute of a `<form>`
	- Server Components: support progressive enhancement (?): form submitted event if JS not loaded
	- Client Components: if JS not loaded, queue submissions, prioritize client hydration
- can be evoked from event handler (`useEffect`, etc.) and elements nested inside `<form>` elements (buttons, inputs, etc.)

## Convention

### For server components:

use inline function level or module level:

```typescript file:/app/page.tsx
export default function Page() {
  // Server Action
  async function create() {
    'use server'
    // Mutate data
  }
 
  return '...'
}
```


### For client components:

create new file and add directive:

```typescript file:app/actions.ts
'use server'
 
export async function create() {}
```

```typescript file:app/button.tsx 
'use client'
 
import { create } from './actions'
 
export function Button() {
  return <button onClick={() => create()}>Create</button>
}
```

functions defined in files with "use server" can be reused in both client and server components

### Passing as props

```
<ClientComponent updateItemAction={updateItem} />
```

```
'use client'
 
export default function ClientComponent({
  updateItemAction,
}: {
  updateItemAction: (formData: FormData) => void
}) {
  return <form action={updateItemAction}>{/* ... */}</form>
}
```

Next.js TypeScript plugin flags it but no worry, 
Props ending with action or Action are expected to receive Server Actions
heuristic. can't know if receiving function or server action, no worry
runtime type checking ensures no function is passed as prop

## Passing additional arguments

```typescript file:app/actions.ts
'use server'
 
export async function updateUser(userId: string, formData: FormData) {}
```

```typescript hl:6
'use client'
 
import { updateUser } from './actions'
 
export function UserProfile({ userId }: { userId: string }) {
  const updateUserWithId = updateUser.bind(null, userId)
 
  return (
    <form action={updateUserWithId}>
      <input type="text" name="name" />
      <button type="submit">Update User Name</button>
    </form>
  )
}
```

Server Action receives userId, in addition of form data

## Redirecting

Use redirect API:

```typescript file:app/actions.ts
'use server'
 
import { redirect } from 'next/navigation'
import { revalidateTag } from 'next/cache'
 
export async function createPost(id: string) {
  try {
    // ...
  } catch (error) {
    // ...
  }
 
  revalidateTag('posts') // Update cached posts
  redirect(`/post/${id}`) // Navigate to the new post page
}
```

## Cookies

Set cookies with cookies API: 

```typescript file:app/actions.ts
'use server'
 
import { cookies } from 'next/headers'
 
export async function exampleAction() {
  const cookieStore = await cookies()
 
  // Get cookie
  cookieStore.get('name')?.value
 
  // Set cookie
  cookieStore.set('name', 'Delba')
 
  // Delete cookie
  cookieStore.delete('name')
}
```
## Use in Event Handlers

```typescript file:app/like-button.tsx
'use client'
 
import { incrementLike } from './actions'
import { useState } from 'react'
 
export default function LikeButton({ initialLikes }: { initialLikes: number }) {
  const [likes, setLikes] = useState(initialLikes)
 
  return (
    <>
      <p>Total Likes: {likes}</p>
      <button
        onClick={async () => {
          const updatedLikes = await incrementLike()
          setLikes(updatedLikes)
        }}
      >
        Like
      </button>
    </>
  )
}
```

- here used with onClick
- can be useful when used inside a form element to save onChange, etc.
- can be used inside a useEffect
## Server-side form validation and pending requests

### With useActionState: 

```typescript file:app/actions.ts hl:3,5-9
'use server'
 
import { z } from 'zod'
 
const schema = z.object({
  email: z.string({
    invalid_type_error: 'Invalid Email',
  }),
})
 
export default async function createUser(prevState: any, formData: FormData) {
  const validatedFields = schema.safeParse({
    email: formData.get('email'),
  })
 
  // Return early if the form data is invalid
  if (!validatedFields.success) {
    return {
      errors: validatedFields.error.flatten().fieldErrors,
    }
  }
  
  const res = await fetch('https://...')
  const json = await res.json()
 
  if (!res.ok) {
	return { message: 'Please enter a valid email' }
  }
 
  redirect('/dashboard')
 
	  // Mutate data
}
```

```typescript file:app/ui/signup.tsx
'use client'
 
import { useActionState } from 'react'
import { createUser } from '@/app/actions'
 
const initialState = {
  message: '',
}
 
export function Signup() {
  const [state, formAction, pending] = useActionState(createUser, initialState)
 
  return (
    <form action={formAction}>
      <label htmlFor="email">Email</label>
      <input type="text" id="email" name="email" required />
      {/* ... */}
      <p aria-live="polite">{state?.message}</p>
      <button disabled={pending}>Sign up</button>
    </form>
  )
}
```

- zod: used to validate form fields before mutating data
- in Client Component, use `useActionState`: pass action and initialState as param
- server action receives new argument prevState
- pending: boolean, can be used to make specific display while action is being executed

### With useFormStatus

```typescript file:app/ui/button.tsx
'use client'
 
import { useFormStatus } from 'react-dom'
 
export function SubmitButton() {
  const { pending } = useFormStatus()
 
  return (
    <button disabled={pending} type="submit">
      Sign Up
    </button>
  )
}
```

```typescript file:app/ui/signup.tsx
import { SubmitButton } from './button'
import { createUser } from '@/app/actions'
 
export function Signup() {
  return (
    <form action={createUser}>
      {/* Other form elements */}
      <SubmitButton />
    </form>
  )
}
```

	- need to create separate component and nest it inside form

## Programmatic form submission 

**TODO**



## Optimistic updates

```typescript file:app/page.tsx
'use client'
 
import { useOptimistic } from 'react'
import { send } from './actions'
 
type Message = {
  message: string
}
 
export function Thread({ messages }: { messages: Message[] }) {
  const [optimisticMessages, addOptimisticMessage] = useOptimistic<
    Message[],
    string
  >(messages, (state, newMessage) => [...state, { message: newMessage }])
 
  const formAction = async (formData: FormData) => {
    const message = formData.get('message') as string
    addOptimisticMessage(message)
    await send(message)
  }
 
  return (
    <div>
      {optimisticMessages.map((m, i) => (
        <div key={i}>{m.message}</div>
      ))}
      <form action={formAction}>
        <input type="text" name="message" />
        <button type="submit">Send</button>
      </form>
    </div>
  )
}
```

- used to update UI before server action is finished (think chatbox)

## Behavior

- integrates with caching and revalidation architecture
	- action invoked => Next returns UI and new data in single roundtrip
- actions use POST method
- arg and return values must be serializable by React
- server actions = functions, can be used anywhere
- inherit runtime from page/layout they are used on
- inherit Route Segment Config

