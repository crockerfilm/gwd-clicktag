# GWD → IAB clickTag Converter (moved)

**This tool now lives at https://crockerfilm.github.io/figma-html5/gwd.html**, part of the
Copy Motion site (repo `crockerfilm/figma-html5`, file `gwd.html`). This repo's page
redirects there. The history below is kept for reference.


Converts Google Web Designer (Studio / DV360) HTML5 banner zips to the standard IAB clickTag format, then checks every converted banner in a real browser frame.

**Use it:** https://crockerfilm.github.io/gwd-clicktag/ or download `index.html` and open it locally. It works offline.

Everything runs in your browser. Banners are never uploaded anywhere.

## What it does

- Drop zips, folders, or a zip of zips. Batches of any size work, and folder structure is kept.
- Adds `var clickTag = ""`, filled at runtime from `?clickTag=` or set directly by the ad server. Only `http(s)` values are accepted, so `javascript:` is refused.
- Adds `<meta name="ad.size">` from the GWD ad metadata.
- Rewires every Studio exit to `window.open(window.clickTag, "_blank")`.
- Removes Enabler.js, the gwdgooglead component, StudioExports, `<gwd-exit>`, GWD ad metadata and the GWD self-identification metas.
- Replaces Studio's `initAd()` start-up with a direct `goToPage`, so the banner doesn't render blank without Enabler.
- Removes `__MACOSX` and `.DS_Store`.
- Takes backup images dropped in beside the zips (same name, JPG or PNG), checks their size, compresses them under 50 KB and saves them beside the converted zip.

Each banner is then loaded three times (no clickTag, a test `?clickTag=`, and a `javascript:` payload) and runs through 17 checks. The checks cover hardcoded URLs, remote dependencies, click behavior, ad-server overwrite, ad.size, component start-up, script errors, missing or unused files, weight (over 200 KB fails, over 150 KB is noted), whether the animation stops by 30 s, and the backup JPG. Results are written for non-developers.

The **QA preview** tab, plus a downloadable QA page, show every converted banner with a test clickTag so someone can check rendering and animation by eye.

Settings (test URLs, weight limit) live in the `SPEC` object at the top of the main script.
