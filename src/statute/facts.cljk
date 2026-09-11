(ns statute.facts
  "General-law compliance catalog for Moldova (MDA) -- extends this
  repo's existing `marketentry.facts` (public-procurement market-entry
  only, narrow scope) with a second, orthogonal catalog of statutes a
  company operating in this jurisdiction must generally track for
  compliance. Mirrors cloud-itonami-iso3166-jpn/-deu/-bgr/-aze/-alb/
  -arm's `statute.facts` (ADR-2607141700, cloud-itonami-compliance-
  fact-federation).

  Every entry cites an OFFICIAL Moldovan government-hosted URL --
  never fabricated. Moldova's primary legal-acts database, legis.md
  (run by the Ministry of Justice, successor to the former
  lex.justice.md), returned a Cloudflare \"Just a moment...\"
  bot-detection challenge on every direct attempt AND on every Wayback
  Machine snapshot this iteration tried (the archived pages only
  captured legis.md's client-side-rendered \"Conținutul se încarcă...\"
  loading shell, never the AJAX-fetched article text) -- per this
  fleet's hard rule, that challenge was NOT bypassed. Every entry below
  instead cites the law number/date as stated directly on a DIFFERENT
  official government body's own page (curl/WebFetch-verified
  2026-07-23):

  - Labour Code (Codul muncii) -- `ism.gov.md/ro/pagini/legi-13555`,
    the State Labour Inspectorate's (Inspectoratul de Stat al Muncii)
    own legislation page, directly confirmed: \"Codul muncii, nr. 154-XV
    din 28.03.2003\".
  - Fiscal/Tax Code (Codul fiscal) -- confirmed via the Ministry of
    Finance's own site (`mf.gov.md`), which links to \"Codul fiscal al
    Republicii Moldova\" under its own Legislation section, pointing to
    a WebLex identifier `LPLP199704241163`, decoding to law number 1163
    adopted 24.04.1997. This iteration deliberately does NOT append the
    conventional '-XIII' legislature-number suffix seen in some
    secondary sources for this law, because that suffix specifically
    was not independently re-confirmed from a primary source this
    session (the WebLex item's own full-text view rendered empty/JS-
    only) -- citing only what was directly read, not filling the gap
    from memory.
  - Law on Investments in Entrepreneurial Activity (Legea cu privire la
    investițiile în activitatea de întreprinzător) -- Invest Moldova's
    own \"Ghidul Investitorului\" (Investor's Guide, PDF dated June
    2026, `invest.gov.md` -- curl + `pdftotext` used, WebFetch rendered
    only the page shell for this PDF), which states directly: \"Legea
    nr. 81/2004 cu privire la investiţiile în activitatea de
    întreprinzător urmează a fi înlocuită odată cu adoptarea noilor
    prevederi legale privind investiţiile\" (Law No. 81/2004 on
    investments in entrepreneurial activity IS ABOUT TO BE REPLACED
    upon adoption of new investment-law provisions). This is reported
    HONESTLY with a `:pending-replacement` topic tag rather than as
    simple, settled current law -- the same currency-checking
    discipline ARM's own iteration applied when it found Article 6 had
    been amended since the text it could read.

  A fourth topic this iteration deliberately did NOT populate: company
  law (the LLC law, Legea nr. 135/2007 privind societăţile cu
  răspundere limitată, and/or the joint-stock company law). Agenția
  Servicii Publice's own registration-service pages, the Ministry of
  Justice's own site, and the Ministry of Finance's own site were all
  fetched directly and NONE of them named this law's exact number/date
  on the page content this iteration could read; legis.md itself
  (where the primary text lives) was Cloudflare-blocked as above. This
  is an honest, disclosed coverage gap -- NOT a fabricated citation --
  left for a future iteration once legis.md is reachable or another
  primary-source mirror is found.

  A law not in this table has NO spec-basis, full stop; extend
  `catalog`, do not invent an id/url.")

(def catalog
  "iso3 -> vector of statute entries. `:statute/url` + `:statute/law-number`
  are the citation the governor requires before any compliance-fact
  proposal referencing this law can commit. NOTE on `:statute/enacted-
  date` for MDA entries: unlike ARM's catalog (which separately
  verified BOTH an adoption date and a distinct entry-into-force date),
  this iteration confirmed only an ADOPTION date for each MDA law from
  the cited official source -- no separate entry-into-force date was
  independently checked, so `:statute/enacted-date` here should be read
  as 'adoption date per the cited source', not as a verified
  entry-into-force date."
  {"MDA"
   [{:statute/id "mda.labour-code"
     :statute/title "Codul muncii al Republicii Moldova (Labour Code of the Republic of Moldova)"
     :statute/jurisdiction "MDA"
     :statute/kind :law
     :statute/law-number "nr. 154-XV, adopted 28 March 2003"
     :statute/url "https://ism.gov.md/ro/pagini/legi-13555"
     :statute/url-provenance :official-labour-inspectorate
     :statute/enacted-date "2003-03-28"
     :statute/retrieved-at "2026-07-23"
     :statute/topic #{:labor :employment}}
    {:statute/id "mda.fiscal-code"
     :statute/title "Codul fiscal al Republicii Moldova (Fiscal/Tax Code of the Republic of Moldova)"
     :statute/jurisdiction "MDA"
     :statute/kind :law
     :statute/law-number "nr. 1163, adopted 24 April 1997 (WebLex id LPLP199704241163 -- the conventional '-XIII' legislature suffix was NOT independently re-confirmed this session, see namespace docstring)"
     :statute/url "https://www.mf.gov.md/ro"
     :statute/url-provenance :official-ministry-of-finance
     :statute/enacted-date "1997-04-24"
     :statute/retrieved-at "2026-07-23"
     :statute/topic #{:tax :fiscal}}
    {:statute/id "mda.investment-law"
     :statute/title "Legea cu privire la investiţiile în activitatea de întreprinzător (Law on Investments in Entrepreneurial Activity)"
     :statute/jurisdiction "MDA"
     :statute/kind :law
     :statute/law-number "nr. 81/2004 -- per Invest Moldova's own June-2026 Investor's Guide, about to be REPLACED by new investment-law provisions (see namespace docstring); reported here with a :pending-replacement topic tag, not as settled current law"
     :statute/url "https://invest.gov.md/wp-content/uploads/2026/06/Ghidul-Investitorului-RO-2026-1.pdf"
     :statute/url-provenance :official-invest-moldova
     :statute/enacted-date "2004 (exact day/month NOT independently confirmed this session -- the fetched source states only 'nr. 81/2004', not a full date; deliberately left unspecified rather than filled from memory)"
     :statute/retrieved-at "2026-07-23"
     :statute/topic #{:investment :pending-replacement}}]})

(defn spec-basis
  "The jurisdiction's statute vector, or nil -- nil means NO spec-basis
  for that jurisdiction yet."
  [iso3]
  (get catalog iso3))

(defn coverage
  "Honest coverage report, same shape/discipline as `marketentry.facts/coverage`:
  never report a missing jurisdiction as covered."
  ([] (coverage (keys catalog)))
  ([iso3s]
   (let [have (filter catalog iso3s)
         missing (remove catalog iso3s)]
     {:requested (count iso3s)
      :covered (count have)
      :covered-jurisdictions (vec (sort have))
      :missing-jurisdictions (vec (sort missing))
      :note (str "cloud-itonami-iso3166-mda statute.facts Wave 0 (ADR-2607141700): "
                 (count (get catalog "MDA")) " MDA statutes seeded with an "
                 "official government-hosted citation. Extend "
                 "`statute.facts/catalog`, never fabricate a law-id or URL.")})))

(defn by-topic
  "Statutes for `iso3` tagged with `topic` (e.g. :labor, :tax, :investment)."
  [iso3 topic]
  (filterv #(contains? (:statute/topic %) topic) (spec-basis iso3)))
