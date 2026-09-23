# Growvert — Coach Funnel Website

A conversion-focused React website for Growvert, built for business, sales,
marketing, and health/fat-loss coaches. Includes the full homepage funnel:
Hero, Problem (4 leaks), Solution, Process, Proof, Pricing, a Lead Leak Score
quiz, a self-serve website audit tool, Why Choose Me, Try Before You Buy, FAQ,
and a final CTA — all pointing to one action: message Zainab on WhatsApp.

## Tech stack

- **React 19 + Vite 8** — fast dev server, instant HMR
- **Tailwind CSS v4** — utility styling, theme defined in `src/index.css`
- **Framer Motion** — scroll reveals, flip cards, accordion, quiz interactions
- **GSAP + ScrollTrigger** (`@gsap/react`) — the scroll-progress line in the Process section
- **Lenis** — smooth-scroll feel site-wide
- **react-helmet-async** — per-page SEO meta tags + JSON-LD
- **react-icons** — icon set (Lucide + WhatsApp glyph)

No Redux/state library — the only shared state is which coach segment is
selected (Hero pills), handled with plain React Context (`src/context/SegmentContext.jsx`).
That's the only global state the site needs, so a heavier state library would
have been unnecessary weight.

## Getting started

```bash
npm install
npm run dev       # local dev server
npm run build     # production build -> dist/
npm run preview   # preview the production build locally
```

## Project structure

```
src/
  components/
    layout/     Navbar, Footer, WhatsAppButton, SEO (Helmet + JSON-LD)
    sections/   One file per homepage section (Hero, Problem, Solution, ...)
    ui/         Reusable primitives (Reveal, FlipCard, Accordion, Icon, ...)
  context/      SegmentContext — selected coach type (business/sales/marketing/health)
  data/
    siteContent.js   <-- ALL copy, pricing, FAQ, quiz questions live here
  hooks/        useLenis (smooth scroll), useCountUp
  assets/images/  founder photo + OG banner (already compressed to .webp/.jpg)
public/
  robots.txt, sitemap.xml, favicon.svg, og-image.jpg
```

**To change any text on the site — headlines, pricing, FAQ answers, quiz
questions — edit `src/data/siteContent.js`.** You should not need to touch
component files for copy changes.

## Zaroori baatein (before you publish)

1. **WhatsApp number**: currently wired to `03314277790` in `siteContent.js`
   (`brand.whatsappNumber`). Change it there if it's ever different.
2. **Pricing**: `$697 / $1,497 / Custom` in `pricingTiers` are placeholders
   matched to your ~$600–$2,500 positioning. Edit freely.
3. **Case studies** (`caseStudies` in `siteContent.js`): placeholder client
   names/stats — swap in real names and numbers once a client has confirmed
   you can share them publicly.
4. **Lead capture forms** (Lead Leak Score email gate, Try-Before-You-Buy
   notify-me): these currently only show a success message on-screen —
   there's no backend yet. Look for the `// TODO` comments in
   `LeadLeakQuiz.jsx` and `TryBeforeYouBuy.jsx` and connect them to an email
   tool (ConvertKit, Mailchimp) or a Zapier/Make webhook to actually capture
   leads and deliver the PDF report.
5. **Domain**: `SEO.jsx` and `index.html` reference `https://growvertcoach.com`
   as a placeholder domain for canonical/OG URLs — update both once you pick
   the real domain.
6. **Founder photo / OG image**: sourced from your project files and already
   compressed (`src/assets/images/founder-photo.webp`, `public/og-image.jpg`).
   Replace them the same way (compress to webp/jpg first) if you swap photos.

## SEO notes

- Meta title/description, Open Graph, Twitter Card, and `ProfessionalService`
  JSON-LD are set in `src/components/layout/SEO.jsx` (via react-helmet-async)
  and mirrored as static tags in `index.html` so WhatsApp/LinkedIn/Facebook
  link previews work correctly even without JS (those crawlers don't execute
  JavaScript, unlike Googlebot).
- `public/robots.txt` and `public/sitemap.xml` are in place; update the
  sitemap if you add more pages later.
- This is a client-rendered (CSR) React app. Google's crawler does render JS
  and index CSR sites, but if you ever want stronger, faster indexing (or you
  add a blog/case-study pages), migrating to a pre-rendering/SSG setup (e.g.
  Vite SSG or Next.js) is the next natural step — not required to launch.

## Performance

- Images are pre-compressed (founder photo: 1.8MB → 24KB webp).
- Vendor code is split into cacheable chunks (`vendor-react`, `vendor-motion`,
  `vendor-gsap`) so repeat visits reuse cached JS.
- Respects `prefers-reduced-motion` (Lenis smooth-scroll and the GSAP scroll
  line both turn off for users who’ve requested reduced motion).

## Design system

- Colors: `--color-sage` (#ACAF79), `--color-cream` (#FBF9EF), `--color-ink`
  (near-black), all defined in `src/index.css` under `@theme`. Change them
  there and every component updates automatically.
- Fonts: **Bricolage Grotesque** for all headings (`h1–h4`), **Inter** for
  body text — loaded via Google Fonts in `index.html`.
