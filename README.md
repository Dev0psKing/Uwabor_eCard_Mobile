# Uwabor_eCard_Mobile# Uwabor eCard

A mobile-first digital business card for **Collins Uwabor**, Founder & Principal Consultant at **Uwabor Consulting**.

It is a single static page (`index.html`) with no build step, no framework and no external requests. Fonts, portrait, logo and QR code are all built into the file, so it loads fast and looks the same on every device.

**Live:** https://card.uwabor.me *(after the custom domain is set up, see [Hosting](#hosting-on-github-pages))*

## What it does

- **Flip card:** tap the card to switch between the logo side and a QR code that saves the contact when scanned.
- **Save contact:** builds a `.vcf` file in the browser, so the phone offers to add Collins to its contacts.
- **Quick actions:** one-tap Call, Email, Website and LinkedIn.
- **Contact list:** phone numbers, email, website and LinkedIn, each with a copy button.
- **Services list:** what Uwabor Consulting offers.
- **Share:** uses the phone's share sheet, or copies the link where sharing is not available.
- **Compact top bar:** appears when the header scrolls out of view.
- **App-like behaviour:** safe-area padding for notches, a fixed bottom action bar, and Home Screen support (add it from the browser's share menu).

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole e-card. Deploy this. |
| `README.md` | This file. |
| `CNAME` | Created automatically by GitHub when a custom domain is set. Do not delete it. |

## Hosting on GitHub Pages

1. Make sure the repository is **public** (Pages on the free plan needs this).
2. Go to **Settings → Pages**. Under **Source**, choose **Deploy from a branch**, then the `main` branch and the `/ (root)` folder. Save.
3. Add the custom domain `card.uwabor.me` in the same settings page **before** changing DNS.
4. At the DNS provider for `uwabor.me`, add a **CNAME** record:
   - Name: `card`
   - Target: `<your-github-username>.github.io`
5. Wait for DNS to update (it can take up to 24 hours), then tick **Enforce HTTPS**.

Do not change or remove any existing DNS records for the domain, especially the mail records used by `collins@uwabor.me`.

GitHub adds a `CNAME` file to the repository when the domain is saved. If you edit files in Codespaces afterwards, run a pull first so you have it:

```bash
git pull
```

## Updating the card

Open `index.html` and search for the text you want to change.

- **Phone, email, links:** update the rows in the "Contact" list and the four buttons under "Quick actions" (the `href` values).
- **Saved contact details:** edit the `VCARD` list inside the `<script>` block near the bottom of the file.
- **Services:** edit the four lines under the "Services" heading.
- **Colours:** change the variables at the top of the `<style>` block:

  | Variable | Colour | Use |
  |---|---|---|
  | `--bg` | `#080D18` | Background (navy) |
  | `--accent` | `#C9785C` | Logo, labels, buttons (terracotta) |
  | `--ivory` | `#F2EEE7` | Main text |
  | `--dim` | `#7B7B77` | Muted text |

  These match the main website, so change them there too if you change them here.
- **Portrait:** the photo is embedded as a base64 image in two places (`<img src="data:image/jpeg;base64,...">`). To replace it, convert a square photo (about 360 x 360 px) to base64 and swap both.

### Regenerating the QR code

The QR code on the back of the card is an inline SVG that holds the contact details. **Whenever a phone number, email or link changes, regenerate it**, otherwise scanning will save the old details.

```bash
pip install segno
```

```python
import segno

vcard = "\r\n".join([
    "BEGIN:VCARD", "VERSION:3.0",
    "N:Uwabor;Collins;;;", "FN:Collins Uwabor",
    "ORG:Uwabor Consulting",
    "TITLE:Founder & Principal Consultant",
    "TEL;TYPE=CELL:+2348139371889",
    "TEL;TYPE=CELL:+2348158055605",
    "EMAIL:collins@uwabor.me",
    "URL:https://collins.uwabor.me",
    "END:VCARD",
])

qr = segno.make(vcard, error="l", encoding="utf-8", boost_error=False)
print(qr.svg_inline(scale=1, border=0, dark="#080D18", light=None))
```

Paste the printed `<svg ...>` in place of the existing one inside the `qrtile` element. Keep the `viewBox` attribute so it scales, and test the result by scanning it with a phone before publishing.

## Notes

- The page makes **no network requests** (no analytics, no external fonts), so it also works offline once opened.
- The printed business card has a separate QR code that points to https://collins.uwabor.me. If the e-card becomes the main link, the printed QR can be regenerated to point to the e-card address.
- If **Save contact** does not open the contacts app on a particular phone, the file still downloads and can be opened from the downloads list.

## Credits

- Fonts: [Cormorant Garamond](https://fonts.google.com/specimen/Cormorant+Garamond) and [Jost](https://fonts.google.com/specimen/Jost), both open source (SIL Open Font License), subset and embedded.
- Icons: hand-drawn inline SVG in the style of [Feather](https://feathericons.com) (MIT).
- QR code: generated with [segno](https://pypi.org/project/segno/).

## Copyright

© Uwabor Consulting. All rights reserved. The logo, name and portrait are not licensed for reuse.