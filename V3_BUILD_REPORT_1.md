# Decimal Tech V3 — Build Report
_21 September 2026_

## 0. Internal audit (V2 → V3)

**Source audited:** live site decimaltech.ai (+ /contact), the local `Decimal Tech` folder (logos, company profile, Website V2 PRD). **The V2 source code was not in the connected folders**, so V3 is a fresh Next.js codebase. The live site is Next.js (`/_next/image`), so the stack and deployment stay the same.

| KEEP | REWRITE | REMOVE | VERIFY |
|---|---|---|---|
| Logo (blue circle + red "point") → it's now the brand mark and the single accent | "AI-first engineering startup" → engineering-teams company, AI as leverage | 3x velocity, 40% lower cost, 48-hour discovery/onboarding, 4–6 week MVP, "Traditional vs DecimalTech" table | Melbourne office (from company notes; no public address) |
| US · Nepal · Australia; US + Nepal addresses, phone, email, WhatsApp | Services (MVP / Staff Aug / Squads / Modernization) → Embedded / Pods / Dedicated / Technical Leadership | "18+ years combined experience" (not attributable per person) | SAMARTH MIS wording (Ministry of Industry + UKaid) |
| `/contact#book-strategy-call` 30-minute call path | Memories Labs "Staff Augmentation" card → the main case study | AI Pipeline Engine as the lead product; GitHub Copilot/Claude branding in the pipeline | Nepal Immigration / Visa scope wording |
| NDA / "reply personally" tone | FAQ → spread across How It Works and the contact page | Nepal Nomad Co., 16 Sanskars, PropTech, "AI Workflow Automation" cards (internal/NDA work, off-message) | A Cloud Guru title for Subash (supplied by Decimal; no public source found) |
| Legal pages (privacy/terms/cookies) | Footer copy | "Get My AI-Powered Proposal" funnel (→ 308 to /contact) | Regendra's Motorsport.tv / Noodle / BidBayt / Baron roles (supplied by Decimal) |

## 1. Existing codebase audited
Only partly: the live site was audited, but the source code was not available. **Action:** bring over anything the V2 repo had that the public site doesn't show (analytics IDs, the form handler, blog/CMS content, legal text, the Careers and Employee Portal routes).

## 2. Mobbin research
**Blocked.** The Mobbin connector returned "requires a paid plan". Instead I used well-known interaction patterns (Linear, Vercel, Stripe, Resend): a sticky case-study rail, editorial founder list, ruled grids, a mono label system and a horizontal process that stacks vertically on mobile. Nothing was copied.

## 3. Design system (`app/globals.css`)
- **Colour:** paper `#f4f3ef` and ink `#0a0b0d`. The single accent is the logo's red point `#ee3b24`. Logo blue appears only in the mark. Night sections are used for rhythm: the case study, the AI layer, the CTA and the footer.
- **Type:** Geist Sans for large uppercase display type with tight tracking. Geist Mono for labels, indexes and metadata.
- **Grid:** 1320px max width and fluid gutters (16–40px). Thin 1px rules replace cards wherever possible, and radii stay at 6/10px.
- **Motion:** CSS only. A hero line reveal, packets flowing through the system diagram, a scroll reveal via IntersectionObserver, and the case-study progress rail. `prefers-reduced-motion` switches all of it off.

## 4. Pages / routes
`/` · `/teams` · `/work` · `/engineering` · `/about` · `/insights` · `/contact` · `/privacy` · `/terms` · `/cookies` · `/api/contact` · `/sitemap.xml` · `/robots.txt` · `/opengraph-image` · `/icon.svg` · custom 404

**Redirects (308):** `/services` → `/teams`, `/services/hire` → `/teams#embedded-engineer`, `/services/squads` → `/teams#dedicated-team`, `/services/mvp` → `/teams#engineering-pod`, `/services/modernization` → `/engineering`, `/case-studies(/*)` → `/work`, `/get-proposal` → `/contact`.

## 5. Components created
Nav (with mobile menu) · Footer · Logo/Brand · ButtonLink/BookCall · Label · PageHero · Portrait · Reveal · LegalPage.

Sections: Hero (system-stack visual) · Problem · EngineeringTeams · TeamModel · MemoriesCase (sticky rail + 5 stages) · Engineers · AILayer + AgentAvatar (5 illustrated SVG agents) · Experience · **TechStack** (the stack you sent mid-build) · Streaming · Portfolio (current vs. previous) · WhyDecimal (+ Not a Marketplace) · EngagementModels · HowItWorks · Founders (editorial layout + accordion) · GlobalPresence (live local clocks) · FinalCTA · ContactForm.

## 6. Components reused
None from V2, because the code wasn't available. Every section is reused across pages, e.g. `EngagementModels` appears on both `/` and `/teams`.

## 7. Existing links preserved
- `/contact#book-strategy-call`: same anchor id, on the contact page aside.
- `/contact`, `/privacy`, `/terms`, `/cookies`.
- `info@decimaltech.ai`, +1 (720) 576-9024, WhatsApp +977 9851128817.
- Legacy service and case-study URLs via 308 redirects.
- **Not preserved:** `/blog/*`, `/careers` and the Employee Portal. These need migrating from the V2 repo.

## 8. Calendly / discovery URL
**No external scheduler was found.** The live V2 "Book Strategy Call" points to `/contact#book-strategy-call`, a section that funnels into the contact form. V3 keeps that destination. If a real Calendly/Cal.com link exists in the V2 code or env, set `NEXT_PUBLIC_BOOKING_URL` and every "Book a 30-Minute Call" button will switch to it automatically. No URL was invented.

## 9. Forms
The V2 fields (name, email, company, message) are expanded to the V3 spec: name, work email, company, what are you building, capacity needed, team size, stack, context, and an "I'd like a 30-minute call" checkbox. Other details:
- Client-side validation with focus management, a honeypot and an `aria-live` status message.
- Server route validates input and delivers through **Resend** or a **webhook** (Slack/n8n/CRM).
- **Until one is configured it returns 503.** The V2 form endpoint wasn't visible publicly; port it if it differs.
- `?model=` preselects the engagement model from the model cards.

## 10. SEO
- Title, description and H1 exactly as specified.
- Per-page titles and descriptions built around the commercial topics (staff augmentation, dedicated teams, embedded engineers, AI-native engineering, streaming/SaaS/AWS).
- Canonicals, OpenGraph and Twitter tags, plus a generated OG image.
- Organization JSON-LD with both addresses, sitemap and robots.
- One H1 per page and a clean H2/H3 hierarchy.

## 11. Accessibility
- Semantic landmarks, a skip link and a visible focus ring (accent colour).
- Keyboard-operable mobile menu (Esc closes it and returns focus) and founder accordion (`aria-expanded`, `inert` when collapsed).
- Labelled form fields with `aria-invalid`/`aria-describedby` errors, 48px targets and reduced motion.
- **axe-core** was run on all 7 main pages at 1440px and 390px. The only remaining flags are decorative `aria-hidden` text: the giant step numerals and the footer wordmark.

## 12. Performance
- Every page is statically prerendered; only the contact API runs on the server.
- No UI, animation or icon libraries. SVG is inline, fonts are self-hosted with `next/font`, and `next/image` (AVIF/WebP) handles portraits.
- Homepage: about 37 KB CSS, about 138 KB fonts, and JS that is mostly the React/Next runtime. First contentful paint was about 150 ms on a local production build.
- No third-party scripts yet. Add analytics deliberately.

## 13. Mobile
Checked at 390px with no horizontal scroll on any page.
- Compact nav: logo, Build Your Team, Menu.
- A full-screen typographic menu.
- The hero diagram stacks under the copy.
- The team-model timeline turns vertical.
- The case-study rail collapses and its stages stack.
- The engineers become a swipeable snap row with a peek of the next card.
- Agents switch to an avatar-left layout.
- Grids become single ruled lists.

## 14. Real assets still required
1. **Team photographs** for Subash, Anup, Gaurab, Regendra, Dipesh, Prabesh and Girban. Put them in `public/images/team/` and map them in `lib/photos.ts`. Placeholders are clearly labelled "Photo pending"; no faces were generated.
2. Memories product screenshots or UI recordings, with Memories Labs' permission.
3. The logo as SVG (it's currently a vector recreation of the PNG).
4. Legal text for privacy, terms and cookies from V2.
5. Blog posts for `/insights/[slug]`.
6. Analytics ID.
7. Resend key or webhook.
8. A booking URL, if one exists.

## 15. Claims requiring verification
- **Memories Labs:** the quoted line is from memorieslabs.com ("…helping millions connect with their past, present, and future"). It's attributed to them, not claimed by Decimal. Confirm Memories Labs is OK being named and quoted.
- **Subash:** CTO of Memories Labs, and Lead Software Engineer / Lead Developer at A Cloud Guru (supplied; no public source checked). The line "A Cloud Guru later became part of Pluralsight" is public record.
- **Motorsport.tv:** described as having "operated" as a global OTT platform, because Wikipedia records it closing on 31 Jan 2026. Regendra's involvement in the app and website was supplied by you.
- **Noodle Rex:** described as a Nepal-built music platform (public). The role is supplied.
- **BidBayt, Baron Boutique, SAMARTH MIS, Nepal Immigration / Visa:** the descriptions are yours. SAMARTH's link to the Ministry of Industry and UKaid couldn't be publicly confirmed.
- **Anup:** US-based. **Gaurab:** Microsoft-related products and government cybersecurity in Nepal. Both supplied.
- **Melbourne office:** no public address shown.
- **Tech stack:** shown only as company-level capability and never attached to a specific project.

## 16. How to run locally
`npm install && cp .env.example .env.local && npm run dev`

## 17. How to build
`npm run build && npm start`. Node 20+ is required.

## 18. How to deploy
Deploy to Vercel with the Next.js preset and set the env vars from `.env.example`. Before production:
- Keep the domain on the existing project so the redirects take over V2 URLs.
- Migrate the blog, then add `/blog/:slug` → `/insights/:slug` redirects.

---
## Update — 21 Sep 2026 (v3.1)
- **Portfolio:** individual names are gone from the portfolio and streaming cards. Projects are now presented as "Previous projects from the founders" and tagged "Founder project". Regendra's founder bio no longer lists them.
- **Light + dark theme:**
  - Follows the visitor's system setting by default, with a toggle in the nav (inside the menu on mobile).
  - The choice is remembered, and the page doesn't flash the wrong theme on load.
  - Feature blocks stay dark in both themes. An automated accessibility check (axe, WCAG 2 A/AA) finds no issues in either theme.
- **Logo:** the nav, footer, favicon and Apple touch icon now come from `Logos/Logo-Decimal-Tech.png`. It's cut out onto a transparent background, with a light-text wordmark for dark backgrounds, in `public/brand/`.
- **First Insights article:** `/insights/engineering-staff-augmentation-vs-outsourcing`
  - **SEO:** a targeted title and description, a canonical URL, headings that follow the reader's questions, a comparison table and internal links.
  - **AEO:** a short-answer box, a key-takeaways list, questions as headings, an FAQ, and structured data (JSON-LD) marking it up as an article, an FAQ and a breadcrumb trail.
  - **GEO:** clear statements about who Decimal Tech is, a glossary, an "About Decimal Tech" block, plus `/llms.txt` and `/insights/rss.xml`.
  - It contains no statistics or client claims.

---
## Update — 30 Sep 2026 (v3.2) — hero rework
Only the hero changed. The rest of the site, its design system, navigation and components are untouched.

- **Message order:** eyebrow → headline → subhead → Engineer/Team/Pod selector → CTAs → trust line, on every screen size. The copy is exactly as supplied.
- **Engineer → Team → Pod:** one segmented selector (a proper tablist: arrow keys, Home/End, roving focus, visible focus ring), not three cards. Engineer is the default and the only filled segment. Selecting a step swaps the tagline and the supporting line; the panel beside it keeps a fixed height so nothing jumps.
- **Hero visual:** the old four-layer system diagram is replaced by a "your product team" panel — your existing team, then Decimal capacity that grows with the selector (1 engineer → 3 → a 4-person pod), with unused seats shown as quiet "capacity to add" rows. No robots, no fake dashboards, no stock imagery.
- **CTAs:** "Hire a Top-Tier Engineer →" is the solid primary and points at the preserved discovery destination (`/contact#book-strategy-call`, or `NEXT_PUBLIC_BOOKING_URL` when set). "Build Your Team →" is a quiet text button to `/contact`.
- **Typography:** the headline keeps the site's uppercase display style but drops to weight 550 with a 4.4rem cap, so it dominates through scale and whitespace rather than weight. The subhead is capped at 640px.
- **Micro-proof:** "From one engineer to an entire engineering team — without rebuilding your hiring process." sits on the hero's bottom rule beside the regions line.
- **Checked:** 1440, 1280, 1024, 768, 390 and 375. No horizontal scrolling anywhere; the primary CTA and trust line sit inside the first viewport at every size. axe (WCAG 2 A/AA + best practice) is clean on the hero in both themes. Animation is limited to a short fade on state change and respects reduced-motion.
- **Note:** the "Impeccable" skill is not available in this session; the critique pass used the available design-critique framework plus an automated accessibility and responsive check.

---
## Update — 1 Oct 2026 (v3.3) — commercial hierarchy + Our Story
- **Engineer → Team → Pod everywhere.** The engagement models are reordered and renamed: **One Engineer** (now the featured card, CTA "Hire a Top-Tier Engineer"), **Team** ("Build Your Team"), **Pod** ("Build a Pod"). The four "Engineering teams" cards follow the same order. Old anchors still resolve (`/services/mvp` → `#engineering-team`, `/services/squads` → `#engineering-pod`).
- **Primary CTA is now "Hire a Top-Tier Engineer"** in the nav (shortened to "Hire an Engineer" below 1100px), on /teams, and in the closing CTA, with "Build Your Team" as the secondary.
- **Positioning lines added:** "You don't get a CV. You get an engineer." is the big statement in the Engineering Teams section; "AI makes them faster. Experience makes them useful." opens the AI layer section.
- **New: Our Story** (`/about#our-story`) — headline, lead, a six-step timeline (20 years ago → today), the belief as a pull quote, and the closing paragraphs on hiring. The homepage Founders section links to it; the hero and the commercial model are untouched.
- **New: The standard** (`/about#our-standard`) — "Would we put this engineer on our own team?", the five hiring principles, "We don't hire for volume. We hire for the standard.", closing with the Hire a Top-Tier Engineer CTA.
- **Anup's bio** rewritten to the supplied text, with no agency named.
- **Checked:** axe (WCAG 2 A/AA) clean on /, /teams and /about in both themes at 1440 and 390; no horizontal scrolling.
- **Note:** there is still no "Impeccable" skill in this session, and the Mobbin connector again returned "requires a paid plan", so the refinement pass used the available design-critique framework plus automated accessibility and responsive checks.

---
## Update — 1 Oct 2026 (v3.4) — Impeccable craft pass
Same visual world, restrained. No copy, route, structure or Calendly change.
- **Kickers and section numbers removed** sitewide (the briefed hero eyebrow stays). Headings now open their own sections, which lifted the whole page rhythm.
- **Engagement models are no longer three equal cards:** One Engineer is a wide dark lead card with larger type; Team and Pod sit beside it. The capacity glyphs now actually count.
- **Mono diet:** Geist Mono is reserved for data, metadata and system lines. Chips, "best for" labels, role meta and the micro-proof line moved to sans.
- **Motion diet:** one authored moment (hero) plus state transitions. The identical fade-up on every section is gone.
- **No coloured side-tabs:** the answer box, the belief quote, the standard panel and the case-study quote use a 1px frame, a hanging quote mark or a top rule instead.
- **Browser surfaces themed:** selection (both themes), caret, scrollbars, underline offset, tabular numerals for times and counters.
- **No layout-property animation** left (the case-study rail animates transform).
- **Checks:** Impeccable's detector reports 0 findings. axe (WCAG 2 A/AA) is clean on all 8 pages in both themes at 1440 and 390, with no horizontal scrolling.
- `DESIGN.md` now documents the shipped system (tokens, rules, components, content rules).

---
## Update — 6 Oct 2026 (v4.0) — positioning rewrite, no client names
The site now sells one thing: a pre-vetted engineer who joins your team. Same identity, same hero, same Engineer → Team → Pod model.

**Removed** (per brief — no client names, no large case studies, nothing that makes Decimal look bigger than it is):
- The Memories Labs case study, the streaming cards, the founder portfolio list and the five AI-agent grid.
- The `/work` page. `/work` and `/case-studies/*` now redirect to `/engineering`; "Work" is out of the nav.
- The client name in Subash's bio (his role there is described without naming the company).

**New / rewritten sections**
- **Problem** — "You don't need more CVs. You need the right engineer.", the sourcing → onboarding chain drawn as one line, closing on "We do the filtering *before* you meet the engineer."
- **What you get** — pre-vetted, team-ready, experienced, AI-native, engineering judgment, each with a drawn mark; closes on "You don't get a pile of CVs. You get an engineer worth meeting."
- **Process** — the five-step path as the page's strongest diagram: a shrinking field of candidates down the left, four steps marked as ours and one, "You meet the engineer", marked "Your call".
- **Philosophy** — Experience + AI tools + Ownership = Better engineering, drawn as an actual equation on the dark section. Decimal is not presented as an AI company.
- **Where our engineers have worked** — five engineering environments with technical glyphs, replacing the project portfolio.
- **Proof** — "The model is already working." plus the honest paragraph and "Real engineers. Real teams. Real work." No names, no numbers.
- **About strip** — "We're young. Our experience isn't." leading to /about, ending on "We're ready for the work."
- **Final CTA** — "Have the work. Need the engineer?" with the same two buttons and "We're ready for the work."

**Copy** rewritten in a direct, founder-to-CTO voice throughout. No jargon, no claims we can't support.

**Checks:** Impeccable's detector reports 0 findings. axe (WCAG 2 A/AA) is clean on all 7 pages at 1440, 1280, 768 and 390, in both themes, with no horizontal scrolling.

---
## Update — 6 Oct 2026 (v4.1) — Mobbin-informed process diagram
Mobbin is on a paid plan now, so the research pass that was blocked earlier actually ran. Patterns studied: Cursor, Customer.io, Mistral AI, Speakeasy, Humble and Glide (hero composition); Trawelt, Kastle, Adaline, Beside and Wild (process sections); Teak, Fluz, Railway, Stripe and Fiverr (value grids). Nothing was copied — the useful principle was Wild's: one wide drawn explanation above the step labels, rather than a small ornament beside each row.

**Changed:** the Process section is now the signature visual. A full-width field of candidate marks narrows across five stages — 18, 11, 6, 2, then one in accent — with each cluster sitting above its own labelled column, and "You meet the engineer" still marked "Your call". Below 960px it falls back to the stacked list.

The value grids were already close to the pattern the references use (hairline rules, modest marks, bold short titles), so they were left alone rather than churned.

**Checks:** detector 0 findings; axe (WCAG 2 A/AA) clean on all pages at 1440, 1024, 768 and 390 across both themes; no horizontal scrolling.
