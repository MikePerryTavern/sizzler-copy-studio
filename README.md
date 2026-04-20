# Sizzler Copy Studio

Internal copy generation tool for the Sizzler brand team and approved franchisees. Generates on-brand copy across social, email, SMS, and Google Business using the June 2025 Sizzler Brand Guidelines, baked in as a system prompt.

---

## What's in this folder

- `index.html` — the interface franchisees see
- `api/generate.js` — the backend that talks to the Anthropic API
- `package.json` — lists the one dependency (the Anthropic SDK)
- `vercel.json` — tells Vercel how to route the API
- `sizzler_system_prompt.md` — the full brand voice prompt, for reference only (not deployed, it's baked into generate.js)

---

## Deploy to Vercel (first-time, read carefully)

### 1. Make three accounts

All three free to start. Sign up with the same email so you don't get confused.

- **GitHub** — github.com — this is where the code lives
- **Vercel** — vercel.com — this is the hosting. When signing up, click "Continue with GitHub" so they link automatically
- **Anthropic API** — console.anthropic.com — this is what powers the copy generation

### 2. Get your API key

In the Anthropic console, go to API Keys, create a new key. It starts with `sk-ant-api03-`. Copy it somewhere safe. You only see the full thing once.

Also add a small amount of credit ($10 is plenty to start) in the Billing section. Without credit the tool won't generate anything.

### 3. Upload this folder to GitHub

- Go to github.com, click the `+` button top right, pick "New repository"
- Name it `sizzler-copy-studio`
- Set it to **Private**
- Click "Create repository"
- On the next page, click "uploading an existing file"
- Drag the entire contents of this folder (not the folder itself, the files inside it) into the browser
- Scroll down, click "Commit changes"

### 4. Deploy on Vercel

- Go to vercel.com
- Click "Add New... → Project"
- It'll show your GitHub repos. Click the `sizzler-copy-studio` one
- Before clicking Deploy, expand "Environment Variables"
- Add one:
  - Name: `ANTHROPIC_API_KEY`
  - Value: paste the API key from step 2
- Click Deploy

Wait about 60 seconds. You'll get a live URL like `sizzler-copy-studio-abc123.vercel.app`. Open it, try it, it works.

### 5. Custom domain (optional)

If you want `sizzlercopy.com` or similar:

- Buy the domain. Namecheap and Cloudflare are both cheap. About $12/year
- In Vercel, go to your project → Settings → Domains
- Type your domain in, click Add
- Vercel gives you two DNS records (an A record and a CNAME)
- Paste those into your domain registrar's DNS settings
- Wait 10-30 minutes for it to go live

---

## Ongoing costs

- Domain: ~$12/year (if you bought one)
- Vercel hosting: free on the Hobby plan (plenty for this tool)
- Anthropic API: pay per use. Each generation (three variations) costs a few cents. Budget $10-30/month depending on how many franchisees use it

To set a spend cap on the Anthropic side, go to console.anthropic.com → Settings → Limits. Set a monthly usage limit so you can't get surprised.

---

## Updating the brand voice later

The system prompt lives in `api/generate.js` in the `SYSTEM_PROMPT` constant. To update it:

1. Edit `api/generate.js` in GitHub (click the file, then the pencil icon)
2. Change what you want
3. Click "Commit changes"
4. Vercel automatically redeploys in about a minute

If you want to keep the markdown version in sync too, update `sizzler_system_prompt.md` the same way.

---

## Troubleshooting

**"Method not allowed" when I click Generate**
The API function didn't deploy. Check Vercel's Deployments tab for errors. Usually means the `api` folder is in the wrong place.

**"Missing required fields"**
Pick a channel, campaign type, CTA, and write a brief before generating.

**"Generation failed" or "invalid JSON"**
Usually means the Anthropic API key is missing, wrong, or out of credit. Check the Environment Variables in Vercel → Project Settings, and check your credit balance at console.anthropic.com.

**Anything else**
Check the Vercel Functions logs. Project → Deployments → click the latest → Functions tab. Error messages show up there.
