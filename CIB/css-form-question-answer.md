# CSS: Form

## Question

Complete a website layout application with the following UI components.

The form contains 8 input fields and a submit button.

- All elements should have a 10px margin between them.
- The "First name", "Last name" and "Age" fields should be positioned on the first row and have equal widths.
- The "Address" field should be positioned on the second row and take all the width of the form.
- The "Place of work" and "Job title" fields should be positioned on the third row and have equal widths.
- The "Phone number" and "Skype" fields should be positioned on the fourth row and have equal widths.
- The button should be positioned on the last row and take the width of the form.
- Elements should not have `position: absolute`.

The markup for the question is provided. Complete the `src/css/style.css` file to implement the required features.

### Read-only files
- ctest/*
- src/index.js
- src/index.html
- src/css/readonly.css
- app.js

---

## Answer (`src/css/style.css`)

Using CSS Grid — explicit placement, no wrap-guessing, no `calc()` math:

```css
.form {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  gap: 10px;
}

/* Row 1: First name, Last name, Age — 3 equal columns (2 tracks each = 6) */
.form .first-name,
.form .last-name,
.form .age {
  grid-column: span 2;
}

/* Row 2: Address — full width */
.form .address {
  grid-column: 1 / -1;
}

/* Row 3: Place of work, Job title — 2 equal columns (3 tracks each = 6) */
.form .place-of-work,
.form .job-title {
  grid-column: span 3;
}

/* Row 4: Phone number, Skype — 2 equal columns (3 tracks each = 6) */
.form .phone-number,
.form .skype {
  grid-column: span 3;
}

/* Row 5: Submit button — full width */
.form button[type="submit"] {
  grid-column: 1 / -1;
}
```

### Why this works

- `grid-template-columns: repeat(6, 1fr)` creates 6 equal-width tracks — a common denominator that lets both 3-column rows (2 tracks each) and 2-column rows (3 tracks each) divide evenly.
- `gap: 10px` handles spacing between all elements automatically (both directions), satisfying the "10px margin between them" rule without needing individual margins.
- `grid-column: 1 / -1` spans the full row width for Address and the Submit button.
- No `position: absolute` is used anywhere, and no flex-wrap/percentage math is needed — each element is explicitly placed, so there's no risk of items unexpectedly packing onto the same line.

### Alternative (Flexbox version)

```css
.form {
  display: flex;
  flex-wrap: wrap;
}

.form > * {
  margin: 5px; /* 5px + 5px between adjacent elements = 10px */
  box-sizing: border-box;
}

.form .first-name,
.form .last-name,
.form .age {
  flex: 0 0 calc(33.333% - 10px);
}

.form .address {
  flex: 0 0 calc(100% - 10px);
}

.form .place-of-work,
.form .job-title,
.form .phone-number,
.form .skype {
  flex: 0 0 calc(50% - 10px);
}

.form button[type="submit"] {
  flex: 0 0 calc(100% - 10px);
}
```

Note: with flexbox, widths must be fixed (`flex: 0 0 ...`) rather than allowed to grow (`flex: 1 1 0`), otherwise items with room to spare can pack onto the same line instead of wrapping at the intended row boundary. Grid avoids this problem entirely since placement is explicit.
