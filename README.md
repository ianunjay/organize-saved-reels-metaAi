# Saved Reels Library — Turn Your Saved Reels Into a Beautiful Library

*Two steps to start. Everything else is just details below.*

Turn **all** of your saved Instagram / Facebook reels — not just recent ones — into a searchable library with separate sections for **Movies, Books, Tools & Websites, and Music**.

This repo contains a single, self-contained guide. No build step. No backend. No dependencies.

**Live guide:** open `saved_reels_guide_light.html` in your browser, or enable GitHub Pages (below) to share a public link.

## What you end up with

- A **standalone HTML library** that works on its own — all your reels embedded inside it
- Sections: **All Reels • Movie Library • Book Library • Tools & Websites • Music Library**
- Search + filters by topic, type, decoded-only, sort by newest / most liked
- Separate columns for **movies suggested, books suggested, websites/tools suggested, songs suggested** — not buried in one description

Example from a real run: **731 saved reels** → decoded → library.

## Step 1 — Get Meta AI

Go to the Meta AI website, **or** download the Meta AI app — depending on your phone.

| Where | Link |
| ----- | ---- |
| 🌐 Website — works on any device | [meta.ai](https://meta.ai/) |
| 🍎 iPhone / iPad — iOS | [App Store — Meta AI](https://apps.apple.com/app/id6443721219) |
| 🤖 Android | [Google Play — Meta AI](https://play.google.com/store/apps/details?id=com.facebook.stella) |

> **Tip:** Sign in with the same account where you saved your reels on Instagram / Facebook. That's how it can find them.

## Step 2 — Paste this prompt

Open a new chat in Meta AI, paste the **whole prompt below**, and send it. That's it — it does the rest.

*You can also copy it from the guide: open `saved_reels_guide_light.html` and hit **Copy prompt**.*

```text
I want you to go through ALL of my saved reels - not just recent ones, every single one I have ever saved on Instagram and Facebook.

Step 1 - Fetch them all: Pull every saved reel (post_type = REEL, saved_by_me = true, since 2004-02-04, no limit). Tell me the total count. If Instagram and Facebook differ, do both and say how many came from each.

Step 2 - Decode each reel: For every reel, create these columns:
- created_at, author_username, author_name, url, like_count, comment_count, hashtags, tagged_users, music_track, music_artist
- topic_category (choose one: Movies & TV, Books & Reading, Music, Tech / AI / Tools & Websites, Design & Art, Comedy, Sports, Food & Cooking, Travel & Places, Fashion & Style, Fitness & Health, Business & Money, Relationships & Lifestyle, Gaming, News & Society, Spirituality & Philosophy, Education & Learning, Other / Mixed)
- what_it_is_talking_about (1-line subject)
- description_enriched (merge core insight + narrative - which includes on-screen text, overlays, book/film covers, spoken dialogue - + tagged users, hashtags, music/audio credits, author bio. If a reel shows a UI tool name, book cover, or film title on screen even if nobody says it out loud, include it here.)
- movies_suggested (separate column: only quoted Title-Case titles near film/movie/series/clip words. Filter out dialogue/shouting/fragments. Do NOT invent titles.)
- books_suggested (separate column: only 'Title by Author' pattern, e.g. 'Atomic Habits by James Clear'. Do NOT invent books.)
- websites_tools_apps_suggested (separate column: domains + 'X website / app / tool / game' patterns. Do NOT invent tools.)
- songs_music_suggested (separate column: from music_track + music_artist credits)
- people_tagged_mentioned, places_mentioned
Do NOT invent a title/tool/book if it is not named in the available metadata. Empty means 'no clean title in metadata', not 'no suggestion'.

Step 3 - Save the decoded data: Save it as BOTH saved_reels_decoded.json (all reels, with topic counts) and saved_reels_decoded.csv (same columns). Confirm the total count matches Step 1.

Step 4 - Build me a standalone HTML file (do NOT use the artifact builder, build the HTML file directly with Python):
File name: saved_reels_library.html
The HTML must EMBED the decoded JSON data inside itself (const DATA = [...] in a <script>), so it works on its own with no separate CSV/JSON needed. It must contain ALL reels, not a sample.
The HTML must have:
- Header: 'My Saved Reels Library' + total count + subtitle
- Stats row: counts by topic_category + counts for movies/books/tools/songs decoded
- Sticky controls: text search (searches what, description, author, movies/books/websites/songs), topic chips (All + every topic), type filter (All / Movies only / Books only / Tools only / Music only / Only with decoded titles), sort (Newest, Oldest, Most liked, Most commented, A-Z), 'Only decoded' toggle
- Tabs/Sections:
  1) All Reels - full library grouped by topic
  2) Movie Library - only reels with movies_suggested, card per movie: title + genre inferred from description (Drama, Comedy, Thriller, Romance, Horror, Documentary, Animation, Crime, Sci-Fi, Biopic, Family, Unknown)
  3) Book Library - only reels with books_suggested, card per book: split 'Title by Author', genre inferred (Self-help, Fiction, Comedy, Biography, Business, Poetry, Unknown)
  4) Tools & Websites Library - only reels with websites_tools_apps_suggested, card per tool: name + tool type inferred (AI Tool, Design Tool, Productivity, Website/Fun Site, Game, Video Editing, Music, Learning, Other) + what it does
  5) Music Library - only reels with songs_music_suggested, card per song: song + artist
- Every card: topic pill, movie/book/tool/music pills, author_username, created_at, like_count, comment_count, description (collapsed with Expand/Collapse), hashtags, and a working 'Open Reel' link using the url field
- Empty state: 'No reels match these filters' only if filter truly returns 0
- Footer note: 'Genres and tool types are inferred from descriptions. Empty decoded columns mean no clean title/tool was named in the metadata.'
- Clean, modern, responsive grid, dark editorial design, no placeholder images, typography + colored pills
- Before finishing, verify the embedded DATA length equals the total from Step 1 and that a known title from the data appears in the HTML.

Deliver: the HTML file link + a short summary (total count, topic split, how many movies/books/tools/songs decoded).
```

## The rest — details, in case you want them

You don't need to read this to use it. It's here so you know what the prompt is doing, what to check, and what to do if something looks off.

## What the prompt actually does (4 parts)

1. **Fetch everything** — all saved reels, no limit, from `2004-02-04`, Instagram + Facebook.
2. **Decode each reel** — topic, what it's about, enriched description (on-screen text, overlays, covers, tags, music credits, author bio — so a tool name shown on screen counts even if nobody says it), plus separate columns for movies / books / websites / songs. It does *not* invent titles.
3. **Save the data** — as `saved_reels_decoded.json` + `saved_reels_decoded.csv`, with total confirmed.
4. **Build the HTML directly** — with Python, embedding the JSON inside the HTML (`const DATA = [...]`), so it works with no separate file. It verifies the embedded count before finishing.

## What it looks like

| Section | Card shows |
| ------- | ---------- |
| 🎬 Movie Library | Title + genre. Example: *Sapta Sagaradaache Ello – Side A* → Drama / Romance |
| 📖 Book Library | Title + author. Example: *Atomic Habits by James Clear* → Self-help |
| 🛠️ Tools Library | Name + type (AI / Design / Game / Learning) + what it does |
| 🎵 Music Library | Song + artist from the audio credit |

## Checklist — before you call it done

- [ ] Total in HTML = total fetched?
- [ ] Can you search a movie / book / tool you *know* you saved and find it?
- [ ] Do Open Reel links open the right reel?
- [ ] Do Movies / Books / Tools / Music tabs show non-zero counts?
- [ ] Is the HTML self-contained (works without the CSV)?

## 3 traps to avoid

1. **"Last 7 days" trap:** if your latest save is older, last 7 days = 0. Always ask for ALL, then filter in the HTML.
2. **Combined-entities trap:** if you only ask for "named entities", movies / books / tools get mashed into one field. Ask for *separate columns* — the prompt already does this.
3. **Empty-file trap:** if the HTML shows "No reels match", the data file handed to the builder was empty. Ask it to rebuild from the backup / re-verify `len(DATA) == total`.

## Want a smaller version first?

Add this line at the very top of the prompt:

```text
Only do this for my Books + Tools reels first (topic = Books & Reading OR Tech / AI / Tools & Websites), then build the HTML for just those. Once that works, do the rest.
```

## Reuse it later

1. Save a new reel → run the same prompt again.
2. Or ask: *"Re-run the same saved-reels library prompt, but only for reels saved after `[date]`. Append them to the library."*
3. Keep the HTML file — it's your library. The CSV is your backup / spreadsheet.

## Host it on GitHub (public)

This repo is just the guide — no personal data in it. To publish it:

1. Create a new public repo on GitHub (e.g. `saved-reels-library`).
2. Upload these files:
   - `saved_reels_guide_light.html` — the guide (rename to `index.html` if you want it as the homepage)
   - `README.md` — this file
3. Go to **Settings → Pages**:
   - **Source:** Deploy from a branch
   - **Branch:** `main` / `(root)`
   - Save.
4. Your public link will be: `https://<your-username>.github.io/<your-repo-name>/`
   - If you renamed the guide to `index.html`, that link opens the guide directly.
   - If you kept the original name, share: `https://<your-username>.github.io/<your-repo-name>/saved_reels_guide_light.html`

To update the guide later, just replace the HTML file and commit — Pages redeploys automatically.

> ⚠️ **Privacy — read this before you publish:** Do **not** commit your personal `saved_reels_decoded.json`, `saved_reels_decoded.csv`, or `saved_reels_library.html` (with your reels embedded) to a public repo. Those contain your saved reels, authors, and links. Keep those files private / local. Only publish the guide + README, which contain no personal data.

## Files in this repo

| File | What it is |
| ---- | ---------- |
| `saved_reels_guide_light.html` | The guide — Step 1, Step 2 prompt, details. Light theme. Works standalone. |
| `README.md` | This file — same guide in GitHub-friendly format. Copy-paste ready. |

## License

MIT — use it, share it, remix it. If you improve the prompt, a PR / star is appreciated.

*Made from a real run: 731 saved reels → decoded → library. Genres & tool types are inferred from descriptions.*
