# Secure Development Basics – Input slides

Three Reveal.js decks sharing one project (Markdown slides, speaker notes in keywords):

| Deck | File | Slides* |
|------|------|---------|
| Input 1 – OWASP Top 10 & Cheat Sheets | `01-owasp-top-10.html` + `slides/01_Top10.md` | 14 |
| Input 2 – Classifying Vulnerabilities (incl. 2 exercise slides) | `02-classification.html` + `slides/02_Classification.md` | 10 |
| Input 3 – AI and the Security Landscape | `03-ai-and-security.html` + `slides/03_AI.md` | 10 |

\*without title and thank-you slide. `index.html` is a landing page linking all three.

see it life on [Github Pages](https://dominikschopper.github.io/pres_websecurity-basics/)

## Local use

```bash
pnpm install
pnpm start        # serves the folder; open the printed URL
```

The Markdown plugin loads `slides/*.md` via HTTP, so opening the HTML files directly via `file://` will not work.
In a deck: `S` = speaker view with notes, `O` = overview, `F` = fullscreen.

## Theme
cofinpro theme
small extras (tables, left-aligned text) live in `css/deck.css`.

## GitHub Pages

`.github/workflows/pages.yml` installs reveal.js, assembles the site and deploys it.
In the repo: *Settings → Pages → Source: GitHub Actions*, then push to `main`.
Commit `pnpm-lock.yaml` (created by `pnpm install`) so builds are reproducible.

## Sources used for the facts on the slides

- OWASP Top 10:2025 – https://owasp.org/Top10/2025/ (A01 page: SSRF/CWE mapping)
- OWASP Top 10 for LLM Applications 2025 – https://genai.owasp.org/
- Veracode GenAI Code Security Report 2025 / 2026
- Spracklen et al., "We Have a Package for You!" (USENIX Security 2025) – https://arxiv.org/abs/2406.10279
- Shai-Hulud npm worm, Sept 2025 – Wiz, Flashpoint, Fluid Attacks write-ups
- CERT-EU blog on AI and vulnerability discovery; Microsoft Digital Defense Report 2026; Google M-Trends 2026 (as cited by CERT-EU); Forescout Vedere Labs
- International AI Safety Report 2026 (DARPA AIxCC results)
