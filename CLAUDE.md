# SKI Academy: working notes for Claude Code

This repo hosts Adiaha's interactive study guides. It is a static site on GitHub Pages:
repo `enoete/ski-academy` (public), branch `main`, folder `/ (root)`.

- Landing page: https://enoete.github.io/ski-academy/
- A guide:      https://enoete.github.io/ski-academy/<subject>/<file>.html

This project is separate from CFBC. Don't mix in CFBC conventions, the lesson manifest, or the JSONBin pipeline.

## Layout

```
index.html        landing page; renders a card per subject from guides.json (don't hand-edit the list into it)
guides.json       the single source of truth for what appears on the landing page
.nojekyll         serve files as-is
<subject>/        one folder per subject, kebab-case: social-studies, english, science, math, ...
  <topic>.html    one self-contained HTML file per guide
```

## Workflow: "here's a new guide for <subject>"

The user will give a guide (usually a claude.ai artifact link) and a subject.

1. **Fetch it.** For a claude.ai artifact link, use the Artifact tool with `action: "read"` (never WebFetch or curl).
   It saves the full HTML to a local file. Read all of it before publishing, since everything here goes public.
   If the link can't be read, **stop and tell the user.** Never recreate, rewrite or "improve" a guide.
2. **Save it unmodified.** Copy the saved file byte-for-byte into `<subject>/<topic>.html`
   (`cp` it, then `cmp` it against the source). Use a short kebab-case file name taken from the topic,
   e.g. `social-studies/ancient-egypt.html`. Don't edit the guide's content, and don't inject a back link
   or nav into it. If something in the guide looks wrong, tell the user and don't touch the file.
3. **Register it in `guides.json`.** Add an object to that subject's `guides` array:
   ```json
   { "title": "…", "file": "topic.html", "blurb": "one short, kid-friendly line", "added": "YYYY-MM-DD" }
   ```
   `title` usually comes from the guide's `<title>`, minus any "— Adiaha's Study Guide" suffix. `added` is today's date;
   the landing page shows a "New" tag for 14 days after it. Keep newest guides at the bottom of each list.
   For a **new subject**, create the folder and add a subject block:
   ```json
   { "id": "science", "name": "Science", "emoji": "🔬", "color": "#1BB5A8", "guides": [ … ] }
   ```
   `id` must equal the folder name. Pick a bright colour that differs from the other subjects and keeps white
   text readable (used so far: social-studies `#E8932A`). Suggested: english `#FF5C8A`, science `#1BB5A8`,
   math `#5B3FD1`. Validate with `python3 -m json.tool guides.json`.
4. **Commit and push.**
   ```bash
   git add -A && git commit -m "Add <title> (<subject>)" && git push
   ```
5. **Confirm it's live.** Pages rebuilds automatically on push. Wait for the build, then check that both
   pages return 200:
   ```bash
   gh api repos/enoete/ski-academy/pages/builds/latest -q .status   # wait for "built"
   curl -sI https://enoete.github.io/ski-academy/<subject>/<file>.html | head -1
   ```
   Give the user the guide's live URL.

## Things to keep true

- The repo is public, so never commit secrets, keys or anything private.
- `index.html` fetches `guides.json` at runtime, so the site has to be viewed over http(s), not `file://`.
  To preview locally, run `python3 -m http.server` in the repo root.
- The landing page must stay mobile-first (Adiaha uses a phone or tablet): check it at 360px wide.
