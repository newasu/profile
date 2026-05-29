# Wasu Kudisthalert, Ph.D. - Portfolio & Development Rules

This document consolidates all core design rules, layout architectures, export schemas, and printing guidelines established during our pair-programming sessions. Adhere strictly to these rules in all future modifications.

---

## 📂 1. Portfolio Architecture & Data Profiles

### Dual-Mode Separation Rule
- **index.html** operates in two separate modes controlled by the interactive header tabs:
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

### Typography & Fonts
- **Serif Accents**: `Cormorant Garamond` (Prestige Serif) for headers and major name displays.
- **Body & Sans**: `Plus Jakarta Sans` for geometric, clean, and highly readable body texts at small print dimensions.

### Color Palette
- **Forbidden Colors**: Warm orange, yellow, and amber tones are completely prohibited.
- **Refined Theme**: Utilize an ultra-premium **Imperial Navy** (`#1b365d` brand brand-600), **Cool Slate** (`#475569`), and **Platinum Silver** color system.

### Contact Info & Bio List Styling
- **Header Address**: The address `Prawet, Bangkok, Thailand` must reside at the very end of the inline contact details row. Icons must precede their respective text labels.
- **Personal Bio Boxes**: Rendered as ultra-minimal inline tables/lists without background boxes or heavy borders.
- **Gender & Birthday Alignment**: Render `Sex: Male` inside the Personal Bio box on both views. The order must place **Sex** before **Birthdate**.
- **No Personal Email**: Only the academic email `wasu.ku@kmitl.ac.th` may be displayed in the headers. Personal emails (e.g. gmail) are suppressed.

---

## 💻 3. Interactive Digital Controls & Single-Line Header

### Header UI Single-Line Rule
- All interactive controls in the sticky header bar **must remain on a single line** under all view conditions.
- **Layout Constraints**:
  * Set header container to `flex flex-row flex-nowrap items-center justify-between gap-3 overflow-x-auto`.
  * Add `flex-shrink-0` to all action buttons and the toggle pill to prevent layout collapsing.
  * Hide the logo subtitle `Academic & Professional Portfolio` on smaller screens using Tailwind's responsive class: `hidden sm:block`.
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
- **Avoid Orphan Sheets**: Enforce strict `page-break-after: avoid !important;` on the active last visible page being printed:
  * `.resume-container .a4-page:last-child` (Resume View)
  * `.cv-container .a4-page:last-child` (CV View)

### Theme Preservation & Suppression
- **Dark Mode Printing**: Decouple theme color print overrides so that if dark mode is active on screen, the PDF is printed with pristine navy backgrounds. Apply `-webkit-print-color-adjust: exact !important` and `print-color-adjust: exact !important`.
- **Suppress File Paths**: Uncheck "Headers and Footers" in Chrome/Safari print settings to hide default browser-injected URLs (`file:///...`).
- **Whitespace Protection**: All publication years and quartile rankings (e.g. `CORE A*`, `Q1`) must be wrapped in standard inline `<span class="whitespace-nowrap">` wrappers and utilize the unified, stable `.pub-badge` class strictly on a single code line (no source carriage returns or tab spaces inside spans) to prevent vertical alignment overlap and bounding box offset bugs inside `html2canvas` and print engines.
- **Clickable Links**: All email addresses and LinkedIn tags must be wrapped in active anchor `<a>` tags to preserve interactive clickability inside exported PDF files.

---

## 📐 6. Scalable A4 Vector Exporter (SVG)

### Graphics Canvas Dimensions
- To guarantee perfect A4 canvas alignment when SVG assets are imported into Figma or Adobe Illustrator, the exporter must lock standard physical bounds at **72 DPI PostScript Points**:
  * Physical bounds: `width="595.27"` and `height="841.89"`
  * Style constraints: `style="width: 595.27px; height: 841.89px;"`
  * Coordinate viewBox: `viewBox="0 0 793.7 1122.52"` (scales high-fidelity Tailwind elements automatically).

### Stylesheet Extraction & Scrollbar Hiding
- The SVG parser must extract all active styles (Tailwind compiled rules + custom inline overrides) and encapsulate them.
- External Google Fonts and Phosphor Icons must be bundled inside `@import` rules inside the SVG `<defs>`.
- Block default browser scrollbars inside the generated SVG file by enforcing `overflow: hidden !important;` on the `<foreignObject>` container.

---

## 🖼️ 7. High-Resolution Image Exporter (PNG)

### Bypassing Security Taints
- Traditional `<foreignObject>` canvas drawing triggers strict security taints in WebKit/Blink engines. Bypass this entirely by utilizing the **`html2canvas`** library.
- `html2canvas` constructs pixel-perfect renders by parsing active computed CSS nodes from the DOM.

### Exporter Configurations
- **Crisp Pixel Scale**: Use `scale: 3` to achieve ultra-sharp high-resolution density exports.
- **Cross-Origin Handling**: Set `useCORS: true` to guarantee Google Fonts and Phosphor Icon CDNs fetch cleanly.
- **Active State Binding**: Exporter background color must adapt dynamically based on `body.classList.contains('dark')` (`#0f172a` for dark, `#ffffff` for light).


