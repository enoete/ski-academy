# SKI Academy

Interactive study guides for Adiaha, served with GitHub Pages.

**Live site:** https://enoete.github.io/ski-academy/

## Layout

```
index.html          landing page (reads guides.json)
guides.json         list of subjects and their guides
social-studies/     one folder per subject
  mesopotamia.html  each guide is a single self-contained HTML file
```

## Adding a guide

1. Save the guide's HTML file into its subject folder, for example `science/volcanoes.html`.
2. Add an entry for it under that subject in `guides.json`. If the subject is new, add a new subject block too.
3. Commit and push to `main`. Pages redeploys in a minute or two.
