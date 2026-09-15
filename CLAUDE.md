# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A static HTML5/CSS3 online resume (CV) website for a Korean front-end developer. This is a simple static website with no build process or dependencies beyond basic web technologies.

The single `index.html` is the source for **two documents**: the 1–2 page 이력서 (compact view — the default, what GitHub Pages shows and what prints) and the full 경력기술서 (`?full`). See Content Writing Guidelines → Two Outputs from One Source.

## Architecture

- **Single-page static site**: Built with semantic HTML5 structure
- **CSS Architecture**:
  - `assets/css/common.css` - Global CSS reset (removes default margins, padding, list styles)
  - `assets/css/style.css` - Main layout and component styles with absolute positioning for header elements
- **Asset Organization**:
  - `assets/font/noto/` - Korean Noto Sans font files for proper Korean text rendering
  - `assets/bower_components/components-font-awesome/` - Font Awesome icons for contact info
  - `assets/img/` - Profile images and favicon

## Development Commands

This is a static website - no build process, dependencies, or package managers required. Simply open `index.html` in a browser to view the resume.

For local development:
```bash
# Serve locally using Python (if available)
python -m http.server 8000
# Then open http://localhost:8000 in your browser

# OR using Node.js (if available)
npx serve .

# OR simply open the file directly
open index.html
```

No linting, building, or testing commands are configured - this is a pure static HTML/CSS project.

## Key Design Patterns

- **Layout Strategy**: Uses absolute positioning for header elements (h1, name, job title, profile picture) with precise pixel positioning
- **Typography Hierarchy**:
  - h1 (42px) - Main "Résumé" title
  - h2 (24px) - Section headings (경력사항, 프로젝트, 기술스택)
  - h3 (18px) - Sub-headings within sections
- **List Styling Conflict**: `common.css` removes all list styles globally (`list-style: none`), but `style.css` adds disc bullets for experience details (`list-style: disc`) and circle bullets for nested lists (`list-style: circle !important`). When editing lists, be aware of this override pattern.
- **Responsive Considerations**: Fixed 960px width layout designed for desktop viewing

## Content Structure

The page renders in two modes (compact 이력서 by default / full 경력기술서 with `?full` — see Content Writing Guidelines → Two Outputs from One Source). The section list below is the full view; the default compact view hides sub-bullets and `.career-only` elements.

The resume contains these main sections:
- **Header**: Contact info, profile picture, name/title with flexbox layout and absolute positioning
- **자기소개 (Introduction)**: Personal introduction paragraph in `<p class="introduction">`
- **경력사항 (Experience)**: Work history with company names, dates, and nested bullet-point details
- **프로젝트 (Projects)**: Project listings with URLs, descriptions, and technology stacks (uses `experience` class structure)
- **사이드 프로젝트 (Side Projects)**: Personal projects separate from work projects (uses same `experience` class structure)
- **기술스택 (Skills)**: Category text lists (`Languages: …`, `Technologies: …`, `Tools: …`) - ONLY section using `resume-item skill` class. Skill bars were retired 2026-08-16 (see Content Writing Guidelines → Skills Section)
- **자격증 (Certifications)**: Professional certifications with issuing organizations (uses `experience` class)
- **대외활동 (External Activities)**: Awards, competitions, and volunteer work (uses `experience` class)
- **교육 (Education)**: Academic background (uses `experience` class)
- **Footer**: Copyright notice with year and GitHub link (update year manually when needed), plus the screen-only mode toggle link (`.mode-link.only-compact` → `?full`, `.mode-link.only-full` → `index.html`)

## Important Technical Details

- **Fixed Width Layout**: 960px container with centered positioning
- **Header Layout**: Uses `display: flex` with `align-items: end` for contact container
- **Section Positioning**: Main content uses absolute positioning for section headers (right: -240px)
- **Korean Typography**: Noto Sans Korean font family specifically for Korean text rendering
- **Icon Integration**: Font Awesome icons positioned absolutely within contact list items
- **Print Support (Cmd+P → PDF)**: `assets/css/style.css` ends with an `@page` rule and an `@media print` block that make the printout deterministic instead of depending on browser defaults. Printing produces the compact 이력서 unless the URL has `?full` (see Editing Guidelines → Two-Mode Markup). What the block does, and why:
  - `@page { size: A4; margin: 10mm 0 }` — fixes paper and margins so the browser's default margins do not stack on top of the page's own side padding (the "double margin" look). Side margin is 0 because the wrapper's 60px padding already provides it
  - `.resume-wrapper { zoom: 0.826 }` — 960px → 210mm, so the print is the screen layout scaled to the paper width, the same in every browser, rather than each browser's own shrink-to-fit. `body`/`html` backgrounds are forced white, the card shadow removed
  - Vertical rhythm is tightened (header/section/footer padding, sub-bullet gap 10→3px) so the compact view fits 2 A4 pages and the full view does not spill a lone footer onto an extra page. Print rules that fight `#main ul li` must carry `#main` too — the id selector wins otherwise (this bit us once)
  - `#main .resume-item h2 { top: 0 }` — the screen value `-10px` clips the section heading when a section starts a new page
  - Break rules: `break-inside: avoid` on bullet groups (`.detail > li`), skills rows, the intro, and — compact only — whole company entries; `break-after: avoid` on entry headings so a company/project title never ends a page alone. Deliberately **not** on tall blocks (a company entry in full view, a whole section): an unbreakable block taller than the space left is pushed to the next page and leaves a blank gap
  - Measured 2026-09-12 (Chromium `page.pdf`, `preferCSSPageSize`, every page rasterized and inspected): compact 2 pages, full 4 pages, A4, selectable text, no clipped edge, no half-empty page. Same result with an extra 10mm user margin on all sides
  - Cmd+P checklist for the user: 용지 A4, 여백 "기본값" (Chrome takes the `@page` values), 배율 "기본값", "머리글 및 바닥글" 체크 해제 (otherwise the browser prints date/URL into the 10mm band), "배경 그래픽" not needed. After any layout or spacing change, re-run the measurement above — page count is the regression test

## Content Writing Guidelines

Sources:

- Primary: [GreatFrontEnd — Ultimate Guide to Front End Engineer Resumes](https://www.greatfrontend.com/front-end-interview-playbook/resume) (front-end-specific advice; the only resume page in that playbook. Last re-read against the live page 2026-09-12 — every point of the article is reflected below)
- Secondary: [Tech Interview Handbook — Resume](https://www.techinterviewhandbook.org/resume/) (the general software-engineer guide the primary one links to for non-front-end-specific advice: ATS, section order, summary, contact info)

Apply these when writing or reviewing resume **content** (wording, what to include), as opposed to the markup rules in Editing Guidelines below. Where this repo deliberately deviates from a guide, the deviation is noted inline as "Repo decision".

### Two Outputs from One Source (Repo decision 2026-09-12)

The guides' length rules (1 page under 5 years, 2 max) and the Korean convention of submitting 이력서 and 경력기술서 as separate documents are reconciled by producing **both from this one file**. Never maintain them as two files — drift between copies (a metric updated in one and not the other) is the failure this rule exists to prevent.

- **이력서 = compact view (the default).** This is what GitHub Pages shows, what a recruiter sees first, and what prints. Must fit 1–2 A4 pages. What survives:
  - Header, 자기소개, 기술스택, 자격증, 교육, footer — unchanged
  - 경력사항: **top-level bullets only**, 3–5 per company. Each is a single line that carries its own metric — it is the only line the 이력서 reader sees
  - 프로젝트 / 사이드 프로젝트: title + link + period + the first top-level bullet as a one-line summary (stack named in it)
  - 대외활동: title, organization, date only
- **경력기술서 = full view (`?full`).** Everything on the page: every section, every sub-bullet, live links. No page limit. Reached from the footer link "경력기술서 전체 보기" or by sharing the `?full` URL directly; it doubles as the portfolio
- **How the split is expressed in markup**: nested `<ul>` inside `ul.detail` (sub-bullets) are hidden in compact mode automatically. Anything else that belongs only to the 경력기술서 — a project's 2nd+ top-level bullet, 대외활동 detail lists, a 6th 경력 bullet — gets `class="career-only"`. Mechanics are in Editing Guidelines → Two-Mode Markup
- **Writing consequence**: a top-level 경력 bullet must stand alone — action verb, what, one number, stack — and read naturally both with and without its sub-bullets. Put the headline metric in the top-level line; sub-bullets carry the how (approach, secondary numbers, technology detail). Never duplicate a sentence across the two levels to make compact read well; reword the top line instead
- **Verification after any 경력사항 / 프로젝트 edit**: open `index.html` (default = compact) and check it reads as a complete resume on its own, print it to PDF and confirm it is ≤ 2 pages with selectable text; then open `index.html?full` and check nothing is orphaned (a sub-bullet whose parent line was reworded, a `.career-only` line that now repeats its parent)

### Bullet Point Formula

Every achievement bullet should follow: **Action verb + specific work + measurable outcome + technology** (TIH phrasing: `[Accomplishment summary]: [Action] that resulted in [quantifiable outcome]`).

- Bad: "웹사이트 개발 (HTML, CSS, JavaScript 사용)" — generic, no scale or impact
- Good (existing example in this resume): "React.cache() 적용 및 Promise.all 병렬화로 서버 중복 요청 90% 감소"
- Guide's model bullet: "Built a performant e-commerce website that allowed users to browse from hundreds of items, add to their cart and checkout with PayPal. The site has 20k MAU, loads under 2 seconds and a Lighthouse score of 98. Technologies used: Next.js, React, GraphQL, CSS modules" — note the order: what the product does (feature scope) → **scale, complexity, and impact** numbers → the stack. Generic bullets like "built a website" get filtered out
- Quantify scale, complexity, and impact wherever possible. A bullet without a number should prompt: "can this be measured?"
- **Only include a number when it works in your favor.** A small figure (e.g., two-digit user counts, a handful of seed feeds) reads as "no traction" and hurts more than no number — leave it out and let the live URL speak; add it once it has grown. Numbers pulled from a repo's docs must be checked against the *current* state before use (Repo decision 2026-08-16, learned on Indieblog)
- Use active voice. Repo convention (not from the guide): lead bullets with a strong action verb — 구축, 설계, 개선, 자동화, 최적화, 도입, 마이그레이션. Bullets that end in a bare noun phrase ("투표 등록 시스템", "관리자 대시보드") are task listings, not achievements — rewrite them
- List positions and projects in reverse chronological order

### Front-End Metrics Worth Quantifying

- **Product work**: what the feature does + user impact (MAU, users served, feature scope)
- **Performance**: % gain in load time, page/bundle size reduction, Lighthouse score
- **Testing**: number of tests written, critical flows covered, coverage % (e.g., "커버리지 90%+")
- **SEO**: number/% of errors and warnings reduced (measurable via Ahrefs, Semrush, Search Console)
- **Accessibility (a11y)**: errors fixed, workflows meeting WCAG AA/AAA, components improved
- **Tooling / modernization**: migrations (e.g., "Next.js 14→15", jQuery→React, TypeScript introduction) with scale context — LoC touched, number of engineers affected, time saved (e.g., "월 4시간 이상 반복 작업 자동화")

### Section Priority & Ordering

- 경력사항 (Work Experience) is the most important section — it should take up the majority of the page and carry the most detail. Frame bullets as contributions to the employer's **business goals**, not as a task log. Each entry needs company, title, team, and `MM/YYYY`-style dates
- Do not repeat the same bullet in both 경력사항 and 프로젝트. If a project belongs to a job, put the metrics-bearing bullets in one place and keep the other a short pointer
- 교육 (Education) goes below experience unless recent grad / under ~3 years experience with weak work history. GPA only if ≥ 3.5/4.0
- 자격증 / 대외활동 (Awards, Certifications): include only job-relevant items and quantify — TIH format `[Year] | [Quantification] | [Competition]` (e.g., "2023 | 전라북도지사상 (N팀 중 최우수) | 전북 청년 빅데이터 경진대회")
- TIH's recommended order for ATS: summary → contact → skills → experience → education → projects → other. **Repo decision**: this resume keeps 자기소개 → 경력사항 → 프로젝트 → 사이드 프로젝트 → 기술스택 → 자격증 → 대외활동 → 교육 (skills after projects). Reordering is a deliberate layout change, not a routine content edit
- Use standard section headings; never put symbols in headings (ATS parsing)

### Professional Summary (자기소개)

- Under ~50 words, active voice, action words, and **start with the role noun** ("프론트엔드 개발자 …") — this is the ATS keyword anchor
- Answer "why am I a fit for this job", not general philosophy. Lead with the strongest selling points (years, stack, domain), then working style
- TIH suggests turning the section title itself into a < 10-word headline (e.g., "Next.js 기반 서비스 개발 3년차 프론트엔드 개발자"). **Repo decision**: heading stays "자기소개"; put the headline as the first sentence of `.introduction` instead

### Projects (프로젝트 / 사이드 프로젝트)

- Include at least 2 projects; each should state the technology stack explicitly — especially modern technologies the target company uses. This helps ATS ranking and leaves a deeper impression on the reader
- Side projects signal passion and currency — technologies NOT used at the current job are a positive signal, so name them
- **Every project links to both the code (GitHub) and the live site where possible.** Repos should have a polished README with screenshots. If a live URL is a company dev/staging domain, verify it is publicly reachable — a dead link is worse than none
- Non-trivial open-source contributions or maintained OSS projects are worth their own bullet

### Skills Section (기술스택)

- **Format: category text lists, not proficiency bars.** Guide example: `Languages: HTML, CSS, JavaScript, TypeScript` / `Technologies: React, Next.js, Tailwind CSS, Jest, Storybook, Cypress, React Query, Yarn, webpack`. TIH format: `[Category]: [skills separated by "|"]`
- **Do NOT use skill bars, percentages, star ratings, or any self-assessed proficiency scale.** Neither guide uses them: the number is unverifiable, anything under ~70% reads as a weakness, and ATS cannot parse a bar. **Repo decision (2026-08-16)**: the original template's `.skill-bar` markup is retired; see Editing Guidelines for the replacement markup
- Keep at least one line each for Languages and Technologies. HTML, CSS, JavaScript must be in Languages ("otherwise there's something hugely wrong"); TypeScript belongs there too
- Do NOT list every technology touched — "two React-family frameworks beats a wall of React, Angular, Vue, Svelte, and Solid". Cap at 1–2 entries per category
- Guide's categories (1–2 entries each; not every category applies — include only the relevant ones): JavaScript libraries (React, Angular, Vue, Svelte), metaframeworks (Next.js, Gatsby, Nuxt, SvelteKit), state management (Redux, Flux, Jotai, Zustand, Relay), CSS (CSS Modules, Styled Components, Tailwind CSS, Sass, Less), CSS frameworks (Bootstrap, MUI, Chakra UI, Bulma), unit testing (Jest, Vitest, Storybook, Mocha, Jasmine, Karma), data fetching (GraphQL, tRPC, TanStack Query, SWR), integration testing (Cypress, Selenium, Puppeteer, WebdriverIO, Playwright), language tooling (Babel, TypeScript, Flow, ESLint), build tools — optional (webpack, Vite, Parcel, Gulp, Rollup, Browserify), package management — optional (npm, Yarn, pnpm)
- Selection criteria, in priority order: (1) appears in the target job listing and you have real experience with it, (2) architecture-influencing (React dictates the surrounding choices) rather than utility (Underscore/Lodash is swappable), (3) widely known or rapidly gaining popularity — shows you keep up with the ecosystem, and the company may be moving to it, (4) demonstrates breadth — never two technologies serving the identical purpose (few job listings ask for it, and it reads as a red flag); too many entries of one category makes your actual skills harder to read
- Only list what you can defend in an interview
- AI tooling (Claude Code) is a repo-specific addition not covered by the guides — keep it to a single "Tools" line. List only tools actually in daily use (Cursor dropped 2026-08). Protocols/specs like MCP are not skills — if there is real experience building MCP servers or similar, it belongs in an experience bullet with an outcome, not in the skills list

### Keyword & ATS Strategy

- Use "프론트엔드" / "Front End" terminology consistently (job title, intro, experience entries) — recruiters and ATS search by these keywords, so sprinkle "Front End" across the resume where possible. If the official title was "Software Engineer" but the work was mostly front end, retitle as "Front End Engineer" / "Front End Software Engineer" / "Software Engineer (Front End)" (Korean: "프론트엔드 개발자") — only where truthful
- Mirror the language of the target job description in Skills, and repeat the same keywords naturally in Experience bullets. Some ATS weight a skill by frequency and by which section it appears in — but never keyword-stuff; a human reads it next
- Spell out abbreviations at least once (e.g., "Amazon Web Services (AWS)")
- To generalize across a role type: collect 3–5 job descriptions, run a word-frequency pass, and make sure the recurring keywords with real experience behind them appear in the resume
- Contact info: name at the very top; personal email; GitHub and personal site are good-to-haves. TIH also lists phone, city, and LinkedIn — **Repo decision**: this is a public web page, so phone/address are omitted; add them to the PDF export for a specific application if required

### Format & Delivery (guide rules that apply when exporting the 이력서 — the compact view — to PDF)

- Front-end engineers are expected to have a keen sense of design, so a resume that is not visually neat reflects poorly on the candidate. It does not need to be fancy — just neat and visually pleasing
- Single column, common fonts (Arial, Calibri, Garamond, or the OS-installed Korean equivalent), 1–2 fonts total (at most one for headings and one for body), minimum ~11px/10pt body text
- The guide says to author in Word / Google Docs / Pages / LaTeX so ATS can parse the file, and to avoid design software (Figma, Canva, Photoshop). **Repo decision**: this semantic HTML page is the source of truth and browser print-to-PDF is the export path; that is only acceptable because the output is real text — never render text as images or rely on CSS-generated content to carry information
- Submit as PDF with **selectable text** — never a screenshot/image. When printing this page to PDF, run the "plain-text test": copy-paste the PDF into a .txt and check nothing is missing, garbled, or reordered
- Do not rely on headers/footers or decorative graphics to carry information (ATS may drop them)
- Under 5 years of experience: one page preferred, two maximum

### Length & Density

- The page-count rule applies to the **이력서 (compact) output**; the full 경력기술서 view has no page limit. Adding detail to the full view is fine, but every addition must decide its tier: does it belong in the top-level line (both views) or in a sub-bullet / `.career-only` element (full view only)?
- Less is more: a few strong, quantified achievements beat many average ones. Do not list everything just to show quantity
- The guide's emphasis is quantifying impact, not listing tasks — bullets that cannot show scale or outcome are candidates for rewriting with metrics first, trimming second
- Trim order when the compact view is over length: (0) demote to `.career-only` before deleting — the full view keeps it, (1) task-list bullets with no metric, (2) sub-bullets that restate the parent, (3) duplicated content between 경력사항 and 프로젝트

### Review Checklist

Run this before calling a content edit done:

- [ ] Every achievement bullet has a number (scale, %, time, count) or an explicit reason it cannot
- [ ] Every bullet starts with an action verb, active voice
- [ ] No bullet is duplicated across 경력사항 and 프로젝트
- [ ] Skills: Languages + Technologies lines present, ≤ 2 per category, no proficiency bars, everything defensible in interview
- [ ] Every project has a live URL and/or GitHub link, and the link resolves
- [ ] "프론트엔드 / Front End" appears in title, intro, and each role
- [ ] 자기소개 starts with the role noun and is under ~50 words
- [ ] Default (compact) view reads as a complete resume on its own: every 경력 top-level line has a number and names the stack, 3–5 lines per company
- [ ] Default (compact) view fits one page (max two) when printed to PDF; PDF text is selectable. The `?full` view has no page limit

## Editing Guidelines

When making content updates to the resume:

- **Section Wrapper Classes**:
  - Most sections use `<section class="resume-item experience">` with `.inner` div
  - Skills section is the ONLY exception: `<section class="resume-item skill">`
- **Date Formatting**: Use format `YYYY.MM - YYYY.MM` or `YYYY.MM - 진행중` for ongoing projects/jobs
- **Ongoing Projects**: Use `진행중` for projects still in progress (not completed)
- **Employment Duration**: When showing employment duration like "재직중(X년 Y개월)", calculate from start date to current date
- **Chronological Order**: Projects and side projects should be ordered by date (most recent first)
- **Project Structure Pattern**:
  - Title: `<h3>Project Name</h3>`
  - URL: `<a class="company" href="URL" target="_blank">URL</a>`
  - Period: `<span class="date">YYYY.MM - YYYY.MM</span>`
  - Details: `<ul class="detail"><li>Main point<ul><li>Sub-point</li></ul></li></ul>`
- **Experience Structure Pattern**:
  - Company: `<h3>Company Name</h3>`
  - Position: `<h4 class="company">Position • Team Name</h4>`
  - Period with ongoing status: `<span class="date">YYYY.MM - <span class="now-working">재직중</span></span>`
  - Details: `<ul class="detail"><li>Achievement</li></ul>`
- **Certification/Education Pattern**:
  - Title: `<h3>Certification/Degree Name</h3>`
  - Issuer: `<h4 class="company">Organization Name</h4>`
  - Date: `<span class="date">YYYY.MM</span>`
- **List Styling**: Use `<ul class="detail">` for main bullet points; nested `<ul>` automatically get circle bullets
- **Spacing Control**: Add `.add-margin-bottom` class to `<li>` elements when extra spacing needed between major items
- **Two-Mode Markup** (full 경력기술서 vs compact 이력서 — see Content Writing Guidelines → Two Outputs from One Source):
  - Nested `<ul>` inside `ul.detail` need no marker; compact mode hides every sub-bullet list globally
  - Put `class="career-only"` on any other element that should appear only in the full view: a project's 2nd and later top-level `<li>`, the `ul.detail` of a 대외활동 entry, a 경력 top-level `<li>` beyond the 5th. It combines with existing classes (`class="career-only add-margin-bottom"`)
  - Mode is selected by URL, not by editing markup: `?full` adds `html.full` (the full 경력기술서, on screen and in print); any other URL — GitHub Pages, a plain open, print — is the compact 이력서. The class is set by a 5-line inline `<script>` in `<head>` (on `documentElement`, before body paints, so there is no flash); the hiding rules live in `assets/css/style.css` under "two modes from one source". No build step — keep it that way
  - The footer carries the toggle: `.mode-link.only-compact` (→ `?full`) and `.mode-link.only-full` (→ `index.html`); both are hidden in print
  - Never fork content per mode. If a top-level line only reads well with its sub-bullets underneath, rewrite the line; do not add a compact-only duplicate
- **Skills List** (replaces the retired skill-bar markup): one `<li>` per category, `<h3>` for the category label and `<p class="skill-list">` for the comma-separated items — `<li><h3>Languages</h3><p class="skill-list">HTML, CSS, JavaScript, TypeScript</p></li>`. Never reintroduce `<div class="skill-bar">` / `style="width: XX%"`
- **HTML Validation Rules**:
  - Never nest heading tags (e.g., `<h3>` inside `<h3>`)
  - All URLs should have `target="_blank"` attribute
  - Always wrap contact list URLs in `<a>` tags with appropriate Font Awesome icon in `<span><i class="fa fa-*"></i></span>`
- **Date Logic Verification**:
  - Employment duration calculations must be accurate from start date to current date
  - Education periods should be realistic (typical 4-year university: ~4 years, with 복수전공 possibly longer)
  - Certification dates must not be in the future
- **Footer Updates**: When a new year begins, update footer copyright year: `© YYYY`

