# US peptide and NAD+ prices (PeptideCompare.co)

What US online clinics charge for prescribed peptides and NAD+, read from each clinic's own page, dated, and put on one basis: the cost per 30 days. About 460 clinics and 2,400 prices across 25 compounds (NAD+, BPC-157, sermorelin, GHK-Cu, tesamorelin, CJC-1295, ipamorelin, TB-500, MOTS-c, PT-141, glutathione, semax, selank, epitalon, KPV and more) and 5 peptide stacks (Wolverine, GLOW, KLOW and others), with where each compound stands with the FDA.

[PeptideCompare.co](https://peptidecompare.co) keeps these figures current: a daily check confirms each price is still on its clinic's page, and a price that has not been confirmed there for 21 days leaves the lists. This repository carries the open data files and documents the ways to use the live figures.

- **Website:** [peptidecompare.co](https://peptidecompare.co), with one price page per compound, such as [NAD+](https://peptidecompare.co/prices/nad-plus), [sermorelin](https://peptidecompare.co/prices/sermorelin), [BPC-157](https://peptidecompare.co/prices/bpc-157), [GHK-Cu](https://peptidecompare.co/prices/ghk-cu) and [tesamorelin](https://peptidecompare.co/prices/tesamorelin)
- **MCP server:** `https://peptidecompare.co/mcp` (remote, Streamable HTTP, no key), in the Official MCP Registry as `co.peptidecompare/peptide-prices`
- **JSON API:** `https://peptidecompare.co/api/v1/...` ([OpenAPI](https://peptidecompare.co/openapi.json), no key)
- **Price box for your page:** one self-updating list per compound ([codes for every compound](https://peptidecompare.co/price-box))
- **Data files:** the CSVs in [`data/`](data), CC BY 4.0, refreshed weekly; a dated version to cite is on Zenodo, DOI [10.5281/zenodo.24281286](https://doi.org/10.5281/zenodo.24281286)

## The data files

| File | One row per | What it holds |
|------|-------------|---------------|
| [`data/prices.csv`](data/prices.csv) | current price | The clinic, the product, the price in the clinic's own words (`price_as_stated`), what it comes to per 30 days (`usd_per_30_days`) and how that was worked out (`how_compared`), the form, plan length, members-only and compounded flags, any fee charged beside the price, whether an HSA or FSA card works, the day it was read, and the pages on PeptideCompare.co that show it |
| [`data/compounds.csv`](data/compounds.csv) | compound or stack | The lowest price per 30 days and where, the typical one (the middle of each clinic's lowest), how many clinics price it, and its FDA standing in a few words |
| [`data/fda-status.csv`](data/fda-status.csv) | compound | FDA-approved products, its 503A compounding standing in a few words and in the FDA's own, the FDA page and the day it was read |
| [`data/research.csv`](data/research.csv) | compound | PubMed counts (papers, animal-only papers, clinical and randomized trials, systematic reviews), ClinicalTrials.gov studies that give it and the conditions they list, reports to the FDA naming it, FDA recalls and warning letters, each with its search and day. Counts only, never a finding that anything works |

## Ask your assistant

| Tool | What it answers | Try asking |
|------|-----------------|------------|
| `find_peptide_prices` | One compound's or stack's prices, cheapest per 30 days first, optionally one form or one US state | "What does sermorelin cost per month at US online clinics?" |
| `list_compounds` | Every priced compound and stack with its lowest and typical cost | "Which peptides can I get prescribed online, and what do they cost?" |
| `get_fda_status` | FDA approval, 503A compounding standing, research counts, reports, recalls and warning letters | "Is BPC-157 FDA approved?" |
| `get_clinic` | One clinic's current prices, its public record and its current codes | "What does Everlife charge for NAD+?" |
| `find_peptide_deals` | Promo codes and sales clinics state on their own pages now | "Are there Black Friday deals on NAD+ injections?" |

Every tool is read-only and needs no account. Each answer links the page on PeptideCompare.co that shows the price beside the clinic's own words and the day it was read.

**Claude (web and desktop):** Settings, Connectors, Add custom connector, URL `https://peptidecompare.co/mcp`, no authentication.

**Claude Code:**

```bash
claude mcp add --transport http peptidecompare https://peptidecompare.co/mcp
```

**Cursor, Windsurf, Cline, VS Code and other MCP clients:**

```json
{ "mcpServers": { "peptidecompare": { "url": "https://peptidecompare.co/mcp" } } }
```

## JSON API

| Endpoint | Returns |
|----------|---------|
| `GET /api/v1/prices?compound=nad-plus` | The cheapest prices of one compound or stack; `form`, `state` (two letters) and `limit` narrow it |
| `GET /api/v1/compounds` | Every priced compound and stack |
| `GET /api/v1/fda-status?compound=bpc-157` | One compound's FDA standing |
| `GET /api/v1/clinics?clinic=everlife` | One clinic's prices and record |
| `GET /api/v1/deals?compound=sermorelin` | Current codes and sales, optionally for one compound |

```bash
curl "https://peptidecompare.co/api/v1/prices?compound=sermorelin&limit=5"
```

## Show the prices on your page

A calculator, a compound reference or a clinic finder can show a compound's lowest current prices with one paste. The box updates itself, runs no script and sets no cookie, and the line under it credits the source:

```html
<iframe src="https://peptidecompare.co/embed/nad-plus" title="NAD+ prices" width="100%" height="330" style="border:0;max-width:560px" loading="lazy"></iframe>
<p style="margin:4px 0 0;font-size:12px">NAD+ prices from <a href="https://peptidecompare.co/prices/nad-plus">PeptideCompare.co</a></p>
```

Every compound's code, with its height worked out for you: [peptidecompare.co/price-box](https://peptidecompare.co/price-box).

## How the prices are read

- Only US clinics that sell after a clinician's review, filled by a licensed pharmacy. Products sold "for research use only", GLP-1 drugs, hormones and blends with another drug are left out.
- A price stated for a supply of days is brought to 30 days; a monthly price with no supply stated is counted as four weeks and says so. A first-month or members-only price is marked and never becomes a clinic's listed price while a regular one exists. Fees charged beside a price (a visit, a membership, shipping) are shown with it.
- Most compounded peptides are not FDA-approved, and several are under FDA review for compounding. Each compound's standing is quoted from the FDA's own pages on [its page](https://peptidecompare.co/compounds). Nothing here is medical advice: these are prescription treatments a clinician decides on.
- Some clinics pay PeptideCompare.co a referral fee when a reader buys through its pages. It never changes a price, and lists sorted by price read the price alone. The full method: [peptidecompare.co/methodology](https://peptidecompare.co/methodology).

## Licence

The data is CC BY 4.0 ([LICENSE](LICENSE)). Credit "PeptideCompare.co" and link [peptidecompare.co](https://peptidecompare.co) or the page a figure comes from. Prices change, so check the live page before relying on one.

## About

PeptideCompare.co is operated by O-mega Enterprise, Inc. Questions: hello@peptidecompare.co. The index itself runs at peptidecompare.co; this repository is its open data and documentation.
