(ns culture.facts
  "Country-level regional-culture catalog for Moldova (MDA) -- national
  dishes, protected products, beverages, crafts, festivals and heritage
  sites, per ADR-2607171400 addendum 2 (cloud-itonami-municipality-
  culture-catalog Wave 1, in com-junkawasaki/root). Sibling namespace to
  `marketentry.facts` / `statute.facts` (ADR-2607141700); city-level
  counterparts live in the cloud-itonami-municipality-* repos.

  Catalog is keyed by UPPERCASE ISO3 (mirrors `statute.facts`); entries
  carry no :culture/municipality (that attribute is city-level only).

  Every entry cites a source URL that was actually fetched and read on
  :culture/retrieved-at -- never fabricated. Summaries state only what the
  cited source confirms. An item not in this table has NO spec-basis, full
  stop; extend `catalog`, do not invent an id/url.")

(def catalog
  "iso3 -> vector of culture entries."
  {"MDA"
   [{:culture/id "mda.dish.mamaliga"
     :culture/name "Mămăligă"
     :culture/country "MDA"
     :culture/kind :dish
     :culture/summary "Dish made out of yellow maize flour, traditional in Romania, Moldova, Hungary, Bulgaria, south-western Ukraine and among Poles in Ukraine."
     :culture/url "https://en.wikipedia.org/wiki/M%C4%83m%C4%83lig%C4%83"
     :culture/url-provenance :wikipedia-en
     :culture/retrieved-at "2026-07-17"}
    {:culture/id "mda.dish.placinta"
     :culture/name "Plăcintă"
     :culture/country "MDA"
     :culture/kind :dish
     :culture/summary "Romanian and Moldovan traditional pastry resembling a thin, small round or square-shaped cake, usually filled with apples or a soft cheese."
     :culture/url "https://en.wikipedia.org/wiki/Pl%C4%83cint%C4%83"
     :culture/url-provenance :wikipedia-en
     :culture/retrieved-at "2026-07-17"}
    {:culture/id "mda.dish.sarmale"
     :culture/name "Sarmale"
     :culture/country "MDA"
     :culture/kind :dish
     :culture/summary "Stuffed cabbage rolls popular in all historical regions of Romania and Moldova, including Moldavia, Transylvania and Wallachia."
     :culture/url "https://en.wikipedia.org/wiki/Sarmale"
     :culture/url-provenance :wikipedia-en
     :culture/retrieved-at "2026-07-17"}
    {:culture/id "mda.product.moldovan-wine"
     :culture/name "Moldovan wine"
     :culture/country "MDA"
     :culture/kind :product
     :culture/summary "Moldova produces approximately 2 million hectolitres of wine annually from 148,500 hectares of vineyards, the 11th largest European wine-producing country with a winemaking tradition spanning thousands of years."
     :culture/url "https://en.wikipedia.org/wiki/Moldovan_wine"
     :culture/url-provenance :wikipedia-en
     :culture/retrieved-at "2026-07-17"}
    {:culture/id "mda.festival.national-wine-day"
     :culture/name "National Wine Day"
     :culture/country "MDA"
     :culture/kind :festival
     :culture/summary "Moldovan observance on 8 October that commemorates the production of Moldovan wine."
     :culture/url "https://en.wikipedia.org/wiki/Public_holidays_in_Moldova"
     :culture/url-provenance :wikipedia-en
     :culture/retrieved-at "2026-07-17"}
    {:culture/id "mda.heritage.old-orhei"
     :culture/name "Old Orhei"
     :culture/name-local "Orheiul Vechi"
     :culture/country "MDA"
     :culture/kind :heritage
     :culture/summary "Moldovan historical and archaeological complex located in Trebujeni, containing remnants from multiple civilizations spanning from the Paleolithic era through the 16th century."
     :culture/url "https://en.wikipedia.org/wiki/Orheiul_Vechi"
     :culture/url-provenance :wikipedia-en
     :culture/retrieved-at "2026-07-17"}
    {:culture/id "mda.heritage.cricova"
     :culture/name "Cricova wine cellars"
     :culture/country "MDA"
     :culture/kind :heritage
     :culture/summary "Town in Moldova best known for its vast underground wine cellars, among the largest in the world, making Cricova one of Moldova's leading tourist destinations."
     :culture/url "https://en.wikipedia.org/wiki/Cricova"
     :culture/url-provenance :wikipedia-en
     :culture/retrieved-at "2026-07-17"}]})

(defn spec-basis [iso3] (get catalog iso3))

(defn coverage
  ([] (coverage (keys catalog)))
  ([iso3s]
   (let [have (filter catalog iso3s)
         missing (remove catalog iso3s)]
     {:requested (count iso3s)
      :covered (count have)
      :covered-jurisdictions (vec (sort have))
      :missing-jurisdictions (vec (sort missing))
      :note (str "cloud-itonami-iso3166-mda culture catalog "
                 "(ADR-2607171400 addendum 2, Wave 1): " (count (get catalog "MDA"))
                 " MDA entries, each with a fetched-and-read citation. "
                 "Extend `culture.facts/catalog`, never fabricate an id/url.")})))

(defn by-kind [iso3 kind]
  (filterv #(= (:culture/kind %) kind) (spec-basis iso3)))
