# Frontend Testing interview questions and answers

A practical interview revision guide for frontend testing. Answers focus on observable behavior, reliable tests, and choices you can explain in a React or Next.js interview. Library-specific guidance is linked to its official documentation.

## Study checklist

- [ ] Understand each answer without memorizing wording
- [ ] Explain one example from a production application
- [ ] Practice at least one hands-on implementation

## Testing foundations

Sources: [Testing Library guiding principles](https://testing-library.com/docs/guiding-principles/), [Playwright test assertions](https://playwright.dev/docs/test-assertions).

### 1. What is the difference between unit, component, integration, E2E, and contract tests?

Unit tests isolate small logic. Component tests render UI and exercise behavior. Integration tests check cooperating modules. E2E tests exercise user journeys in a real browser. Contract tests verify that API producers and consumers agree on requests and responses.

### 2. What is the testing pyramid?

Use many fast unit/component tests, fewer integration tests, and a focused set of slower E2E tests. The right mix depends on risk and architecture.

### 3. What should you test rather than implementation details?

Assert observable behavior: rendered text, accessible roles, validation, network outcomes, navigation, and state changes. Avoid asserting private state or exact internal function calls unless those are the contract.

### 4. What is the difference between a mock, stub, spy, and fake?

A stub provides controlled responses; a spy records calls; a mock commonly combines expectations with substituted behavior; a fake is a working simplified implementation such as an in-memory repository.

### 5. What makes a test deterministic?

Control time, randomness, network responses, and test data; isolate state; clean up resources; avoid arbitrary sleeps and dependence on test order.

### 6. What is test coverage and why is 100% not enough?

Coverage measures executed code, not whether assertions detect defects. Prioritize critical behavior, edge cases, and meaningful assertions.

### 7. What is the Arrange–Act–Assert pattern?

Set up the test, perform one meaningful action, then assert the observable result.

### 8. What is regression testing?

Rerunning tests after changes to detect previously working behavior that has broken.

## Jest and Vitest

Sources: [Jest asynchronous testing](https://jestjs.io/docs/asynchronous), [Jest timer mocks](https://jestjs.io/docs/timer-mocks), [Vitest guide](https://vitest.dev/guide/).

### 9. How do Jest and Vitest compare?

Both provide test runners, assertions, spies, mocks, and coverage. Vitest integrates closely with Vite; Jest has a broad ecosystem. Environment, transformation, and mocking details differ.

### 10. What do describe, test, and expect do?

describe groups tests, test/it declares a case, and expect creates assertions such as toEqual or toHaveBeenCalledWith.

### 11. When should you use toBe versus toEqual?

toBe checks Object.is identity/value semantics; toEqual compares nested structures.

### 12. How do you test asynchronous code?

Return or await the Promise, or use async/await with rejects/resolves. Never leave async work unawaited.

### 13. How do fake timers help?

They let you control scheduled timeouts and intervals without real waiting. Restore real timers afterward. When combining them with user-event, configure its timer advancement and await the interaction; timers do not automatically complete unrelated Promise work.

### 14. When is mocking a module a bad idea?

When it hides the actual integration you intend to verify, couples tests to imports, or makes refactoring brittle.

### 15. How do you test a function that throws?

Wrap synchronous calls in a function passed to expect(...).toThrow(); use await expect(promise).rejects for async errors.

### 16. What should happen between tests?

Restore spies and mocks, clear relevant state, reset timers, and clean up resources to avoid leakage.

## React Testing Library

Sources: [Queries](https://testing-library.com/docs/queries/about/), [user-event](https://testing-library.com/docs/user-event/intro/), [disappearance](https://testing-library.com/docs/guide-disappearance/), [React `act`](https://react.dev/reference/react/act).

### 17. What is React Testing Library’s philosophy?

Test components the way users interact with them; query the rendered DOM using accessible semantics.

### 18. Why prefer getByRole over getByTestId?

Roles and accessible names reflect the user-facing interface and reveal accessibility issues. Test IDs are a fallback when semantic queries do not fit.

### 19. What is the difference between getBy, queryBy, and findBy?

getBy throws if no match; queryBy returns null and is useful for absence; findBy retries until an element appears or times out. For an element that must disappear, use `waitForElementToBeRemoved` or `waitFor` with an absence assertion.

### 20. When do you use screen and within?

screen queries the document; within scopes queries to a region such as a dialog or table row.

### 21. Why use userEvent instead of fireEvent for most interactions?

userEvent simulates more complete user interaction sequences such as typing and clicking; fireEvent is useful for low-level events. Set up `userEvent` and await its async actions.

### 22. How do you test loading, success, empty, and error states?

Control the request response and assert each state from the UI, including accessible status/error feedback.

### 23. How do you test a controlled form?

Render, type into labeled inputs, submit, and assert validation and the submitted result rather than internal state.

### 24. How do you test a modal?

Assert its dialog role and name, keyboard dismissal where supported, focus behavior, and the action that closes it.

### 25. What is act and when is it needed?

`act` lets React process updates caused by a tested interaction before assertions. React Testing Library wraps common helpers; use explicit `act` when an update occurs outside them, such as a manually advanced timer.

### 26. How do you avoid brittle snapshots?

Prefer targeted assertions on important behavior and semantics; snapshots are useful only when intentional output changes are carefully reviewed.

## Network, state, and Next.js

Sources: [MSW documentation](https://mswjs.io/docs/), [TanStack Query testing](https://tanstack.com/query/latest/docs/framework/react/guides/testing), [Next.js testing guide](https://nextjs.org/docs/app/guides/testing).

### 27. What is MSW?

Mock Service Worker intercepts HTTP requests at the network boundary, allowing realistic success, error, and latency scenarios without mocking every fetch hook. It uses a Service Worker in browsers and request interception in Node.js tests.

### 28. How would you test a TanStack Query component?

Use a fresh QueryClient per test, wrap in QueryClientProvider, mock network responses, and await observable states. Disable retries when they add irrelevant delays.

### 29. How do you test optimistic updates?

Assert the optimistic UI appears immediately, then verify server success persists it and server failure restores the authoritative state with an error.

### 30. How do you test an authenticated route?

Check authorized and unauthorized responses, redirects, and protected server data access; hiding a button alone is not security.

### 31. How should Server Components be tested?

Test pure server logic and data boundaries separately. Use E2E tests for framework rendering and routing, especially async Server Components when the unit test tool cannot render them. Verify the actual response and permissions for Server Actions rather than trusting a hidden UI control.

### 32. How do you test API contracts?

Validate response shapes, status codes, error payloads, and compatibility against a shared schema or provider contract.

## Playwright and Cypress

Sources: [Playwright locators](https://playwright.dev/docs/locators), [actionability](https://playwright.dev/docs/actionability), [assertions](https://playwright.dev/docs/test-assertions), [trace viewer](https://playwright.dev/docs/trace-viewer/intro).

### 33. What does an E2E test verify?

A full user-visible workflow in a browser, often including navigation, authentication, API integration, and persistence.

### 34. Why prefer Playwright getByRole locators?

They target semantic user-facing elements, reducing brittle selectors. The locator identifies the element; actions such as `click()` auto-wait for actionability, and web-first `expect(locator)` assertions retry until their condition is met or times out.

### 35. How do you avoid flaky E2E tests?

Use stable test data, isolated environments, deterministic APIs, meaningful locators, and assertions that auto-retry. Avoid fixed sleep delays.

### 36. What is the Page Object Model?

A pattern that encapsulates page locators and user actions. Keep assertions and abstractions understandable rather than hiding all behavior.

### 37. How do you test across browsers?

Run critical flows on Chromium, Firefox, and WebKit where supported, and check responsive viewports separately.

### 38. What are Playwright traces useful for?

They capture actions, DOM snapshots, and diagnostics to investigate failed tests.

### 39. How do Cypress and Playwright differ?

Both automate browser tests. Playwright has multi-browser contexts and browser automation APIs; Cypress provides its own command-chain model and debugging workflow. Choose based on team needs.

## Practical interview scenarios

Sources: [Next.js testing guide](https://nextjs.org/docs/app/guides/testing), [Playwright test isolation](https://playwright.dev/docs/browser-contexts), [Playwright authentication](https://playwright.dev/docs/auth), [axe-core limitations](https://github.com/dequelabs/axe-core#supported-browsers).

### 40. A search field sends requests quickly. What do you test?

Debouncing, query changes, cancellation or stale-result handling, loading, empty results, error UI, and keyboard accessibility.

### 41. A save form fails after an optimistic update. What do you test?

Rollback, error message, retry behavior, duplicate submission protection, and preservation of user input.

### 42. A protected dashboard hides actions based on RBAC. What do you test?

Role-specific UI plus server authorization for direct requests. Test denial cases and role transitions.

### 43. A test passes locally but fails in CI. How do you investigate?

Check environment differences, timezone, data leakage, race conditions, retries, logs, screenshots, and browser traces; fix root causes instead of adding sleeps.

### 44. What tests would you add to an order checkout?

Validation, totals, discounts, stock changes, idempotent submission, payment success/failure, redirect/receipt, and core E2E purchase flow.

### 45. How do you test that a loading indicator disappears?

Trigger the request and assert the indicator appears. Start `waitForElementToBeRemoved` while it is still present, resolve the request, then await removal and assert the result. Do not assert absence immediately after starting asynchronous work.

### 46. When should you mock the network, and when should you use a real backend?

Mock requests in component tests to cover errors and edge cases reliably. Keep a smaller set of integration or E2E tests against a controlled backend to catch mismatches between client and server. Make the boundary explicit in each test.

### 47. How do you test debounce without a fixed sleep?

Use fake timers in a unit or component test, advance the clock to just before and after the threshold, and assert the request count and query. In a browser test, wait for an observable result or request rather than sleeping for a guessed duration.

### 48. Can a passing jsdom accessibility test prove a page is accessible?

No. DOM checks can catch missing names, roles, and some rule violations. They cannot replace browser checks for keyboard focus, visual order, zoom, contrast, and assistive technology behavior. For example, axe-core does not support color-contrast checks in jsdom.

### 49. How do you keep E2E authentication tests independent?

Create known test accounts and isolated browser contexts. Reuse a saved authenticated state only when the tests do not mutate shared server state; use separate accounts for tests that do. Also test an unauthenticated request directly.

### 50. How would you test a Server Action that updates data?

Test its validation and authorization at the server boundary, including direct invalid requests. Then test the user flow in the framework: pending feedback, success, failure, and whether the next render shows fresh data after revalidation.

### 51. What would you check after a hydration warning?

Reproduce with server rendering in a browser and compare the initial HTML with the first client render. Check time-dependent values, random IDs, browser-only APIs, invalid HTML nesting, and data that differs between server and client. Assert the user-visible behavior after hydration too.

### 52. How do you prevent test data from leaking between tests?

Give each test its own data or namespace, reset the database or use transactions where practical, and clean up any external resources. Do not rely on execution order or a previous test's account or cache.

### 53. What should an API contract test verify?

Verify method and path, required request fields, response status and shape, and documented error cases. Keep producer and consumer expectations aligned through a shared schema or a contract checked in CI.

### 54. A Playwright test sometimes clicks the wrong button. What do you change?

Scope the locator to the relevant dialog, form, or row and give the button an accessible name that distinguishes it. Assert the expected container is visible before acting. Avoid positional selectors unless position is the product requirement.

## Practical code exercise

Write a React form test that verifies a required email error, successful submission, loading state, and server failure. Use React Testing Library, user-event, and MSW. Then automate the happy path in Playwright.

```tsx
// Excerpt from a test after rendering the form
const user = userEvent.setup();
const submit = screen.getByRole("button", { name: /submit/i });
await user.click(submit);
expect(await screen.findByRole("alert")).toHaveTextContent(/required/i);
```

## Further reading

- [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/)
- [Playwright](https://playwright.dev/docs/intro)
- [Vitest](https://vitest.dev/guide/)
- [Jest](https://jestjs.io/docs/getting-started)
- [MSW](https://mswjs.io/docs/)
- [Next.js testing](https://nextjs.org/docs/app/guides/testing)
