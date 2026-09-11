(ns marketentry.governor
  "Market-Entry Compliance Governor -- the independent compliance layer
  that earns the MarketEntry-LLM the right to commit. The LLM has no
  notion of Moldovan procurement law, whether a claimed engagement fee
  actually equals base + months x rate, whether an engagement's own
  declared List-of-Interdiction membership was actually checked,
  whether an IDNO (Numărul de identificare de stat / state
  identification number) has been verified for a filing that requires
  it, or when a draft stops being a draft and becomes a real-world
  MTender portal submission, so this MUST be a separate system able to
  *reject* a proposal and fall back to HOLD.

  `:itonami.blueprint/governor` is `:market-entry-compliance-governor`
  (shared family keyword on blueprints).

  This blueprint's own text (docs/business-model.md Trust Controls:
  'any actual portal registration or filing submission requires
  Market-Entry Compliance Governor clearance and always escalates to
  human sign-off'; 'a false or fabricated regulatory-requirement claim
  is a HARD hold') names exactly the checks below.

  Six checks, in priority order, ALL HARD violations: a human
  approver CANNOT override them. The confidence/actuation gate is
  SOFT: it asks a human to look (low confidence / actuation), and the
  human may approve -- but see `marketentry.phase`: for `:stake
  :actuation/draft-filing`/`:actuation/submit-filing` NO phase ever
  allows auto-commit either. Two independent layers agree that
  actuation is always a human call.

    1. Spec-basis                  -- did the jurisdiction proposal cite
                                       an OFFICIAL source
                                       (`marketentry.facts`), or invent
                                       one?
    2. Evidence incomplete         -- for `:filing/draft`/
                                       `:filing/submit`, has the
                                       jurisdiction actually been
                                       assessed with a full evidence
                                       checklist on file?
    3. Interdiction-list
       membership                    -- for `:filing/submit`,
                                       INDEPENDENTLY verify the
                                       engagement's own declared
                                       `:on-interdiction-list?` is not
                                       true. FLAGSHIP check for this
                                       repo -- UNCONDITIONAL (evaluated
                                       for every `:filing/submit`, not
                                       gated behind a `:requires-X?`
                                       flag) and a boolean registry-
                                       membership check rather than a
                                       numeric threshold recompute --
                                       the same 'always applies' SHAPE
                                       several sibling actors'
                                       comparable checks use, applied
                                       here to an independently-
                                       researched Moldovan mechanism:
                                       Legea nr. 131/2015 privind
                                       achizițiile publice +
                                       Hotărârea Guvernului nr. 1418
                                       din 28.12.2016, which
                                       establishes and governs the
                                       Lista de interdicţie a
                                       operatorilor economici (List of
                                       Interdiction of Economic
                                       Operators), published live by
                                       Agenția Achiziții Publice at
                                       tender.gov.md/ro/lista-de-interdictie
                                       -- confirmed CURRENTLY POPULATED
                                       (20 real entries) when this
                                       check was researched
                                       (2026-07-23). See
                                       `marketentry.facts`'s namespace
                                       docstring for the full citation
                                       and the honest limits of what
                                       this iteration could confirm.
    4. Engagement fee mismatch     -- for `:filing/submit`,
                                       INDEPENDENTLY recompute whether
                                       the engagement's own `:claimed-
                                       fee` equals `base-fee +
                                       monthly-rate x monitoring-
                                       months` -- honest reapplication
                                       of the ground-truth-recompute
                                       discipline sibling actors use.
    5. IDNO unverified              -- for `:filing/submit`, when the
                                       engagement declares
                                       `:requires-idno? true`,
                                       INDEPENDENTLY check
                                       `:idno-verified?`. CONDITIONAL on
                                       the engagement's own ground
                                       truth. Grounded in the IDNO
                                       (Numărul de identificare de
                                       stat) Agenția Servicii Publice
                                       issues at the same moment as
                                       state registration, which also
                                       serves as the fiscal code (see
                                       `marketentry.facts`).
    6. Confidence floor / actuation
       gate                          -- LLM confidence below threshold,
                                       OR the op is `:filing/draft`/
                                       `:filing/submit` (REAL acts)
                                       -> escalate.

  Two more guards, double-draft/double-submit prevention, are enforced
  off dedicated `:drafted?`/`:submitted?` facts (never a `:status`
  value)."
  (:require [marketentry.facts :as facts]
            [marketentry.registry :as registry]
            [marketentry.store :as store]))

(def confidence-floor 0.6)

(def high-stakes
  "Stakes grave enough to always require a human, even when clean.
  Drafting a real portal package and submitting a real portal
  registration are the two real-world actuation events this actor
  performs."
  #{:actuation/draft-filing :actuation/submit-filing})

;; ----------------------------- checks -----------------------------

(defn- spec-basis-violations
  "A `:jurisdiction/assess` (or `:filing/draft`/`:filing/submit`)
  proposal with no spec-basis citation is a HARD violation -- never
  invent a jurisdiction's market-entry requirements."
  [{:keys [op]} proposal]
  (when (contains? #{:jurisdiction/assess :filing/draft :filing/submit} op)
    (let [value (:value proposal)]
      (when (or (empty? (:cites proposal))
                (and (contains? value :spec-basis) (nil? (:spec-basis value))))
        [{:rule :no-spec-basis
          :detail "公式spec-basisの引用が無い提案は法域要件として扱えない"}]))))

(defn- evidence-incomplete-violations
  "For `:filing/draft`/`:filing/submit`, the jurisdiction's required
  registration evidence must actually be satisfied."
  [{:keys [op subject]} st]
  (when (contains? #{:filing/draft :filing/submit} op)
    (let [e (store/engagement st subject)
          assessment (store/assessment-of st subject)]
      (when-not (and assessment
                     (facts/required-evidence-satisfied?
                      (:jurisdiction e) (:checklist assessment)))
        [{:rule :evidence-incomplete
          :detail "法域の必要書類(国家登記/MTender登録/IDNO等)が充足していない状態での提案"}]))))

(defn- interdiction-list-violations
  "For `:filing/submit`, INDEPENDENTLY verify the engagement's own
  declared List-of-Interdiction membership is not true -- the flagship
  genuinely new check this vertical adds. This is evaluated for EVERY
  `:filing/submit`, unconditionally (not gated behind a `:requires-X?`
  engagement flag), because membership on the Lista de interdicție a
  operatorilor economici is itself a categorical bar to further public-
  procurement participation under Moldovan law (Legea nr. 131/2015 +
  Hotărârea Guvernului nr. 1418 din 28.12.2016) -- there is no
  engagement-declared precondition to check first."
  [{:keys [op subject]} st]
  (when (= op :filing/submit)
    (let [e (store/engagement st subject)]
      (when (true? (:on-interdiction-list? e))
        [{:rule :interdiction-list-listed
          :detail (str subject " este înscris pe Lista de interdicţie a operatorilor economici (List of Interdiction of Economic Operators) -- 提出提案は進められない")}]))))

(defn- engagement-fee-mismatch-violations
  "For `:filing/submit`, INDEPENDENTLY recompute whether the
  engagement's own claimed fee equals base + months x rate."
  [{:keys [op subject]} st]
  (when (= op :filing/submit)
    (let [e (store/engagement st subject)]
      (when-not (registry/engagement-fee-matches-claim? e)
        [{:rule :engagement-fee-mismatch
          :detail (str subject " の申告手数料(" (:claimed-fee e)
                      ")が独立再計算値(" (registry/compute-engagement-fee e) ")と一致しない")}]))))

(defn- idno-unverified-violations
  "For `:filing/submit`, when the engagement declares `:requires-idno?
  true`, INDEPENDENTLY check `:idno-verified?` -- CONDITIONAL on the
  engagement's own ground truth. Grounded in the IDNO (Numărul de
  identificare de stat) Agenția Servicii Publice issues at the same
  moment as state registration, which also serves as the fiscal code."
  [{:keys [op subject]} st]
  (when (= op :filing/submit)
    (let [e (store/engagement st subject)]
      (when (and (true? (:requires-idno? e))
                 (not (true? (:idno-verified? e))))
        [{:rule :idno-unverified
          :detail (str subject " はIDNO(Numărul de identificare de stat)確認を要するが未確認 -- 提出提案は進められない")}]))))

(defn- already-drafted-violations
  "For `:filing/draft`, refuses to draft the SAME engagement twice."
  [{:keys [op subject]} st]
  (when (= op :filing/draft)
    (when (store/engagement-already-drafted? st subject)
      [{:rule :already-drafted
        :detail (str subject " は既にドラフト済み")}])))

(defn- already-submitted-violations
  "For `:filing/submit`, refuses to submit the SAME engagement twice."
  [{:keys [op subject]} st]
  (when (= op :filing/submit)
    (when (store/engagement-already-submitted? st subject)
      [{:rule :already-submitted
        :detail (str subject " は既に提出済み")}])))

(defn check
  "Censors a MarketEntry-LLM proposal against the governor rules.
  Returns {:ok? bool :violations [..] :confidence c :escalate? bool
  :high-stakes? bool :hard? bool}."
  [request _context proposal st]
  (let [hard (into []
                   (concat (spec-basis-violations request proposal)
                           (evidence-incomplete-violations request st)
                           (interdiction-list-violations request st)
                           (engagement-fee-mismatch-violations request st)
                           (idno-unverified-violations request st)
                           (already-drafted-violations request st)
                           (already-submitted-violations request st)))
        conf (:confidence proposal 0.0)
        low? (< conf confidence-floor)
        stakes? (boolean (high-stakes (:stake proposal)))
        hard? (boolean (seq hard))]
    {:ok?          (and (not hard?) (not low?) (not stakes?))
     :violations   hard
     :confidence   conf
     :hard?        hard?
     :escalate?    (and (not hard?) (or low? stakes?))
     :high-stakes? stakes?}))

(defn hold-fact
  "The audit fact written when a proposal is rejected (HOLD)."
  [request context verdict]
  {:t          :governor-hold
   :op         (:op request)
   :actor      (:actor-id context)
   :subject    (:subject request)
   :disposition :hold
   :basis      (mapv :rule (:violations verdict))
   :violations (:violations verdict)
   :confidence (:confidence verdict)})
