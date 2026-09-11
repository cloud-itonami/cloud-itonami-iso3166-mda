(ns marketentry.facts-test
  (:require [clojure.test :refer [deftest is testing]]
            [marketentry.facts :as facts]))

(deftest mda-has-spec-basis
  (let [sb (facts/spec-basis "MDA")]
    (is (some? sb))
    (is (string? (:provenance sb)))
    (is (seq (:required-evidence sb)))
    (is (some? (facts/corporate-number-spec-basis "MDA")))
    (is (some? (facts/ineligible-bidders-list-spec-basis "MDA")))))

(deftest rep-spec-basis-is-an-honest-gap-for-mda
  (testing "MDA's rep-spec-basis is nil -- an honest gap, not a fabricated requirement (see facts.cljc docstring)"
    (is (nil? (facts/rep-spec-basis "MDA")))))

(deftest unknown-jurisdiction-has-no-spec-basis
  (is (nil? (facts/spec-basis "ATL")))
  (is (nil? (facts/spec-basis "ZZZ"))))

(deftest required-evidence-satisfied
  (let [sb (facts/spec-basis "MDA")
        all (:required-evidence sb)]
    (is (true? (facts/required-evidence-satisfied? "MDA" all)))
    (is (not (facts/required-evidence-satisfied? "MDA" (take 1 all))))
    (is (nil? (facts/required-evidence-satisfied? "ATL" all)))))

(deftest coverage-is-honest
  (let [c (facts/coverage ["MDA" "ATL" "ZZZ"])]
    (is (= 3 (:requested c)))
    (is (= 1 (:covered c)))
    (is (= ["ATL" "ZZZ"] (:missing-jurisdictions c)))))
