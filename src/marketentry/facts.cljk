(ns marketentry.facts
  "Per-jurisdiction public-procurement market-entry regulatory catalog
  -- the G2-style spec-basis table the Market-Entry Compliance Governor
  checks every `:jurisdiction/assess` proposal against ('did the advisor
  cite an OFFICIAL source for this jurisdiction's requirements, or did
  it invent one?').

  Moldova's real market-entry surface (curl/WebFetch-verified
  2026-07-23, see each entry's own citation): MTender
  (mtender.gov.md / achizitii.md), the electronic public-procurement
  system the Ministry of Finance of the Republic of Moldova operates
  (confirmed directly: mtender.gov.md's own self-description --
  \"Ministerul Finanțelor al Republicii Moldova conduce o tranziție la
  achizițiile publice electronice\"; achizitii.md itself is the live
  operational platform, confirmed by fetching real, currently-open
  tender listings and live organization/IDNO records directly from it).
  The legal basis is Legea nr. 131 din 3 iulie 2015 privind achizițiile
  publice (Law No. 131 of 3 July 2015 on Public Procurement) --
  confirmed directly from the Agenția Achiziții Publice's (Public
  Procurement Agency, the sectoral regulator) own official page
  `tender.gov.md/ro/content/legea-privind-achizitiile-publice`, which
  names the exact law number and adoption date. Moldova's primary legal
  database, legis.md, returned a Cloudflare \"Just a moment...\"
  bot-detection challenge on every attempt (both live and via Wayback
  Machine snapshots, which only captured the page's JS loading shell,
  never the AJAX-fetched article text) -- per this fleet's hard rule,
  that challenge was NOT bypassed. Every citation below instead comes
  from an official government body's OWN page that WAS directly
  readable (Agenția Achiziții Publice, Agenția Servicii Publice,
  Invest Moldova, the Ministry of Finance, the State Labour
  Inspectorate), not from legis.md.

  Business/tax identity: this iteration specifically investigated,
  rather than assumed, whether business registration and tax-identifier
  issuance are ONE act or TWO separate ones (the same question ARM/AZE/
  BGR/ALB's own iterations investigated for their jurisdictions).
  Invest Moldova's own \"Ghidul Investitorului\" (Investor's Guide, PDF
  dated June 2026, invest.gov.md -- WebFetch rendered only the page
  shell for this PDF, so curl + `pdftotext` were used to extract and
  directly read it) states the state-registration procedure's own
  step 6 in these words: \"Companiei i se atribuie, la înregistrare, un
  număr de identificare de stat (IDNO), care reprezintă și cod fiscal\"
  (\"The company is assigned, upon registration, a state identification
  number (IDNO), which ALSO serves as the fiscal code\") -- a SINGLE
  act performed by Agenția Servicii Publice (the Public Services
  Agency, which absorbed the former Camera Înregistrării de Stat /
  State Registration Chamber), not a separate subsequent act by
  Moldova's tax authority. This catalog cites Agenția Servicii Publice
  as `:corporate-number-owner-authority` on the strength of that
  directly-read primary-source sentence, not an assumption.

  `ineligible-bidders-list-spec-basis` grounds this vertical's flagship
  governor check (see `marketentry.governor`'s own docstring for the
  full discussion). Grounded in TWO independently-confirmed, directly-
  fetched sources: (1) Agenția Achiziții Publice's own \"Hotărâri de
  Guvern\" legislation-index page (tender.gov.md), which names
  \"Hotărârea Guvernului nr. 1418 din 28.12.2016 pentru aprobarea
  Regulamentul cu privire la modul de întocmire a Listei de interdicţie
  a operatorilor economici\" (Government Decision No. 1418 of
  28.12.2016 approving the Regulation on compiling the List of
  Interdiction of Economic Operators) -- directly read from the raw
  HTML of the agency's own page, not an AI paraphrase; and (2) the
  live published list itself (`tender.gov.md/ro/lista-de-interdictie`),
  fetched directly and found CURRENTLY POPULATED with 20 real entries
  (e.g. SRL \"MOSDRAG-SUCCES\", SRL \"Gameon Trade\") and two concrete,
  directly-quoted inclusion grounds: \"neexecutarea clauzelor
  contractuale\" (non-execution of contractual clauses) and
  \"prezentarea documentelor false în cadrul procedurilor de achiziţii
  publice\" (submission of false documents in public-procurement
  procedures). This iteration could NOT retrieve the full text of HG
  1418/2016 itself (its own official host is legis.md, Cloudflare-
  blocked as above; the Wayback Machine's archived snapshots of that
  page only captured the same unrendered \"Conținutul se încarcă...\"
  (\"Content is loading...\") JS shell) -- the exclusion-period/appeal
  procedure detail is therefore NOT claimed here, only the decision's
  number, date, title, and the regulator/list's own live, current
  operation, all independently confirmed.

  Representative-related requirement (`rep-spec-basis`): this
  iteration could NOT independently confirm whether Law No. 131/2015
  mandates a resident/domestic representative for public-tender
  participation (the specific article text is on legis.md, unreachable
  as above, and no secondary source this iteration reached quoted the
  relevant article). Rather than guess, `rep-spec-basis` returns nil
  for MDA -- an honest gap, not a fabricated requirement.

  Coverage is reported HONESTLY (see `coverage`): a jurisdiction not in
  this table has NO spec-basis, full stop -- the advisor must not
  fabricate one, and the governor holds if it tries.")

(def catalog
  "iso3 -> requirement map. `:required-evidence` mirrors the generic
  intake/portal-registration/filing evidence set; `:legal-basis` /
  `:owner-authority` / `:provenance` are the G2 citation the governor
  requires before any `:jurisdiction/assess` proposal can commit.
  `:rep-owner-authority` / `:rep-legal-basis` / `:rep-provenance` are the
  SEPARATE representative-related citation `facts/rep-spec-basis`
  exposes -- absent for MDA, see the `catalog` docstring's honest-gap
  note. `:ineligible-bidders-list-owner-authority` /
  `:ineligible-bidders-list-legal-basis` /
  `:ineligible-bidders-list-provenance` ground this vertical's flagship
  governor check (`ineligible-bidders-list-spec-basis`)."
  {"MDA" {:name "Moldova"
          :owner-authority "Agenția Achiziții Publice (Public Procurement Agency, under the Ministerul Finanțelor al Republicii Moldova / Ministry of Finance of the Republic of Moldova) / MTender (unified electronic public-procurement system)"
          :legal-basis "Legea nr. 131 din 3 iulie 2015 privind achizițiile publice (Law No. 131 of 3 July 2015 on Public Procurement) -- confirmed directly via Agenția Achiziții Publice's own official legislation page"
          :national-spec "MTender (mtender.gov.md / achizitii.md) supplier/participant profile registration and electronic tender participation"
          :provenance "https://tender.gov.md/ro/content/legea-privind-achizi%C8%9Biile-publice"
          :required-evidence ["Extras din Registrul de stat al persoanelor juridice și al întreprinzătorilor individuali (State Register extract, Agenția Servicii Publice)"
                              "IDNO (Numărul de identificare de stat), atribuit la înregistrare și utilizat și ca cod fiscal (state identification number, assigned at registration, also serving as fiscal code)"
                              "Înregistrare profil furnizor/participant MTender (MTender supplier/participant-profile registration record)"
                              "Confirmare că operatorul economic nu figurează pe Lista de interdicție a operatorilor economici (confirmation the economic operator is not on the List of Interdiction of Economic Operators)"]
          :rep-owner-authority nil
          :rep-legal-basis nil
          :rep-provenance nil
          :ineligible-bidders-list-owner-authority "Agenția Achiziții Publice (publishes and maintains the list) per Hotărârea Guvernului nr. 1418 din 28.12.2016"
          :ineligible-bidders-list-legal-basis "Legea nr. 131/2015 privind achizițiile publice + Hotărârea Guvernului nr. 1418 din 28.12.2016 pentru aprobarea Regulamentul cu privire la modul de întocmire a Listei de interdicţie a operatorilor economici (Government Decision No. 1418 of 28.12.2016 approving the Regulation on compiling the List of Interdiction of Economic Operators) -- decision number/date/title directly confirmed from Agenția Achiziții Publice's own legislation-index page; the live list itself (fetched 2026-07-23) is currently populated (20 entries) with directly-quoted inclusion grounds 'neexecutarea clauzelor contractuale' (non-execution of contractual clauses) and 'prezentarea documentelor false în cadrul procedurilor de achiziţii publice' (submission of false documents in procurement procedures). The regulation's full text (hosted on legis.md) could not be retrieved -- Cloudflare-blocked, not bypassed -- so the exact exclusion-period/appeal-procedure detail is NOT claimed here."
          :ineligible-bidders-list-provenance "https://tender.gov.md/ro/lista-de-interdictie"
          :corporate-number-owner-authority "Agenția Servicii Publice (Public Services Agency -- absorbed the former Camera Înregistrării de Stat / State Registration Chamber)"
          :corporate-number-legal-basis "IDNO (Numărul de identificare de stat / state identification number) -- per Invest Moldova's own Investor's Guide (Ghidul Investitorului, June 2026): the company is assigned the IDNO AT THE MOMENT of state registration, and it ALSO serves as the fiscal code (cod fiscal) -- a SINGLE act performed by Agenția Servicii Publice, not a separate subsequent act by Moldova's tax authority."
          :corporate-number-provenance "https://invest.gov.md/wp-content/uploads/2026/06/Ghidul-Investitorului-RO-2026-1.pdf"}})

(defn spec-basis
  "The jurisdiction's requirement map, or nil -- nil means NO spec-basis,
  and the governor must hold any proposal that tries to assess or file
  on it."
  [iso3]
  (get catalog iso3))

(defn coverage
  "Honest coverage report: how many of the requested jurisdictions actually
  have a spec-basis entry. Never report a missing jurisdiction as covered."
  ([] (coverage (keys catalog)))
  ([iso3s]
   (let [have (filter catalog iso3s)
         missing (remove catalog iso3s)]
     {:requested (count iso3s)
      :covered (count have)
      :covered-jurisdictions (vec (sort have))
      :missing-jurisdictions (vec (sort missing))
      :note (str "cloud-itonami-iso3166-mda R0: " (count catalog)
                 " jurisdictions seeded with an official spec-basis. "
                 "This is a starting catalog for market-entry navigation, "
                 "not a survey of all ~194 jurisdictions -- extend "
                 "`marketentry.facts/catalog`, never fabricate a "
                 "jurisdiction's requirements.")})))

(defn required-evidence-satisfied?
  "Does `submitted` (a set/coll of evidence keywords or strings) satisfy
  every evidence item listed for `iso3`? Missing spec-basis -> never
  satisfied."
  [iso3 submitted]
  (when-let [{:keys [required-evidence]} (spec-basis iso3)]
    (let [need (count required-evidence)
          have (count (filter (set submitted) required-evidence))]
      (= need have))))

(defn evidence-checklist [iso3]
  (:required-evidence (spec-basis iso3) []))

(defn rep-spec-basis
  "The jurisdiction's representative-related requirement map, or nil when
  this catalog has no such regime (or, as for MDA, could not confirm
  one -- see the `catalog` docstring's honest-gap note)."
  [iso3]
  (when-let [sb (spec-basis iso3)]
    (when (:rep-owner-authority sb)
      (select-keys sb [:rep-owner-authority :rep-legal-basis :rep-provenance]))))

(defn corporate-number-spec-basis
  "The jurisdiction's corporate-number / tax-id regime, or nil."
  [iso3]
  (when-let [sb (spec-basis iso3)]
    (when (:corporate-number-owner-authority sb)
      (select-keys sb [:corporate-number-owner-authority
                       :corporate-number-legal-basis
                       :corporate-number-provenance]))))

(defn ineligible-bidders-list-spec-basis
  "The jurisdiction's ineligible-bidders/interdiction-list regime, or
  nil. For MDA this is real and current -- the flagship check this
  vertical adds is grounded here."
  [iso3]
  (when-let [sb (spec-basis iso3)]
    (when (:ineligible-bidders-list-owner-authority sb)
      (select-keys sb [:ineligible-bidders-list-owner-authority
                       :ineligible-bidders-list-legal-basis
                       :ineligible-bidders-list-provenance]))))
