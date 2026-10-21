# Wasu Kudisthalert, Ph.D. - Portfolio & Development Rules

This document consolidates all core design rules, layout architectures, export schemas, and printing guidelines established during our pair-programming sessions. Adhere strictly to these rules in all future modifications.

---

## 📂 1. Portfolio Architecture & Data Profiles

### Two pages
- **`index.html`** (profile.wkneil.com) is the public profile landing page, also embedded by course landing pages. Its rules are in **§9**.
- **`cv/index.html`** (profile.wkneil.com/cv/) is the Resume / CV. Everything in §1 to §8 below is about this page unless it says otherwise.

### Dual-Mode Separation Rule
- **cv/index.html** operates in two separate modes controlled by the interactive header tabs:
  1. **Resume (1-Page)**: A high-level professional summary.
  2. **CV (2-Pages)**: A detailed and comprehensive academic curriculum vitae.
- Data is mapped statically in HTML elements using values from `WK_CV.json` to enable zero-CORS, lightweight, and offline-compatible rendering.

### Resume Layout Rules (1-Page)
- **Strictly Flat Design**: No timeline lines, no timeline indicator circles, and no timeline dots on the Resume view.
- **Section Exclusions**: The "Key Skills" and "Languages" sections must **never** be rendered on the Resume page. They reside exclusively in the CV.
- **Content Requirements**:
  * All **Research Experiences** listed in the JSON profile must be fully rendered in a clean flat layout on the left column.
  * All **Selected Publications** listed in the JSON profile must be fully rendered in a clean flat paragraph list on the right column.
  * The **Research Focus** section must list all **interested research topics** listed in the JSON profile in full.
  * Must include the **High School Diploma** in the Education section.

### CV Layout Rules (2-Pages)
- **Timeline Accents**: Timeline lines, circular bullets, hollow square indicators, and CEFR language proficiency tags reside **exclusively** on the CV pages.
- **Timeline Symmetrical Grid**: Maintain a consistent left-aligned timeline grid inside both CV page columns.
- **Content Distribution**:
  * Page 1: Personal Bio, Education, Detailed Skills & Specializations, and Full Work Experience (6 roles).
  * Page 2: Research Experience & Projects, and all publications listed in the JSON profile.

---

## 🎨 2. Design System & Symmetrical Aesthetics

These are the CV page's (`cv/index.html`) design rules. The profile page has its own type and colour tokens (§9); the forbidden-colour rule below applies to both.

### Typography & Fonts
- **Sizes**: body text 10–11px, roles 13.5px, section headings 15.5px (raised 1px across the board on 2026-10-05 for readability). Page padding is 12mm on screen, export and print alike. Keep ≥20px free per column in both languages; the contact row and the CV page-2 header must stay on one line.
- **Serif Accents**: `Cormorant Garamond` (Prestige Serif) for headers and major name displays.
- **Body & Sans**: `Plus Jakarta Sans` for geometric, clean, and highly readable body texts at small print dimensions.

### Color Palette
- **Forbidden Colors**: Warm orange, yellow, and amber tones are completely prohibited.
- **Refined Theme**: Utilize an ultra-premium **Imperial Navy** (`#1b365d` brand brand-600), **Cool Slate** (`#475569`), and **Platinum Silver** color system.

### Contact Info & Bio List Styling
- **Header Contact Row**: email · phone · LinkedIn · classes.incortx.com · Google Scholar (`ph-google-logo`, links to the Scholar profile). The whole row must fit on one line in both languages. Icons must precede their respective text labels.
- **Location**: `Bangkok, Thailand` (TH: `กรุงเทพฯ ประเทศไทย`) is the last row of the Personal Bio box (`Location` / `ที่อยู่`), not in the header.
- **Personal Bio Boxes**: Rendered as ultra-minimal inline tables/lists without background boxes or heavy borders.
- **Gender & Birthday Alignment**: Render `Sex: Male` inside the Personal Bio box on both views. The order is Nickname, Nationality, **Sex**, **Birthdate**, Location.
- **No Personal Email**: Only the academic email `wasu.ku@kmitl.ac.th` may be displayed in the headers. Personal emails (e.g. gmail) are suppressed.

---

## 💻 3. Interactive Digital Controls & Single-Line Header

### Header UI Single-Line Rule
- All interactive controls in the sticky header bar **must remain on a single line** under all view conditions.
- **Layout Constraints**:
  * Set header container to `flex flex-row flex-nowrap items-center justify-between gap-3 overflow-x-auto`.
  * Add `flex-shrink-0` to all action buttons and the toggle pill to prevent layout collapsing.
  * Hide the bar subtitle `Academic & Professional Profile` on smaller screens (the WK monogram stays in the bar) using Tailwind's responsive class: `hidden sm:block`.
  * Set all action buttons and the toggle pill to a unified height of `h-8` (32px).
- **Compact Button Sizing**:
  * Export buttons must display concise text: **`SVG`** and **`PNG`** with their respective icons.
  * Print action button must say **`Print PDF`** with the printer icon.

### Switcher Animation Math (240px Pill)
- The main switcher container width is set to `240px` with `p-1` (4px padding), creating an inner sliding space of `232px`.
- The sliding active block background `toggleSlider` width must be set to exactly `116px`.
- **Slide JavaScript Offsets**:
  * **Resume view**: `left = '4px'`
  * **CV view**: `left = '120px'`

---

## 🔗 3b. URL Parameters (cv/index.html)

- `?view=cv|resume` · `?lang=th|en` · `?reveal=1` (also `true`/`yes`; anything else stays masked). Example: `cv/?view=cv&lang=th&reveal=1`.
- Defaults are omitted (resume, masked, saved language or English). URL `lang` beats the saved `cv-lang` preference.
- Every toggle rewrites the address bar with `history.replaceState` (via `updateDocumentTitle()` → `syncUrl()`), so the current URL is always a shareable link to exactly what is on screen. A `reveal=1` link shows real company names to whoever opens it.

---

## 👁️ 4. Dynamic Anonymization & Company Masking

### Privacy Mode Behavior
- An eye-icon toggle button (`Real Names` vs `Masked`) triggers anonymization by toggling the `.public-mode` class on the `<body>` element.
- **Academic Preservation**: King Mongkut's Institute of Technology Ladkrabang (KMITL) and BIOTEC **must always remain fully visible and unchanged** in both modes.
- **Company Masking Scheme**:
  * **Real**: `AI & Robotics Ventures (ARV)`  
    **Masked**: `Deep-Tech Venture Builder & AI/Robotics Subsidiary of a Fortune 500 Global Energy Conglomerate`
  * **Real**: `Avalant`  
    **Masked**: `Enterprise Software & CMS Solutions Vendor`
  * **Real**: `The Expertise`  
    **Masked**: `Consolidated Public Sector & Enterprise Consultancies`
- **Department Masking**: In public/masked mode, specific departments `(Marine Dept, NSO, Dept of Health)` in CV Page 1, Role 2 must be replaced with the generic label: `Various Government Agencies`.

---

## 🖨️ 5. Printer Optimization & Flawless PDF Rules

### Strict Pagination Constraints
- **Resume Mode** must print on **exactly 1 page**.
- **CV Mode** must print on **exactly 2 pages**.
- **Zero Overflow Page-Breaks**: Neutralize wrappers, gaps, paddings, and heights of container layouts under `@media print`.
- **Mobile Print Scale**: `adjustScale()` shrinks `.a4-page` with an inline `transform: scale()` on narrow screens (≈0.45 on an iPhone). The print CSS must keep `transform: none !important` and the `beforeprint` handler must clear the inline scale, or phones print the page at 45% size.
- **iOS Print Margins**: iOS Safari ignores `@page { margin: 0 }`, prints inside its own margins (plus a URL/date header and footer), and shrinks anything wider than its printable area. On iPhone/iPad the page adds `body.ios-print`, which scales `.a4-page` with `transform: scale(0.77)` from `0 0`, centred by `text-align: center` on the containers (the page resets `left`; a fixed left margin came out ~6% wider on the iPad and pushed the page against the right edge) and negative right/bottom margins (−23% of 210mm/297mm) so it takes 162 × 229mm in the layout too. An iPad A4 sheet paginates every ~237mm (measured from an iPad PDF in which a paragraph was pushed to the break); 0.86 (255mm) and 0.82 (244mm) each printed a blank second sheet. Note the footer printing fine does NOT prove a page fits: iPad draws an unsplittable box whole on its sheet, spilling into the margin; the page is `display: inline-flex` (an inline box is never split across sheets; as a block, still 297mm tall in the layout, it was cut at the printable height), html/body/main/containers are `width: auto`, the containers `display: flow-root` (else the negative margin collapses out and a blank sheet follows). Never use `zoom` for this: in an iPad print it shrank the box but not the text. `html { -webkit-text-size-adjust: 100% }` stops iOS Text Autosizing. Check with an iPad Save-as-PDF: one sheet for the Resume, text wrapping as on desktop. A desk check: Chromium `page.pdf()` with `body.ios-print` and `@page { margin: 14.3mm 13.7mm 46mm 13.7mm }` (182.6 × 237mm, the iPad's measured page) reproduces it; Chromium counts overflow toward pages, iPad may not, so keep the drawn size within 237mm too.
- **Avoid Orphan Sheets**: Enforce strict `page-break-after: avoid !important;` on the active last visible page being printed:
  * `.resume-container .a4-page:last-child` (Resume View)
  * `.cv-container .a4-page:last-child` (CV View)

### Theme Preservation & Suppression
- **Dark Mode Printing**: Decouple theme color print overrides so that if dark mode is active on screen, the PDF is printed with the Deep space sheet `#0D0F13`. Apply `-webkit-print-color-adjust: exact !important` and `print-color-adjust: exact !important`.
- **Suppress File Paths**: Uncheck "Headers and Footers" in Chrome/Safari print settings to hide default browser-injected URLs (`file:///...`).
- **Whitespace Protection**: All publication years and quartile rankings (e.g. `CORE A*`, `Q1`) must be wrapped in standard inline `<span class="whitespace-nowrap">` wrappers and utilize the unified, stable `.pub-badge` class strictly on a single code line (no source carriage returns or tab spaces inside spans) to prevent vertical alignment overlap and bounding box offset bugs inside `html2canvas` and print engines.
- **Clickable Links**: All email addresses and LinkedIn tags must be wrapped in active anchor `<a>` tags to preserve interactive clickability inside exported PDF files.

---

## 🌗 Theme (2026-10-21)

- **Dark = Deep space**, the profile page's palette: page `#050608`, sheet `#0D0F13`, lines `#1C2027`/`#2A2F38`, text `#F4F5F7`/`#C4C8D0`/`#9EA3AD`, accent `#9DB8E6`. The `.dark .x` overrides near the top of the `<style>` map the Tailwind slate classes onto it.
- **Theme button**: two modes only, Light (the default, whatever the device is set to) and Dark; the icon shows what a click switches to (`ph-moon` / `ph-sun`). The choice is stored under `theme`, the same key as the profile page (same origin); the old `cv-theme` key is read once and removed.
- **Control bar** (`header.cvbar`) follows the theme: white in light, `#050608` in dark. Its Tailwind classes are the dark look; the `header.cvbar …` rules override them for light.

## 📐 6. Scalable A4 Vector Exporter (SVG)

### True vector, no `<foreignObject>`
- `exportSVG()` runs **dom-to-svg@0.12.2** (`cdn.jsdelivr.net/npm/dom-to-svg@0.12.2/+esm`) on the live, laid-out page: text becomes `<text>/<tspan>`, boxes and borders `<rect>/<path>`. Figma, Illustrator and Inkscape can open and edit it.
- Never go back to wrapping HTML in a `<foreignObject>`: design tools show a blank page, and WebKit (Safari, Preview, Quick Look) ignores the viewBox scale for it, so a 595 × 842 outer size showed the page at 1.33×, cropped.
- Canvas: `width`/`height` = the page box (793.7 × 1122.5 px = A4 at 96 DPI) with a matching viewBox.

### Justified text and word positions
- dom-to-svg fits every line chunk with `textLength` + `lengthAdjust="spacingAndGlyphs"`, which stretches the glyphs of justified lines (they look a size larger); `lengthAdjust="spacing"` letter-spaces them instead, and WebKit ignores `word-spacing` on SVG text. So the exporter drops `textLength` and places **one tspan per word at its own x**, spreading the line's gap over its word spaces as HTML justify does. Lines are measured on an off-screen copy (`getComputedTextLength()`, `getSubStringLength()`).
- A line is split into one tspan per inline element (bold name, italic venue), each at its own x. Browsers drop a tspan's leading/trailing spaces when measuring: trim both, shift x right by one space for a leading space, and use the next tspan's x as the chunk's end. Chunks followed by another chunk are always split into words, because WebKit sets text ~0.5% wider than the Chromium layout, enough to close a space.
- Check after any exporter change: gap before every word that follows another tspan on the same line > 1.5 px in both Chromium and WebKit, rightmost text ≤ ~751 px (column edge 748). Thai lines without spaces stay ragged.

### Embedded Fonts & Icons
- On export, `buildEmbeddedFontCss()` inlines as base64 only the font faces `document.fonts` reports as loaded (Google Latin subset + `Sarabun Doc`) in a `<style>` at the top of the SVG, merging variable-font weights that share one file. Browsers and viewers use them; design tools ignore embedded fonts and substitute installed ones of the same name.
- `inlineIcons()` swaps each Phosphor `<i class="ph ph-*">` on the live page for that icon's own path from `@phosphor-icons/core@2.1.1/assets/regular/` and returns the swaps; the exporter puts the `<i>` back after the run.
- Result: ~230–275 KB per English page, ~340 KB per Thai page. Export itself needs internet to fetch the files once.

### Masking & Phone Scale
- dom-to-svg only draws rendered boxes, so the hidden mask variant (`display: none`) and HTML comments never reach the SVG: masked files contain no real company names (check with a grep after any exporter change).
- The page's inline phone-screen transform from `adjustScale()` is cleared for the run and restored after.

---

## 🖼️ 7. High-Resolution Image Exporter (PNG)

### Bypassing Security Taints
- Traditional `<foreignObject>` canvas drawing triggers strict security taints in WebKit/Blink engines. Bypass this entirely by utilizing **`html2canvas-pro`** (drop-in fork; html2canvas 1.4.1 drew text a few px below its box).
- `html2canvas` constructs pixel-perfect renders by parsing active computed CSS nodes from the DOM.

### Exporter Configurations
- **Phone Scale**: Clear the inline `adjustScale()` transform on every page before rendering, export pages one at a time, and call `adjustScale()` once at the end; otherwise phones export a 45%-size, low-resolution image.
- **Crisp Pixel Scale**: Use `scale: 3` to achieve ultra-sharp high-resolution density exports.
- **Cross-Origin Handling**: Set `useCORS: true` to guarantee Google Fonts and Phosphor Icon CDNs fetch cleanly.
- **Active State Binding**: Exporter background color must adapt dynamically based on `body.classList.contains('dark')` (`#0D0F13` for dark, `#ffffff` for light).



---

## 🌐 8. EN / TH Language Switch (cv/index.html)

- A `TH`/`EN` pill in the control bar calls `toggleLanguage()`; the choice persists in `localStorage` (`cv-lang`) and `?lang=th` in the URL overrides it.
- Thai text lives in the `TH_TEXT` object, **keyed by the element's English innerHTML (whitespace-collapsed)**. Editing an English string means updating its key too, or that element silently stays English in Thai mode.
- Only the A4 pages are translated; the control bar (header buttons, mask label) always stays English.
- Publication citations and the Research Focus tags stay in English. Thai dates use พ.ศ. (CE + 543) with abbreviated Thai months.
- Thai wording: technical terms and industry job titles stay in English (Computer Vision, Data Science, ML production pipeline, Lead Machine Learning Engineer); native Thai words, official Thai titles (อาจารย์มหาวิทยาลัย) and organisation names stay Thai.
- Thai typography: `body.lang-th .a4-page` (scoped to the pages, never the control bar, or the bar reflows on every switch) turns off justify, letter-spacing (tracking) and italics; transliterated loanwords are wrapped in `.th-word` (nowrap) so the browser does not split them mid-word.
- Fonts: Thai glyphs use `Sarabun Doc`, the page's own `@font-face` for the Thai subset of Sarabun (Google build of TH Sarabun New, the government document font) with `size-adjust: 110%` so Thai matches the Latin x-height. It sits after the Latin fonts in each stack, so English glyphs are unchanged.
- Thai wording: formal, complete sentences with connectives (โดย, เพื่อ, ทั้ง…และ, อีกทั้ง), not stacked noun phrases. Dates keep month and year together with `&nbsp;`; loanwords and words like ภาครัฐ / ไบโอเทค sit in `.th-word` so they never split.
- Exports and the print title get a ` - TH` suffix in Thai mode. Resume must still print as 1 page and CV as 2 pages in both languages.

---

## 🪪 9. Profile Landing Page (index.html)

### Content
- **Always blind**, like the CV's masked mode: masked company names, no work or research dates (current roles show only `Present`). No Education section (removed 2026-10-20 at the user's request; degrees live in the CV).
- **Section order**: Hero → Teaching → Professional Experience → Research Interests → Research Experience → Selected Publications → footer. No mark in the hero (the WK monogram was removed 2026-10-18); the name carries it.
- **Research interests sphere**: on the standalone page at every width, a script reads the Research Interests chips, draws them as a rotating, draggable word sphere (`.wkp-ri-stage`, colours from the `--wkp-*` tokens, no auto-rotation under reduced motion, paused off-screen) and hides `#research-interests` so the interests appear once. ≥861px: 300px square on the right of the hero. ≤860px: stacked under the buttons, `min(300px, 100%)` wide at 6:5 (horizontal drag rotates, vertical swipes still scroll via `touch-action: pan-y`). Embedded copies keep the plain chips section. Edit the interests in the chips (and `WK_CV.json`), never in the script.
- **Hero buttons**: email (copy button), LinkedIn, Google Scholar, INCORTX Classes. ORCID is kept in `WK_CV.json` but not shown.
- **Teaching** comes from `WK_CV.json` → `teaching` (courses and talks, absolute links to classes.incortx.com). No terms, years or session counts.
- **Professional Experience** lists two current roles: University Lecturer (KMITL) and Founder & Director, INCORTX (`WK_CV.json` → `incortx`).
- **INCORTX** is described broadly in two parts, Classes and research (`incortx.research`, from the ClickUp Research space). Keep it broad: one line ("medical imaging AI with clinical partners"), no project-level detail, no partner, hospital or collaborator names. The Publications section opens with one broad line on the fields of the published work (`incortx.research.published_themes`).
- **Publications** link to `https://doi.org/<doi>` from each entry's `doi` field, show the DOI in mono under the venue, and end with "All publications on Google Scholar".

### Embed contract (course landing pages fetch this page)
- Every `<style>` and the single `.container` are injected into the host page and scripts never run there. All rules stay scoped under `.wkp`, variables are `--wkp-*`, links are absolute, sections keep `.fade-up`.
- **Standalone-only** rules use `body > .wkp` or `body:has(> .wkp)`: the dark theme, the page background and the theme button. An embedded copy is always the light theme and never shows the theme button.
- The email stays a `<span id="instructor-email" data-u="wasu.ku" data-d="kmitl.ac.th">`; never write `mailto:` in the HTML (Cloudflare rewrites it). On the standalone page a script turns every `[data-u][data-d]` span into a button that copies the address and shows "Copied".

### Look
- **Type**: `Instrument Serif` (name, section heads, years), `Plus Jakarta Sans` (body), `JetBrains Mono` (small uppercase labels, DOIs), loaded with one `@import` inside the style block so embedded copies get them too.
- **Light**: white page, near-white cards `#FCFCFD` lifted by a soft shadow, text `#0B0D12`, navy accent `#1b365d`.
- **Dark** (Deep space, as on classes.incortx.com): page `#050608`, cards `#0D0F13`, no shadows. Text accent becomes `#9DB8E6` (navy is unreadable on black); navy stays for fills such as the CORE A* badge.
- No warm colours (§2 applies): the gold "Light" accent of classes.incortx.com is never used here.
- Every text colour is checked at 4.5:1 or better on every background it sits on, in both themes.

### Theme button
- Fixed top-right, standalone only, cycles Auto → Light → Dark by setting `data-theme` on `<html>` (Auto removes it). The choice is kept in `localStorage` under `theme` and a one-line script in `<head>` applies it before first paint. Phones pad the page top (76px) so the button never covers the hero.

### Share preview
- `og:*` and `twitter:card` tags point at `https://profile.wkneil.com/og-image.png` (1200 × 630, no monogram: name, role, KMITL, URL). Re-render it whenever the name, title or institution changes.
