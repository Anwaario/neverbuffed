# NEVERBUFFED

> A graffiti validation network. Document it before it's gone.

## What Is This

NeverBuffed is a community-powered platform for documenting graffiti and street art before it gets buffed (painted over). Spotters capture pieces with GPS metadata, the system auto-detects whether a capture is a **Genesis** (first-ever documentation of that location) or a **Verification** (confirming a piece is still there), and rewards are distributed via **$BUFF tokens**.

---

## Repo Structure

```
neverbuffed/
├── capture/
│   └── index.html          ← Mobile capture app (camera + GPS + Supabase upload)
├── feed/
│   ├── terminal.html       ← Terminal green feed (CRT aesthetic)
│   └── bw.html             ← Black & white brutalist feed
├── demo-images/            ← Sample graffiti photos for local dev/demo
│   ├── img_maers.jpg
│   ├── img_dashere.jpg
│   ├── img_stane.jpg
│   ├── img_tepar.jpg
│   ├── img_aba.jpg
│   ├── img_asek.jpg
│   ├── img_klr.jpg
│   ├── img_maison.jpg
│   ├── img_red_wild.jpg
│   ├── img_beef.jpg
│   └── img_nest.jpg
├── docs/
│   └── DECISIONS.md        ← Architecture decisions log
├── supabase/
│   └── schema.sql          ← Database schema to paste into Supabase SQL editor
└── README.md
```

---

## Current Stack

| Layer | Tech | Notes |
|---|---|---|
| Frontend | Vanilla HTML/CSS/JS | No build step, deploy anywhere |
| Database | Supabase (PostgreSQL) | Free tier |
| File Storage | Supabase Storage | `photos` bucket, public |
| Realtime | Supabase Realtime | Live feed updates on insert |
| Auth | None yet | Spotter ID is manual string for now |
| Hosting | TBD | Netlify / Vercel / GitHub Pages |

---

## How Genesis Detection Works

```
On upload →
  Query: SELECT COUNT(*) FROM captures WHERE gps_grid = [current]
  0 results → GENESIS  (you're first, clock starts, +100 $BUFF base)
  1+ results → VERIFY  (links back to genesis capture, +10 $BUFF base)
```

GPS grid = lat/lng rounded to 2 decimal places (~1km resolution).  
No manual toggle. The database decides.

---

## Supabase Setup

1. Create a new project at [supabase.com](https://supabase.com) (free tier)
2. Go to **SQL Editor** → paste contents of `supabase/schema.sql` → run
3. Go to **Storage** → New Bucket → name: `photos` → set to **Public**
4. Go to **Settings → API** → copy Project URL + anon public key
5. Open `capture/index.html` → tap the config banner → paste credentials

Credentials are saved to `localStorage` and shared between capture app and feed automatically.

---

## Reward Logic

| Event | Base | Multiplier |
|---|---|---|
| Genesis capture | 100 $BUFF | × quality rating |
| Verification | 10 $BUFF | × quality rating |

| Quality | Multiplier |
|---|---|
| Excellent | 1.5× |
| Good | 1.0× |
| Fair | 0.7× |
| Poor | 0.5× |

---

## Running Locally

No build step needed. Just open the HTML files in a browser.

For the feed to show your real captures:
1. Set up Supabase (see above)
2. Open `capture/index.html` and connect
3. Take a capture — it uploads photo + metadata
4. Open `feed/terminal.html` — it auto-connects using saved credentials

For demo mode (no Supabase): just open the feed files directly. They fall back to mock data with the real demo images.

---

## Soft Launch Checklist

- [ ] Supabase project created + schema applied
- [ ] `photos` storage bucket live and public
- [ ] Capture app tested end-to-end (photo → upload → appears in feed)
- [ ] Feed deployed to a public URL
- [ ] Domain connected (neverbuffed.io)
- [ ] iOS Shortcut updated to point at capture app URL
- [ ] 10 real captures in the database before announcing

---

## Open Questions / Next

- [ ] Auth — wallet connect vs simple username/password vs Apple Sign In
- [ ] $BUFF token — mock ledger first, real token later
- [ ] Buffed detection — how do we mark a piece as gone?
- [ ] Mobile PWA — add to home screen prompt
- [ ] Marketplace v1 — "What does this say?" translation bounties

---

## Build Log

See `docs/DECISIONS.md` for architecture decisions and context.

---

*Built with Claude. Documented before it's gone.*
