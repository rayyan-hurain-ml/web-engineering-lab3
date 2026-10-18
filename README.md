<h1 align="center">Web Engineering Lab – Office Hours Page</h1>

<p align="center">
  A responsive, accessible office-hours page built with only HTML, modern CSS and vanilla JavaScript.<br>
  <b>Web Engineering – Lab 04: The Cascade, Flexbox and Grid, and the JavaScript DOM</b><br>
</p>

<p align="center">
  <img alt="HTML5" src="https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white">
  <img alt="CSS3" src="https://img.shields.io/badge/CSS3-1572B6?logo=css3&logoColor=white">
  <img alt="JavaScript" src="https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black">
  <img alt="Lighthouse accessibility 100" src="https://img.shields.io/badge/Lighthouse%20Accessibility-100-brightgreen">
</p>

It extends the `web-engineering-lab` project from Labs 1 and 2 – no frameworks and no build step.

**Author:** Rayyan Hurain &nbsp;|&nbsp; &nbsp;|&nbsp; **Course:** Web Engineering &nbsp;|&nbsp; **Lab:** 04

---

## Preview

## Preview

<table>
  <tr>
    <td align="center"><b>Light mode</b></td>
    <td align="center"><b>Dark mode</b></td>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/ad2301f6-bab9-487d-b461-a5ff6359000d" alt="Page in light mode" width="450"></td>
    <td><img src="https://github.com/user-attachments/assets/e3339169-aaeb-41da-97b8-332538f53f70" alt="Page in dark mode" width="450"></td>
  </tr>
  <tr>
    <td align="center"><b>Phone (390 px)</b></td>
    <td align="center"><b>Booking a slot</b></td>
  </tr>
  <tr>
    <td align="center"><img src="https://github.com/user-attachments/assets/f1c1d4d0-900b-47cc-948c-2237aa521fc1" alt="Page on a phone" width="220"></td>
    <td><img src="https://github.com/user-attachments/assets/4291dcc5-3b64-4bec-b0e7-1f78d541e79d" alt="A booked slot with status message" width="450"></td>
  </tr>
</table>

### Lighthouse accessibility

<table>
  <tr>
    <td align="center"><b>Light mode – 100</b></td>
    <td align="center"><b>Dark mode – 100</b></td>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/5bf1f142-9751-4aa4-89eb-ce70a43197be" alt="Lighthouse accessibility score in light mode" width="450"></td>
    <td><img src="https://github.com/user-attachments/assets/ddf58dca-1acd-43ad-93bb-9d7dab007e4c" alt="Lighthouse accessibility score in dark mode" width="450"></td>
  </tr>
</table>


> **Note:** the images load from the `docs/screenshots/` folder, so keep that folder next to this file.

## Features

- **Light and dark themes from one stylesheet** – colours are design tokens built with `oklch()`, `color-mix()` and `light-dark()`, so the page follows the visitor's system setting.
- **Organised cascade** – the stylesheet is split into `reset`, `base`, `components` and `utilities` layers with `@layer`, so rule order is predictable and `!important` is never needed.
- **Responsive layout** – Flexbox for the header and navigation, Grid for the office-hours section and feedback form, and a single media query that collapses both grids to one column at `40rem`.
- **Interactive booking** – a visitor can select an office-hours slot. One delegated click listener handles every button, and a live status message (`role="status"`) announces the result to screen readers.
- **Modern JavaScript runtime concepts** – ES modules, an `async` handler using a promise-based `delay()`, and a `makeCounter()` closure that numbers each confirmed booking.
- **Accessible by design** – skip link, visible focus ring, labelled controls, table headers with scope, and full keyboard operation (Tab, Enter, Space).

## Project structure

```text
web-engineering-lab/
├── public/
│   ├── index.html      # page structure and semantics
│   ├── styles.css      # layered stylesheet (reset, base, components, utilities)
│   ├── script.js       # classic script from Labs 1 and 2
│   ├── main.js         # ES module: delegated listener and async handler
│   └── utils.js        # ES module: delay() and makeCounter()
├── docs/screenshots/   # images used in this README
├── NOTES.md            # specificity predictions and event-loop explanation
└── README.md
```

## Getting started

JavaScript modules are blocked on `file://` addresses, so the page must be served over `http`.

**Option 1 – VS Code Live Server**

1. Open the project folder in VS Code.
2. Install the *Live Server* extension.
3. Right-click `public/index.html` and choose **Open with Live Server**.

**Option 2 – Python**

```bash
cd public
python -m http.server 8000
```

Then open <http://localhost:8000>.

## How it works

### Cascade and specificity

When rules conflict, the browser checks origin and `!important`, then cascade layer, then specificity, then source order. The navigation demonstrates this with three competing rules:

| Selector | Specificity | Colour | Applies to |
|---|---|---|---|
| `:where(nav ul li) a` | (0,0,1) | purple | loses to `.nav-link` |
| `.nav-link` | (0,1,0) | dark green | Send Feedback link |
| `a[aria-current="location"]` | (0,1,1) | crimson | Office Hours link |

The over-specific `nav ul li a` (0,0,4) was lowered with `:where()` instead of adding `!important`. Each colour has a lighter dark-mode value so contrast stays high on dark backgrounds. Full predictions and measured results are in [`NOTES.md`](NOTES.md).

### Design tokens

All colours and spacing come from custom properties on `:root`:

| Token | Purpose |
|---|---|
| `--space` | base spacing unit (`1rem`) |
| `--color-bg`, `--color-text` | page background and text |
| `--color-accent` | headings, icons, buttons, focus ring |
| `--color-border`, `--color-muted` | derived with `color-mix()` from the text and background tokens |

Changing a token re-themes the whole page.

### Event delegation and the runtime

`main.js` attaches a single `click` listener to the table body. `closest("button[data-day]")` finds the clicked button, `dataset.day` reads its day, and `classList.toggle("selected")` updates the row. Because the handler is `async`, `await delay(1000)` pauses only that handler – the call stack stays free and the page remains responsive.

## Accessibility and testing

| Check | Result |
|---|---|
| Lighthouse Accessibility – light mode | **100** |
| Lighthouse Accessibility – dark mode (`prefers-color-scheme: dark`) | **100** |
| Console errors while booking slots | none |
| Keyboard-only booking (Tab, Enter, Space) | works |
| Horizontal scrolling at 320, 360 and 390 px | none |

Lighthouse reports: [light](docs/screenshots/lh_light.png) · [dark](docs/screenshots/lh_dark.png)

## Browser support

The page uses recent CSS features (`@layer`, `oklch()`, `color-mix()`, `light-dark()`) and ES modules. Use an up-to-date version of Chrome, Edge, Firefox or Safari.

## Lab tasks covered

1. Box-sizing and margin reset, viewport meta tag, logical properties
2. Conflicting colour rules resolved with `:where()`
3. Cascade layers and the `.text-muted` utility
4. Design tokens and inherited typography
5. `currentColor` SVG icon and dark-mode testing
6. Flex header and navigation
7. Grid layouts and the `.span-all` utility
8. Single-column media query at `40rem`
9. Book column, status paragraph and module script
10. Delegated click listener
11. Event-loop notes, `utils.js` and the async handler
12. Keyboard check, Lighthouse audits and Git commit

## Acknowledgements

Concepts and syntax were studied from [MDN Web Docs](https://developer.mozilla.org/) and the [Lighthouse documentation](https://developer.chrome.com/docs/lighthouse/overview). Course material: Web Engineering Lab 04 statement.
