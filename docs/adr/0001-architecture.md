# ADR-0001: Architecture — Moldova market-entry compliance actor (`marketentry`)

**Status**: accepted
**Date**: 2026-07-23

## Context

`cloud-itonami-iso3166-mda` was published as a `:blueprint` (docs +
`blueprint.edn` only, plus a country-level `culture.facts` catalog in a
separate Wave 1 batch) but carried ZERO `src/marketentry` or
`src/statute` content -- its `:public-sector/market-entry-compliance`
domain, declared in `blueprint.edn`, was unimplemented. This ADR closes
that gap, following the harness pattern established by
`cloud-itonami-iso3166-jpn` (origin) and studied directly from three
Eastern-European/post-Soviet siblings that already carry the full
implementation: `cloud-itonami-iso3166-arm` (Armenia),
`cloud-itonami-iso3166-aze` (Azerbaijan) and `cloud-itonami-iso3166-kgz`
(Kyrgyzstan) -- all read in full (`src/marketentry/*.cljc`,
`src/statute/facts.cljk`, `test/`, `deps.edn`, README/CONTRIBUTING/
GOVERNANCE, docs) before writing anything here.

## Decision

Build the full governed-actor architecture for `marketentry`, mirroring
JPN/ARM/AZE/KGZ's harness verbatim (StateGraph node names, governor
hard/escalate contract, phase 0-3 rollout, `Store` protocol with
MemStore + DatomicStore parity) and researching Moldova's own real
market-entry rules from scratch for the country-specific content.

- **Store**: `marketentry.store`, MemStore + DatomicStore, proven parity
  via contract test.
- **Registry**: `marketentry.registry`, pure DRAFT-certificate
  construction via `unsigned-certificate`, jurisdiction-scoped sequence
  numbering (`MDA-DFT-000000`, `MDA-SUB-000000`). No additional numeric
  threshold function -- MDA's flagship check is a registry-membership
  boolean read, not a computed formula (see below).
- **Governor**: `:market-entry-compliance-governor` (family keyword from
  `blueprint.edn`).
- **Entity shape**: `engagement`, sequential draft -> submit on the same
  record. `high-stakes` = `#{:actuation/draft-filing
  :actuation/submit-filing}`.
- **Phase**: 0->3; `:filing/draft` and `:filing/submit` NEVER auto-
  commit at any phase.

### Flagship HARD check: `interdiction-list-listed`

Researching Moldova's own procurement regulator, Agenția Achiziții
Publice (`tender.gov.md`), surfaced a genuinely Moldovan mechanism: the
**Lista de interdicţie a operatorilor economici** (List of Interdiction
of Economic Operators). This iteration confirmed, by fetching the
agency's own pages directly (2026-07-23):

1. **The legal basis**: Legea nr. 131 din 3 iulie 2015 privind
   achizițiile publice (Law No. 131 of 3 July 2015 on Public
   Procurement) is the enabling law (confirmed via the agency's own
   `legea-privind-achizitiile-publice` page); the list itself is
   established and governed by **Hotărârea Guvernului nr. 1418 din
   28.12.2016** pentru aprobarea Regulamentul cu privire la modul de
   întocmire a Listei de interdicţie a operatorilor economici
   (Government Decision No. 1418 of 28.12.2016 approving the
   Regulation on compiling the list) -- this decision's number, date,
   and exact title were read directly from the raw HTML of the
   agency's own "Hotărâri de Guvern" legislation-index page, not an AI
   paraphrase.
2. **The list is live and currently populated**: fetching
   `tender.gov.md/ro/lista-de-interdictie` directly returned 20 real
   entries (e.g. SRL "MOSDRAG-SUCCES", SRL "Gameon Trade") with two
   concrete, directly-quoted inclusion grounds: "neexecutarea
   clauzelor contractuale" (non-execution of contractual clauses) and
   "prezentarea documentelor false în cadrul procedurilor de achiziţii
   publice" (submission of false documents in procurement procedures).

`interdiction-list-listed` independently re-verifies the engagement's
own declared `:on-interdiction-list?` flag rather than trusting it --
evaluated UNCONDITIONALLY for every `:filing/submit` (not gated behind
a `:requires-X?` engagement flag), because list membership is itself a
categorical bar to further procurement participation. This is
structurally the same boolean-registry-membership SHAPE several
sibling actors' own flagship checks use (e.g. ARM's
`ineligible-bidder-listed`, AZE's `unreliable-supplier-listed`) --
disclosed honestly here, as this fleet's convention requires: the
SHAPE resembles siblings' because of similar drafting heritage (a
public-procurement exclusion register is a common regulatory pattern
across jurisdictions), but the CITATION is independently researched
and grounds a real, current, Moldova-specific mechanism, not copied
from any sibling. A GitHub code search across the `cloud-itonami` org
for the rule keyword `interdiction-list-listed` / `economic-operator-
interdiction` before writing this code returned zero hits.

### What this iteration could NOT confirm (honest gaps)

- **Full text of HG 1418/2016**: its official host is `legis.md`,
  Moldova's primary legal database, which returned a Cloudflare "Just
  a moment..." bot-detection challenge on every direct attempt AND on
  every Wayback Machine snapshot tried (the archived pages only
  captured the page's client-side-rendered loading shell, never the
  AJAX-fetched article text). Per this fleet's hard rule, that
  challenge was NOT bypassed. The exact exclusion-period/appeal-
  procedure detail is therefore NOT claimed in `marketentry.facts` --
  only the decision's number, date, title, and the list's own live,
  current operation.
- **Representative-mandate provision**: whether Legea nr. 131/2015
  mandates a resident/domestic representative for public-tender
  participation could not be independently confirmed (again, the
  relevant article text lives on the Cloudflare-blocked `legis.md`, and
  no reachable secondary source quoted it). `marketentry.facts/rep-
  spec-basis` returns `nil` for MDA -- an honest gap, not a fabricated
  requirement.
- **Company law (LLC/JSC)**: Agenția Servicii Publice's own
  registration-service pages, the Ministry of Justice's own site, and
  the Ministry of Finance's own site were all fetched directly and
  none named the LLC law (Legea nr. 135/2007 or similar) or joint-
  stock company law's exact number/date in content this iteration
  could read. `statute.facts/catalog` for MDA therefore has 3 entries
  (Labour Code, Fiscal Code, Investment Law), not 4 -- the same
  honest-narrower-than-hoped-for coverage pattern ARM's own iteration
  documented for its own gaps.
- **Investment law currency**: Invest Moldova's own June-2026
  Investor's Guide states Legea nr. 81/2004 cu privire la investiţiile
  în activitatea de întreprinzător "urmează a fi înlocuită odată cu
  adoptarea noilor prevederi legale privind investiţiile" (is about to
  be REPLACED upon adoption of new investment-law provisions) --
  reported in `statute.facts` with a `:pending-replacement` topic tag,
  not as simple, settled current law.

### Other HARD checks (all unoverridable)

1. **spec-basis** -- never invent a jurisdiction's market-entry
   requirements (`marketentry.facts` G2 catalog: MTender, Agenția
   Servicii Publice, IDNO for MDA).
2. **evidence-incomplete** -- draft/submit require a full assessment
   checklist on file.
3. **interdiction-list-listed** -- see above (FLAGSHIP).
4. **engagement-fee-mismatch** -- recompute `base-fee + monthly-rate ×
   monitoring-months` (ground-truth-recompute discipline).
5. **idno-unverified** -- conditional on `:requires-idno?` (IDNO,
   issued by Agenția Servicii Publice AT THE SAME MOMENT as state
   registration, also serving as the fiscal code -- see
   `marketentry.facts` for the one-act-vs-two-act investigation).
6. **already-drafted / already-submitted** -- dedicated booleans, never
   a `:status` value.

### `statute.facts` (second, orthogonal catalog)

Three Moldovan statutes, each confirmed via a DIFFERENT official
government body's own directly-readable page (since `legis.md` itself
was unreachable, see above): the Labour Code (via the State Labour
Inspectorate's own `ism.gov.md` legislation page: "Codul muncii, nr.
154-XV din 28.03.2003"), the Fiscal Code (via the Ministry of
Finance's own `mf.gov.md`, which links to it as WebLex id
`LPLP199704241163`, decoding to law number 1163 adopted 24.04.1997 --
this iteration deliberately did NOT append the conventional "-XIII"
legislature-suffix seen in some secondary sources, since that detail
specifically was not independently re-confirmed this session), and the
Law on Investments in Entrepreneurial Activity (via Invest Moldova's
own June-2026 Investor's Guide PDF, with the pending-replacement
caveat above).

## Consequences

- `src/` now genuinely exists with real, tested, curl/WebFetch-cited
  content for this blueprint's declared domain (`:public-sector/
  market-entry-compliance`) -- moves this repo's
  `manifest/itonami-fleet-audit.edn` `:prod-ready?` signal from `:stub`
  to `:active`.
- The existing `culture.facts` catalog (Wave 1, unrelated batch) is
  untouched.
- The three honest gaps above (HG 1418/2016 full text, representative-
  mandate provision, company law citation) are genuine, disclosed,
  NOT-fabricated extension points for a future iteration -- only to be
  filled once `legis.md` is reachable or another primary-source mirror
  is found, never by filling in from memory.
- Sibling country blueprints can continue forking JPN/ARM/AZE/KGZ/MDA
  and swapping in their own genuinely-researched `marketentry.facts` /
  `statute.facts` content and whichever flagship check their own law
  actually supports -- this ADR is itself evidence that the flagship
  check should be chosen from real, currency-checked research, not
  copied by rote, and that finding a provision is not the same as
  confirming it is still law.
