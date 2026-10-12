# Next.js 16.3.6 interview questions and answers

This question bank uses the docs in the `v16.3.6` Next.js source checkout fetched with `opensrc path --cwd <lockfile-directory> next`, where the lockfile pinned `next@16.3.6`. The answers below follow that snapshot rather than older Next.js behavior. Each section links to the exact docs files in the tag. App Router is the main focus; Pages Router has its own section because it remains supported.

## Framework and rendering model

Sources: [App Router overview](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/index.mdx), [project structure](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/02-project-structure.mdx), [Server and Client Components](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/05-server-and-client-components.mdx), [rendering philosophy](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/02-guides/rendering-philosophy.mdx).

### 1. What does Next.js add to React?

Next.js supplies routing, server rendering, data and cache APIs, asset optimization, and build and deployment tools. React still defines the component model.

### 2. What are the App Router and Pages Router?

They are two supported routing systems. The App Router uses the `app` directory, React Server Components, nested layouts, and newer data APIs. The Pages Router uses the `pages` directory and APIs such as `getStaticProps` and `getServerSideProps`.

### 3. What is a route segment?

A folder under `app` represents one URL segment. A route becomes publicly accessible when the segment contains a `page` file or a `route` handler.

### 4. What is the difference between rendering and hydration?

Server rendering produces HTML for the initial visit. Hydration lets React attach event handling to the rendered Client Components in the browser. Server Components themselves do not hydrate.

### 5. Does using a Client Component mean no server-rendered HTML?

No. Next.js can prerender Client Components into HTML for the initial load, then hydrate them in the browser. The `'use client'` boundary controls where interactive code runs and what JavaScript ships.

### 6. Is a route entirely static or entirely dynamic?

Not necessarily. With Cache Components, a route can have a static shell and dynamic sections that stream when request data is available. Think about the boundary around each piece of work.

### 7. What is the React Server Component payload?

It is the server-produced representation of the rendered Server Component tree. Next.js uses it to reconcile the tree with Client Components during initial and subsequent navigations.

### 8. What is the root layout responsible for?

`app/layout.tsx` wraps the application and is required. It must render the `<html>` and `<body>` elements.

### 9. Where can non-route code live?

You can colocate components, utilities, and tests in `app`; a file is not a public route just because it is there. Private folders prefixed with `_` opt out of routing, and `src` can hold application code.

## Files, routes, and navigation

Sources: [layouts and pages](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/03-layouts-and-pages.mdx), [linking and navigating](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/04-linking-and-navigating.mdx), [dynamic routes](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/03-api-reference/03-file-conventions/dynamic-routes.mdx), [route groups](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/03-api-reference/03-file-conventions/route-groups.mdx), [parallel routes](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/03-api-reference/03-file-conventions/parallel-routes.mdx), [intercepting routes](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/03-api-reference/03-file-conventions/intercepting-routes.mdx), [template](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/03-api-reference/03-file-conventions/template.mdx).

### 10. What do `page.tsx` and `layout.tsx` do?

`page.tsx` renders the UI for a route. `layout.tsx` wraps child segments and persists across navigation, so it can keep shared UI and state mounted.

### 11. How does `template.tsx` differ from a layout?

A template creates a new instance for each navigation. Use it when child state or Effects should reset between visits; a layout normally persists.

### 12. How do dynamic segments work?

A folder such as `[slug]` matches one URL segment. The route receives `params`; in Next.js 16 these request-bound params are asynchronous, so await them in a Server Component.

### 13. What is the difference between `[...slug]` and `[[...slug]]`?

`[...slug]` matches one or more segments. `[[...slug]]` also matches the parent path with no captured segments.

### 14. What does `generateStaticParams` do?

It returns parameter values for dynamic routes that Next.js should prerender at build time. Other values can be generated on demand according to the route's configuration.

### 15. What is a route group?

A folder wrapped in parentheses, such as `(marketing)`, groups routes for organization or layouts without adding the group name to the URL.

### 16. What is a private folder?

A folder prefixed with `_` opts itself and its descendants out of routing. Use it for implementation files that should not become route segments.

### 17. What are parallel routes?

Folders named `@slot` let a layout render multiple route branches at once. A `default` file supplies fallback UI when a slot's active state cannot be recovered on a hard navigation.

### 18. What are intercepting routes for?

They let one route display another route within the current layout during client navigation, often for a modal. A direct visit can still render the destination as a full page.

### 19. Why use `<Link>` instead of a plain anchor for internal navigation?

`<Link>` enables Next.js client transitions and route prefetching. A normal anchor performs a full browser navigation.

### 20. When should you use `useRouter`?

Use it in a Client Component for programmatic navigation such as `push`, `replace`, or `refresh`. Prefer `<Link>` when navigation can be expressed as a link.

### 21. How do you read the current pathname and query string on the client?

Use `usePathname()` for the pathname and `useSearchParams()` for read-only URL search parameters. Both are App Router hooks from `next/navigation`.

### 22. Why should a layout not read a Page's search parameters as a prop?

Layouts persist during navigation and do not rerender for every query change. A Page receives `searchParams`, while a Client Component can use `useSearchParams()`.

### 23. What does `loading.tsx` do?

It creates a route-segment Suspense fallback while content streams. A nested `<Suspense>` boundary gives finer control over which part shows loading UI.

## Server and Client boundaries

Sources: [Server and Client Components](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/05-server-and-client-components.mdx), [server and client boundary](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/02-guides/server-and-client-boundary.mdx), [data security](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/02-guides/data-security.mdx).

### 24. Are App Router pages and layouts Server Components by default?

Yes. Add a Client Component when you need stateful interaction, Effects, event handlers, or browser APIs.

### 25. What does `'use client'` mark?

It marks a module as a client entry point. Its imports become part of the client module graph, so place the boundary around the smallest interactive subtree that fits.

### 26. Can a Client Component import a Server Component?

Do not import server-only code into a client module. A Server Component can pass rendered server content to a Client Component as a prop, including `children`.

### 27. What can cross from a Server Component to a Client Component?

Props must be serializable by React. Keep secrets, database clients, and non-serializable server objects on the server.

### 28. Where should providers go?

Put a provider in a Client Component and render it as deep in the tree as practical. This leaves more of the surrounding layout available for server rendering and static optimization.

### 29. Can a Server Component use `useState` or a browser API?

No. Move interactive state or browser access into a Client Component. A Server Component can still pass data and server-rendered children into that component.

### 30. Does `'use client'` mean the component runs only in the browser?

No. Its initial HTML can be prerendered on the server. The directive establishes the client bundle and hydration boundary.

### 31. How do you prevent server-only code from leaking into a client bundle?

Keep it in server modules, avoid importing those modules from client entries, and use the documented `server-only` marker to catch accidental imports.

## Data fetching and streaming

Sources: [fetching data](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/06-fetching-data.mdx), [loading convention](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/03-api-reference/03-file-conventions/loading.mdx), [linking and navigating](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/04-linking-and-navigating.mdx).

### 32. Where is the usual place to fetch data for an App Router page?

Fetch in an async Server Component with `fetch`, an ORM, or a database client. Server Components can read server-side resources without shipping credentials to the browser.

### 33. How can a slow request delay less of the page?

Move the slow part into a child component behind `<Suspense>` or a route's `loading.tsx`. Next.js can stream the ready shell while the child completes.

### 34. How do you avoid a sequential data-fetch waterfall?

Start independent requests before awaiting them and combine them with `Promise.all`. Data dependencies still have to run in sequence.

### 35. What happens if one request in `Promise.all` rejects?

The combined promise rejects. Use `Promise.allSettled` if each result must be handled independently.

### 36. When is client-side data fetching appropriate?

Use it for browser-specific or frequently updated client state. The docs also describe client data libraries and passing a Server Component promise to a Client Component that reads it with React `use`.

### 37. What does prefetching do?

It fetches route data ahead of likely navigation so a client transition can show a result sooner. `<Link>` participates in this behavior; the exact amount prefetched depends on route characteristics and configuration.

### 38. Can streaming change the HTTP status after headers have been sent?

No. A streamed page may already have a `200` status when later content calls `notFound()` or `redirect()`. If the status must be a real 404, check before streaming starts.

## Caching and revalidation in 16.3.6

Sources: [caching](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/08-caching.mdx), [revalidating](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/09-revalidating.mdx), [`use cache`](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/03-api-reference/01-directives/use-cache.mdx), [`cacheLife`](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/03-api-reference/04-functions/cacheLife.mdx), [`updateTag`](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/03-api-reference/04-functions/updateTag.mdx), [`revalidateTag`](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/03-api-reference/04-functions/revalidateTag.mdx), [caching without Cache Components](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/02-guides/caching-without-cache-components.mdx).

### 39. Is every `fetch` automatically cached in Next.js 16?

No. Do not apply old blanket caching rules. Check whether Cache Components is enabled and which caching API the code uses. In Cache Components, use `use cache` to mark async work as cacheable.

### 40. What is Cache Components?

It is an opt-in rendering and caching model enabled with `cacheComponents: true`. It allows a static shell, cached content, and request-time sections to coexist in one route.

### 41. What does `'use cache'` do?

It marks an async function, component, or route as cacheable. Next.js caches its result using a key derived from its inputs and relevant captured values.

### 42. Why must a function using `'use cache'` be async?

That is a constraint of the directive in this version. The directive goes at the top of the async function or module and caches its returned work.

### 43. What does `cacheLife` control?

It selects or defines a cache profile that controls how long content is fresh, when it can be revalidated, and when it expires. Use it inside cached work.

### 44. What does `cacheTag` do?

It attaches a tag to cached work so a later mutation or webhook can invalidate the relevant data without naming every route that uses it.

### 45. What does `revalidateTag(tag, 'max')` do?

It marks tagged content stale and uses stale-while-revalidate behavior. A later request can receive stale content while fresh content is generated.

### 46. When should you use `updateTag`?

Use it in a Server Action when the user must read their own write immediately. It expires matching tagged data so the next read waits for fresh data.

### 47. How does `revalidatePath` differ from tag invalidation?

It invalidates a named page or layout path. Tags follow the cached data across routes; path invalidation is tied to a route location.

### 48. Can `use cache` read `cookies()` or `headers()` directly?

No. Those are request-time APIs. Read them outside shared cached work and pass a serializable value into a cached function when appropriate, or use the documented private cache mode for personalized work.

### 49. What is the difference between React `cache()` and Next.js `'use cache'`?

React `cache()` deduplicates work within a server request. Next.js `'use cache'` participates in Next.js caching across renders according to its key, lifetime, and storage configuration.

### 50. What does `connection()` signal?

It tells Next.js to wait for an incoming request before continuing the render. Use it when work must happen at request time, such as a synchronous source of per-request data.

### 51. What is the static shell in partial prerendering?

It is the prerendered portion of a page. Dynamic sections behind Suspense stream into that shell when their request-time data is ready.

### 52. Can a Cache Components route with dynamic params prerender no sample params?

No. The `generateStaticParams` docs say it must return at least one param when Cache Components is enabled; an empty array causes a build error.

## Mutations, forms, and request APIs

Sources: [mutating data](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/07-mutating-data.mdx), [forms](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/02-guides/forms.mdx), [cookies](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/03-api-reference/04-functions/cookies.mdx), [headers](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/03-api-reference/04-functions/headers.mdx), [refresh](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/03-api-reference/04-functions/refresh.mdx).

### 53. What is a Server Function?

It is an async function marked with `'use server'` that can be called from client code through Next.js. A Server Action is a Server Function used for a mutation, often a form submission.

### 54. How does a Server Action receive form values?

When used as a form action, it receives a `FormData` argument. Validate that data on the server before changing persistent state.

### 55. Do Server Action forms work before client JavaScript loads?

Forms defined in Server Components support progressive enhancement and can submit before JavaScript loads. Client Component forms queue submissions until hydration.

### 56. Can a Server Action be called from an event handler?

Yes. Forms are common, but a Client Component can also invoke a Server Function from an event handler.

### 57. Where can you set or delete a cookie?

Use a Server Function or Route Handler, where response headers can be written. A Server Component can read cookies but cannot change them during rendering.

### 58. Are `cookies()` and `headers()` synchronous in Next.js 16?

No. These request APIs are async. The temporary synchronous compatibility from Next.js 15 was removed in version 16.

### 59. Why must a Server Action authorize its mutation?

It is a server endpoint reachable by a client. Treat its inputs as untrusted and check the current user's permission inside the Action, even if the UI hides the button.

### 60. How can a Server Action update cached UI after a write?

Use `updateTag`, `revalidateTag`, or `revalidatePath` according to the cache you need to invalidate. `refresh()` can also refresh the client router from a Server Action.

## HTTP endpoints, Proxy, and security

Sources: [Route Handlers](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/15-route-handlers.mdx), [Proxy](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/16-proxy.mdx), [proxy file convention](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/03-api-reference/03-file-conventions/proxy.mdx), [authentication](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/02-guides/authentication.mdx), [data security](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/02-guides/data-security.mdx).

### 61. What is a Route Handler?

An `app/.../route.ts` file exports HTTP method functions such as `GET` and `POST`. It handles a request with the Web `Request` and `Response` APIs, with Next.js extensions available where needed.

### 62. How does a Route Handler differ from a Server Action?

A Route Handler is an HTTP endpoint that outside clients can call. A Server Action is a React-integrated Server Function used for application mutations, often through a form.

### 63. Are `GET` Route Handlers always cached?

No. Check the route's data use and caching mode. With Cache Components enabled, `GET` handlers follow the same prerendering model as UI routes and run at request time when they read uncached or request-specific data.

### 64. Can a Route Handler use `'use cache'` in its handler body?

No. Extract cacheable work to an async helper and put `'use cache'` there. The handler itself remains the request boundary.

### 65. What replaced Middleware in Next.js 16?

The file convention is `proxy.ts` or `proxy.js`, and the function is called Proxy. The old `middleware` convention is deprecated.

### 66. What is Proxy good for?

It runs before a request completes and can redirect, rewrite, change headers, or return a response. Use its `matcher` to limit which paths it handles.

### 67. Should Proxy perform full authorization or slow data fetching?

No. The docs reserve Proxy for lightweight request decisions, such as an optimistic redirect. Verify permissions again where data is read or mutated.

### 68. Why should Proxy matchers exclude static assets when appropriate?

Without a matcher, Proxy runs for every request, including JavaScript, CSS, image optimization, and public assets. A broad auth redirect can accidentally block those assets.

### 69. Is checking authentication in a page enough to secure its Server Actions?

No. A Server Action can receive a direct POST. Authenticate and authorize the mutation inside the Action or its server-only data layer.

### 70. What is the difference between authentication and authorization?

Authentication establishes who the user is. Authorization checks whether that user may access or change a specific resource.

## Metadata, assets, and configuration

Sources: [metadata and OG images](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/14-metadata-and-og-images.mdx), [images](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/12-images.mdx), [fonts](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/13-fonts.mdx), [CSS](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/11-css.mdx), [environment variables](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/02-guides/environment-variables.mdx), [static exports](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/02-guides/static-exports.mdx).

### 71. How do you define static and dynamic metadata?

Export `metadata` for fixed values or `generateMetadata` for values that depend on route data. These exports are supported in Server Components, not Client Components.

### 72. How are Open Graph images generated?

Use a static metadata image file or a generated image route. `ImageResponse` can produce an image from JSX and CSS when the image depends on data.

### 73. What does `next/image` add?

It provides image sizing and optimization behavior, including responsive delivery and lazy loading. Remote images require permitted source configuration.

### 74. Why use `next/font`?

It optimizes and self-hosts fonts, reducing external runtime font requests. Both Google and local fonts are supported.

### 75. Which CSS approaches are documented?

The docs cover Tailwind CSS, CSS Modules, global CSS, and external stylesheets. Use CSS Modules for component-scoped styles and keep global CSS genuinely global.

### 76. What is special about `NEXT_PUBLIC_` environment variables?

Next.js includes them in the browser bundle. Their values are fixed at build time, so do not put secrets in them or expect an already built bundle to pick up a changed runtime value.

### 77. Can a static export run Server Components?

Yes, at build time. A static export cannot support features that require a server at request time, so check the unsupported-feature list before choosing it.

## Errors, redirects, and observability

Sources: [error handling](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/10-error-handling.mdx), [`error.tsx`](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/03-api-reference/03-file-conventions/error.mdx), [`notFound`](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/03-api-reference/04-functions/not-found.mdx), [`redirect`](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/03-api-reference/04-functions/redirect.mdx), [`after`](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/03-api-reference/04-functions/after.mdx), [instrumentation](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/02-guides/instrumentation.mdx).

### 78. How should expected and unexpected errors be handled differently?

Return a useful error state for expected failures such as validation. Let unexpected rendering failures reach an error boundary so `error.tsx` can show fallback UI.

### 79. What does `error.tsx` catch?

It catches uncaught errors in its child route segment and renders a fallback. It must be a Client Component and receives error and reset behavior for recovery.

### 80. What is `global-error.tsx` for?

It handles errors that escape the root layout. Its UI replaces the root document, so it must render its own `<html>` and `<body>`.

### 81. What does `notFound()` do?

It stops rendering that route segment and shows its nearest `not-found.tsx` UI. Calling it after streaming begins cannot change headers already sent.

### 82. How does `redirect()` behave in a Server Action?

It triggers navigation after the Action. For a progressively enhanced form submission, Next.js sends a 303 response; in other server contexts the default is a 307 redirect.

### 83. What is `after()` for?

It schedules work after a response or prerender completes, such as logging. It does not itself make a route dynamic; a static page's callback can run during build or revalidation.

## Pages Router questions

Sources: [Pages Router overview](https://github.com/vercel/next.js/blob/v16.3.6/docs/02-pages/index.mdx), [pages and layouts](https://github.com/vercel/next.js/blob/v16.3.6/docs/02-pages/03-building-your-application/01-routing/01-pages-and-layouts.mdx), [`getStaticProps`](https://github.com/vercel/next.js/blob/v16.3.6/docs/02-pages/04-api-reference/03-functions/get-static-props.mdx), [`getStaticPaths`](https://github.com/vercel/next.js/blob/v16.3.6/docs/02-pages/04-api-reference/03-functions/get-static-paths.mdx), [`getServerSideProps`](https://github.com/vercel/next.js/blob/v16.3.6/docs/02-pages/04-api-reference/03-functions/get-server-side-props.mdx), [API Routes](https://github.com/vercel/next.js/blob/v16.3.6/docs/02-pages/03-building-your-application/01-routing/07-api-routes.mdx).

### 84. Is the Pages Router removed in Next.js 16.3.6?

No. It remains supported. The docs recommend the App Router for newer React features, but a Pages Router application does not need an immediate rewrite.

### 85. How does file routing work in `pages`?

A file in `pages` becomes a route. `index.tsx` maps to its folder's root URL, and `[id].tsx` captures a dynamic segment.

### 86. What does `getStaticProps` do?

It supplies serializable page props during build-time prerendering. Its `revalidate` return value can enable incremental static regeneration.

### 87. What does `getStaticPaths` do?

For a dynamic page using `getStaticProps`, it lists paths to prerender. Its `fallback` setting controls what happens for a path not listed at build time.

### 88. What does `getServerSideProps` do?

It runs for each request and returns serializable props, a redirect, or a not-found result. It belongs to the Pages Router, not an App Router page.

### 89. What is an API Route?

A file under `pages/api` defines an HTTP endpoint using Node-style request and response objects. App Router Route Handlers use the Web Request and Response APIs instead.

### 90. How are shared layouts built in the Pages Router?

Use a shared layout through a custom App, or a per-page `getLayout` pattern. App Router layouts have their own file convention and nesting behavior.

## Changes interviewers may expect in version 16

Sources: [version 16 upgrade guide](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/02-guides/upgrading/version-16.mdx), [installation](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/01-installation.mdx), [Proxy](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/16-proxy.mdx), [Cache Components](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/08-caching.mdx).

### 91. Which bundler does `next dev` and `next build` use by default?

Turbopack is the default in Next.js 16. Use `--webpack` when a project still needs Webpack. A custom Webpack configuration can make the default Turbopack build fail until you migrate it or choose Webpack explicitly.

### 92. What changed for request-time APIs in version 16?

Synchronous compatibility was removed. Await `params`, `searchParams`, `cookies()`, `headers()`, and `draftMode()` where the docs require it.

### 93. What changed from `middleware.ts` to `proxy.ts`?

Next.js 16 renamed the convention and function to Proxy. The older Middleware name is deprecated, so new code should use `proxy.ts` and `proxy`.

### 94. Is Cache Components enabled by default?

No. Opt in with `cacheComponents: true` in `next.config`. Then use `use cache`, `cacheLife`, and Suspense boundaries to choose cached and request-time regions.

### 95. Does `next build` run the linter automatically?

No. Starting with Next.js 16, run your linter separately through a script or CI command.

### 96. Does Next.js 16 support React Compiler?

Yes. The upgrade guide describes stable built-in support behind its configuration option. Enable it deliberately and account for the extra compilation work.

### 97. What should you check before deploying a static export?

Check whether the application needs request-time APIs, Server Actions, or other server features unsupported by static export. `output: 'export'` generates files for static hosting.

## Fast interview scenarios

Sources: [linking and navigating](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/04-linking-and-navigating.mdx), [caching](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/08-caching.mdx), [revalidating](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/09-revalidating.mdx), [error handling](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/10-error-handling.mdx), [data security](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/02-guides/data-security.mdx).

### 98. A page reads `params.slug` synchronously and fails after a version 16 upgrade. What is the fix?

Make the page async and await `params` before reading `slug`. The version 16 upgrade guide removed synchronous access to request-time route parameters.

### 99. A dashboard waits for one slow widget before displaying anything. What would you change?

Put the widget behind a Suspense boundary or its segment's `loading.tsx` so the ready shell can stream first. Start independent data requests in parallel.

### 100. A product changes, but one page still shows old data. Should you call `revalidatePath` or invalidate a tag?

If several routes share product data, tag that cached data and use `updateTag` or `revalidateTag`. Use `revalidatePath` when the invalidation is specific to a route path.

### 101. A user must see their own mutation immediately. Which tag API fits?

Call `updateTag` in the Server Action after the write. It expires the tag immediately, so the next read waits for fresh data.

### 102. A client navigates to `/login` and sees a modal, but a refresh should show a full page. Which route conventions fit?

Combine an intercepting route with a parallel route slot. The intercepted client transition can show the modal while the direct route renders its page.

### 103. An auth check in Proxy redirects anonymous users, but a mutation can still be invoked directly. What is missing?

The Server Action needs its own authentication and resource authorization check. Proxy is an early navigation check, not the mutation's security boundary.

### 104. A Client Component imports a database module to fetch its initial data. What would you change?

Fetch in a Server Component and pass serializable results to the Client Component. Keep the database module server-only.

### 105. A site needs an endpoint for a third-party webhook. What would you create?

Create an App Router Route Handler, such as `app/api/webhook/route.ts`, export `POST`, verify the request, and update data and caches as needed.

### 106. A cached function needs the current locale from a request header. Where should the header be read?

Read it outside shared cached work with `await headers()`, then pass the locale as a serializable argument into the cached function so it becomes part of the cache key.

## Tooling, testing, and deployment

Sources: [Next.js CLI](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/03-api-reference/06-cli/next.mdx), [testing](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/02-guides/testing/index.mdx), [self-hosting](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/02-guides/self-hosting.mdx), [deployment](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/01-getting-started/17-deploying.mdx), [production checklist](https://github.com/vercel/next.js/blob/v16.3.6/docs/01-app/02-guides/production-checklist.mdx).

### 107. What do `next dev`, `next build`, and `next start` do?

`next dev` runs the development server. `next build` produces an optimized production build. `next start` serves that build, so run the build first.

### 108. What does `next typegen` provide?

It generates route-aware TypeScript definitions without a full build. CI can run it before `tsc --noEmit` to check route types.

### 109. How would you test an async Server Component?

The versioned testing guide recommends end-to-end tests when unit tools do not fully support async Server Components. Test the user-visible route behavior in a browser-like environment.

### 110. Which testing layers does the Next.js guide describe?

It distinguishes unit, component, integration, end-to-end, and snapshot tests. Pick the layer that can observe the behavior under test.

### 111. What changes when self-hosting multiple Next.js instances?

Coordinate shared caching and revalidation, and keep deployment versions consistent across instances. Server Functions also need compatible encryption configuration across instances.

### 112. Why can a reverse proxy affect streaming?

Some proxies buffer responses. Configure the proxy so streamed HTML can reach the browser as chunks rather than waiting for the full response.

### 113. What should a production review include beyond a successful build?

Check accessibility, security, metadata, type safety, caching behavior, Core Web Vitals, and bundle size for the routes users actually visit.

## Source and revision notes

- opensrc resolved the npm package `next` to **16.3.6** and cached the `vercel/next.js` repository at its `v16.3.6` tag. The source checkout includes the docs under `docs/` and `packages/next/package.json` reporting version `16.3.6`.
- The links above target the exact tag, so later edits to the live Next.js website do not silently change this guide's source material.
- To revise for a different version, pin that version in a lockfile, resolve it with `opensrc path --cwd <lockfile-directory> next`, and read the docs in that checkout before changing an answer.
