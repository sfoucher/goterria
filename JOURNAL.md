## 2026-03-16 (publications quality fixes)

- Fix 1: Corrected corrupted author name in `tlili2020InterferometricPhaseUnwrapping` — `Cavayas, Franois` → `Cavayas, Fran\c{c}ois` (missing cedilla restored).
- Fix 2: Replaced all `"University Name"` placeholders in `_data/coauthors.yml` with real affiliations: Université de Sherbrooke for core team, INRS for Apparicio. Removed all empty `url: ""` fields (only Foucher retains a URL).
- Prettier left `_data/coauthors.yml` unchanged.
- Committed as `fix: correct author encoding, update coauthor affiliations` (commit `357a084`).

## 2026-03-16 (Task 7 critical fix: project link buttons)

- Fixed critical invalid-HTML bug: `{% if project.links %}` block was nested inside the outer `<a>` card wrapper in both `projects.liquid` and `projects_horizontal.liquid`, making button clicks trigger card navigation instead of opening the external URL.
- Moved the links `<div>` to after the closing `</a>` tag in both partials (still inside the outer `.col` wrapper for layout integrity).
- Added `{% if link.url and link.label %}` guard inside the loop to skip malformed entries.
- Applied `| escape` to both `link.url` (href) and `link.label` (button text) to prevent XSS.
- Removed non-existent `img: assets/img/projects/example-thumb.png` field from `_projects/example-project.md`.
- Added `.project-links` comment in `_sass/_components.scss` inside the `// Projects` section.
- Prettier reformatted the `<a>` button tag to split the label onto its own line with `{{- ... -}}` whitespace trimming — functionally identical.
- Committed as `fix: move project link buttons outside card anchor, add input guards` (commit `5412274`).

## 2026-03-16 (Code review: Task 7 project link buttons)

- Reviewed Task 7 implementation across `_includes/projects.liquid`, `_includes/projects_horizontal.liquid`, `_projects/example-project.md`, and `assets/img/projects/.gitkeep`.
- Button class `btn btn-sm z-depth-0` confirmed to be an exact match with `_layouts/bib.liquid` — visually consistent.
- Security attributes `target="_blank" rel="noopener noreferrer"` confirmed present on both templates.
- Critical issue found: buttons are nested inside the block-level outer `<a>` card wrapper, which is invalid HTML5. Browsers parse inner anchors outside the outer one; button clicks will not navigate to `link.url`.
- Important issue: `link.url` output directly into `href` with no scheme validation — javascript: URI injection is possible from frontmatter.
- Important issue: no guard against empty `link.url` or `link.label` values inside the loop.
- Minor issue: `project-links` CSS class has no corresponding entry in `_sass/_components.scss`.
- Minor issue: `example-project.md` references `assets/img/projects/example-thumb.png` which does not exist.
- Minor issue: Prettier run not confirmed on changed files; CI `prettier.yml` will fail if formatting differs.
- Assessment: Needs fixes (Critical nested-anchor defect blocks the feature from working).

## 2026-03-16 (Task 6: publications fixes)

- Fix 1: Removed `volume = {n/a}` and `number = {n/a}` from `durandLackingDataNo` (early-access article; literal "n/a" strings would render on the page).
- Fix 2: Renamed BibTeX key `clabaud2026SystematicEvaluationCOTQ` → `clabaut2026SystematicEvaluationCOTQ` (author is Clabaut, not Clabaud).
- Fix 3: Added `"Apparicio, Philippe"` entry to `_data/coauthors.yml` (he appears in two publications but was missing from the file).
- Ran prettier on `_data/coauthors.yml` (unchanged).
- Committed as `fix: correct BibTeX key typo, remove n/a field values, add co-author` (commit `af84a11`).
- Marked Task 6 as completed.

## 2026-03-16 (Task 6: publications setup)

- Populated `_bibliography/papers.bib` with 21 real publications fetched from the GOTERRIA Zotero collection via zotero-mcp.
- Entry types: 17 journal articles, 2 conference papers, 1 book; attachments were skipped.
- Removed all Albert Einstein demo entries; replaced the two placeholder entries with actual group research output.
- Cleaned each entry: stripped local `file = {...}` paths, fixed malformed single-string author fields (two items with `zotero-item-*` keys were rewritten with proper `Lastname, Firstname and ...` author format and meaningful keys).
- Entries with `annotation = {Soumis}` (submitted) and `annotation = {Accepted}` are included — al-folio renders these fine.
- Marked `clabaut2024SyntheticDataSentinel2` as `selected = {true}` as a featured entry example.
- Replaced Albert Einstein demo entries in `_data/coauthors.yml` with the five group co-authors in `"Lastname, Firstname"` key format per the yaml-configuration instructions.
- Created `assets/img/papers/.gitkeep` directory for paper thumbnail images.
- Ran prettier on `_data/coauthors.yml` (unchanged).
- Committed as `content: populate publications from Zotero GOTERRIA collection` (commit `79e4059`).

## 2026-03-16 (Task 7: project links)

- Implemented Task 7: Added `links` list rendering to both `_includes/projects.liquid` and `_includes/projects_horizontal.liquid`.
- Inserted a `{% if project.links %}` block after the GitHub icon row in each partial; renders each entry as an `<a class="btn btn-sm z-depth-0">` button with `target="_blank"`.
- Created `assets/img/projects/` directory with `.gitkeep` for future project thumbnails.
- Created `_projects/example-project.md` as a temporary test fixture with two links (Paper, Code).
- Noted existing al-folio demo projects: `1_project.md` through `9_project.md` — left untouched (deletion is Task 10).
- Ran prettier on both modified partials — no formatting issues.
- Committed as `feat: add project links button support to project cards` (commit `7f60d22`).

## 2026-03-16 (quality fixes)

- Applied five quality fixes to the People page implementation (commit `b3634f7`).
- Fix 1: Added `@use "variables" as v;` to `_sass/_members.scss` to match the pattern used by all other SCSS partials.
- Fix 2: Replaced fixed `width: 200px` on `.member-card` with `flex: 1 1 200px`, `min-width: 150px`, `max-width: 220px` for responsive reflow on narrow viewports.
- Fix 3: Replaced Unicode `✉` glyph in `_pages/people.md` with `<i class="fas fa-envelope"></i>` for consistency with al-folio Font Awesome usage.
- Fix 4: Added Liquid `{%- comment -%}` blocks above both `where: "alumni"` filter lines documenting the boolean-vs-string pitfall.
- Fix 5: Moved `@use "members"` above the font-awesome imports in `assets/css/main.scss`, grouped with `cv`, `teachings`, `typograms`.
- Prettier left all three files unchanged (already well-formatted after edits).

## 2026-03-16

- Implemented Task 4: Created `_pages/people.md` with Liquid filtering of `site.members` collection, separating current members (`alumni: false`) and alumni (`alumni: true`) sections with member card grid layout.
- Implemented Task 5: Created `_members/` directory with five member files: `samuel-foucher.md`, `mickael-germain.md`, `jerome-theeau.md`, `yacine-bouroubi.md`, `etienne-clabaut.md`. All have `alumni: false` (boolean), `order` fields 1–5, and placeholder bios/emails.
- Created `_sass/_members.scss` with member card grid styles (`.members-grid`, `.member-card`, `.member-photo`, `.member-photo-placeholder`) and imported it in `assets/css/main.scss` via `@use "members"`.
- Created `assets/img/members/` directory with `.gitkeep` for future member photos.
- Ran prettier on all new files (all were already well-formatted, no changes).
- Made two separate commits: `feat: add People page with member card layout` and `content: add team member files for all five members`.
- Note: The `members` collection was already declared in `_config.yml` with `output: false` from a prior session.
