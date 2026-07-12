# Jaideep Phatak — Executive Virtual Business Card

A self-contained, dependency-free virtual business card, designed to be hosted
at `https://phatakjaideep.github.io/card/` alongside the main executive
portfolio at `https://phatakjaideep.github.io/`.

No frameworks, no build step, no server, no analytics, no tracking, no forms.
One HTML file, one vCard file, and a small set of image/QR assets.

---

## 1. File Structure

```
card/
├── index.html                  ← the card itself (HTML + CSS + minimal JS, all inline)
├── contact.vcf                 ← downloadable vCard (VCard 3.0)
├── README.md                   ← this file
└── assets/
    ├── jaideep-avatar.jpg      ← circular portrait crop (600×600, from the approved caricature)
    ├── og-card.jpg             ← social-preview image (1200×630)
    ├── favicon.svg             ← small "JP" monogram favicon
    └── qr/
        ├── card-qr.png         ← QR code (900×900 PNG) → https://phatakjaideep.github.io/card/
        └── card-qr.svg         ← QR code (vector, print-ready) → same URL
```

Everything the page needs lives inside this `card/` folder. It does not
depend on anything from the parent portfolio folder, so it can be uploaded,
tested, and updated independently.

---

## 2. What's Public vs. Private

| Field | Value used | Where it lives | Visible as page text? |
|---|---|---|---|
| Name, titles, location | Jaideep Phatak, Program & Portfolio Leadership, Pune, India | Visible page content | Yes — intentionally |
| LinkedIn URL | `https://www.linkedin.com/in/jaideepphatak/` | `href` of LinkedIn button + JSON-LD `sameAs` | No (button label only) |
| Portfolio URL | `https://phatakjaideep.github.io/` | `href` of Portfolio/Demonstrations buttons + JSON-LD `url` | No (button label only) |
| Card URL | `https://phatakjaideep.github.io/card/` | Canonical tag, OG/Twitter tags, QR code target, Share Card | Yes, in the QR caption only (a URL is not sensitive) |
| **Email address** | `jaideepphatak16@gmail.com` | `mailto:` href of the Email button, and inside `contact.vcf` | **No** (Mode B — button only, see §5) |
| **WhatsApp / mobile number** | `+91 72760 87697` | `href="https://wa.me/917276087697"` on two buttons, and inside `contact.vcf` | **Never**, anywhere (see §4) |

Nothing sensitive appears in Open Graph tags, Twitter Card tags, JSON-LD
structured data, image filenames, image alt text, HTML comments, or the
QR code payload (the QR code only ever points at the public card URL).

---

## 3. Placeholders to Replace (if reusing this template)

This deliverable has already been filled in with the real values Jaideep
supplied. If you ever rebuild this page from scratch as a template, replace
these values — each is marked in the source with a comment:

**Private configuration** (edit with care — see §4 and §5):
- WhatsApp number inside the two `href="https://wa.me/..."` links in `index.html`
- Email address inside the one `href="mailto:..."` link in `index.html`
- Mobile number and email inside `contact.vcf`

**Public values** (safe to change freely):
- `LINKEDIN_URL` — the LinkedIn button's `href`
- `PORTFOLIO_URL` — the "View Executive Portfolio" and "Explore Working
  Demonstrations" buttons' `href`
- `CARD_URL` — canonical tag, OG/Twitter meta, and the `CONFIG.CARD_URL`
  JavaScript constant used by Share Card
- `PROFILE_IMAGE_PATH` — `./assets/jaideep-avatar.jpg`
- `OPEN_GRAPH_IMAGE_PATH` — `./assets/og-card.jpg`
- `CONTACT_VCF_PATH` — `./contact.vcf`

If a placeholder like `REPLACE_WITH_...` is ever left in an `href` by
mistake, a small script in `index.html` detects it on page load and
disables that specific button rather than letting it open a broken link.

---

## 4. WhatsApp Privacy — Read This Before Changing Anything

**The number is never rendered as visible text anywhere on the page** — not
in the header, buttons, tooltips, accessibility labels, footer, alt text,
print styles, metadata, structured data, or source comments. Every place a
person could read it as plain text uses only the words "WhatsApp" or
"Start a Conversation."

**Where the number does exist in the source**, by necessity:
1. The `href` of the "WhatsApp" button in Primary Actions
2. The `href` of the "Start a Conversation" button in the closing prompt
   (an intentional duplicate of #1, so that button still works if
   JavaScript is disabled — see the comment above it in `index.html`)
3. Inside `contact.vcf`, which is only downloaded when someone explicitly
   taps "Save Contact"

**Important limitation, stated plainly:** a public `wa.me` link only works
because the destination number is present in that link's URL. Not printing
the number as page text stops casual copying by a person glancing at the
page or reading it aloud — it does **not** hide the number from anyone who
opens browser developer tools, views page source, or uses a scraping tool.
There is no way to make a functioning public WhatsApp link fully private.
This page reduces casual exposure; it does not guarantee secrecy, and this
README will not claim otherwise.

**If Jaideep wants a stronger long-term boundary:** the most reliable option
is a dedicated WhatsApp Business number used only for this card and similar
public-facing material, kept separate from his personal number. That number
can be swapped into the two `href="https://wa.me/..."` links and into
`contact.vcf` at any time — search the file for `wa.me/917276087697` to find
both occurrences.

---

## 5. Email Visibility Modes

The page defaults to **Mode B — button only**, per the brief (email is only
shown as text with explicit approval).

- **Mode B (current default):** the Email button's `href` is
  `mailto:jaideepphatak16@gmail.com`, but the address is never printed as
  visible text, alt text, or metadata anywhere on the page.
- **Mode A (visible email):** if Jaideep later approves showing the address
  publicly, add a text line near the Email button, e.g.
  ```html
  <p class="email-visible">jaideepphatak16@gmail.com</p>
  ```
  styled to match `.descriptor-secondary`. Do not add it to Open Graph tags,
  JSON-LD, or image alt text even in Mode A — those remain address-free by
  design so the address isn't harvested from shared-link previews.

---

## 6. VCard (`contact.vcf`) Notes

- Format: VCard 3.0, for the broadest compatibility across iOS, Android,
  Outlook, and Google Contacts.
- Contains: full name, title, org (EmbedOps), a short note, Pune/Maharashtra/
  India as work address (city-level only — no street address), mobile
  number, email, and three URLs (portfolio, LinkedIn, card).
- Deliberately excludes: residential address, date of birth, any government
  ID, financial details, family details, or private notes.
- The file is only fetched when a visitor taps "Save Contact." A small
  script does a `HEAD` request to confirm `contact.vcf` actually exists
  before showing that button, so a missing file fails silently rather than
  showing a dead button.
- To update contact details, edit `contact.vcf` directly — it's a plain
  text file. Keep the `TEL`, `EMAIL`, and `NOTE` lines free of anything not
  meant for a phone's address book, since once downloaded this file is
  outside the page's control.

---

## 7. Image Assets

- `assets/jaideep-avatar.jpg` — a square, face-forward crop of the approved
  executive caricature, used as the circular header portrait. If this file
  is ever removed, the `onerror` handler on the `<img>` tag in `index.html`
  automatically swaps in a clean "JP" initials fallback — no broken image
  icon.
- `assets/og-card.jpg` — a wider crop of the same artwork (1200×630), used
  only for link-preview cards on WhatsApp, LinkedIn, iMessage, etc.
- To replace either image: keep the same filename and aspect ratio, or
  update the `PROFILE_IMAGE_PATH` / `OPEN_GRAPH_IMAGE_PATH` references in
  `index.html`'s `<img>` tag and `<meta property="og:image">` tag
  respectively.
- `assets/favicon.svg` — a small inline "JP" monogram matching the site's
  navy/amber brand mark. Replace with a different SVG of the same filename
  if a different favicon is preferred later.

---

## 8. Local Testing

No build step is required — this is static HTML.

**Quick local preview:**
```bash
cd card
python3 -m http.server 8000
```
Then open `http://localhost:8000/` in a browser.

Opening `index.html` directly via `file://` mostly works, but the
`fetch(...)` check for `contact.vcf` and the Content-Security-Policy header
behave more accurately when served over `http://` — use the command above
rather than double-clicking the file.

**Things to check locally:**
- All six action buttons are visible and correctly labeled
- "Save Contact" downloads `contact.vcf` with the correct file name
- "WhatsApp" and "Start a Conversation" both open the same WhatsApp chat
- "Email" opens the default mail client with the correct address pre-filled
- "LinkedIn" and "View Executive Portfolio" open in new tabs
- "Share Card" either opens the native share sheet or copies the link and
  shows the "Link copied to clipboard" message
- The QR code renders and is scannable from the screen

---

## 9. Deploying to GitHub Pages

Assuming the parent portfolio repository already publishes
`https://phatakjaideep.github.io/`:

1. In that same repository, create a folder named `card/` at the repo root
   (if it doesn't already exist).
2. Copy the contents of this `card/` folder — `index.html`, `contact.vcf`,
   `README.md`, and the `assets/` folder — into that repo folder, preserving
   the folder structure exactly.
3. Commit and push:
   ```bash
   git add card/
   git commit -m "Add executive virtual business card"
   git push
   ```
4. GitHub Pages will publish it at `https://phatakjaideep.github.io/card/`
   automatically (usually within a minute or two of the push, sometimes up
   to ~10 minutes on the first deploy).
5. Confirm the live URL matches the `canonical`, `og:url`, and QR-code
   target already baked into `index.html`. If the final URL ever changes,
   update those three places together.

---

## 10. QR Code

Two QR assets are already included in `assets/qr/`, both encoding the
**stable card URL** `https://phatakjaideep.github.io/card/` — never the
phone number, never a `wa.me` link, and never the raw vCard. This is
deliberate: the physical/printed QR code can stay valid indefinitely even
if WhatsApp numbers, email addresses, or page content change later, because
it always resolves to whatever the live page currently says.

- `card-qr.svg` — vector, use this for any printed material (physical
  business cards, lanyards, signage). Scales to any size without quality
  loss.
- `card-qr.png` — 900×900 raster, use this for digital sharing (slides,
  email signatures, chat apps).
- Error correction level: **High (H)** — the code will still scan even if
  up to ~30% of it is obscured or damaged, which matters for print use and
  small logo overlays.
- Both already include a white quiet zone and strong black-on-navy
  contrast for reliable scanning.

**If a secondary QR code is wanted later** (e.g. one that opens WhatsApp
directly, or downloads the vCard directly), generate it the same way but
point it at that specific URL instead of the card page. Do this only if
Jaideep explicitly requests it — the default recommendation is to keep a
single QR code pointing at the page, per this project's privacy and
maintainability goals.

**To regenerate either file** (e.g. after the final canonical URL is
confirmed), any standard QR generator works — for example, with Python:
```bash
pip install "qrcode[pil]"
python3 -c "
import qrcode
from qrcode.constants import ERROR_CORRECT_H
qr = qrcode.QRCode(error_correction=ERROR_CORRECT_H, box_size=20, border=4)
qr.add_data('https://phatakjaideep.github.io/card/')
qr.make(fit=True)
qr.make_image(fill_color='#0B1220', back_color='#FFFFFF').save('card-qr.png')
"
```

---

## 11. Mobile-Browser Testing Checklist

- [ ] iOS Safari: page loads, avatar renders, all six buttons work
- [ ] iOS Safari: "Save Contact" opens the native "Add Contact" sheet
- [ ] Android Chrome: page loads, all six buttons work
- [ ] Android Chrome: "Save Contact" downloads and offers to open in Contacts
- [ ] Android Chrome / Samsung Internet: Share Card opens the native share sheet
- [ ] Desktop Chrome/Edge/Firefox: Share Card falls back to "copy link" and
      shows the confirmation message
- [ ] Page renders correctly at 320px, 375px, 414px, and 768px widths
- [ ] No horizontal scrolling at any tested width
- [ ] Dark-mode and light-mode both render legibly (toggle OS appearance
      setting to confirm)
- [ ] `prefers-reduced-motion` respected (disable animations in OS
      accessibility settings, confirm the status-badge pulse stops)

---

## 12. Accessibility Testing Checklist

- [ ] Tab through the entire page using only a keyboard — every button and
      link receives a visible focus ring
- [ ] Skip link ("Skip to content") appears on first Tab press and jumps to
      the main card
- [ ] Screen reader (VoiceOver / TalkBack) announces the name, headings,
      and each button's label correctly, including the QR code's alt text
- [ ] All six action buttons show a text label alongside their icon — no
      icon-only controls
- [ ] Color contrast: body text and buttons meet WCAG AA against both the
      dark and light background variants
- [ ] Page remains usable and legible with browser text size increased to
      200%

---

## 13. Privacy-Verification Checklist

Run this after every edit that touches contact details:

- [ ] View page source (`Ctrl+U` / `Cmd+Option+U`) — the WhatsApp number
      appears only inside the two `href="https://wa.me/..."` attributes,
      nowhere else
- [ ] Search the rendered page (not source) for any digit sequence — the
      phone number should not appear anywhere as visible text
- [ ] Confirm `<meta property="og:description">`, `<meta name="twitter:...">`,
      and the JSON-LD block contain no phone number or email address
- [ ] Confirm image `alt` text on the avatar, OG image, and QR code contain
      no phone number or email address
- [ ] Paste `https://phatakjaideep.github.io/card/` into LinkedIn's Post
      Inspector or a WhatsApp chat and confirm the link preview shows only
      the name/title/description — never a phone number
- [ ] Confirm `contact.vcf` only downloads after an explicit tap on
      "Save Contact" (it should not auto-download on page load)

---

## 14. Post-Deployment Smoke Test

Once the page is live at `https://phatakjaideep.github.io/card/`:

- [ ] Open the live URL directly (not from cache) on both mobile and desktop
- [ ] Scan the printed/shared QR code with both an iPhone and an Android
      phone and confirm it opens the live page
- [ ] Click all six buttons from the live page (not a local copy) and
      confirm each destination is correct
- [ ] Share the live URL on WhatsApp to yourself and confirm the preview
      card renders the intended title, description, and image
- [ ] Check that the favicon appears correctly in a browser tab
- [ ] Re-run the Privacy-Verification checklist (§13) against the live URL

---

## 15. Updating Contact Information Later

- **LinkedIn or portfolio URL changes:** update the relevant `href`
  attribute(s) directly in `index.html`; both appear once each.
- **WhatsApp number changes:** update both occurrences of
  `href="https://wa.me/917276087697"` in `index.html`, plus the `TEL` line
  in `contact.vcf`. Search the repository for `917276087697` to find every
  occurrence before publishing.
- **Email changes:** update the single `href="mailto:..."` in `index.html`,
  plus the `EMAIL` line in `contact.vcf`.
- **Card URL changes** (e.g. moving off GitHub Pages): update `canonical`,
  `og:url`, the `CONFIG.CARD_URL` JavaScript constant, and regenerate the
  QR code assets to point at the new URL.

---

## 16. Rotating to a Dedicated Business Number Later

If Jaideep decides to separate his personal WhatsApp number from public
material:

1. Set up a WhatsApp Business account on a second SIM or a virtual number.
2. Replace both `wa.me` links in `index.html` and the `TEL` field in
   `contact.vcf` with the new number, in the international format used
   throughout this file (`91` + 10 digits, no spaces, no leading `0`, no
   `+` inside the URL itself).
3. Re-run the Privacy-Verification checklist (§13) to confirm the old
   number no longer appears anywhere in the source.
4. This is the strongest available privacy boundary for a public-facing
   WhatsApp link — stronger than any amount of front-end obfuscation, which
   this project deliberately avoids implying works better than it does.

---

## 17. Physical Business-Card Adaptation

**Front:**
```
Jaideep Phatak
Program & Portfolio Leadership
AI-Enabled Program Delivery
Pune, India
```
No phone number on the front unless Jaideep later chooses to add one.

**Back:**
```
Helping leadership teams turn fragmented program information
into decision-ready executive visibility.

[ QR code — assets/qr/card-qr.svg ]

Scan to connect, save contact, or view portfolio

EmbedOps, a consulting practice led by Jaideep Phatak
```

**Print notes:**
- Use `card-qr.svg` (vector) at print resolution, not the PNG.
- Keep a clear quiet zone (blank margin) around the QR code equal to at
  least 4 modules — the SVG already includes this.
- Do not overlay a logo on top of the QR code without re-testing scan
  reliability at the final print size; the current design has no overlay.
- Test the printed QR at the actual card size (standard is roughly
  85mm × 55mm) with both an iPhone and an Android phone before ordering a
  full print run.
- Do not print a second QR code that links directly to WhatsApp or to
  `contact.vcf` on the standard card — the single card-page QR code is the
  recommended default, per §10.
- No pricing, service packages, or fee information on the physical card.

---

## 18. What This Page Deliberately Does Not Do

Per the design brief, this page contains no forms, no newsletter signup, no
booking/calendar integration, no cookies, no `localStorage`, no analytics or
tracking scripts, no third-party embeds, no chat widgets, and no pricing.
The only network request the page ever makes on its own is a same-origin
`HEAD` check for `contact.vcf` to decide whether to show the Save Contact
button — nothing is ever sent to a server, logged, or transmitted about a
visitor.
