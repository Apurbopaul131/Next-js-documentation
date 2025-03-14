# Next-js-documentation

**Rendering:** Rendering, in web development, is the process of converting code into viewable, interactive web content. Rendering can completed by two way:

1. Pre-rendering
2. Client side rendering

The performance optimizations of pre-rendering can completed in two way:

1. Static Site Generation
2. Server Site Rendering

**Client-side-rendering:** An request send from client to server webpage content then the server send js bundle files to client. The html content generate into the browser by combining with html,css and javascript bundles.

**Cons in client side rendering:** Less seo friendly and Initial loading state.

**Pre-rendering:** The static html content are generated into the server in build time. An request send from client to server for webpage content then the server send html content to the browser.

**Props:** More seo friendly, No loading state

**Cashing:** Caching stores data so it doesn't need to be re-fetched from your data source on every request.By default, Next.js automatically caches the returned values of fetch in the Data Cache on the server.

```javascript
// 'force-cache' is the default, and can be omitted
fetch("https://...", { cache: "force-cache" });
```

However, there are exceptions, fetch requests are not cached when:

1. Used inside a Server Action
2. Used inside a Route Handler that uses the POST method.

**Revalidation Data:** Revalidation is the process of purging the Data Cache and re-fetching the latest data. This is useful when your data changes and you want to ensure you show the latest information.

Cached data can be revalidated in two ways:

**1.Time-based revalidation:** Automatically revalidate data after a certain amount of time has passed

```javascript
fetch("https://...", { next: { revalidate: 3600 } });
//alternative wat to revalidate data cache
export const revalidate = 3600; // revalidate at most every hour
```

**2.On-demand revalidation:**Manually revalidate data based on an event

**error.js:** An error file defines an error UI boundary for a route segment.

It is useful for catching unexpected errors that occur in Server Components and Client Components and displaying a fallback UI.

```javascript
'use client' // Error components must be Client Components

import { useEffect } from 'react'

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  const [errorMessage, setErrorMessage] = useState(error.message)
  useEffect(() => {
    // Log the error to an error reporting service
    setErrorMessage(error.message);
  }, [error.message])

  return (
    <div>
      <h2>Something went wrong!</h2>
      <h2>{errorMessage}</h2>
      <button
        onClick={
          // Attempt to recover by trying to re-render the segment
          () => reset()
        }
      >
        Try again
      </button>
    </div>
  )
}
```

**generateStaticParams:**The generateStaticParams function can be used in combination with dynamic route segments to statically generate routes at build time instead of on-demand at request time.

```javascript
// Return a list of `params` to populate the [slug] dynamic segment
export async function generateStaticParams() {
  const posts = await fetch("https://.../posts").then((res) => res.json());

  return posts.map((post) => ({
    slug: post.slug,
  }));
}

// Multiple versions of this page will be statically generated
// using the `params` returned by `generateStaticParams`
export default async function Page({
  params,
}: {
  params: Promise<{ slug: string }>,
}) {
  const { slug } = await params;
  // ...
}
```
