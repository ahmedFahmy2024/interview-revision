# General frontend interview questions and answers

This guide covers browser security, identity, authorization, HTTP, asynchronous behavior, performance, and state management. Each answer is a starting point for an interview response. Explain the trade-off and give an example from a project when you revise it.

## Study checklist

- [ ] Explain the answer without memorizing the wording.
- [ ] Give one concrete example or failure mode.
- [ ] Identify which checks belong on the server.
- [ ] For a design choice, state what would change your decision.

## Browser security

Sources: [MDN cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies), [OWASP CSRF prevention](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html), [OWASP XSS prevention](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html), [MDN CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS).

### 1. What does `HttpOnly` do on a cookie?

It prevents JavaScript from reading or changing the cookie through `document.cookie`. The browser can still send that cookie with requests. It reduces direct session theft through XSS but does not stop injected code from making requests as the user.

### 2. What do `Secure` and `SameSite` do?

`Secure` restricts cookie transmission to HTTPS, apart from browser-specific localhost handling. `SameSite` controls when cookies accompany cross-site requests: `Strict` is narrowest, `Lax` allows some top-level navigations, and `None` allows cross-site use but requires `Secure`.

### 3. What do `Domain`, `Path`, and cookie prefixes change?

`Domain` and `Path` control where the browser sends a cookie; they are not access-control checks. A host-only cookie omits `Domain`. A `__Host-` cookie requires `Secure`, `Path=/`, and no `Domain`, which helps prevent subdomains from setting it.

### 4. What is CSRF?

Cross-site request forgery tricks a user's browser into sending an unwanted authenticated request. Cookie-based sessions are exposed because browsers attach eligible cookies automatically. Defenses include CSRF tokens, appropriate `SameSite` settings, and server-side `Origin` or `Referer` checks where suitable.

### 5. Does `HttpOnly` or CORS prevent CSRF?

No. `HttpOnly` controls JavaScript access to a cookie. CORS controls whether browser JavaScript may read a cross-origin response; some cross-origin requests can still be sent. The server must protect state-changing requests.

### 6. What is XSS, and what are the main defenses?

Cross-site scripting runs attacker-controlled code in a site's origin. Treat untrusted content as data, use context-appropriate output encoding, sanitize allowed HTML, and avoid unsafe DOM sinks. A Content Security Policy adds protection but does not replace safe rendering.

### 7. Why is `innerHTML` risky?

It parses a string as HTML. If the string contains untrusted content, the browser may create dangerous elements or attributes. Use `textContent` for plain text and a reviewed sanitizer when users must supply HTML.

### 8. What is the difference between stored, reflected, and DOM-based XSS?

Stored XSS comes from content saved and later shown to users. Reflected XSS comes from request data returned in a response. DOM-based XSS happens when client code moves untrusted data into an unsafe browser sink. The data path differs, but the browser executes the resulting code in the site's origin.

### 9. What does CORS do?

Cross-Origin Resource Sharing lets a server specify which origins may read responses to browser cross-origin requests. It is a browser-enforced sharing policy, not authentication or authorization for the API. Non-browser clients do not rely on CORS for access control.

### 10. What is a CORS preflight?

For certain cross-origin requests, the browser sends an `OPTIONS` request first to ask whether the intended method and headers are allowed. The server must answer with suitable CORS headers before the browser sends the actual request.

### 11. Why does `Access-Control-Allow-Origin: *` fail with credentials?

Credentialed browser requests need an explicit allowed origin and `Access-Control-Allow-Credentials: true`. The wildcard cannot authorize a credentialed response. The client also needs the appropriate `fetch` credentials setting, and cookie policy may still block the cookie.

### 12. What is the difference between origin and site?

An origin combines scheme, host, and port. A site is based on scheme and registrable domain. Two subdomains can be same-site but cross-origin, so `SameSite` and CORS answer different questions.

## Identity, OAuth, and sessions

Sources: [OAuth 2.0 framework](https://www.rfc-editor.org/rfc/rfc6749), [OAuth security best current practice](https://www.rfc-editor.org/rfc/rfc9700), [PKCE](https://www.rfc-editor.org/rfc/rfc7636), [OpenID Connect Core](https://openid.net/specs/openid-connect-core-1_0.html), [JWT specification](https://www.rfc-editor.org/rfc/rfc7519), [JWT access token profile](https://www.rfc-editor.org/rfc/rfc9068), [OAuth for browser-based applications, draft 27](https://datatracker.ietf.org/doc/draft-ietf-oauth-browser-based-apps/27/).

### 13. What is the difference between authentication and authorization?

Authentication establishes who a user is. Authorization decides what that user may do with a particular resource. A valid login does not grant access to every account or object.

### 14. What problem does OAuth 2.0 solve?

OAuth 2.0 lets a client obtain delegated access to protected resources, usually through access tokens and defined scopes. OAuth alone does not standardize a claim that the client has authenticated the user.

### 15. What does OpenID Connect add to OAuth 2.0?

OpenID Connect adds an authentication layer. It defines an ID token and standard claims so a client can verify information about the end user's authentication. The `openid` scope requests this behavior.

### 16. How do an ID token, access token, and refresh token differ?

An ID token tells the client about authentication. An access token is presented to a resource server to access an API. A refresh token can obtain new access tokens and needs strong protection. Do not send an ID token to an API as a substitute for an access token.

### 17. What is a JWT?

A JSON Web Token is a compact format for claims. Common signed JWTs provide integrity and authenticity after correct verification; their payload is encoded, not encrypted. A JWT can be an ID token or access token, but tokens do not have to be JWTs.

### 18. What must an API check when it receives a JWT access token?

Use a trusted issuer's keys and allowed algorithms to verify the signature. Check expiration, issuer, intended audience, and token type or scope as required by the token profile. Then enforce authorization for the requested operation and object.

### 19. What is Authorization Code with PKCE?

The client starts an authorization flow with a code challenge and later proves possession of the matching verifier when exchanging the code. PKCE reduces the value of an intercepted authorization code. Current OAuth security guidance uses it for browser-based clients.

### 20. What do `state` and `nonce` protect in an OAuth or OIDC flow?

The client uses `state` to bind the authorization response to the request and mitigate callback CSRF. In OIDC, `nonce` binds an ID token to an authentication request and helps prevent replay. Validate returned values against those stored for the original flow.

### 21. What is a backend for frontend, or BFF, in an OAuth web app?

The server component handles the OAuth flow, stores access and refresh tokens server-side, and proxies API calls. The browser gets a session cookie instead of API tokens. This limits token exposure to browser JavaScript, while the cookie session still needs CSRF protection.

### 22. What is the difference between a session cookie and a JWT?

A session cookie is a browser storage and transport mechanism, often containing an opaque session ID. A JWT is a token format. A cookie can carry a JWT, but a server-side session with an opaque ID can simplify revocation and avoid exposing claims to the browser.

### 23. Where should browser tokens be stored?

There is no universally safe client storage. JavaScript-readable storage exposes tokens to XSS. A BFF can keep OAuth tokens on the server and use a protected session cookie. Choose based on the application's architecture and threat model, then defend against both XSS and CSRF.

### 24. How do logout and revocation differ?

Logout ends the application's local session. Revocation invalidates a token or grant at the authorization server if supported. Removing a token from browser storage alone does not invalidate a token that was copied earlier.

### 25. What is refresh token rotation?

The authorization server issues a new refresh token when the old one is used and invalidates the old token. Reuse can signal theft. The server still needs a policy for expiry, reuse detection, and ending the token family.

### 26. Why should a web app avoid the implicit and password grants for a new OAuth integration?

Current OAuth security guidance favors Authorization Code with PKCE for browser flows. The implicit grant exposes tokens through the authorization response; the resource-owner password grant asks the client to handle user credentials directly and is prohibited by the current security best practice.

## Authorization and access control

Sources: [OWASP authorization guidance](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html), [OWASP API1 BOLA](https://api-security.owasp.org/editions/2023/en/0xa1-broken-object-level-authorization/), [OWASP IDOR prevention](https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html).

### 27. How do RBAC and ABAC differ?

Role-based access control grants permissions through roles such as editor or admin. Attribute-based access control evaluates properties of the subject, resource, action, and context, such as tenant, ownership, or time. Many systems combine them.

### 28. What is BOLA?

Broken Object Level Authorization occurs when an API accepts an object identifier but fails to check whether the caller may access that object. A user who can view `/orders/123` must not automatically be able to view `/orders/124`.

### 29. How is BOLA related to IDOR?

IDOR is a common description of accessing an object by changing a direct identifier without a proper access check. BOLA names the object-level authorization failure in APIs. Guess-resistant IDs make enumeration harder but do not replace server-side checks.

### 30. Why is hiding an admin button insufficient authorization?

A caller can send the API request without the button. Check permission on the server for every protected action and resource. The UI can still hide unavailable actions to improve usability.

### 31. What is the difference between object-level and function-level authorization?

Object-level checks ask whether this caller may access this particular record. Function-level checks ask whether the caller may invoke the operation at all, such as deleting users. A request may need both checks.

### 32. What does least privilege mean in a frontend system?

Give users, services, and tokens only the permissions they need. The browser should receive only data it is allowed to show. A scoped token does not replace checks on the target resource.

### 33. How do you test for BOLA?

Create records for two accounts or tenants. Use one account's session to request, update, and delete the other's records through direct API calls. Include nested resources and identifiers found in normal responses.

### 34. Where should tenant isolation be enforced?

At the server's data access or authorization boundary, based on trusted tenant context. Do not rely on a tenant ID supplied by the browser without checking membership. Apply the check to reads, writes, exports, and background jobs.

## HTTP, APIs, and caching

Sources: [MDN HTTP overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview), [MDN HTTP caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching), [MDN ETag](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/ETag), [MDN Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch).

### 35. What is the difference between `GET`, `POST`, `PUT`, `PATCH`, and `DELETE`?

`GET` retrieves a representation. `POST` submits data for processing or creation. `PUT` replaces the target resource's representation, while `PATCH` applies a partial change. `DELETE` requests removal. API contracts may define details, but safe and idempotent method semantics matter for retries and caching.

### 36. What does idempotent mean?

Repeating the same request has the same intended server effect as sending it once. `PUT` and `DELETE` are defined as idempotent; `POST` is not generally. A payment API can make a `POST` safely retryable with an idempotency key.

### 37. When do you use `401`, `403`, and `404`?

`401` means authentication is needed or invalid. `403` means the server understood the request but refuses it. `404` means the resource was not found; some APIs deliberately return it to avoid revealing a protected resource.

### 38. What does `fetch()` do with HTTP error responses?

`fetch()` usually fulfills its promise when the server returns a response, including `404` or `500`. Check `response.ok` or `response.status`. It rejects for failures such as network errors or an aborted request.

### 39. What is the difference between `Cache-Control: no-cache` and `no-store`?

`no-cache` allows storage but requires validation before reuse. `no-store` tells caches not to store the response. `private` permits a private browser cache but prevents reuse by shared caches.

### 40. How do `ETag` and `If-None-Match` work?

The server sends an `ETag` for a representation. A client later sends it in `If-None-Match`; if the representation is unchanged, the server can return `304 Not Modified` without the body. `If-Match` can also prevent overwriting a newer version during an update.

### 41. What is stale-while-revalidate?

A cache may serve a stale response during a limited window while it checks for a newer response in the background. This can reduce wait time, but users may briefly see old data.

### 42. How do you prevent one user's private data from reaching another through a cache?

Set appropriate cache headers for personalized responses, such as `private` or `no-store` when storage itself is unacceptable. Configure CDN cache keys and bypass rules deliberately. Never assume that the presence of a session cookie automatically makes a shared cache safe.

### 43. What is the difference between pagination with offsets and cursors?

Offset pagination skips a count of records and supports jumping to a page, but inserts can shift results between requests. Cursor pagination continues from a stable sort position and often handles changing feeds better. Both need a deterministic order.

### 44. Why are timeouts and retries separate concerns?

A timeout bounds how long a caller waits. A retry sends the operation again and can duplicate a write. Retry only when the operation is safe or has an idempotency mechanism, and use backoff for transient failures.

### 45. What should an API error response give the frontend?

A stable status and machine-readable error code, with safe field-level details when validation fails. Avoid exposing secrets or internal stack traces. The client can map codes to accessible, user-facing messages.

## Promises, timing, and race conditions

Sources: [MDN `Promise.all`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all), [MDN `Promise.allSettled`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled), [MDN promise guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises), [MDN AbortController](https://developer.mozilla.org/en-US/docs/Web/API/AbortController).

### 46. How do `Promise.all()` and `Promise.allSettled()` differ?

`Promise.all()` fulfills when every input fulfills and rejects when one rejects. `Promise.allSettled()` waits for every input to settle and returns each fulfilled value or rejection reason. Both preserve input order in their result arrays.

### 47. Does `Promise.all()` cancel the other tasks after one rejects?

No. It rejects its combined promise, but work already started continues unless the underlying operations are canceled separately. Use an `AbortSignal` where supported when cancellation is required.

### 48. When would you use `Promise.any()` or `Promise.race()`?

`Promise.any()` fulfills with the first fulfillment and rejects only if all inputs reject. `Promise.race()` settles with the first settled input, whether fulfillment or rejection. A timeout built with `race()` does not itself cancel the slower operation.

### 49. What is the difference between concurrency and parallelism?

Concurrency means tasks overlap in time. Parallelism means tasks execute at the same instant. Starting several network requests before awaiting them enables concurrency; it does not prove that JavaScript runs their callbacks in parallel.

### 50. When should requests run sequentially rather than concurrently?

Run them sequentially when the next request needs the previous result or when order matters. Start independent requests together to avoid unnecessary waiting. Apply a concurrency limit if launching all requests would overload an API.

### 51. What is a race condition in a search UI?

A request for an older query may finish after a newer request and overwrite the latest results. Use a request ID or compare the query before committing results; cancel obsolete requests where possible.

### 52. What does `AbortController` solve, and what does it not solve?

It signals supported operations, including `fetch`, to stop work. It can prevent an obsolete response from being used. It does not roll back a mutation the server already processed, so destructive actions also need server-side safeguards.

### 53. What is debounce?

Debounce waits for a quiet period before running a function. It suits search suggestions after typing pauses. Choose a delay, decide whether the call happens at the leading or trailing edge, and cancel pending work when the UI goes away.

### 54. What is throttle?

Throttle limits calls to a maximum rate while events continue. It suits repeated scroll or pointer events. Unlike a typical trailing debounce, it can produce updates during a continuous event stream.

### 55. How do debounce and cancellation work together?

Debounce limits how many requests start. Cancellation stops an obsolete request after it starts. A latest-request guard still protects the UI if cancellation arrives too late or the operation cannot be canceled.

### 56. What is a stale closure?

A callback retains values from the render or scope where it was created. A delayed callback can use an old state value. Use a state updater, correct dependencies, or a ref when the logic needs the current value without recreating the callback.

### 57. How do you prevent duplicate form submissions?

Disable or guard the submit action while one request is pending, but also protect the server with idempotency when duplicate writes would be costly. UI state alone cannot prevent retries, double requests, or calls from another client.

## Performance and Core Web Vitals

Sources: [web.dev Core Web Vitals](https://web.dev/articles/vitals), [web.dev LCP](https://web.dev/articles/lcp), [web.dev INP](https://web.dev/articles/inp), [web.dev CLS](https://web.dev/articles/cls), [web.dev lab and field data](https://web.dev/articles/lab-and-field-data-differences).

### 58. What are the Core Web Vitals?

Largest Contentful Paint measures loading, Interaction to Next Paint measures responsiveness, and Cumulative Layout Shift measures visual stability. The recommended good thresholds at the 75th percentile are LCP at most 2.5 seconds, INP at most 200 milliseconds, and CLS at most 0.1.

### 59. What does LCP measure?

It reports when the largest eligible content element in the viewport is rendered during page load. A hero image or large text block can be the LCP element. Diagnose whether delay comes from server response, resource discovery, download, or rendering.

### 60. How would you improve a slow LCP image?

Serve an appropriately sized, compressed image; make it discoverable early; avoid lazy loading the LCP image; and give it suitable fetch priority where supported. Check server response time and render-blocking work too.

### 61. What does INP measure?

It reflects a page's responsiveness across user interactions, using the duration from input until the next paint. Long JavaScript tasks, expensive event handlers, and rendering work can increase it. A page can load quickly yet still have poor INP.

### 62. How would you investigate poor INP?

Identify slow interactions in field data, reproduce them in browser performance tools, and inspect input delay, handler work, and presentation delay. Break up long tasks, reduce unnecessary rendering, and move non-urgent work away from the interaction path.

### 63. What does CLS measure?

It measures unexpected shifts of visible content. Common causes include images or ads without reserved space, injected content above existing content, and font changes. Reserve dimensions and inspect shifts during the full page lifecycle, not only initial load.

### 64. Why might Lighthouse and field data disagree?

Lighthouse runs a controlled lab load. Field data includes real devices, networks, viewports, and interactions. INP needs user interaction, so a load-only Lighthouse run cannot directly measure it; its Total Blocking Time can act as a lab clue.

### 65. What is the difference between TTFB and LCP?

Time to First Byte measures when the first response byte arrives. LCP measures when the largest eligible viewport element renders. TTFB can contribute to LCP, but a slow image or blocked render can keep LCP poor even with fast TTFB.

### 66. How does code splitting help, and when can it hurt?

It can reduce JavaScript needed for the initial route. It can hurt when a required component becomes a late network dependency. Split code by real usage and inspect the request waterfall.

### 67. What is the difference between preload, prefetch, and lazy loading?

Preload requests a resource needed soon for the current page. Prefetch hints that a resource may help a later navigation. Lazy loading delays non-urgent resources until closer to use. Wrong hints compete for bandwidth with critical resources.

### 68. Why are performance budgets useful?

They set measurable limits for bundles, requests, or user metrics before regressions ship. Pick budgets from a real baseline and user devices. Track both lab diagnostics and field outcomes.

### 69. What performance problems can too much client-side JavaScript cause?

It costs download, parse, compile, and execution time. Long main-thread tasks can delay input and paint. Reduce unused code, defer non-urgent work, and avoid sending server-only logic to the browser.

## State and data architecture

Sources: [React context](https://react.dev/learn/passing-data-deeply-with-context), [React reducer with context](https://react.dev/learn/scaling-up-with-reducer-and-context), [Redux FAQ](https://redux.js.org/faq/general), [Redux Toolkit](https://redux-toolkit.js.org/introduction/getting-started), [Zustand introduction](https://zustand.docs.pmnd.rs/learn/getting-started/introduction.html), [TanStack Query overview](https://tanstack.com/query/latest/docs/framework/react/overview).

### 70. How do local state, Context, Redux, and Zustand differ?

Local state belongs to a component or nearby subtree. Context passes a value through a tree. Redux provides a central store with explicit update flow and tooling. Zustand provides an external store with selector-based subscriptions and less setup. Choose based on sharing, update frequency, and debugging needs.

### 71. Is Context a state manager?

Context is a way to pass a value to descendants. Pair it with `useState` or `useReducer` to manage updates. A changed provider value can re-render consumers, so separate unrelated values or use a store when frequent shared updates become awkward.

### 72. When would you choose Redux Toolkit?

Use it when shared state and update logic are complex enough that explicit reducers, middleware, and DevTools help the team trace changes. Redux Toolkit is the recommended way to write Redux logic. Do not move every local input value into the store.

### 73. When would you choose Zustand?

Use it for shared client state when a small store and direct selectors fit the application. Keep store ownership clear, especially with server rendering, and avoid turning the entire app into one global store.

### 74. Why is server state different from client state?

Server data has a remote owner and can become stale. It needs fetching, caching, invalidation, and error handling. Local UI state, such as an open dialog, usually does not. A data-fetching cache can manage server state while Context or a store manages client state.

### 75. What is optimistic updating?

Show the expected result before the server confirms it, then reconcile on success or roll back on failure. Handle overlapping mutations and stale responses so one failure does not erase a later successful change.

### 76. How do you avoid duplicated sources of truth?

Keep authoritative data in one place and derive display values when possible. If you copy server data into another store, define when it syncs and invalidates. Duplicates often produce a stale UI after mutations.

### 77. When should state live in the URL?

Use the URL for state users should bookmark, share, or recover with Back and Forward, such as search, filters, sort, and page. Keep transient state such as an unsaved input draft local unless product requirements say otherwise.

### 78. What is the difference between prop drilling and explicit data flow?

Passing props through many unrelated layers can be noisy. Passing a few props through composed components is clear and easy to trace. Try component composition before putting all values into Context.

### 79. How do selectors affect re-renders?

A selector subscribes a component to part of a store. If it returns an unchanged value under the store's equality rule, the component can avoid a store-driven re-render. Returning a new object each time may defeat that benefit unless the library supports an appropriate comparison.

## Practical interview scenarios

Use the source links in the sections above when checking your answers.

### 80. A login works locally but its cookie is missing in a cross-origin deployment. What do you inspect?

Check the cookie's `Secure`, `SameSite`, `Domain`, and `Path` attributes, the request's credentials mode, the server's exact CORS origin and credentials headers, and browser third-party cookie policy. Also confirm whether the two URLs are cross-origin, cross-site, or both.

### 81. A user changes `/api/invoices/42` to `/api/invoices/43` and sees another user's invoice. What failed?

Object-level authorization failed. Restrict the server query or permission check to the authenticated user's allowed invoices, then test reads and writes with another account's ID.

### 82. Search results for `rea` replace newer results for `react`. How do you fix it?

Track which query or request ID is current before committing results. Abort older fetches when possible. Debounce can reduce requests, but it cannot guarantee response order.

### 83. A dashboard needs five independent API calls, but one may fail. Which promise combinator fits?

Use `Promise.allSettled()` if the page should show four successful panels and an error for one panel. Use `Promise.all()` if the page cannot be useful unless all five succeed. Neither combinator cancels the requests for you.

### 84. A page has good LCP but feels slow when a filter is clicked. What do you measure?

Check INP and profile the filter interaction. Look for long handlers, expensive list rendering, and delayed paint. LCP covers the initial load and will not explain a slow later interaction.

### 85. A team proposes Redux for every form field. What would you ask?

Ask which components need the values, whether they must survive navigation, and what debugging problem a global store solves. Keep short-lived form state local unless sharing or coordination justifies moving it.

## Further reading

- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
- [OAuth 2.0 security best current practice](https://www.rfc-editor.org/rfc/rfc9700)
- [OpenID Connect Core](https://openid.net/specs/openid-connect-core-1_0.html)
- [MDN HTTP documentation](https://developer.mozilla.org/en-US/docs/Web/HTTP)
- [MDN JavaScript promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)
- [Core Web Vitals](https://web.dev/articles/vitals)
- [React context](https://react.dev/learn/passing-data-deeply-with-context)
- [Redux FAQ](https://redux.js.org/faq/general)
- [Zustand introduction](https://zustand.docs.pmnd.rs/learn/getting-started/introduction.html)
