# cloud-itonami-iso3166-mda

Open ISO 3166 Blueprint for **MDA**: Republic of Moldova --
**`:implemented`**.

This repository designs **and implements** a forkable OSS business for
an independent public-sector market-entry consultant: an already-
incorporated operator (e.g. a `cloud-itonami-cofog-{code}`,
`cloud-itonami-isco-{code}`, `cloud-itonami-unspsc-{segment}` or
`cloud-itonami-{ISIC}` blueprint fork) gets a Compliance Advisor +
independent **Market-Entry Compliance Governor** to navigate public-
procurement registration, local business/tax registration, and
regulatory-compliance rules in Moldova, so the operator can win and
service a government contract without hiring a full in-house compliance
department.

## Official surface (curl/WebFetch-verified 2026-07-23)

- Procurement: MTender (`https://mtender.gov.md/` / `https://achizitii.md/`),
  the electronic public-procurement system the Ministerul Finanțelor al
  Republicii Moldova (Ministry of Finance of the Republic of Moldova)
  operates, confirmed live and self-describing directly ("Ministerul
  Finanțelor al Republicii Moldova conduce o tranziție la achizițiile
  publice electronice"); achizitii.md itself is the real operating
  platform (confirmed via live, currently-open tender listings and
  organization/IDNO records fetched directly from it). Legal basis:
  Legea nr. 131 din 3 iulie 2015 privind achizițiile publice (Law
  No. 131 of 3 July 2015 on Public Procurement), confirmed directly
  from Agenția Achiziții Publice's (Public Procurement Agency) own
  official legislation page.
- Business/tax identity: Agenția Servicii Publice (Public Services
  Agency, which absorbed the former Camera Înregistrării de Stat /
  State Registration Chamber). This repo specifically investigated,
  rather than assumed, whether registration and tax-identifier
  issuance are one act or two: Invest Moldova's own Investor's Guide
  (June 2026) states the state-registration procedure assigns the IDNO
  (Numărul de identificare de stat) AT THE MOMENT of registration, and
  it ALSO serves as the fiscal code -- a SINGLE act performed by
  Agenția Servicii Publice, not a separate subsequent act by Moldova's
  tax authority.
- Dispute/exclusion mechanism: Legea nr. 131/2015 + Hotărârea
  Guvernului nr. 1418 din 28.12.2016 establish the Lista de interdicţie
  a operatorilor economici (List of Interdiction of Economic
  Operators), published live by Agenția Achiziții Publice and
  confirmed CURRENTLY POPULATED (20 real entries) when fetched directly
  on 2026-07-23.

Moldova's primary legal database, `legis.md`, returned a Cloudflare
"Just a moment..." bot-detection challenge on every attempt (both live
and via Wayback Machine snapshots, which only captured the page's JS
loading shell) -- per this fleet's hard rule against bypassing bot
detection, it was NOT bypassed. Every citation in this repo instead
comes from a DIFFERENT official government body's own directly-
readable page or document -- see `src/marketentry/facts.cljk` and
`src/statute/facts.cljk` for the full per-citation discussion,
including the honest gaps this iteration could NOT independently
confirm (a representative-mandate provision, and the company/LLC-law
citation).

## Implementation (R0)

| Piece | Location |
|---|---|
| Actor namespaces | `src/marketentry/*` |
| Governor | `:market-entry-compliance-governor` |
| Ops | `:engagement/intake` · `:jurisdiction/assess` · `:filing/draft` · `:filing/submit` |
| Flagship HARD check | `interdiction-list-listed` (Legea nr. 131/2015 + Hotărârea Guvernului nr. 1418 din 28.12.2016, an unconditional registry-membership check independently re-verified -- see `docs/adr/0001-architecture.md`) |
| Compliance catalog | `src/statute/facts.cljk` -- Labour Code, Fiscal Code, Law on Investments in Entrepreneurial Activity |
| Tests | `kbb -M:dev:test` |
| Demo | `kbb -M:dev:run` |
| Architecture ADR | [`docs/adr/0001-architecture.md`](docs/adr/0001-architecture.md) |

`:filing/submit` is never in any phase's `:auto` set -- human sign-off
is structural, not a rollout milestone.

## No robotics premise -- digital/data service exemption

Market-entry and procurement-compliance navigation is a pure data/software
service with no physical-domain work (portal registration, document
checklists, regulatory-change monitoring) -- the same exemption class as
`cloud-itonami-6310` (HR SaaS replacement) and `cloud-itonami-gtin-*`.
`blueprint.edn` sets `:itonami.blueprint/robotics false` and
`:required-technologies` lists only real capabilities (`:identity`,
`:forms`, `:dmn`, `:bpmn`, `:audit-ledger`), no `:robotics`.

## Core Contract

```text
operator intake + prior filing history
        |
        v
Compliance Advisor -> Market-Entry Compliance Governor -> filing draft, or human sign-off
        |
        v
gated portal registration / filing submission + audit ledger
```

No automated proposal can submit a portal registration or filing the
governor refuses, suppress a compliance record, or claim a legal/tax
conclusion the governor has not cleared. `:filing/submit` is never in any
phase's `:auto` set -- it always requires human sign-off.

## What this is NOT

- **Not the government of Moldova.** This blueprint is an independent
  operator the government contracts with or that bids into its
  procurement -- never the government itself, and never an official
  channel.
- **Not legal or tax advice.** Every regulatory claim must cite the
  official source and route final filings to Moldovan-licensed counsel
  or a registered agent where the law requires licensed representation.

## Capability layer

Required capabilities (`blueprint.edn`):

- :identity
- :forms
- :dmn
- :bpmn
- :audit-ledger

See [`docs/business-model.md`](docs/business-model.md) and
[`docs/operator-guide.md`](docs/operator-guide.md).

## License

AGPL-3.0-or-later.

## Culture catalog

Alongside the market-entry / statute catalogs, this repo carries a
**country-level regional-culture catalog** (ADR-2607171400 addendum 2,
`cloud-itonami-municipality-culture-catalog` Wave 1, in
`com-junkawasaki/root`) — national dishes, protected products, beverages,
crafts, festivals and heritage sites for Moldova:

- `src/culture/facts.cljk` — the catalog, source of truth (keyed by
  uppercase ISO3, mirroring `statute.facts`).
- `schema/culture.edn` — DataScript schema.
- `data/culture-tx.edn` — derived DataScript tx-data (regenerated from
  the catalog, never hand-edited).

City-level counterparts live in the `cloud-itonami-municipality-*` repos.
Same provenance discipline as the compliance catalogs: every entry cites a
source URL that was actually fetched and read on `:culture/retrieved-at`;
summaries state only what the cited source confirms. An item not in
`culture.facts/catalog` has no spec-basis — never fabricate one.
