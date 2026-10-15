# Web Accessibility interview questions and answers

A practical Mid-Level React interview revision bank. Answers are concise, with emphasis on real-world React and Next.js decisions. This guide expands on the uploaded React/Next.js question-bank style; its new topic-specific answers are supplementary study material, not excerpts from those files.

## Study checklist

- [ ] Understand each answer without memorizing wording
- [ ] Explain one example from a production application
- [ ] Practice at least one hands-on implementation

## Accessibility foundations and WCAG

### 1. What is web accessibility?

Building interfaces usable by people with diverse visual, auditory, motor, and cognitive abilities, including assistive technology users.

### 2. What does POUR stand for?

Perceivable, Operable, Understandable, Robust: the four WCAG principles.

### 3. What are WCAG A, AA, and AAA?

Conformance levels. AA is a common target; meeting AA requires satisfying applicable Level A and AA success criteria.

### 4. What is the difference between WCAG 2.1 and 2.2?

WCAG 2.2 adds criteria including Focus Not Obscured, Dragging Movements, Target Size (Minimum), and Accessible Authentication; one older criterion was removed.

### 5. Is passing axe enough to claim WCAG compliance?

No. Automated tools catch a subset of issues; keyboard, screen reader, visual, cognitive, and manual reviews remain essential.

### 6. What is the accessibility tree?

A browser representation of semantic roles, names, states, and relationships exposed to assistive technologies.

### 7. Why is semantic HTML important?

Native elements supply built-in roles, keyboard behavior, focus handling, and familiar assistive-technology semantics.

### 8. What is progressive enhancement?

Deliver functional core content and interactions with HTML first, then add JavaScript enhancements without unnecessarily breaking baseline access.

## Semantic HTML and ARIA

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

Removes an element and its descendants from the accessibility tree; do not apply it to focusable interactive content.

### 16. What is the difference between aria-live polite and assertive?

polite announces updates when convenient; assertive interrupts more urgently. Avoid noisy announcements.

### 17. What is the difference between disabled and aria-disabled?

Native disabled blocks standard control interaction and usually removes it from tab order; aria-disabled conveys disabled state but requires you to prevent activation yourself.

### 18. What are landmark elements?

Elements such as header, nav, main, aside, and footer that help assistive-technology users navigate page regions.

## Keyboard and focus management

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

Use a well-tested dialog primitive or native dialog appropriately; provide an accessible name, focus handling, keyboard dismissal, and focus restoration.

### 25. What is focus-visible?

A CSS pseudo-class that shows focus indicators when the browser determines they are needed, often for keyboard interaction.

### 26. What does Focus Not Obscured mean?

WCAG 2.2 requires keyboard-focused content not be completely hidden behind author-created overlays at AA; stricter criteria apply at AAA.

## Forms, errors, and feedback

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

### 33. What are common WCAG contrast ratios?

For WCAG AA, normal text needs at least 4.5:1 and large text at least 3:1; essential non-text UI indicators generally need 3:1.

### 34. When should an image have empty alt text?

When purely decorative and its information is already conveyed elsewhere; meaningful images need concise purpose-based alternatives.

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

### 40. How does useId help accessibility?

It generates stable IDs to connect input labels, descriptions, and error messages across React rendering.

### 41. How do you make SPA route changes understandable?

Update document title and main heading, manage focus appropriately, and consider announcing navigation changes to screen-reader users.

### 42. How do you test accessibility in React Testing Library?

Query controls by role and accessible name, exercise keyboard behavior, and assert associated labels, errors, and states.

### 43. What can jest-axe or axe-core detect?

Many machine-detectable rule violations such as missing names and some contrast or structural problems; not the full accessibility experience.

### 44. What are common RTL accessibility concerns?

Set correct lang and dir, verify logical CSS properties, keyboard order, icon direction, readable mixed-language content, and screen-reader pronunciation.

### 45. How should loading states be represented?

Expose meaningful status or busy state when useful, preserve understandable focus behavior, and avoid announcing every minor update.

## Practical interview scenarios

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

- https://www.w3.org/TR/WCAG22/
- https://www.w3.org/WAI/ARIA/apg/
- https://developer.mozilla.org/en-US/docs/Web/Accessibility
- https://react.dev/reference/react/useId
