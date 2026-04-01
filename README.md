# LegalBuddy — GitHub Pages Deployment Guide

This is the **static version** of LegalBuddy — everything runs in the browser.  
No Python, no server, no hosting fees. Just push to GitHub and it's live.

---

## What You Need

- A free [GitHub account](https://github.com)
- Your **Anthropic API key** (get one free at [console.anthropic.com](https://console.anthropic.com) → API Keys)
- That's it.

---

## Step-by-Step Deployment (takes ~5 minutes)

### Step 1 — Create a GitHub repository

1. Go to **https://github.com** and sign in (or create a free account)
2. Click the **green "New"** button (top-left, next to "Repositories")
3. Fill in:
   - **Repository name**: `lexisai` (or any name you like)
   - **Visibility**: ✅ Public *(required for free GitHub Pages)*
   - Leave everything else as-is
4. Click **"Create repository"**

---

### Step 2 — Upload your files

You'll see an empty repo page with a message like *"Quick setup"*.

**Option A — Drag and drop (easiest, no coding required):**
1. Open the folder containing this README and `index.html` on your computer
2. Drag **both files** directly into the GitHub browser window
3. Scroll down to the "Commit changes" box
4. Type a message like `Add LexisAI app`
5. Click the green **"Commit changes"** button

**Option B — Using Git (if you're comfortable with the terminal):**
```bash
cd lexisai-pages/        # the folder with index.html and README.md
git init
git add .
git commit -m "Deploy LexisAI"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/lexisai.git
git push -u origin main
```

---

### Step 3 — Enable GitHub Pages

1. In your repository, click **"Settings"** (tab at the top, with a gear icon)
2. In the left sidebar, click **"Pages"**
3. Under **"Source"**, click the dropdown that says *"None"* and select **"main"** (or "master")
4. Make sure the folder is set to **"/ (root)"**
5. Click **"Save"**

GitHub will show a banner:  
> ✅ *"Your site is live at https://YOUR_USERNAME.github.io/lexisai/"*

**Wait 1-2 minutes** for GitHub to build and deploy. Then visit the URL!

---

### Step 4 — Use the app

1. Open your URL: `https://YOUR_USERNAME.github.io/lexisai/`
2. You'll see a **gold banner at the top** asking for your Anthropic API key
3. Enter your key (starts with `sk-ant-...`) and click **Save Key**
4. Your key is stored **only in your own browser** — it never leaves your device
5. Upload a legal document or click a tile in the Library tab → click **Analyze**

---

## Your Live URL

Once deployed, your app is available at:

```
https://YOUR_USERNAME.github.io/lexisai/
```

Replace `YOUR_USERNAME` with your actual GitHub username.  
Replace `lexisai` with whatever you named your repository.

**This URL works 24/7, on any device, anywhere in the world — just like a real website.**

---

## Custom Domain (Optional — makes it look professional)

If you own a domain like `lexisai.com` or `mylegalapp.com`:

1. In GitHub → Settings → Pages → **"Custom domain"**
2. Type your domain (e.g., `lexisai.com`) and click **Save**
3. Go to your domain registrar (Namecheap, GoDaddy, etc.)
4. Add a **CNAME record**: `www` → `YOUR_USERNAME.github.io`
5. Add **A records** pointing to GitHub's IPs:
   ```
   185.199.108.153
   185.199.109.153
   185.199.110.153
   185.199.111.153
   ```
6. Wait 10-30 minutes for DNS to propagate
7. GitHub automatically provisions a free SSL certificate (https://)

---

## How the RAG Works (Technical)

This static app implements a full RAG pipeline in JavaScript:

1. **Classification** — keyword scoring across 17 document classes (NDA, I-485, W-2, leases, bills, etc.)
2. **Retrieval** — TF-IDF cosine similarity search across 15 inlined corpus documents totaling ~76KB of legal text
3. **Grounded Analysis** — top-5 retrieved chunks are sent to Claude as context alongside the uploaded document
4. **Structured Output** — Claude returns specific, citation-anchored JSON (not generic summaries)

All processing happens client-side. The only external call is to `api.anthropic.com`.

---

## Updating the App

To update your deployed app:
1. Edit `index.html` on your computer
2. Go to your GitHub repo → click `index.html` → click the **pencil icon** (Edit)
3. Paste your updated code
4. Click **"Commit changes"**
5. GitHub Pages automatically rebuilds in ~1 minute

---

## Files in This Folder

```
lexisai-pages/
├── index.html    ← The entire app (HTML + CSS + JavaScript + legal corpus)
└── README.md     ← This deployment guide
```

That's it — one file is the entire product.

---

## Frequently Asked Questions

**Is my API key safe?**  
Your key is stored in your browser's localStorage and never sent anywhere except directly to `api.anthropic.com`. It is not logged, stored, or shared.

**Why does the app need an API key?**  
GitHub Pages only hosts static files — there's no server to hide an API key on. Each user provides their own key. This is standard practice for personal-use static AI apps.

**Does it support PDF uploads?**  
PDFs are partially supported — the browser reads them as raw text. For best results, use `.txt` files or copy-paste text. Full PDF parsing requires a backend server (use the Flask version for that).

**How do I add more corpus documents?**  
Open `index.html`, find the `const CORPUS = [` array near the top of the `<script>` section, and add a new object following the same structure as the existing entries.

**Can I make it private?**  
Yes — change your repository to **Private** and upgrade to GitHub Pro ($4/month) which includes Pages for private repos. Alternatively, deploy on Netlify (free, supports private repos).
