# NEVERBUFFED — Decisions Log

A running log of architecture and product decisions. Update this when something important is decided so both accounts / collaborators stay in sync.

---
## Session 2 — Supabase Live

✓ Supabase project created (neverbuffed)
✓ captures table created with full schema
✓ photos storage bucket live and public
✓ Row level security enabled
✓ Capture app live at GitHub Pages URL
✓ First real capture uploaded successfully
✓ Feed design settled — B&W brutalist (not terminal green)
✓ Images need GitHub raw URLs (next session)

NEXT:
- Fix image URLs to point at GitHub raw URLs
- Wire feed to pull real captures from Supabase
- Test full loop: capture on phone → appears in feed


## 2025-03 — Initial Build

### Dropped Airtable
**Decision:** No Airtable. Direct to Supabase.  
**Why:** Airtable adds a manual step between capture and the product going live. Supabase gives us DB + file storage + realtime in one free tier. Captures go directly to the feed.

### Genesis Detection — Time-based
**Decision:** Genesis = first capture ever at a GPS grid location. System auto-detects, no manual toggle.  
**Why:** Simpler UX, more trustworthy. The database is the authority, not the spotter.  
**Implementation:** `gps_grid` = lat/lng rounded to 2 decimal places (~1km). Query on upload: if 0 results at that grid → GENESIS.

### GPS Grid Resolution
**Decision:** Round lat/lng to 2 decimal places for grid matching.  
**Why:** ~1km radius is large enough to catch nearby re-captures of the same piece, small enough to not false-positive across different walls.  
**Revisit:** May need to tighten to 3 decimal places (~100m) in dense urban areas.

### Photo Handling
**Decision:** Watermark burned directly onto photo at capture time (canvas API). Upload to Supabase Storage. Store public URL in DB.  
**Why:** No post-processing needed. Watermark includes GPS, timestamp, type (GENESIS/VERIFY), quality rating, and NEVERBUFFED.IO badge.  
**No base64 in DB:** Photo stored as file, URL stored as string.

### No Airtable / No JSON Downloads
**Decision:** Removed the "download JSON + watermarked photo" flow from the original iOS Shortcut plan.  
**Why:** That was a bridge to Airtable. With direct Supabase upload, one tap does everything.

### Frontend — No Framework
**Decision:** Vanilla HTML/CSS/JS. No React, no build step.  
**Why:** Startup, broke, ship fast. Can migrate later if needed. Supabase JS SDK loads via CDN.

### Two Feed Variants
**Decision:** Maintain both terminal-green and B&W brutalist feed.  
**Why:** Both have merit. Terminal green is more distinctive / memorable. B&W is cleaner for broader audiences. Decision on which to ship deferred until soft launch feedback.

### Design Language
**Decision:** Brutalist. B&W primary palette. Terminal green as an alternative.  
**Reference:** brutalistwebsites.com  
**Fonts:** Bebas Neue (display/numbers) + Space Mono (body) for B&W. VT323 + Share Tech Mono for terminal.  
**Rejected:** Terminal green as primary (too niche), any purple gradients, Inter/Roboto.

### Spotter ID
**Decision:** Manual text string for now. No auth.  
**Why:** Ship faster. Real auth (wallet / Apple Sign In) is a later milestone.  
**Risk:** Spoofing. Acceptable for alpha.

---

## Open / Undecided

- **$BUFF token:** Mock ledger (just a number in the DB) for now. Real token TBD.
- **Buffed detection:** How do we know a piece is gone? Options: community report, time-based decay, re-capture showing painted wall.
- **Hosting:** Netlify vs Vercel vs GitHub Pages. All free tier viable.
- **Domain:** neverbuffed.io — confirm registration status.
- **Mobile PWA:** Add to Home Screen prompt. Shortcut still works but PWA is cleaner.
- **Auth:** Wallet connect (fits the $BUFF token narrative) vs simple username vs Apple Sign In.

---

*Keep this updated. If you make a decision in a Claude session, paste it here.*
