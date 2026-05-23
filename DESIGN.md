# Design

> Visual system for inkz.io v1. Forensic Document aesthetic. Reads as a federal case file, not a website. See [PRODUCT.md](./PRODUCT.md) for register, voice, and anti-references.

## Visual theme

**Forensic Document.** The site is a single-page scroll formatted like an audit dossier or federal case file. Mono captions act as file markers. Body text reads like legal-grade typography. Reveals use redaction-block clip-path animations. Page corners carry case-file numbering.

Visual identity is defined by what is *absent* as much as what is present:
- No card shadows
- No decorative gradients
- No icon system (mono captions handle labeling)
- No background images
- No stock photography

## Color palette

OKLCH only. Every neutral tinted toward the brand hue (cool-cast greys, warm-cast whites). Never `#000`. Never `#fff`.

| Token        | Value                       | Use                                           |
|--------------|-----------------------------|-----------------------------------------------|
| `--ink`      | `oklch(0.18 0.01 270)`      | Primary text. Near-black, slight cool cast.   |
| `--bone`     | `oklch(0.96 0.005 80)`      | Page background. Warm-tinted off-white.       |
| `--graphite` | `oklch(0.4 0.005 270)`      | Secondary text, labels.                       |
| `--graphite-soft` | `oklch(0.55 0.005 270)`| Tertiary text, mono captions.                 |
| `--forensic` | `oklch(0.55 0.18 25)`       | The accent. Used ONLY for redactions, file-stamp ink, and the wedge line. |
| `--hairline` | `oklch(0.85 0.005 270)`     | Dividers and 1px borders.                     |

**Color strategy: restrained.** Tinted neutrals + one saturated accent below 10% surface coverage. The forensic-red is sacred — it never appears on hover states, buttons, or anything decorative. Its presence carries meaning.

## Typography

| Role         | Family             | Notes                                                   |
|--------------|--------------------|---------------------------------------------------------|
| Display + body | **Söhne** (Test license for dev; production license required before launch) | Flat, confident, document-authoritative. Weights: 400, 600, 700. |
| Mono         | **IBM Plex Mono**  | Evidence tags, file headers, page numbers, timestamps. Weights: 400, 500. |

**No serifs in v1.** Serifs are the AI-design reflex for premium consulting.

Type scale (perfect-fourth ratio, 1.333):

| Token        | Value      | Use                          |
|--------------|------------|------------------------------|
| `--type-100` | 0.75rem    | Mono captions, page markers  |
| `--type-200` | 0.875rem   | Small text                   |
| `--type-300` | 1rem       | Body                         |
| `--type-400` | 1.25rem    | Lede paragraph               |
| `--type-500` | 1.667rem   | Section h3                   |
| `--type-600` | 2.222rem   | Section h2                   |
| `--type-700` | 2.963rem   | Page h1                      |
| `--type-800` | 3.951rem   | Hero display                 |
| `--type-900` | 5.268rem   | The wedge line               |

Line lengths capped at ~65–75ch on body text. Tight tracking on display (-0.02em letter-spacing). Default leading 1.5 for body, 1.1 for display.

## Layout

- Single column body, ~720px max width
- Selective breakout grids for evidence-style sections (Methodology, About Exhibits)
- Page-corner markers: `CASE FILE / INKZ-001`, page numbers, version stamps
- Hairlines as dividers (1px, `--hairline`)
- Real margins. Real top/bottom padding. The page breathes like a document, not a SaaS dashboard.

Spacing scale (rems): 0.25 / 0.5 / 0.75 / 1 / 1.5 / 2 / 3 / 4 / 6 / 8 / 12.

## Component patterns

| Pattern             | Treatment                                                        |
|---------------------|------------------------------------------------------------------|
| **File marker**     | Mono caption, uppercase, slight tracking. Top-left of section: `CASE FILE / INKZ-001`. |
| **Exhibit card**    | NOT a SaaS card. Manila-file pattern with a hairline tab on top. No shadow. |
| **Section divider** | Hairline, often labelled in mono with a section number          |
| **Redacted reveal** | Text appears under a solid forensic-red block, which lifts via clip-path on scroll |
| **Form intake**     | Document-styled. Mono headers, single column, intake-form feel. |
| **Button**          | Direct. Mono label, hairline border, no fill. Hover: forensic background, bone text. |
| **Scroll indicator** | Right-side vertical file-tab strip on desktop, hairline progress bar on mobile |

## Motion language

Library: **Motion** (open-source Framer Motion successor). Scoped to islands; not globally hydrated.

Three motion altitudes, applied per section:

| Altitude     | Sections                                | Vocabulary                                                              |
|--------------|-----------------------------------------|-------------------------------------------------------------------------|
| **Cinematic**    | Hero only                          | Title-card sequence. File-stamp scale-in, char-by-char type setting, redaction-lift on subtitle. 6–8s total. Skippable. |
| **Choreographed** | Methodology + About                  | Scroll-triggered. Redacted-text unredact via clip-path. File stamps with 200ms stagger. Each section animates once per page-load. |
| **Quiet**         | Wedge, Whisper, Contact, Footer    | Cursor-proximity reveals only. Fade-only transitions. Crisp button states.   |

Curves: `ease-out-expo` and `ease-out-quart` only. No bounce, no elastic.

Don't animate CSS layout properties. Transforms and opacity only.

`prefers-reduced-motion: reduce` collapses Cinematic and Choreographed to instant reveals.

**Performance budget: ≤60KB gzipped JS on home page.**

## Surface treatments

- **Backgrounds:** flat `--bone` everywhere. One exception: closing block on Contact section, inverted (bone text on ink).
- **Borders:** hairlines only (1px, `--hairline`). Never thicker. Never colored except for the forensic accent in redactions.
- **Shadows:** none.
- **Rounded corners:** none. Right-angle everything.
- **Glass / blur:** banned.

## Iconography

None in v1. Mono captions handle every label that would otherwise need an icon. If we ever add icons, they will be hand-drawn, single-stroke, set in the forensic accent and used sparingly.

## Hard bans

These are absolute. The impeccable skill enforces them on every component built.

- No gradient text (`background-clip: text` on a gradient)
- No glassmorphism (decorative blur/glass cards)
- No side-stripe borders (colored `border-left > 1px`)
- No hero-metric template (big number, small label, gradient accent)
- No identical card grids (same-sized cards with icon + heading + text repeated)
- No em dashes (—) in copy
- No cream + terracotta + serif aesthetic
- No SaaS card shadows
- No icons in v1
- No background gradients on hero sections
