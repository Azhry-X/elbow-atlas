# Elbow — Master Clinical Dossier & Companion Test Atlas

**Live Atlas:** https://azhry-x.github.io/elbow-atlas/  
**Dossier PDF:** [`master_elbow_dossier.pdf`](./master_elbow_dossier.pdf) — 9 pages, A4 portrait, built for Apple Pencil in Notability / GoodNotes.

Sister projects:
- [Knee Complex dossier + atlas](https://github.com/Azhry-X/knee-atlas) — same design system, same page architecture.
- [Ankle & Foot dossier + atlas](https://github.com/Azhry-X/ankle-atlas) — same design system, same page architecture.
- [Lumbar Spine dossier + atlas](https://github.com/Azhry-X/lumbar-atlas) — same design system, same page architecture.
- [Shoulder Complex dossier + atlas](https://github.com/Azhry-X/shoulder-atlas) — same design system, same page architecture.
- [Cervical Spine dossier + atlas](https://github.com/Azhry-X/cervical-atlas) — same design system, same page architecture.

## What is in the dossier
| Page | Content | Accent |
|---|---|---|
| 1 | Triage & red flags (elbow extension test — Appelboam, Lennon normal-radiograph ROM, Darracq active ROM; distal biceps hook test & squeeze test; dislocation, compartment syndrome, septic bursitis/arthritis, paediatric fracture, cardiac & radicular referral), pathway router, subjective, body chart | red |
| 2 | Differential diagnosis matrix (LET, medial epicondylalgia, cubital tunnel, radial tunnel / PIN, UCL injury, PLRI, distal biceps, OCD capitellum, OA / stiffness), prevalence & risk factors (Shiri), natural course vs injection / physiotherapy (Smidt 2002, Bisset 2006), cubital tunnel / UCL / PLRI test accuracy | blue |
| 3 | Objective examination: elbow & forearm goniometry write-in table (Armstrong real-change thresholds), pain-free & maximum grip strength (Smidt 2002 APMR, Sveinall SDC / MIC, JOSPT LET CPG measures quote), palpation map, C5–T1 neurological screen, ULTT2b (radial) & ULTT3 (ulnar) | blue |
| 4 | Special tests & PROMs: LET tests (Cozen, Mill, Maudsley, chair), medial epicondylitis test, instability tests (valgus, varus, moving valgus, milking, lateral pivot-shift, posterolateral rotatory drawer, tabletop relocation), nerve tests (Tinel, elbow flexion, pressure + flexion, scratch collapse), PROM dashboard (PRTEE, Oxford Elbow Score, QuickDASH, KJOC, MEPS, NPRS, PSFS) | blue |
| 5 | Post-op: surgeon-directed restrictions, distal biceps repair (Amarasooriya, Amin, Bergman), UCL reconstruction & internal brace repair (Erickson 2015 OJSM, Cain, Dugas, Kemler), LET surgery vs sham (Kroslak), post-traumatic stiffness & arthrolysis / splinting (Kodde, Lanzerath, Lindenhovius), heterotopic ossification (Shukla, Khan), radial head fractures (Liow, Paschos, Harding, Mulders), operative log | purple |
| 6 | Injections, orthoses & electrophysical agents: corticosteroid injection (Bisset, Coombes 2010, Coombes 2013), PRP / autologous blood (Karjalainen, Kim), ESWT (Buchbinder), low-level laser (Bjordal), orthoses (Struijs), JOSPT LET CPG orthoses / taping / electrophysical-agent recommendations, parameter log | orange |
| 7 | Manual therapy: mobilisation with movement (Vicenzino, Coombes 2013), joint mobilisation SR (Lucado 2019), deep transverse friction (Loew 2014), JOSPT LET CPG manual therapy & dry needling recommendations, cubital tunnel conservative care, safety & informed consent | teal |
| 8 | Exercise & return to activity: JOSPT LET CPG exercise recommendations, strengthening MA (Kim 2021), natural course comparison (Smidt, Coombes), UCL non-operative & interval throwing (Erickson 2015 Sports Health, Kemler 2022), stiffness splinting (Lindenhovius), 12-week LET loading log & interval-throwing log | green |
| 9 | 12-visit flowsheet, discharge milestones (PRTEE, Oxford Elbow Score, QuickDASH, pain-free & maximum grip vs verified MIC / SDC by population), discharge summary | slate |

Every statistic carries an evidence marker: **H** high · **M** moderate · **L** low · **E** expert opinion.
Underlined test names open the matching Atlas card; **▸** marks cards with a verified video.

## Repository layout
```
index.html                  Atlas (GitHub Pages entry point) — generated
master_elbow_dossier.html   Dossier (generated, self-contained CSS)
master_elbow_dossier.pdf    Dossier PDF (generated)
src/pages/p1.html … p9.html Dossier page bodies  ← edit content here
src/dossier.css             Design system
atlas/data/tests.yaml       Atlas cards: steps, criteria, accuracy, evidence, videos, calculators  ← single source of truth
atlas/build_atlas.py        Builds index.html from tests.yaml (page badges/filters = every dossier page that links the card)
atlas/config.py             Atlas base URL
tools/build_dossier.py      Assembles HTML → PDF → runs every check
tools/render_and_check.py   Page count, overflow, overlap, fonts, required safety strings, previews
tools/check_links.py        Verifies every PDF link, Atlas anchor, card source (DOIs via the DOI registry) and video
tools/required_strings.txt  Safety-critical text that must never disappear
fonts/                      Source Sans 3 (OFL)
```

## Rebuild after an edit
One-time setup (Python 3.10+ and Node.js):
```bash
pip install -r requirements.txt
python -m playwright install chromium
npm install
```
Windows, one command (stops at the first failure):
```powershell
powershell -ExecutionPolicy Bypass -File build.ps1            # Atlas + PDF + all checks + link audit
powershell -ExecutionPolicy Bypass -File build.ps1 -SkipLinks # same, without the network link audit
```
Or step by step on any OS:
```bash
python atlas/build_atlas.py                # Atlas
python tools/build_dossier.py              # Dossier PDF + all checks (exit 1 on any problem)
python tools/check_links.py                # links (add --live after the site is published)
```
The dossier build fails on: page count ≠ 9, content overflowing a page / card / cell, overlapping glyphs,
a character missing from Source Sans 3 (font fallback), malformed tables, leaked markup, internal link targets
under 4 mm, or a missing safety string from `tools/required_strings.txt`.
Page previews are written to `preview/` when poppler (`pdftoppm`) or `pip install pypdfium2 pillow` is available.
Authoring shortcuts in `src/pages/*.html` (expanded at build time):
`<a class="test-link" data-atlas="hook-test">Hook test</a>` · `<a class="info-icon" data-atlas="jospt-2022-let-cpg"></a>` ·
`<a class="calc-badge" data-atlas="prtee">Calc</a>` · `<a class="guide-badge" href="https://…">JOSPT CPG</a>`.
An unknown `data-atlas` id stops the build.

## Status
Draft cards under clinical review with open-access citations and video demonstrations.
