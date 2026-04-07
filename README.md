# Recknd — Waitlist Site

The waitlist landing page for **Recknd** — a quoting and scheduling app built for UK tradespeople.

Live at: **[recknd.co.uk](https://recknd.co.uk)**

---

## What This Is

A single-page waitlist site that:
- Captures email signups from tradespeople
- Sends them securely to a Brevo contact list via a Netlify serverless function
- Is hosted on Netlify with the custom domain recknd.co.uk (registered at Names.co.uk)

---

## Project Structure

```
recknd-site/
├── index.html                  ← The waitlist landing page
├── README.md                   ← This file
└── netlify/
    └── functions/
        └── subscribe.js        ← Serverless function (handles Brevo API securely)
```

---

## How It Works

```
User submits email
      ↓
index.html sends POST to /.netlify/functions/subscribe
      ↓
subscribe.js runs server-side (API key never exposed)
      ↓
Brevo API adds contact to "Recknd Waitlist" list
      ↓
User sees success message
```

The Brevo API key is stored as a **Netlify environment variable** — it never appears in the frontend HTML or this repository. This is why the form works securely on a public site.

---

## Tech Stack

| Layer | Tool | Cost |
|---|---|---|
| Frontend | Plain HTML/CSS/JS | Free |
| Serverless function | Netlify Functions | Free |
| Hosting | Netlify | Free |
| Email list | Brevo | Free (up to 100k contacts, 300 emails/day) |
| Domain | Names.co.uk | ~£10/yr |
| Fonts | Google Fonts (Barlow Condensed + Lato) | Free |

---

## Environment Variables

One environment variable is required. Set this in **Netlify → Site configuration → Environment variables**:

| Key | Value | Where to get it |
|---|---|---|
| `BREVO_API_KEY` | Your Brevo API key | Brevo → Account → SMTP & API → API Keys |

**Never put this key in any file that gets committed to GitHub.**

---

## The Netlify Function

`netlify/functions/subscribe.js` handles all email signups. It:

1. Accepts a POST request with `{ email: "..." }` in the body
2. Validates the email is present
3. Calls the Brevo contacts API using the server-side API key
4. Adds the contact to the Recknd Waitlist list (list ID set inside the file)
5. Returns 200 on success, 400/500 on failure

To update the Brevo list ID, edit line 16 of `subscribe.js`:
```javascript
listIds: [YOUR_LIST_ID],   ← change this number
```

Find your list ID in Brevo → Contacts → Lists → click the list → check the URL.

---

## Deployment

The site is deployed on **Netlify** with automatic deploys from this repository.

### To deploy a change manually:
1. Make your edits locally
2. Drag and drop the `recknd-site` folder onto the Netlify deploy drop zone
3. Or push to the connected GitHub branch if auto-deploy is configured

### To redeploy without changes:
Netlify → Deploys → **Trigger deploy** → Deploy site

---

## Domain Setup

The domain `recknd.co.uk` is registered at **Names.co.uk** and points to Netlify via nameservers.

### Current nameservers (set in Names.co.uk):
```
dns1.p0X.nsone.net
dns2.p0X.nsone.net
dns3.p0X.nsone.net
dns4.p0X.nsone.net
```
*(Exact values visible in Netlify → Domain management → your domain)*

### To update DNS in Names.co.uk:
1. Log in → Control Panel → Services → Domains & Services
2. Click `recknd.co.uk`
3. Find Nameservers section
4. Replace with the four Netlify nameservers shown above
5. Save — propagation takes up to 24 hours (usually under 2 hours)

### To verify DNS has propagated:
Go to [dnschecker.org](https://dnschecker.org) → type `recknd.co.uk` → record type A.
Netlify IPs start with `75.2.` or `99.83.` — if you see `185.199.x.x` it's still on GitHub Pages.

---

## Brevo Setup

Signups land in the **Recknd Waitlist** list in Brevo.

### To view signups:
Brevo → Contacts → Lists → Recknd Waitlist

### To email the waitlist:
Brevo → Campaigns → Email → Create campaign → select Recknd Waitlist as recipient list

### Important Brevo notes:
- Free plan: 100,000 contacts stored, 300 emails/day sending limit
- Emails sent from the free plan include a small Brevo logo in the footer
- Do **not** expose the API key in frontend HTML — Brevo's security system will auto-disable it
- Always keep the API key in Netlify environment variables only

---

## Local Testing

To test the Netlify function locally you need the Netlify CLI:

```bash
npm install -g netlify-cli
cd recknd-site
netlify dev
```

This runs the site at `http://localhost:8888` with the function available at `http://localhost:8888/.netlify/functions/subscribe`.

You'll need a `.env` file in the root (never commit this):
```
BREVO_API_KEY=your_api_key_here
```

---

## Brand Assets

Full Recknd brand guidelines, app icons, and landing page designs are maintained separately. Key brand details:

| Element | Value |
|---|---|
| Primary colour | Recknd Amber `#E8A020` |
| Background | Charcoal `#1A1A1A` |
| Display font | Barlow Condensed 900 |
| Body font | Lato 400/700 |
| Domain | recknd.co.uk / recknd.app |

---

## Roadmap

- [x] Waitlist page live at recknd.co.uk
- [x] Brevo email capture working via Netlify function
- [x] Custom domain connected
- [ ] Waitlist email sequence set up in Brevo
- [ ] UKIPO trademark application filed
- [ ] Companies House registration (Recknd Ltd)
- [ ] Beta MVP app built and tested
- [ ] First 50 signups reached
- [ ] Beta launch to waitlist

---

## Contact

Built by the Recknd founder.
Questions or issues → raise them in the GitHub Issues tab.

---

*Recknd is a quoting and scheduling app for UK tradespeople. Built in the UK, for the UK.*
