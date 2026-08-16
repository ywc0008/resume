# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A static HTML5/CSS3 online resume (CV) website for a Korean front-end developer. This is a simple static website with no build process or dependencies beyond basic web technologies.

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
- **Footer**: Copyright notice with year and GitHub link (update year manually when needed)

## Important Technical Details

- **Fixed Width Layout**: 960px container with centered positioning
- **Header Layout**: Uses `display: flex` with `align-items: end` for contact container
- **Section Positioning**: Main content uses absolute positioning for section headers (right: -240px)
- **Korean Typography**: Noto Sans Korean font family specifically for Korean text rendering
- **Icon Integration**: Font Awesome icons positioned absolutely within contact list items
- **Print Support**: Includes a print-specific CSS media query block (`@media print`). Skill bar color rules there are legacy and can be removed with the `.skill-bar` styles

## Content Writing Guidelines

Sources:

- Primary: [GreatFrontEnd — Ultimate Guide to Front End Engineer Resumes](https://www.greatfrontend.com/front-end-interview-playbook/resume) (front-end-specific advice; the only resume page in that playbook)
- Secondary: [Tech Interview Handbook — Resume](https://www.techinterviewhandbook.org/resume/) (the general software-engineer guide the primary one links to for non-front-end-specific advice: ATS, section order, summary, contact info)

Apply these when writing or reviewing resume **content** (wording, what to include), as opposed to the markup rules in Editing Guidelines below. Where this repo deliberately deviates from a guide, the deviation is noted inline as "Repo decision".

### Bullet Point Formula

Every achievement bullet should follow: **Action verb + specific work + measurable outcome + technology** (TIH phrasing: `[Accomplishment summary]: [Action] that resulted in [quantifiable outcome]`).

- Bad: "웹사이트 개발 (HTML, CSS, JavaScript 사용)" — generic, no scale or impact
- Good (existing example in this resume): "React.cache() 적용 및 Promise.all 병렬화로 서버 중복 요청 90% 감소"
- Guide's model bullet: "Built a performant e-commerce website … The site has 20k MAU, loads under 2 seconds and a Lighthouse score of 98. Technologies used: Next.js, React, GraphQL, CSS modules" — note it names **scale, complexity, and impact** plus the stack
- Quantify scale, complexity, and impact wherever possible. A bullet without a number should prompt: "can this be measured?"
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

- 경력사항 (Work Experience) is the most important section — it should carry the most detail. Each entry needs company, title, team, and `MM/YYYY`-style dates
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

- Include at least 2 projects; each should state the technology stack explicitly
- Side projects signal passion and currency — technologies NOT used at the current job are a positive signal, so name them
- **Every project links to both the code (GitHub) and the live site where possible.** Repos should have a polished README with screenshots. If a live URL is a company dev/staging domain, verify it is publicly reachable — a dead link is worse than none
- Non-trivial open-source contributions or maintained OSS projects are worth their own bullet

### Skills Section (기술스택)

- **Format: category text lists, not proficiency bars.** Guide example: `Languages: HTML, CSS, JavaScript, TypeScript` / `Technologies: React, Next.js, Tailwind CSS, Jest, Storybook, Cypress, React Query, Yarn, webpack`. TIH format: `[Category]: [skills separated by "|"]`
- **Do NOT use skill bars, percentages, star ratings, or any self-assessed proficiency scale.** Neither guide uses them: the number is unverifiable, anything under ~70% reads as a weakness, and ATS cannot parse a bar. **Repo decision (2026-08-16)**: the original template's `.skill-bar` markup is retired; see Editing Guidelines for the replacement markup
- Keep at least one line each for Languages and Technologies. HTML, CSS, JavaScript must be in Languages ("otherwise there's something hugely wrong"); TypeScript belongs there too
- Do NOT list every technology touched — "two React-family frameworks beats a wall of React, Angular, Vue, Svelte, and Solid". Cap at 1–2 entries per category
- Guide's categories: JS library (React), metaframework (Next.js), state management (Zustand, Redux, Jotai), CSS (Tailwind CSS, CSS Modules, Styled Components), CSS framework (MUI, Chakra), unit testing (Vitest, Jest, Storybook), integration/E2E testing (Playwright, Cypress), data fetching (TanStack Query, SWR, GraphQL, tRPC), language tooling (TypeScript, ESLint), build tools & package managers (optional: Vite, pnpm)
- Selection criteria, in priority order: (1) appears in the target job listing, (2) architecture-influencing (React) rather than utility (Lodash), (3) widely known or rapidly gaining popularity, (4) demonstrates breadth — never two technologies serving the identical purpose (reads as a red flag)
- Only list what you can defend in an interview
- AI tooling (Claude Code) is a repo-specific addition not covered by the guides — keep it to a single "Tools" line. List only tools actually in daily use (Cursor dropped 2026-08). Protocols/specs like MCP are not skills — if there is real experience building MCP servers or similar, it belongs in an experience bullet with an outcome, not in the skills list

### Keyword & ATS Strategy

- Use "프론트엔드" / "Front End" terminology consistently (job title, intro, experience entries) — recruiters and ATS search by these keywords; retitle roles as "Frontend Developer" / "Front End Engineer" where truthful
- Mirror the language of the target job description in Skills, and repeat the same keywords naturally in Experience bullets. Some ATS weight a skill by frequency and by which section it appears in — but never keyword-stuff; a human reads it next
- Spell out abbreviations at least once (e.g., "Amazon Web Services (AWS)")
- To generalize across a role type: collect 3–5 job descriptions, run a word-frequency pass, and make sure the recurring keywords with real experience behind them appear in the resume
- Contact info: name at the very top; personal email; GitHub and personal site are good-to-haves. TIH also lists phone, city, and LinkedIn — **Repo decision**: this is a public web page, so phone/address are omitted; add them to the PDF export for a specific application if required

### Format & Delivery (guide rules that apply when exporting this HTML resume to PDF)

- Single column, common fonts (Arial, Calibri, Garamond, or the OS-installed Korean equivalent), 1–2 fonts total, minimum ~11px/10pt body text
- Submit as PDF with **selectable text** — never a screenshot/image. When printing this page to PDF, run the "plain-text test": copy-paste the PDF into a .txt and check nothing is missing, garbled, or reordered
- Do not rely on headers/footers or decorative graphics to carry information (ATS may drop them)
- Under 5 years of experience: one page preferred, two maximum

### Length & Density

- Less is more: a few strong, quantified achievements beat many average ones. Do not list everything just to show quantity
- The guide's emphasis is quantifying impact, not listing tasks — bullets that cannot show scale or outcome are candidates for rewriting with metrics first, trimming second
- Trim order when over length: (1) task-list bullets with no metric, (2) sub-bullets that restate the parent, (3) duplicated content between 경력사항 and 프로젝트

### Review Checklist

Run this before calling a content edit done:

- [ ] Every achievement bullet has a number (scale, %, time, count) or an explicit reason it cannot
- [ ] Every bullet starts with an action verb, active voice
- [ ] No bullet is duplicated across 경력사항 and 프로젝트
- [ ] Skills: Languages + Technologies lines present, ≤ 2 per category, no proficiency bars, everything defensible in interview
- [ ] Every project has a live URL and/or GitHub link, and the link resolves
- [ ] "프론트엔드 / Front End" appears in title, intro, and each role
- [ ] 자기소개 starts with the role noun and is under ~50 words
- [ ] Content fits one page (max two) when printed to PDF; PDF text is selectable

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

