# HerbaGlow — Ayurvedic / Herbal Product Landing Page

A premium, fully responsive, conversion-focused single-page landing page for a Bangladeshi Ayurvedic / Herbal product, built with **HTML5 + Tailwind CSS (CDN) + Vanilla JavaScript** with the **Hind Siliguri** Google Font and Font Awesome icons. Brand primary color: `#f011d9`.

The order form auto-submits to **Google Forms** and instantly notifies you on **Telegram** — no backend needed.

---

## ✨ Features

- Hero with animated product, headline, price, countdown, CTA
- Product Benefits (6 glassmorphism cards)
- Features & Ingredients section
- Customer Reviews / Testimonials slider (auto-rotating)
- Before & After interactive comparison slider
- Why Choose Us section
- FAQ accordion
- Sticky bottom CTA (full-width on mobile)
- Animated order modal with **+/- quantity, size, color, full name, phone, address, payment method**
- Form validation (BD phone format, required fields)
- **Google Forms** auto-submission via hidden iframe (no CORS)
- **Telegram Bot** instant order notification (formatted)
- Floating WhatsApp chat button
- Live countdown timer (24h, persisted in localStorage)
- Limited-stock urgency alert with progress bar
- SEO meta tags + Open Graph
- Mobile / tablet / desktop responsive

---

## 📁 Folder Structure

```
my-project/
├── index.html      # Complete landing page (single file)
└── README.md       # This file
```

You can deploy `index.html` directly to **Netlify**, **Vercel**, **GitHub Pages**, **cPanel**, or any static host.

---

## 🚀 Quick Start

1. Open `index.html` in any browser to preview locally.
2. Edit the `CONFIG` object at the bottom of `index.html` to plug in your Google Form and Telegram credentials.
3. Replace product images, copy, prices, and contact numbers as desired.
4. Upload to your hosting provider.

---

## 🔧 Configuration

All credentials live in one place inside `index.html`:

```js
const CONFIG = {
  GOOGLE_FORM_URL: "https://docs.google.com/forms/d/e/REPLACE_FORM_ID/formResponse",
  GOOGLE_FORM_FIELDS: {
    productName: "entry.1111111111",
    quantity:    "entry.2222222222",
    size:        "entry.3333333333",
    color:       "entry.4444444444",
    fullName:    "entry.5555555555",
    phone:       "entry.6666666666",
    address:     "entry.7777777777",
    payment:     "entry.8888888888",
  },
  TELEGRAM_BOT_TOKEN: "REPLACE_BOT_TOKEN",
  TELEGRAM_CHAT_ID:   "REPLACE_CHAT_ID",
  PRICES: { "50ml": 445, "100ml": 745, "200ml": 1290 },
};
```

---

## 📋 Google Forms Setup (step by step)

1. Go to <https://forms.google.com> and create a **new blank form**.
2. Add **8 short-answer questions** with these labels (the order doesn't matter, but keep them all "Short answer"):
   - Product
   - Quantity
   - Size
   - Color
   - Full Name
   - Phone
   - Address
   - Payment
3. Click the **three-dot menu (⋮)** at the top-right of the form → **"Get pre-filled link"**.
4. Type any test value into every field, then click **"Get link"** → **"Copy link"**.
5. Paste that copied URL into a text editor. It will look like:
   ```
   https://docs.google.com/forms/d/e/1FAIpQLSf.../viewform?usp=pp_url
     &entry.1111111111=test
     &entry.2222222222=test
     ...
   ```
6. Copy the **form ID** (the long string between `/d/e/` and `/viewform`) and build the submit URL:
   ```
   https://docs.google.com/forms/d/e/<FORM_ID>/formResponse
   ```
   Paste this into `CONFIG.GOOGLE_FORM_URL`.
7. Map each `entry.XXXXXXXXX` value to the corresponding key in `CONFIG.GOOGLE_FORM_FIELDS`.
8. Open the form's **Responses** tab → link a **Google Sheet** if you want orders to appear in a spreadsheet automatically.
9. Place a test order from the landing page and confirm a row appears in your sheet.

> Why a hidden iframe? Google Forms blocks cross-origin `fetch` responses, but the form **does** accept the POST. Submitting through a hidden iframe makes the request succeed silently without CORS errors.

---

## 🤖 Telegram Bot Setup (step by step)

1. In Telegram, search for **@BotFather** and start a chat.
2. Send `/newbot`. Choose a **name** and a **unique username** (must end in `bot`).
3. BotFather will reply with an **HTTP API token** like `123456789:ABCdefGhIJKlmNoPqRsTuVwXyZ`. Copy it into `CONFIG.TELEGRAM_BOT_TOKEN`.
4. Decide where you want notifications:

   **Option A — Personal chat:**
   - Search your bot in Telegram → press **Start**.
   - Visit this URL in any browser:
     ```
     https://api.telegram.org/bot<YOUR_TOKEN>/getUpdates
     ```
   - Find `"chat":{"id":123456789,...}` — that number is your `chat_id`.

   **Option B — Group chat (recommended for teams):**
   - Create a Telegram group, add your bot as a member, and post any message in the group.
   - Visit `getUpdates` (URL above). The group `chat_id` will start with a minus sign, e.g. `-1001234567890`.

5. Paste the chat ID into `CONFIG.TELEGRAM_CHAT_ID`.
6. Place a test order — you should receive a beautifully formatted message like:

   ```
   🌿 নতুন অর্ডার এসেছে! 🌿
   ━━━━━━━━━━━━━━━━━━
   🛍️ পণ্য: HerbaGlow Ayurvedic Beauty Oil 100ml
   📦 পরিমাণ: 1
   📏 সাইজ: 100ml
   🎨 কালার: Pink
   ━━━━━━━━━━━━━━━━━━
   👤 নাম: রহিম উদ্দিন
   📱 ফোন: 01712345678
   🏠 ঠিকানা: ঢাকা, বাংলাদেশ
   💳 পেমেন্ট: Cash on Delivery
   ━━━━━━━━━━━━━━━━━━
   💰 মোট: ৳ 745
   🕒 24/05/2026, 15:30:00
   ```

> Security note: Anyone who views the source of a public landing page can see the bot token. The worst an attacker can do with the token alone is send messages to your chat. To protect against abuse, consider proxying the Telegram call through a tiny serverless function (Cloudflare Workers, Vercel Functions, etc.) and storing the token there.

---

## 🎨 Customization

| What | Where |
|---|---|
| Brand color | Tailwind `tailwind.config` → `colors.brand`. Default `#f011d9`. |
| Logo & brand name | Header (`HerbaGlow`) and footer |
| Product images | Replace the Unsplash URLs (hero, ingredients, before/after, gallery) |
| Prices | `CONFIG.PRICES` and the hero price tags |
| Phone number | WhatsApp button `href` and footer contact |
| Sizes / colors | `<input name="size">` and `<input name="color">` blocks in the modal |
| FAQ | `FAQ_DATA` array in the script |
| Testimonials | `#sliderTrack` slides in the markup |

---

## 🌐 Deploy

### GitHub Pages
1. Push this repo to GitHub.
2. **Settings → Pages → Deploy from a branch** → pick `main` and `/ (root)`.
3. Your site will be live at `https://<username>.github.io/<repo>/`.

### Netlify / Vercel
- Drag-and-drop `index.html` into the Netlify dashboard, **or** connect the GitHub repo. No build command needed.

### cPanel / Shared Hosting
- Upload `index.html` to your `public_html/` folder via FTP or the file manager.

---

## ⚡ Performance Tips

- The page already preconnects to Google Fonts, Tailwind CDN, and Font Awesome CDN.
- For production, consider:
  - Replacing the Tailwind CDN with a built CSS file (`npx tailwindcss -i input.css -o output.css --minify`).
  - Self-hosting fonts and Font Awesome.
  - Compressing images and serving them via a CDN.
  - Adding `loading="lazy"` (already set on below-the-fold images).

---

## 📞 Support

For questions about this template, reach out via the WhatsApp button on the live site or open an issue in the repository.

— Made with 💖 in Bangladesh
