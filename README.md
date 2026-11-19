# OrgDef Orgchart Viewer

A single-file web viewer that renders orgdef organizations as interactive org charts. It is a worked example for the catdef / Open Governance Framework ecosystem: point it at a set of published OrgDef files and it draws the organization — seats, reporting lines, and cross-links — and lets you inspect every seat's charter.

## Live

The viewer runs the Confused Gorilla published library:

- https://confusedgorilla.com/organization/
- https://labs.confusedgorilla.com/organization/

The five organizations included in this repo as example data are the same files those pages serve.

## How it works

`index.html` is the whole application — the chart library (Mermaid) is inlined, and the page makes no external requests. At load it fetches, relative to its own directory:

- `orgdef.json` — the primary organization
- `orgs/<slug>.json` — each additional organization

It builds a top-down tree from each file's `orgdef:Position` items and `relationships` array:

- `reports_to` and `directs` become solid arrows, parents on top.
- `peer_of`, `validates_for`, `implements_for`, `coordinates_with`, `derives_from`, and `drafts_for` become dashed, labelled arrows.
- A seat with no incumbent renders with a gold outline (vacant); a staffed seat renders blue.
- Clicking a seat opens its panel: status, incumbent, description, authority, charter, the matching roledef's sections, and its place in the structure.

`?org=<slug>` in the URL preselects an organization.

## Use it with your own organization

1. Serve this directory from any static host (the page needs http(s) to fetch its JSON).
2. Replace `orgdef.json` and `orgs/*.json` with your own OrgDef files.
3. Adjust the `SWITCHER` and `FILE_BY_KEY` blocks at the top of the page's script to name your organizations and their files.

## Provenance

The chart-generation approach — transforming an OrgDef's positions and relationships into a Mermaid top-down graph with state-coloured seats — follows the chart builder in OpenBraid (https://github.com/openbraid-org/openbraid), reimplemented here as a static page with no server.

## License

MIT — see [LICENSE](LICENSE).
