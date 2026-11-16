# sg-mas-trmg-oscal

Machine-readable OSCAL 1.1.2 catalog for the **MAS Technology Risk Management
Guidelines** (Monetary Authority of Singapore, 18 January 2021).

> **Unofficial.** Source content remains © Crown Copyright, Government of
> Singapore. This repository is a community conversion published for
> interoperability with [OSCAL](https://pages.nist.gov/OSCAL/) tooling. The
> upstream PDF is the authoritative text. See [`NOTICE`](NOTICE).

## Contents

| File | Purpose |
|---|---|
| `catalog.json` | OSCAL 1.1.2 catalog (canonical) |
| `catalog.yaml` | Same catalog, YAML serialisation |
| `catalog.md`   | Human-readable rendering |
| `profile-full.json` | OSCAL 1.1.2 profile importing every control (`include-all`) |
| `resolved/resolved-profile-full.json` | Flattened catalog from profile resolution (present only when the publisher ran `oscal-cli profile resolve`) |
| `provenance.json` | Builder commit SHA, source URL, PDF SHA256, build time |
| `CHANGELOG.md` | Per-release change log |
| `NOTICE` | Copyright + attribution |

TRMG defines no maturity tiers or applicability classes, so only the full
profile is published. Consumers who need a scoped baseline should author
their own profile that imports `catalog.json` (see example below).

## Stats

- 15 chapters
- 61 sections
- 251 controls (= TRMG numbered paragraphs)
- 42 footnotes (back-matter resources, inline-linked from prose)
- 3 annexes (back-matter resources)

## Validation

Catalog and profile both validate clean against:
- OSCAL 1.1.2 JSON Schema
- [`oscal-cli`](https://github.com/metaschema-framework/oscal-cli) 3.2.0
  (metaschema + Schematron)

When `resolved/` is present, the resolved catalog is validated the same way.

## Identifiers

- Deterministic UUIDv5 under namespace `trmg.mas.gov.sg`. Re-generating from
  the same inputs produces identical UUIDs — stable across builds for diff and
  reference purposes.
- Control IDs mirror TRMG numbering: `trmg-3.1.2`, `trmg-11.2.4`, etc.
- Group IDs: `trmg-g-3`, `trmg-g-3.1`, etc.
- Footnote resource UUIDs: `UUIDv5(ns, "footnote:N")` — the inline markdown
  link anchor in prose equals the back-matter resource UUID.

## Source

- Document: [MAS TRMG, 18 January 2021](https://www.mas.gov.sg/-/media/mas/regulations-and-financial-stability/regulatory-and-supervisory-framework/risk-management/trm-guidelines-18-january-2021.pdf)
- Publisher: Monetary Authority of Singapore
- Copyright: © Crown Copyright, Government of Singapore
- Published: 2021-01-18

## Converter

- Author: Baden Hughes
- License: Apache-2.0 (conversion artifacts in this repository only)
- Builder: private repository

Each release's `provenance.json` records the exact builder commit used to
produce the artifacts. Builds are deterministic: the same builder commit and
the same source PDF SHA256 produce byte-for-byte identical OSCAL output.

## Using the catalog

```python
import json, httpx
cat = httpx.get(
    "https://raw.githubusercontent.com/badenh/sg-mas-trmg-oscal/main/catalog.json"
).json()["catalog"]
print(cat["metadata"]["title"], cat["metadata"]["version"])
# Technology Risk Management Guidelines 2021-01-18
```

Or via an OSCAL profile that imports this catalog by URL:

```yaml
profile:
  imports:
    - href: "https://raw.githubusercontent.com/badenh/sg-mas-trmg-oscal/main/catalog.json"
      include-all: {}
```

## Reporting issues

Conversion bugs (misparsed text, wrong IDs, missing content): open an issue
here.

Questions about the source text itself should go to the Monetary Authority of
Singapore.
