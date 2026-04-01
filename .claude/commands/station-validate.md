---
description: Validate a station or cheat-sheet page against all HAP quality checks
allowed-tools: Read, Glob, Grep
---

Validate the file at `$ARGUMENTS` against all HAP Learning Lab quality standards. Read the file first, then run every check below.

**Output rules — be terse:**

- PASS categories: print only the heading line (e.g., `## Voice: PASS`). No evidence, no bullet points.
- WARN/FAIL categories: print the heading line plus ONLY the specific findings with file:line references. No "everything else looked fine" filler.
- The actionable items summary at the end is the most important part.

## 1. Voice validation

Read `skills/hap-voice/SKILL.md` for the full ruleset, then check:

- All narrative text uses HAP's first-person apprentice voice
- Zero forbidden patterns: "you should", "obviously", "it's simple", "it's easy", dismissive "just", "simply"
- Intro content ends with 🟠 emoji
- Prof. Teeters appearances are earned (not gratuitous)
- Grace Hopper (if present) uses no contractions and no emojis
- No instructional tone ("follow these steps", "do this") — HAP shares what he learned, not what students should do

## 2. Station content structure

Read `skills/station-content/SKILL.md` for the full ruleset, then check:

- StationLayout (or MainLayout for cheat sheets) has all required props
- `stationNumber` matches the filename
- `description` is 150-160 characters
- "What You'll Learn" section has exactly 3 insight cards (stations only)
- HAP's Confession is present with specific mistakes (stations only)
- Quick Reference section is present with 4-6 tip cards (stations only)
- Heading hierarchy: one h1 (from layout), h2 for sections, h3 for subsections, no skipped levels
- For Station 6: verify Section 1 (What You'll Learn) and Learning Objectives Checklist bookends are present
- For cheat sheets: verify back-link to parent station is present
- All variables defined in frontmatter are used in the template body (flag unused/dead code)

## 3. CSS standards

Read `skills/css-standards/SKILL.md` for the full ruleset, then check:

- Zero hex colors (#fff, #E8A557, etc.)
- Zero rgb/rgba colors
- All colors use hsl() format or CSS custom properties
- No inline style attributes with hardcoded colors

## 4. Accessibility

Read `skills/accessibility-check/SKILL.md` for the full ruleset, then check:

- All images have `alt`, `width`, `height` attributes
- Below-fold images have `loading="lazy"` and `decoding="async"`
- Header avatar image has `fetchpriority="high"` (handled by layout, but verify if overridden)
- No empty headings
- No skipped heading levels
- All external links have `target="_blank" rel="noopener noreferrer"`

## 5. Image validation

Read `skills/hap-image-validation/hap-cloudinary-complete-inventory.md` for the verified inventory, then check:

- Every Cloudinary image filename in the file exists in the inventory
- Version numbers match the inventory
- Alt text is descriptive (not just "HAP" or "image")
- No guessed or hallucinated filenames

## 6. Security

Read `skills/security-audit/SKILL.md` for the full ruleset, then check:

- Zero `eval()`, `document.write()`, or `new Function()` calls
- Zero `innerHTML` assignments (educational references in code examples are OK if not executed)
- `setTimeout`/`setInterval` use function callbacks, not string arguments

## 7. Teaching content boundary

Read `.claude/teaching-content-boundary.md` for the full boundary rules, then check:

- Code examples shown to students use plain HTML, CSS, JavaScript — no Astro syntax
- No Astro frontmatter fences, component imports, `set:html`, or expression slots in student-facing code blocks
- If teaching npm/node concepts, examples use `npm`/`node` not `bun`
- Student-facing file references use `.html`, `.css`, `.js` — not `.astro`

## 8. Code blocks

- All code uses `<CodeBlock>` component (not raw `<pre><code>`)
- `lang` attribute matches the actual content (CSS code uses `lang="css"`, not `lang="javascript"`)
- Code is wrapped in template literals (backticks)

## Output format

```
# Station validation: [filename]

## Voice: PASS
## Structure: FAIL
- :62-69 — preventDefaultCode defined but never used in template body
## CSS: PASS
## Accessibility: PASS
## Images: PASS
## Security: PASS
## Teaching boundary: PASS
## Code blocks: FAIL
- :196 — lang="javascript" on CSS content, change to lang="css"

---
Summary: 6 pass, 0 warn, 2 fail
Actionable:
- FAIL [filename]:62-69 — remove unused preventDefaultCode variable
- FAIL [filename]:196 — change lang="javascript" to lang="css"
```
