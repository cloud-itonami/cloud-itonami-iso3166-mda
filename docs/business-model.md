# Business Model: Independent Public-Sector Market-Entry & Procurement Compliance Service — Republic of Moldova

## Classification

- Repository: `cloud-itonami-iso3166-mda`
- ISO 3166: `MDA` (Moldova)
- Activity: public-procurement market-entry and ongoing regulatory-
  compliance navigation for an already-incorporated operator
- Social impact: [:sme-market-access :public-spend-transparency :cross-border-friction-reduction]

## Customer

- an already-incorporated `cloud-itonami-cofog-{code}` /
  `cloud-itonami-isco-{code}` / `cloud-itonami-unspsc-{segment}` /
  `cloud-itonami-{ISIC}` operator wanting to bid on a Moldovan public
  contract
- a foreign SME or civic-tech vendor entering the public sector in
  Moldova for the first time
- a `cloud-itonami-M6910` client that has just completed incorporation
  and now needs public-sector market access

## Offer

- registration walkthrough for MTender (the electronic public-
  procurement system, mandatory under Legea nr. 131/2015 privind
  achizițiile publice), operated under the Ministerul Finanțelor al
  Republicii Moldova (Ministry of Finance of the Republic of Moldova)
- business/tax registration checklist: Agenția Servicii Publice
  registration, which issues the IDNO (Numărul de identificare de
  stat) in the SAME act as registration -- and which also serves as
  the fiscal code
- interdiction-list screening: independent verification that an
  operator is not on Agenția Achiziții Publice's Lista de interdicţie
  a operatorilor economici (List of Interdiction of Economic
  Operators, Legea nr. 131/2015 + Hotărârea Guvernului nr. 1418 din
  28.12.2016)
- ongoing regulatory-change monitoring subscription
- compliance-audit export package for the client's own records

## Revenue

- per-engagement market-entry fee (one-time registration + checklist
  completion)
- recurring regulatory-change monitoring subscription
- compliance-audit export package

## Trust Controls

- any actual portal registration or filing submission requires
  Market-Entry Compliance Governor clearance and always escalates to
  human sign-off (`:filing/submit` is never automated at any phase)
- a false or fabricated regulatory-requirement claim is a HARD hold that
  cannot be overridden by human approval alone -- it must be corrected
  against a cited official source first
- membership on the Lista de interdicţie a operatorilor economici is a
  HARD hold on `:filing/submit`, independently re-verified rather than
  trusted from a claimed flag -- this is an unconditional, categorical
  bar under Moldovan public-procurement law, not gated behind any other
  engagement field
- this service does **not** provide legal or tax advice; characterization
  and filing on the client's behalf beyond checklist/draft assistance
  routes to Moldovan-licensed counsel or a registered agent

## Boundary with adjacent actors (read before forking)

- **`cloud-itonami-M6910`**: helps a client BECOME a legal entity
  (incorporation, ISIC 6910) -- a prior, different regulatory phase
  (company law). This blueprint assumes incorporation is already done and
  handles public-procurement market entry (a different regulatory domain).
- **`cloud-itonami-cofog-{code}`**: a jurisdiction-agnostic operator
  template for ONE public function. This blueprint is the orthogonal
  jurisdiction-specific axis -- the two compose (fork a COFOG-function
  blueprint AND this one to operate in Moldova).
