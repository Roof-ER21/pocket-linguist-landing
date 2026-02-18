# Pocket Linguist Email Capture — Setup Guide

Complete setup takes approximately 30–45 minutes. Everything here is free.

---

## Step 1 — Create a Kit (ConvertKit) Free Account

1. Go to [app.kit.com](https://app.kit.com) and click **Start for free**
2. Enter your name, email, and choose a password
3. On the setup screen, select:
   - Creator type: **"I create content for an audience"**
   - Audience size: choose the closest option (start with 0–500)
4. Complete email verification

The free plan supports up to **10,000 subscribers** and includes:
- Unlimited broadcast emails
- 1 automation sequence
- Landing pages and forms
- Basic reporting

---

## Step 2 — Create the Form and Get Your Form ID

Kit forms are what capture subscriber info and trigger automations.

1. In your Kit dashboard, go to **Grow → Landing Pages & Forms**
2. Click **+ New Form**
3. Select **Inline** form type (this embeds in external pages)
4. Choose any template — you won't be using Kit's visual editor much since the landing page has its own design
5. In the form editor:
   - Add a **First Name** field (click "Add a field")
   - Keep the **Email Address** field (default)
   - Change the button text to: "Send Me the Cheat Sheet"
6. Click **Save**
7. Your **Form ID** is visible in the browser URL bar:
   ```
   https://app.kit.com/forms/XXXXXXXX/edit
                               ^^^^^^^^ — this is your Form ID
   ```
   It is also shown in the form's **Settings** tab as "Form ID"

8. **Get your API Key:**
   - Go to your profile icon (top right) → **Settings**
   - Click **Developer** in the left sidebar
   - Your **API Key** (v3) is shown here — copy it

---

## Step 3 — Connect the Landing Page to Your Form

Open `/Users/a21/pocket-linguist-marketing/email-capture/index.html` in a text editor.

Find all instances of the following placeholders and replace them:

| Placeholder | Replace with | Where to find it |
|-------------|-------------|-----------------|
| `YOUR_FORM_ID` | Your numeric form ID (e.g. `7483921`) | Kit → Forms → URL or Settings tab |
| `YOUR_UID` | Your form UID (on the embed code page) | Kit → Forms → Embed → HTML snippet → `data-uid` |
| `YOUR_API_KEY` | Your Kit API key | Kit → Settings → Developer |
| `#APP_STORE_LINK` | Your App Store URL | App Store Connect |

There are **3 places** `YOUR_FORM_ID` appears (main form, hidden input, bottom form) and **2 places** `YOUR_API_KEY` appears. Replace all of them.

**How to get the UID:**
1. In Kit, open your form → click **Embed**
2. Select the **HTML** tab
3. Look for `data-uid="XXXXXXXXX"` in the snippet — copy that value

After replacing, the main form action URL should look like:
```
https://app.kit.com/forms/7483921/subscriptions
```

---

## Step 4 — Host the Language Cheat Sheet PDF

You need to create the actual PDF and host it somewhere publicly accessible.

**Creating the PDF:**
- Use Canva (free) — search "cheat sheet" templates
- Design 10 sections (one per language) with the phrases from your welcome email
- Export as PDF

**Hosting options (all free):**

| Option | Steps | Link format |
|--------|-------|-------------|
| Google Drive | Upload PDF → right-click → "Share" → "Anyone with link" → copy link → change `/view` to `/preview` or use direct download link | `https://drive.google.com/uc?id=FILE_ID&export=download` |
| Dropbox | Upload → share → copy link → change `?dl=0` to `?dl=1` for direct download | `https://www.dropbox.com/s/XXXXX/file.pdf?dl=1` |
| GitHub (same repo) | Commit PDF → use raw.githubusercontent.com URL | `https://raw.githubusercontent.com/USER/REPO/main/cheatsheet.pdf` |

Once hosted, **update the CTA link in Email 1** of your welcome sequence (see Step 5).

---

## Step 5 — Set Up the Welcome Email Sequence in Kit

The welcome series in `welcome-emails.md` maps to a **Visual Automation** in Kit.

### Create the automation:

1. In Kit, go to **Automate → Automations**
2. Click **+ New Automation**
3. Click **"Start from scratch"**

### Add the trigger:

1. Click **"+ Add trigger"**
2. Select **"Subscribes to a form"**
3. Pick the form you created in Step 2
4. Click **Save**

### Add each email:

For each of the 5 emails:

1. Click the **+** button after the previous step
2. Select **"Send email"**
3. Click **"+ Create new email"** (or select an existing draft)
4. Fill in:
   - **Subject:** (copy from `welcome-emails.md`)
   - **Preview text:** (copy from `welcome-emails.md`)
   - **Body:** Paste the markdown content and format using Kit's editor
5. Click **Save**

### Add delays between emails:

After each email (except the last), add a **Wait** step:
1. Click **+** after the email
2. Select **"Wait"**
3. Set the duration:
   - After Email 1: **Wait 2 days**
   - After Email 2: **Wait 2 days** (delivers on Day 4)
   - After Email 3: **Wait 3 days** (delivers on Day 7)
   - After Email 4: **Wait 7 days** (delivers on Day 14)

### Activate the automation:

1. Review the full sequence in the visual editor
2. Toggle the automation from **Draft** to **Live** (top right)
3. The automation will now trigger for every new subscriber

---

## Step 6 — Hosting the Landing Page (3 Free Options)

### Option A — GitHub Pages (Recommended, fully free)

1. Create a new GitHub repo (e.g. `pocket-linguist-landing`)
2. Push your `email-capture/` folder to the repo
3. Go to repo **Settings → Pages**
4. Under "Source", select **main branch**, folder **/ (root)** or **/docs**
5. GitHub gives you a URL like: `https://yourusername.github.io/pocket-linguist-landing/`

Optionally connect a custom domain (e.g. `get.pocketlinguist.app`) in the Pages settings — GitHub Pages supports custom domains for free.

### Option B — Netlify (Free, drag-and-drop)

1. Go to [netlify.com](https://netlify.com) → sign up free
2. Drag and drop your `email-capture/` folder onto the Netlify deploy zone
3. You get a URL like: `https://random-name.netlify.app`
4. Rename it under **Site settings → Site details → Change site name**
5. Connect a custom domain in **Domain management** (free for Netlify-managed domains)

### Option C — Vercel (Free, best performance)

1. Go to [vercel.com](https://vercel.com) → sign up with GitHub
2. Click **+ New Project**
3. Import your GitHub repo (from Option A, or create a new one with just the `index.html`)
4. Vercel auto-detects a static site and deploys instantly
5. You get a URL like: `https://pocket-linguist-landing.vercel.app`

All three options support:
- HTTPS by default
- Custom domains
- Free tier with no traffic limits for a landing page

---

## Step 7 — Test the Full Funnel

Before promoting, test the end-to-end flow:

- [ ] Open the landing page on mobile — check layout
- [ ] Submit the form with a real email address
- [ ] Confirm the Kit subscriber was created (Kit dashboard → Subscribers)
- [ ] Check that Email 1 arrived (check spam folder too)
- [ ] Verify the PDF download link works
- [ ] Check that the thank-you state appears on the page after form submit
- [ ] Confirm the 5-email sequence is scheduled in Kit

---

## Step 8 — Drive Traffic to the Page

Once the page is live and tested, traffic sources in order of priority:

### Organic (free, highest quality)
- Link in your Twitter/X bio: `"Free language cheat sheet → [URL]"`
- Pin a tweet about the cheat sheet to your profile
- Reddit: Share in r/languagelearning, r/Spanish, r/LearnJapanese etc. — lead with value, mention the free resource
- TikTok / Instagram Reels: Short video showing 3 surprising phrases → "link in bio for the full 50-phrase cheat sheet"

### Cross-promotion
- Partner with language learning accounts for shoutout swaps
- Mention the cheat sheet in any podcast appearances or blog posts
- Add to your email signature

### Paid (optional, once organic is working)
- Twitter/X Ads targeting: interests in travel, language learning
- Pinterest Ads (high intent for reference resources like cheat sheets)

---

## Quick Reference

| Item | Value |
|------|-------|
| Kit dashboard | https://app.kit.com |
| Form ID location | Kit → Forms → URL bar or Settings tab |
| API key location | Kit → Settings → Developer |
| Form action URL | `https://app.kit.com/forms/YOUR_FORM_ID/subscriptions` |
| Landing page file | `/Users/a21/pocket-linguist-marketing/email-capture/index.html` |
| Welcome emails | `/Users/a21/pocket-linguist-marketing/email-capture/welcome-emails.md` |
| Sequence timing | Immediate → Day 2 → Day 4 → Day 7 → Day 14 |

---

## Troubleshooting

**Form submits but no email arrives:**
- Check your Kit automation is set to "Live" (not Draft)
- Check the form ID and API key in `index.html` are correct
- Look in Kit → Subscribers to see if the address was captured
- Check spam/promotions folder

**CORS error in browser console:**
- This is expected for direct fetch calls to Kit's API from localhost
- The form still works — Kit processes submissions server-side
- Test on a deployed URL (not `file://`) to avoid CORS issues

**"Thank you" state doesn't show:**
- Open browser DevTools → Console tab, look for JS errors
- Make sure `id="form-container"` and `id="thank-you-msg"` are present in the HTML

**Subscribers not triggering the automation:**
- In Kit, check the automation trigger is linked to the correct form
- Ensure the automation is set to "Live"
- New subscribers added before the automation was live won't be retroactively enrolled
