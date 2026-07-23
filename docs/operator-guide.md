# Operator Guide

## First Deployment

1. Confirm the client's incorporation/legal-entity status is complete
   (route to `cloud-itonami-M6910` or local counsel first if not).
2. Register the client's intake: business type, target public function,
   prior filing history in Moldova if any.
3. Run the advisor in read-only mode against MTender
   (`https://mtender.gov.md/` / `https://achizitii.md/`), governed by
   Legea nr. 131 din 3 iulie 2015 privind achizițiile publice (Law
   No. 131 of 3 July 2015 on Public Procurement).
4. Compare the checklist against the client's current documentation
   (State Register extract from Agenția Servicii Publice, IDNO record,
   MTender supplier/participant-profile registration).
5. Enable gated filing-draft assistance once the Market-Entry Compliance
   Governor contract is trusted; actual submission always requires human
   sign-off.

## Minimum Production Controls

- client-owned data store for business/tax registration documents
- clear provenance (official portal/regulation citation) for every
  requirement surfaced
- approval workflow for any portal registration or filing submission
- independent re-verification that the client is not on Agenția
  Achiziții Publice's Lista de interdicţie a operatorilor economici
  (List of Interdiction of Economic Operators, Legea nr. 131/2015 +
  Hotărârea Guvernului nr. 1418 din 28.12.2016) before any
  `:filing/submit` -- never trust a claimed flag
- named referral relationship with Moldovan-licensed counsel or a
  registered agent for anything beyond checklist/draft assistance
- monthly audit export
- disputes/appeals route to the procedure Legea nr. 131/2015 itself
  defines, not this actor -- it has no standing to file an appeal on
  the client's behalf

## Certification

Certified operators must prove data provenance, audit traceability, that
automated actions cannot bypass the Market-Entry Compliance Governor, and
a working referral relationship with Moldovan-licensed counsel or a
registered agent for whatever licensed representation the law of
Moldova requires for actual public-procurement filings.
