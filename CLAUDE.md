# CLAUDE.md — madali.cz

David Madali's personal page. It is one page in two languages: English in `index.html` and Czech in `cs/index.html`.
- Static HTML with everything inline and no build step.
- GitHub Pages serves `main` from the repo root. The custom domain is in `CNAME`, and HTTPS is enforced.
- The OreOrbit atlas and the Bohemia Defense site live elsewhere: the `oreorbit-updater` repo and WordPress. This page links to them and mirrors some of their data (see "Data that mirrors OreOrbit").

Reply to David in Czech unless he writes in English. Be technical and brief.

## Working rules
- **Every change goes through a PR branch.** David merges. A merge is the release, because Pages deploys `main` within a few minutes. Never push to `main`.
- **The repo is public.** Commit no secrets and no private notes. Commit no contact data beyond what the page already shows.
- **Change both languages in the same PR.** The two files are full copies of each other.
- **Verify before saying "done"** (see "Checks"). After the merge, check the live page.

## Files
| File | What |
|---|---|
| `index.html`, `cs/index.html` | The page in EN and CS. Each holds its own head, JSON-LD, CSS, content and scripts. |
| `llms.txt` | Summary for AI search: expertise, profile links, background, projects, answers with links to the OreOrbit FAQ. English only. |
| `robots.txt`, `sitemap.xml` | Allow all. The sitemap lists `/` and `/cs/` with hreflang pairs and `lastmod`. |
| `CNAME` | `madali.cz` |
| `_config.yml` | Pages builds this repo with Jekyll (legacy build). The `exclude` list keeps `CLAUDE.md` off the website; without it, the file would be served as `/CLAUDE.html`. Add any new repo-only note to it. |
| `fonts/` | Hanken Grotesk and DM Mono as self-hosted woff2, with their OFL licences |
| `david-madali.jpg` (600×750), `david-madali-square.jpg` (800), `david-madali-press.jpg` (1280×1600) | Portraits. The press photo is the download in "Speaking and media". |
| `og-david-madali-2026-11.jpg` | The share image (1200×630) |
| `oreorbit-2026-11.jpg` | Atlas screenshot (1600×1000) in the Work section |
| `og-david-madali.jpg`, `oreorbit.jpg` | Older versions. They stay for old links and caches; nothing on the page uses them. |

## Inside each page, top to bottom
1. **`<head>`:**
   - title and description
   - canonical, plus hreflang `en`, `cs` and `x-default`
   - Open Graph and Twitter tags pointing at the share image
   - the `seznam-wmt` verification tag (keep it)
   - the font preload
2. **JSON-LD `@graph`:**
   - `ProfilePage`, with `dateModified`
   - `Person` (`#person`), with `knowsAbout`
   - `Organization` (`https://bohemiadefense.com/#organization`)
   - `WebApplication` (`https://bohemiadefense.com/atlas/#oreorbit`), whose `offers` price is 0 EUR because the atlas is free

   The Organization and the WebApplication use the same `@id`s as bohemiadefense.com, so search engines treat them as one entity. Keep their facts aligned with the site-wide JSON-LD there: `docs/wordpress/jsonld-sitewide.html` in `oreorbit-updater`.
3. **Inline `<style>`.**
4. **Content:**
   - **hero:** the name, mineral buttons (`data-m`, `--c`, `title`), `#readout` and the `#globe` canvas. The `#sound` pill is off by default and remembered in `localStorage` (`madali-sound`). The notes are generated in the browser.
   - **Work:** two project cards. OreOrbit shows the atlas screenshot; Bohemia Defense shows an inline SVG dot picture.
   - **In the press**
   - **Path**
   - **Latest analysis** (`#latest`): stays hidden until the three newest bohemiadefense.com posts load from the WordPress REST API
   - **Speaking and media:** bios `#bio-s` and `#bio-l`, photos
   - **Contact**
   - **footer**
5. **Scripts:**
   - `window.__L`: labels and mineral names in the page's language.
   - `window.__D`: `land` (dots of the land mask) and `sites` as `[name, mineral id, country, lat, lng]`. It is identical in both files.
   - The globe script:
     - `COL`: the mineral colours.
     - `ORDER`: the mineral order.
     - `PENT`: one note per mineral, as long as `ORDER`. A missing entry leaves that mineral silent.
     - `HOME`: Prague.
   - `ooHit("madali:en" | "madali:cs")`: an anonymous counter beacon to the atlas `count.php`. It skips visitors who send Do Not Track or Global Privacy Control.
   - The reveal animation, which respects `prefers-reduced-motion`.

   The scripts in the two files differ only in `__L`, the counter key and the latest-analysis loader.

## Data that mirrors OreOrbit
The page shows the atlas's minerals and sites. When the atlas changes them, update this page in the same round. The `add-mineral` skill in `oreorbit-updater` lists every place.
- **Minerals:** 12, with ids, colours and order exactly as in the atlas (`order` in `atlas/data/reference.json`, the CSS variables in `atlas/index.html`). Update all of these:
  - the hero buttons
  - `COL`, `ORDER`, `PENT`
  - `__L.minerals` in both languages
- **Sites:** `__D.sites` mirrors `sites` in `atlas/data/reference.json`: `name`, `m`, `country`, `lat`, `lng`. There are 43 today.
- **Counts in the copy:**
  - Where: the globe `aria-label`, the Work text, the bios, the JSON-LD descriptions and `llms.txt`.
  - What: "twelve minerals" and "43 sites" in EN; the matching Czech forms in CS.
  - "A cluster of 12 security-industry companies" in Path and the long bio is not a mineral count, so leave it.
- **`knowsAbout`:** the same Wikidata and Wikipedia list as `KNOWS` in `updater/oreorbit_pages.py` and the WordPress JSON-LD.
- **FAQ links:** `llms.txt` links to OreOrbit FAQ anchors (`p/faq.html#…`, `p/faq-cs.html#…`). Those anchors are stable; never rename them.
- **Images with text:** the share image shows twelve dots in atlas order, and the atlas screenshot shows the counts. Text scans cannot read either.
  - A new picture gets a new filename. Keep the old file.
  - After the merge, David re-scrapes `https://madali.cz/` and `https://madali.cz/cs/` in LinkedIn Post Inspector and the Facebook Sharing Debugger.

## Copy rules
- **Style:**
  - British English. Headlines are declarative sentences ending with a period. Labels are in sentence case with no letter-spacing.
  - No hype words, emoji or exclamation marks.
  - Czech is natural, not translated word for word. Put a non-breaking space after single-letter words (`v&nbsp;`, `s&nbsp;`, `a&nbsp;`) and use proper case forms.
- **Brand:**
  - Dark first: background #050607, paper #eceee9, one signal green #3ddc97.
  - Fonts: Hanken Grotesk and DM Mono.
  - Mineral colours only ever mark their mineral, always next to its symbol or name.
- **Never:**
  - mention Claude or AI tooling
  - mention a degree, school or studies anywhere, the JSON-LD included (no `alumniOf`)
  - put a GitHub repo in `sameAs`
  - show a home address; Prague is enough
  - write "s.r.o." or any company form next to Bohemia Defense, which is a project, not a company
- **Prices:** never type a price. The only price on the page is the free atlas's `offers` in the JSON-LD. Paid plans are described on the atlas, which reads them from its `config.js`.
- **Facts about David** (press, path, bios) come from David. Add no claims of your own.
- **When content changes,** bump `lastmod` for both URLs in `sitemap.xml` and `ProfilePage.dateModified` in both pages.

## Checks
Before the PR, run this from the repo root with `oreorbit-updater` checked out beside it:

```bash
python3 - <<'EOF'
import json, re
ref = json.load(open("../oreorbit-updater/atlas/data/reference.json", encoding="utf-8"))
want = sorted((s["name"], s["m"]) for s in ref["sites"])
data = []
for f in ("index.html", "cs/index.html"):
    t = open(f, encoding="utf-8").read()
    json.loads(re.search(r'<script type="application/ld\+json">(.*?)</script>', t, re.S).group(1))
    d = json.loads(re.search(r"window\.__D\s*=\s*(\{.*?\});?</script>", t, re.S).group(1))
    order = json.loads(re.search(r"ORDER = (\[.*?\])", t).group(1))
    col = json.loads(re.search(r"COL = (\{.*?\})", t).group(1))
    pent = re.search(r"PENT = \[(.*?)\]", t).group(1).split(",")
    btn = re.findall(r'data-m="([a-z]+)"', t)
    ok = (sorted((s[0], s[1]) for s in d["sites"]) == want and order == ref["order"] == list(col) == btn[:len(order)]
          and len(pent) == len(order))
    data.append(d)
    print(f, "JSON-LD parses; sites", len(d["sites"]), "minerals", len(order), "in sync" if ok else "OUT OF SYNC")
print("__D same in both:", data[0] == data[1])
EOF
```

Then:
1. **Local render:** serve the repo (`python3 -m http.server`) and open `/` and `/cs/` at desktop width and at 390 px. Check for:
   - no console errors
   - no horizontal overflow
   - mineral buttons that light their sites on the globe and play a note when sound is on
   - in the CS page, Czech everywhere the change touched
2. **JSON-LD changes:** after the merge, run Google's Rich Results Test on both URLs. The OreOrbit item must stay valid.
3. **After the merge:**
   - Confirm the Pages build: `gh api repos/backyardmadeit-bit/madali.cz/pages/builds/latest --jq .status` must return `built` at the merge commit.
   - Compare the live files with the repo: fetch `https://madali.cz/<file>?nocache=<n>` and compare the md5.
   - Repo notes stay off the site: `https://madali.cz/CLAUDE.html` and `https://madali.cz/CLAUDE.md` return 404.
   - From `oreorbit-updater`, run `python3 tools/consistency_scan.py --remote`. It scans the live madali.cz and the WordPress pages for stale counts and reports them as warnings.
