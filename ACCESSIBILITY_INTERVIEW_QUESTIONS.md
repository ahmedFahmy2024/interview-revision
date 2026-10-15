# Web Accessibility interview questions and answers

A React and Next.js interview revision guide. The answers use the W3C WCAG 2.2 standard and ARIA Authoring Practices Guide, with React and browser documentation for implementation details. Check the applicable legal or project conformance target separately.

## Study checklist

- [ ] Understand each answer without memorizing wording
- [ ] Explain one example from a production application
- [ ] Practice at least one hands-on implementation

## Accessibility foundations and WCAG

Sources: [WCAG 2.2](https://www.w3.org/TR/WCAG22/), [what changed in WCAG 2.2](https://www.w3.org/WAI/standards-guidelines/wcag/new-in-22/), [accessibility fundamentals](https://www.w3.org/WAI/fundamentals/accessibility-intro/).

### 1. What is web accessibility?

Building interfaces usable by people with diverse visual, auditory, motor, and cognitive abilities, including assistive technology users.

### 2. What does POUR stand for?

Perceivable, Operable, Understandable, Robust: the four WCAG principles.

### 3. What are WCAG A, AA, and AAA?

Conformance levels. AA is a common target; meeting AA requires satisfying applicable Level A and AA success criteria.

### 4. What is the difference between WCAG 2.1 and 2.2?

WCAG 2.2 adds nine success criteria, including Focus Not Obscured, Dragging Movements, Target Size (Minimum), Redundant Entry, and Accessible Authentication. Criterion 4.1.1 Parsing is obsolete and removed.

### 5. Is passing axe enough to claim WCAG compliance?

No. Automated tools catch a subset of issues; keyboard, screen reader, visual, cognitive, and manual reviews remain essential.

### 6. What is the accessibility tree?

A browser representation of semantic roles, names, states, and relationships exposed to assistive technologies.

### 7. Why is semantic HTML important?

Native elements supply built-in roles, keyboard behavior, focus handling, and familiar assistive-technology semantics.

### 8. What is progressive enhancement?

Deliver functional core content and interactions with HTML first, then add JavaScript enhancements without unnecessarily breaking baseline access.

## Semantic HTML and ARIA

Sources: [ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/), [first rule of ARIA](https://www.w3.org/TR/using-aria/#rule1), [accessible name computation](https://www.w3.org/TR/accname-1.2/), [MDN ARIA reference](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA).

### 9. When should you use a button instead of a div with onClick?

For actions, use button: it supports keyboard activation, focus, and semantics by default. Use links for navigation.

### 10. What is ARIA?

Accessible Rich Internet Applications attributes define roles, states, and properties when native HTML semantics are insufficient.

### 11. What is the first rule of ARIA?

Prefer a native HTML element with the required semantics and behavior rather than recreating it with ARIA.

### 12. What is an accessible name?

The name assistive technologies expose for a control, derived from sources such as labels, content, aria-label, or aria-labelledby.

### 13. How do aria-label and aria-labelledby differ?

aria-label supplies a text string; aria-labelledby references visible or other DOM text and generally takes precedence in name computation.

### 14. What is aria-describedby for?

Associating supplementary instructions or error details with a control without replacing its accessible name.

### 15. What does aria-hidden=true do?

Removes an element and its descendants from the accessibility tree, but does not visually hide them or remove keyboard focusability. Do not use it on focusable content or an ancestor of focusable content.

### 16. What is the difference between aria-live polite and assertive?

polite announces updates when convenient; assertive interrupts more urgently. Avoid noisy announcements.

### 17. What is the difference between disabled and aria-disabled?

Native disabled blocks standard control interaction and usually removes it from tab order; aria-disabled conveys disabled state but requires you to prevent activation yourself.

### 18. What are landmark elements?

Elements such as header, nav, main, aside, and footer that help assistive-technology users navigate page regions.

## Keyboard and focus management

Sources: [modal dialog pattern](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/), [keyboard accessibility](https://www.w3.org/WAI/WCAG22/Understanding/keyboard), [Focus Not Obscured](https://www.w3.org/WAI/WCAG22/Understanding/focus-not-obscured-minimum), [focus appearance](https://www.w3.org/WAI/WCAG22/Understanding/focus-appearance).

### 19. What keyboard interactions should all interactive controls support?

Reachability by Tab, visible focus, and appropriate activation keys: Enter for links/buttons and Space for buttons; complex widgets follow their documented patterns.

### 20. What is a focus trap?

Keeping keyboard focus within a modal dialog while it is active, with a reliable way to close it and restore focus.

### 21. Why avoid positive tabindex values?

They create an artificial tab order that is hard to maintain. Prefer DOM order, tabindex=0, and tabindex=-1 for programmatic focus.

### 22. What is roving tabindex?

A composite-widget pattern with one item in the Tab sequence and arrow keys moving focus among items.

### 23. What is a skip link?

A keyboard-accessible link near the top that lets users jump past repeated navigation to main content.

### 24. How do you implement accessible dialogs?

Use a well-tested modal dialog primitive or `<dialog>.showModal()`. Give it an accessible name, move focus inside, prevent interaction with the background, support Escape, and restore focus to a logical place when it closes.

### 25. What is focus-visible?

A CSS pseudo-class that shows focus indicators when the browser determines they are needed, often for keyboard interaction.

### 26. What does Focus Not Obscured mean?

WCAG 2.2 requires keyboard-focused content not be completely hidden behind author-created overlays at AA; stricter criteria apply at AAA.

## Forms, errors, and feedback

Sources: [labels or instructions](https://www.w3.org/WAI/WCAG22/Understanding/labels-or-instructions), [error identification](https://www.w3.org/WAI/WCAG22/Understanding/error-identification), [status messages](https://www.w3.org/WAI/WCAG22/Understanding/status-messages), [combobox pattern](https://www.w3.org/WAI/ARIA/apg/patterns/combobox/).

### 27. How do you label a form input?

Use a visible label linked by htmlFor/id or wrap the control in label. Placeholder text alone is not a label.

### 28. How should validation errors be exposed?

Show clear text, associate it using aria-describedby or aria-errormessage as appropriate, set aria-invalid, and guide focus to errors when useful.

### 29. How do you make required fields accessible?

Provide visible required indicators and native required where appropriate, with understandable instructions.

### 30. How should asynchronous success/error messages be announced?

Use suitable status/alert roles or live regions for updates, without over-announcing.

### 31. What makes a custom combobox difficult?

It needs coordinated focus, keyboard navigation, listbox semantics, expanded state, active option, and selection behavior.

### 32. How do you make a multi-step form accessible?

Use meaningful headings, step status, labeled controls, validation summary, and focus movement on step changes.

## Visual and media accessibility

Sources: [contrast minimum](https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum), [non-text contrast](https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast), [reflow](https://www.w3.org/WAI/WCAG22/Understanding/reflow), [target size minimum](https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum), [captions](https://www.w3.org/WAI/WCAG22/Understanding/captions-prerecorded).

### 33. What are common WCAG contrast ratios?

For WCAG AA, normal text needs at least 4.5:1 and large text at least 3:1; essential non-text UI indicators generally need 3:1.

### 34. When should an image have empty alt text?

Use `alt=""` when an image is decorative or its information is already conveyed by nearby text. Give informative images an alternative that serves the image's purpose in context.

### 35. How do you make icon-only buttons accessible?

Give the button an accessible name, e.g. aria-label="Close", and ensure its icon is not redundantly announced.

### 36. What is reduced motion?

Respect prefers-reduced-motion and offer reduced or disabled nonessential animation.

### 37. How should video content be made accessible?

Provide accurate captions for prerecorded synchronized audio, and transcripts or audio description as required by the content and applicable WCAG criteria.

### 38. Why test zoom and reflow?

Users enlarge content or use narrow viewports. Ensure content remains usable at 200% text resize and reflows without unnecessary two-dimensional scrolling under applicable criteria.

### 39. What is target size in WCAG 2.2?

At Level AA, pointer targets generally need at least 24 by 24 CSS pixels or sufficient spacing, subject to defined exceptions.

## React and Next.js accessibility

Sources: [React `useId`](https://react.dev/reference/react/useId), [Testing Library role queries](https://testing-library.com/docs/queries/byrole/), [axe-core limitations](https://github.com/dequelabs/axe-core#supported-browsers), [Next.js metadata](https://nextjs.org/docs/app/getting-started/metadata-and-og-images).

### 40. How does useId help accessibility?

It generates stable IDs to connect input labels, descriptions, and error messages across React rendering.

### 41. How do you make SPA route changes understandable?

Update document title and main heading, manage focus appropriately, and consider announcing navigation changes to screen-reader users.

### 42. How do you test accessibility in React Testing Library?

Query controls by role and accessible name, exercise keyboard behavior, and assert associated labels, errors, and states.

### 43. What can jest-axe or axe-core detect?

It can find many machine-detectable violations, such as missing names and invalid ARIA. Browser-based axe can check some contrast cases; axe in jsdom cannot run its color-contrast rule. Neither proves the full interaction is accessible.

### 44. What are common RTL accessibility concerns?

Set correct lang and dir, verify logical CSS properties, keyboard order, icon direction, readable mixed-language content, and screen-reader pronunciation.

### 45. How should loading states be represented?

Expose meaningful status or busy state when useful, preserve understandable focus behavior, and avoid announcing every minor update.

## Practical interview scenarios

Sources: [modal dialog pattern](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/), [status messages](https://www.w3.org/WAI/WCAG22/Understanding/status-messages), [accessible authentication](https://www.w3.org/WAI/WCAG22/Understanding/accessible-authentication-minimum).

### 46. A designer wants a clickable div. What do you say?

Use a button for actions or anchor for navigation, and style it to match the design; native behavior reduces custom keyboard and ARIA work.

### 47. A modal opens but keyboard users remain behind it. How do you fix it?

Use a dialog primitive, move focus inside, make background inert where appropriate, keep focus within, and restore focus to the trigger on close.

### 48. A form shows red borders but no error text. What is wrong?

Color alone is insufficient. Add visible error messages, semantic invalid state, and accessible descriptions.

### 49. A dashboard updates live every second. What could go wrong?

Overly aggressive live announcements interrupt screen readers. Announce only meaningful changes and let users control noisy updates.

### 50. How would you audit an existing React app?

Check headings and landmarks, keyboard-only flows, focus order, accessible names, forms, contrast, zoom/reflow, screen readers, and automated axe scans; prioritize blockers.

## Further interview scenarios

Sources: [WCAG 2.2 new criteria](https://www.w3.org/WAI/standards-guidelines/wcag/new-in-22/), [dragging movements](https://www.w3.org/WAI/WCAG22/Understanding/dragging-movements), [redundant entry](https://www.w3.org/WAI/WCAG22/Understanding/redundant-entry), [accessible authentication](https://www.w3.org/WAI/WCAG22/Understanding/accessible-authentication-minimum), [tables tutorial](https://www.w3.org/WAI/tutorials/tables/), [images tutorial](https://www.w3.org/WAI/tutorials/images/).

### 51. A sortable list works only by dragging. What should change?

Provide a single-pointer way to reorder without dragging, such as move buttons. Make the same operation available by keyboard and announce the new position when it changes.

### 52. A login form blocks paste into its password and one-time-code fields. Why is this a problem?

It can prevent password managers and copying a code from helping users complete authentication. Allow paste and appropriate autofill unless a documented exception applies.

### 53. A checkout asks for an address again on the next step. What does Redundant Entry require?

If the information was already provided in the same process, populate it or make it available to select. Re-entry is allowed when it is essential, required for content security, or the earlier information is no longer valid; confirming a newly created hidden password is one security example.

### 54. How do you make a data table understandable to screen readers?

Use a real `<table>`, `<th>` cells for headers, and `scope` or explicit header associations for complex structures. Do not use a data table solely to arrange a page visually.

### 55. What is the difference between a decorative image and an informative image inside a link?

A decorative image can have `alt=""`. If an image is the only content of a link, its alternative text must convey the link's purpose.

### 56. Why can a sticky header cause a keyboard accessibility failure?

It can completely cover the newly focused control. Test focus while tabbing and scrolling; add layout clearance, such as `scroll-margin`, when the header obscures focus.

### 57. How should an accessible-name regression be tested?

Query by role and name, then check the control's behavior. An automated accessibility scan can catch missing names, but a manual review checks whether the name is accurate and understandable.

### 58. What accessibility checks need a real browser rather than jsdom?

Check computed contrast, zoom and reflow, visible focus, pointer target size, and screen reader interaction in a browser. jsdom is useful for DOM semantics but does not perform full layout or paint.

## Practical code exercise

Audit a React modal and a form using keyboard-only navigation, browser accessibility tree, axe DevTools, and a screen reader. Record issues, WCAG references, and fixes.

```tsx
import { useId } from "react";

function EmailField({ error }: { error?: string }) {
  const id = useId();
  return (
    <div>
      <label htmlFor={id}>Email</label>
      <input id={id} type="email" aria-invalid={!!error}
        aria-describedby={error ? `${id}-error` : undefined} />
      {error && <p id={`${id}-error`}>{error}</p>}
    </div>
  );
}
```

## Further reading

- [WCAG 2.2](https://www.w3.org/TR/WCAG22/)
- [ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/)
- [MDN accessibility guide](https://developer.mozilla.org/en-US/docs/Web/Accessibility)
- [React `useId`](https://react.dev/reference/react/useId)
