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
- **기술스택 (Skills)**: Visual skill bars with percentage-based widths - ONLY section using `resume-item skill` class
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
- **Print Support**: Includes print-specific CSS media queries with `-webkit-print-color-adjust: exact` for skill bars

## Content Writing Guidelines

Based on the [GreatFrontEnd Front End Resume Guide](https://www.greatfrontend.com/front-end-interview-playbook/resume). Apply these when writing or reviewing resume **content** (wording), as opposed to the markup rules in Editing Guidelines below.

### Bullet Point Formula

Every achievement bullet should follow: **Action verb + specific work + measurable outcome + technology**

- Bad: "웹사이트 개발 (HTML, CSS, JavaScript 사용)" — generic, no scale or impact
- Good (existing example in this resume): "React.cache() 적용 및 Promise.all 병렬화로 서버 중복 요청 90% 감소"
- Quantify scale, complexity, and impact wherever possible. A bullet without a number should prompt: "can this be measured?"
- Repo convention (not from the guide): lead bullets with a strong action verb — 구축, 설계, 개선, 자동화, 최적화, 도입, 마이그레이션

### Front-End Metrics Worth Quantifying

- **Performance**: page load time, bundle size reduction, Lighthouse score
- **Testing**: coverage percentage (e.g., "커버리지 90%+"), number of critical flows covered
- **SEO / a11y**: errors/warnings fixed, WCAG compliance level achieved
- **Tooling / migration**: framework version migrations (e.g., "Next.js 14→15"), scale context (LoC, team size)
- **Business / product**: users (MAU), time saved (e.g., "월 4시간 이상 반복 작업 자동화"), feature scope

### Section Priority

- 경력사항 (Work Experience) is the most important section — it should carry the most detail
- Projects and side projects signal continuous learning; technologies NOT used at the current job are a positive signal, so name them explicitly
- Every project should link to a live URL and/or GitHub repo (repos should have a polished README with screenshots)

### Skills Section (기술스택)

- Do NOT list every technology touched — it dilutes the skills that matter. Cap at 1-2 entries per category (framework, meta-framework, CSS, testing, state management, data fetching)
- Prefer architecturally significant technologies (React, Next.js) over utility libraries (Lodash)
- Keep HTML, CSS, JavaScript as the baseline

### Keyword Strategy

- Use "프론트엔드" / "Front End" terminology consistently (job title, intro, experience entries) — recruiters and ATS search by these keywords
- If a skill appears in a target job listing and there is real experience with it, add it to the skills section

### Length & Density

- With under 5 years of experience, keep content equivalent to one page, not exceeding two
- The guide's emphasis is quantifying impact, not listing tasks — bullets that cannot show scale or outcome are candidates for rewriting with metrics first, trimming second

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
- **Skill Bars**: Set percentage width inline: `<span class="bar" style="width: XX%"></span>`
- **HTML Validation Rules**:
  - Never nest heading tags (e.g., `<h3>` inside `<h3>`)
  - All URLs should have `target="_blank"` attribute
  - Always wrap contact list URLs in `<a>` tags with appropriate Font Awesome icon in `<span><i class="fa fa-*"></i></span>`
- **Date Logic Verification**:
  - Employment duration calculations must be accurate from start date to current date
  - Education periods should be realistic (typical 4-year university: ~4 years, with 복수전공 possibly longer)
  - Certification dates must not be in the future
- **Footer Updates**: When a new year begins, update footer copyright year: `© YYYY`

