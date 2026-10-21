# HTML — EXPERT

## 19. DOM understanding

The browser parses HTML source into a DOM, builds a CSSOM from CSS, combines them into a render tree, then performs layout and paint. JavaScript can read and modify the DOM; a DOM change may trigger style calculation, layout, and painting.

**Syntax**

```html
<button id="save">Save</button>
<script>
  document.querySelector("#save").addEventListener("click", () => {
    document.body.dataset.saved = "true";
  });
</script>
```

**Example:** A button event changes state in the DOM; CSS can react to the `data-saved` attribute.

**Use / do not use:** Understand this pipeline when debugging timing or performance. Do not repeatedly force layout by reading geometry immediately after many writes.

**Write yourself:** Build a counter with a button and explain, in your own words, which DOM node changes after each click.

## 20. DOM structure

Elements are nodes, but nodes also include text and comments. Parent, child, sibling, ancestor, and descendant describe relationships in the tree.

**Syntax**

```html
<ul id="menu"><li>Home</li><li>Docs</li></ul>
```

The `ul` is the parent; each `li` is a child and sibling of the other `li`.

**Example:** Event delegation attaches one listener to `ul` and checks which descendant was clicked.

**Use / do not use:** Use relationships to traverse or delegate events. Do not assume whitespace is absent: text nodes can exist between elements.

**Write yourself:** Draw the DOM tree for a small article, labeling element nodes, text nodes, parents, children, and siblings.

## 21. Advanced accessibility

An accessible name is what assistive technology announces for a control. Roles describe what a widget is; states and properties describe its current condition or relationships.

**Syntax**

```html
<button aria-expanded="false" aria-controls="faq-answer">Question</button>
<div id="faq-answer" hidden>Answer</div>
```

**Example:** A disclosure button updates `aria-expanded` and visibility together, while focus remains predictable.

**Use / do not use:** Prefer native `button`, `details`, inputs, and headings. Manage focus when opening dialogs or changing views. Do not invent ARIA roles when native HTML already supplies the behavior.

**Write yourself:** Implement an accessible disclosure with keyboard operation, synchronized state, and focus behavior. Test it with Tab, Enter, and Space.

## 22. SEO-oriented HTML

Search engines use structure and metadata as signals, while users benefit from clear content hierarchy.

**Syntax**

```html
<article>
  <h1>How to grow basil</h1>
  <time datetime="2026-09-27">September 27, 2026</time>
  <figure><img src="basil.jpg" alt="Basil in a pot"><figcaption>Young basil plant</figcaption></figure>
</article>
<link rel="canonical" href="https://example.com/grow-basil">
```

**Example:** A recipe uses one descriptive `h1`, meaningful sections, a `time` machine value, a figure caption, and a canonical URL.

**Use / do not use:** Use structured data only when it accurately describes visible content. Do not create headings or hidden keyword blocks for ranking.

**Write yourself:** Mark up one article for SEO with headings, author/date information, a figure, canonical link, robots metadata, and a small JSON-LD sketch. Explain each choice.

## 23. Responsive images

The browser can choose an image candidate based on viewport size and pixel density.

**Syntax**

```html
<picture>
  <source media="(min-width: 900px)" srcset="hero-wide.jpg">
  <source srcset="hero-small.jpg">
  <img src="hero-small.jpg" srcset="hero-small.jpg 480w, hero-large.jpg 1200w"
       sizes="(min-width: 900px) 50vw, 100vw" alt="Mountain trail">
</picture>
```

**Example:** A phone downloads a smaller crop while a desktop gets a wide image.

**Use / do not use:** Use `picture` for art direction and `srcset`/`sizes` for resolution selection. Keep meaningful alt text on the fallback `img`. Do not send the largest image to every device.

**Write yourself:** Create a responsive hero with two art-directed crops and three width candidates. Write why `sizes` matches your layout.

## 24. Advanced media

WebVTT caption files synchronize text with media. Captions include spoken dialogue and important sounds; subtitles usually translate dialogue.

**Syntax**

```html
<track kind="captions" src="lesson-en.vtt" srclang="en" label="English" default>
```

**Example VTT**

```text
WEBVTT

00:00:01.000 --> 00:00:03.500
Welcome to the lesson.
```

**Use / do not use:** Provide captions for spoken video and transcripts for audio. Do not treat captions as a visual decoration or rely on audio alone to communicate instructions.

**Write yourself:** Create a short VTT file with two cues and connect it to a video. Add a transcript below the player.

## 25. Modern HTML APIs/elements

`dialog` supports modal/non-modal dialogs; `details` and `summary` provide disclosure; `template` stores inert markup; `slot` defines insertion points in Web Components.

**Syntax**

```html
<details><summary>Shipping information</summary><p>Ships in 2 days.</p></details>
<dialog id="help"><p>Help text</p><button autofocus>Close</button></dialog>
<template id="card-template"><article><slot name="title"></slot></article></template>
```

**Example:** FAQ content uses `details`; a confirmation prompt uses `dialog`; a custom card uses a template and slot.

**Use / do not use:** Use native disclosure and dialog behavior where it fits. Do not use a dialog as a substitute for ordinary page content or build a custom element before understanding its accessibility contract.

**Write yourself:** Build an FAQ with `details`, a dialog that opens and closes correctly, and a template you clone with JavaScript.

## 26. Security-related HTML

HTML can create security boundaries, but it cannot make untrusted content safe by itself.

**Syntax**

```html
<a href="https://external.example" target="_blank" rel="noopener noreferrer">External site</a>
<iframe src="/untrusted-widget" sandbox="allow-scripts"></iframe>
```

**Example:** `noopener` prevents a new page from controlling the opener; escaping text before inserting it prevents markup injection.

**Use / do not use:** Treat user-controlled HTML as hostile, sanitize it with a trusted sanitizer when HTML is required, and prefer text insertion APIs. Do not use `innerHTML` with unsanitized input or assume `sandbox` solves every data leak.

**Write yourself:** Make a threat checklist for a comment preview, external links, and an embedded widget. Identify the trust boundary and the safe handling method for each.

## 27. Performance

HTML performance choices affect network cost, rendering, and interaction.

**Syntax**

```html
<link rel="preconnect" href="https://cdn.example">
<img src="photo.jpg" loading="lazy" decoding="async" width="800" height="600" alt="...">
<script src="app.js" defer></script>
```

**Example:** Below-the-fold images load lazily, dimensions reserve space, and a script runs after parsing without blocking document parsing.

**Use / do not use:** Use `lazy` for non-critical content, `preload` only for truly critical resources, and `preconnect` only for origins you will contact. Do not lazy-load the main above-the-fold image by default.

**Write yourself:** Audit one page for blocking resources, image dimensions, lazy loading, preconnect, and unnecessary preload. Record a before/after hypothesis.

## 28. HTML parsing

Classic scripts pause parsing at their location. `defer` downloads in parallel and executes after parsing in document order. `async` downloads in parallel and executes as soon as ready, with no order guarantee.

**Syntax**

```html
<script src="analytics.js" async></script>
<script src="app.js" defer></script>
```

**Example:** Independent analytics can be `async`; an app that depends on parsed markup and ordered modules can be `defer`.

**Use / do not use:** Choose based on dependency and timing, not habit. Do not use `async` for scripts that depend on one another or on DOM order.

**Write yourself:** Create three pages, one with each loading strategy, and use logging to describe parse and execution order.

## 29. Forms + HTTP fundamentals

On submit, a form constructs a request from successful named controls. GET places encoded data in the URL; POST normally sends it in the request body.

**Syntax**

```html
<form action="/login" method="post">
  <input name="email" type="email" required>
  <input name="password" type="password" required>
  <button>Log in</button>
</form>
```

**Example:** A search form uses `method="get"` so its query can be bookmarked; a login form uses POST because credentials should not be placed in the URL.

**Use / do not use:** Use correct `action`, method, names, and server-side validation. Use HTTPS for sensitive submissions. Do not put secrets in query strings or assume client validation is security.

**Write yourself:** Build a search form and a profile-update form. Inspect the resulting URL/request and explain every field sent.

## EXPERT checkpoint

Before considering the course complete, explain:

- The source-to-DOM-to-paint pipeline.
- Why text nodes matter when traversing the DOM.
- How accessible names, focus, and ARIA state work together.
- How `picture`, `srcset`, and `sizes` differ.
- Why `defer`, `async`, and classic scripts execute differently.
- Which form values become a request and why.

## EXPERT project

Build a small **accessible, responsive documentation site** in `projects/expert-docs/`. Include responsive images, a keyboard-tested disclosure and dialog, a captioned media example, safe external links and an iframe, metadata/structured data, a performance audit, and GET/POST forms. Include a short `NOTES.md` explaining browser behavior and security decisions.
