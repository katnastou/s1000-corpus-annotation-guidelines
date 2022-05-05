---
layout: entry
title: Documentation for S1000 corpus annotation
---

## Original Curator Guidelines for S800 corpus

The annotation guidelines as described in the [original publication of the S800 corpus](https://journals.plos.org/plosone/article/file?id=10.1371/journal.pone.0065390&type=printable)

The guidelines to curators were to annotate all substrings, which can meaningfully be identified as _referring to a taxon_. While the main focus was on annotating __species__ mentions, strings referring to __any taxonomic level__, (e.g.kingdoms, orders, genera, strains) were also considered. The data for upper taxonomic level annotations were never officially released.

The main guidelines were:

* All document substrings must be evaluated and all mentions including repetitions should be listed in the order of appearance in the text.
* The annotated name types among others should include: Linnaean binomials, common names, strain names, author defined acronyms.
* For each annotated string, curators must record the name as it appeared in text and report the corresponding NCBITaxonomy database identifier.
* Special cases of adjectives being used to indicate a taxon, misspellings, typographic or other errors and enumerations were indicated as such.
* Taxonomic mentions that did not correspond to an existing NCBI Taxonomy database entry were also indicated.

## Additional guidelines for the annotation of the S1000 corpus

* The first resource that is trusted to resolve issues is [NCBI Taxonomy](https://www.ncbi.nlm.nih.gov/taxonomy). If there is still not enough information there we have resolved inconsistencies using the [Catalogue of Life](http://www.catalogueoflife.org/), [ICTV](https://talk.ictvonline.org/taxonomy/), [ITIS](https://www.itis.gov/) and [WoRMs](https://www.marinespecies.org/).
* The NCBI Taxonomy Ranking has been adopted from [Schoch, _et. al_, 2020](https://academic.oup.com/database/article/doi/10.1093/database/baaa062/5881509#206172258) 

### General annotation guidelines

* Taxonomic mentions that correspond to __Species__, __Genus__ and __Strain__ will receive annotations and normalization to taxonomy reference databases (priority will be the normalizations to NCBI taxonomy, and when that's not possible the resources mentioned above will be used)
* Adjectival forms like __murine__ (__taxid:10090__), __bovine__ (__taxid:9913__), __pneumococcal__ (__taxid:1313__) that map to a specific species should be annotated as such
* The role in which common species names are mentioned should __not__ be taken into account and all species names mentions should be annotated (e.g. _rice_ mentioned as food or _tobacco_ as cigarettes should still be annotated).
* Genus or higher level mentions (e.g. _Arabidopsis_, _yeast_) should only be annotated as the real taxinomic level (or not annotated at all), and not as synonyms of species names. (e.g. _Arabidopsis_ should be annotated as __Genus__ and assigned the genus __taxid:3701__) 
~~~ ann
The second face of a known player: Arabidopsis silencing suppressor AtXRN4 acts organ-specifically
T1 Genus 35 46	Arabidopsis
N1	Reference T1 Taxonomy:3701	Arabidopsis
~~~
* Former __Species__ annotations in the original S800 coprpus that belong to the __Genus__ taxinomic rank have been annotated as the latter in the corpus. Ranks higher than _Genus_ (e.g. _Phylum_, _Kingdom_, _Class_, _Order_, _Family_) should receive an __OOS__ annotation or not be annotated at all 
* For annotations above _Species_ only the "coarse" ranks should be considered, thus mapping mentions at fine-grained levels to their coarse equivalents, e.g. _Subgenus_ should be mapped back to _Genus_.
* For __Subspecies__ mentions: when a subspecies name immediately follows a species name the entire mention is simply annotated as one slightly longer __Species__ mention, e.g. _Phocoenoides dalli dalli_ annotated as __Species__ + __taxid: 9745__ (Rank: Subspecies).
* __Biotypes__ should be treated the same way as __Subspecies__, i.e. they are annotated as __Species__
* __common name__ (_scientific name_)" mentions should be annotated as __two mentions__ e.g from [PMID: 21054435](https://pubmed.ncbi.nlm.nih.gov/21054435/):
~~~ ann
We studied seasonal dynamics in delta^1^3C of CO2 efflux (delta^1^3C(E)) from non-leafy branches, upper and lower trunks and coarse roots of adult trees, comparing deciduous Fagus sylvatica (European beech) with evergreen Picea abies (Norway spruce).
T1 Species 174 189 Fagus sylvatica
T2 Species 191 205 European beech
T3 Species 222 233 Picea abies
T4 Species 235 248 Norway spruce
~~~
* Species names in noun phrase premodifier positions (e.g. __Arabidopsis__ EDR1, __Aspergillus nidulans__ cells) also in cases where they appear as part of the name of an entity of a non-organism type (e.g. __human__ epidermal growth factor receptor 2 (HER2)) are annotated.
* Species names are annotated when they are part of __hyphenated compound words__ (e.g. _human-infecting_) 
* __Clade__ mentions will receive __Clade__ normalizations and will be assigned type according to nearest __non-Clade__ ancestor (if that falls within the scope of the current annotation effort)
* Similarly, __no rank__ mentions will receive __no rank__ normalizations and will be assigned type according to nearest __ranked__ ancestor (if that falls within the scope of the current annotation effort)

##### Rules for common names

* In general, when a __Species__ and a higher-level entry in the taxonomy (e.g. __Genus__) share a common name or synonym, the __Species interpretation should be preferred__ when it is not clear from context which is intended.
* Common names like __human__, __goat__, __horse__, and __rats__ should be __always__ annotated.
* Common names that should be annotated in the __Genus__ level:
  * __fire ant__: __Genus__ and __taxid:13685__ (__Solenopsis__); Note: red fire ant, little fire ant, black fire ant etc should be tagged as the corresponding species)
  * __sunflower__: __Genus__+__taxid:4231__ (__Helianthus__)
  * __galaxias__ : __Genus__+__taxid:51242__ (__Galaxias__)
* Common names that should be annotated in the __Species__ level (but could be annotated in a higher taxonomic level):
  * __rat__: synonym for _Rattus norvegicus_ and __Rattus__. Should be annotated as _Rattus norvegicus_ (__taxid:10116__), unless explicitly referring to a different taxonomic unit (e.g. __cotton rat__: __Genus__ + __taxid:42414__ (__Sigmodon__))
  * __fruit fly__: synonym for _Drosophila melanogaster_ and __Drosophila__ genus and __Tephritidae__ family. Should be annotated as _Drosophila melanogaster_ (__taxid:7227__), unless explicitly referring to a different taxonomic unit
  * __bee__: synonym for _Apis mellifera_, and __Apoidea__ superfamily. Should be annotated as _Apis mellifera_ (__taxid:7460__), unless explicitly referring to a different taxonomic unit (e.g. __bumble bee__)
  * __duck__: synonym for _Anas platyrhynchos_, but can be a synonym for other __Anatidae__. Should be annotated as _Anas platyrhynchos_ (__taxid:8839__), unless explicitly referring to a different taxonomic unit
  * __midge__: synonym for _Chironomus thummi_, but can refer to several species of flies. Should be annotated as _Chironomus thummi_ (__taxid:7154__), unless explicitly referring to a different taxonomic unit

#### Mentions that should __NOT__ receive annotations

* Adjectival forms of Phyla (e.g. __cyanobacterial__: __taxid:1117__) can only be annotated as __OOS__ or not be annotated at all
* Adjectival forms of __Kingdoms__ (e.g. __viral__, __bacterial__) can only be annotated as __OOS__ or not be annotated at all
* Non-name mentions (e.g. __woman__) and species clues (e.g. __patients, children, men, women__) should not be annotated. This includes the non-name mention __man__ which should not be annotated as a synonym for _Homo sapiens_ (__taxid: 9606__)
* Mentions that are __not monophyletic__ (e.g. _fish_) should be annotated as __Out-of-scope (OOS)__ with _Note: not monophyletic_ or not be annotated at all
* Forms identified by place names, like __ecotype__, are not annotated.
~~~ ann
For investigating cadmium uptake, we incubated protoplasts obtained from leaves of Thlaspi caerulescens (Ganges ecotype) with a Cd-specific fluorescent dye.
T1 Species 83 103 Thlaspi caerulescens
~~~
* Species names are __NOT__ annotated when they appear as a __substring in a word not separated by a boundary__ such as a hyphen (e.g. _nonhuman_)
* Abbreviations are marked if the abbreviation stands for an organism mention in scope of the annotation, but not if the full form merely includes an organism mention e.g. in modifier position. For example, the __H__ in __HER2__ is not annotated despite it standing for __human__.
* _Cultivars_ should be annotated as __OOS__ or not be annotated at all. 
~~~ ann
The physiological traits underlying the apparent drought resistance of 'Tomatiga de Ramellet' (TR) cultivars.
T1 Out-of-scope 72 92 Tomatiga de Ramellet
~~~
* _Rootstocks_ should be annotated as __OOS__ e.g. in [PMID: 20837155](http://ann.turkunlp.org:8088/index.xhtml#/S800/20837155?focus=sent~7)
* Non-taxonomic groupings such as __Gram-positive/negative bacteria__, __marine bacteria__ or __enteric bacteria__ should not be annotated. e.g.
~~~ann
The redox-sensitive transcription factor SoxR in enteric bacteria senses and regulates the cellular response to superoxide and nitric oxide.
~~~
~~~ ann
Oscillochloris trichoides is a mesophilic, filamentous, photoautotrophic, nonsulfur, diazotrophic bacterium which is capable of carbon dioxide fixation via the reductive pentose phosphate cycle and possesses no assimilative sulfate reduction.
T1 Species 0 25 Oscillochloris trichoides
~~~
* __tree__ and __bush__ are non-taxonomic mentions and thus not annotated or annotated as __OOS__ + _Note: non-taxonomic_
* Standalone __alga__ (__algae__, __microalgae__, __macroalgae__): can only be annotated as __OOS__ + _Note: non-taxonomic_ or not be annotated at all [e.g.](http://ann.turkunlp.org:8088/index.xhtml#/S800-extension/17259190?focus=T6) Algae is an informal term for a large and diverse group of photosynthetic eukaryotic organisms.
* __protist__ (any eukaryotic organism that is not an animal, plant, or fungus) is a non-taxonomical expression and can be annotated as __OOS__ + _Note: non-taxonomic_ or not be annotated at all [e.g.](http://ann.turkunlp.org:8088/index.xhtml#/S800-extension/7708661?focus=T18)
* __protozoa__ can also be annotated as __OOS__ + _Note: non-taxonomic_ or can not be annotated at all
* __methanotroph__ is a non-taxonomical expression and can be annotated as __OOS__ + __no taxid__ or not be annotated at all [e.g.](http://ann.turkunlp.org:8088/index.xhtml#/S800-extension/1969923?focus=T1)
* __methanogen(s)__ over 50 _Archaea_ species can be annotated as __OOS__ with _Note: not monophyletic_ or not be annotated at all
* __prokaryotes__ includes _Bacteria_ and _Archaea_ in the current three-domain system, so this can be annotated as __OOS__ + __no taxid__ or not be annotated at all [e.g.](http://ann.turkunlp.org:8088/index.xhtml#/S800-extension/1969923?focus=T15) 
* __heterokonts__ and __alveolates__ are clades of microorganisms and can be annotated as __OOS__ + __taxid:33634__ and __taxid:33630__ respectively or not be annotated at all [e.g.](http://ann.turkunlp.org:8088/index.xhtml#/S800-extension/9783459?focus=T18) and [e.g.](http://ann.turkunlp.org:8088/index.xhtml#/S800-extension/9783459?focus=T19)
* __cyanobacteria__, __eubacteria__ and the like can be annotated as __OOS__ or not be annotated at all unless it's clear from context that the reference is definitely to the genus __Cyanobacterium__ or __Eubacterium__ respectively.
* __Non-taxonomic groupings__ of organisms by their behaviour (e.g. _herbivores_, _predators_, _parasites_) should not receive an annotation or should receive an __OOS__ annotation
* __actinorhiza(l)__, __mycorrhiza(l)__, __ectomycorrhiza__ can be __OOS__ + _Note: non-taxonomic_ or not be annotated at all 
* __species complex__ and __clonal complex__ rank can be __OOS__ or not be annotated at all 

##### Rules for common names

* Common names that should not be annotated in the __Species__ level:
  * __ant(s)__: __OOS__+__taxid:36668__ (__Formicidae__) or no annotation
  * __insect(s)__: when standalone assign __OOS__+__taxid:50557__ (__Insecta__) or no annotation
  * __mite__: __OOS__+__taxid:6933__ (__Acari__ subclass) or no annotation
  * __trout__: several species of fish, annotate as __OOS__ + __no taxid__ or no annotation
  * __leafminer__ and __leaf miner__: insects that eat the tissue of plants, annotate as __OOS__ + _Note: non-taxonomic_ or no annotation
  * __fishes__: __OOS__ (Clade-like concept, non-tetrapoda vertebrata) or no annotation
  * __bug__: __OOS__ + _Note: non-taxonomic_ or no annotation
  * __field cricket__: __OOS__ + _Note: non-taxonomic_ or no annotation
  * __mirid bug__: __OOS__+__taxid:30084__ (__Miridae__) or no annotation
  * __clownfish__: __OOS__+__taxid:30863__ (__Pomacentridae__) or no annotation
  * __elephant__: 3 species, not monophyletic (both __Elephas__ and __Loxodonta__ genera), annotate as __OOS__ + __no taxid__ or no annotation
  * __crab__: infraorder containing 850 species, so it should be annotated as __OOS__ + __taxid:6752__ (__Brachyura__) or no annotation
  * __grass__:  __OOS__+__taxid:4479__ (__Poaceae__) or no annotation
  * __seabird(s)__: __OOS__ with _Note: non-taxonomic_ or no annotation
  * __marsupial__ (animals carry the young in a pouch) is a mammalian clade, [e.g.](http://ann.turkunlp.org:8088/index.xhtml#/S800-extension/7700877) and will be annotated as __OOS__ + __taxid:9263__ or not annotated at all
  * __coral(s)__: Hexacorallia + Octocorallia, but paraphyletic because sea anemones are also part of Hexacorallia: annotated as __OOS__ with _Note: not monophyletic_ or not annotated
  * __DNA viruses__, __RNA viruses__ map to no rank entries: annotated as __OOS__ or not annotated at all
  * __dsRNA mycoviruses__: __OOS__ with _Note: non-taxonomic_
  * __cereal__: __OOS__ with _Note: non-taxonomic_
  * __kittiwake__: __OOS__ and _Note: non-taxonomic_
* __Young animals__ (e.g. chicks, calfs etc) should not receive an annotation or should receive an __OOS__ annotation

#### Special rules for Strains

* Strain aliases such as __CC-12301__(T) (=__DSM 45298__(T) =__CCM 7727__(T)) should be annotated in all instances as type __Strain__.
* _name strain_ mentions should be annotated as __two mentions__ of __Species__+__Strain__, e.g. from [PMID: 20154326](https://pubmed.ncbi.nlm.nih.gov/20154326/)
~~~ ann
Strain GSW-R14(T) exhibited 97.6 % 16S rRNA gene sequence similarity to F. gelidilacus LMG 21477(T) and similarities of 91.2-95.2 % to other members of the genus Flavobacterium
T1 Species 7 14 GSW-R14
T2 Species 72 86 F. gelidilacus
T3 Strain 87 96 LMG 21477
~~~
* mentions of the form _[Genus] sp. [Strain]_, should have a separate __Genus__ and __Strain__ annotation [e.g.](http://ann.turkunlp.org:8088/index.xhtml#/S800-extension/9268298?focus=37~59)
* descriptive references to _Strains_ using gene names are not annotated as organisms [e.g.](http://ann.turkunlp.org:8088/index.xhtml#/S800/21097612?focus=1449~1471)

#### Special rules for Viruses

* __Viruses__ (or other taxonomic units) that have species level of entry as "unidentified" (e.g. "retrovirus" __taxid:31931__ ("unidentified retrovirus" equivalent: "retrovirus") or "adenovirus" __taxid:10535__ ("unidentified adenovirus" equivalent: "adenovirus")) should __NOT__ be annotated in the __Species__ level.
* The following mentions should be annotated as __OOS__ or not be annotated at all:
  * "virus"/"viral" __OOS__+__taxid:10239__ "Viruses" _superkingdom_
  * "retrovirus" __OOS__+__taxid:11632__ "Retroviridae" _family_
  * "influenza virus" __OOS__+__taxid:11308__ "Orthomyxoviridae" _family_
  * "herpesvirus" __OOS__+__taxid:10292__ "Herpesviridae" _family_
  * "adenovirus" __OOS__+__taxid:10508__ "Adenoviridae" _family_
  * "baculovirus" __OOS__+__taxid:10442__ "Baculoviridae" _family_
  * "reovirus" __OOS__+__taxid:10880__ "Reoviridae" _family_
* The following mentions should be annotated as __Genus__:
  * "norovirus" __Genus__+__taxid:142786__ "Norovirus" _genus_
  * "ebola virus" __Genus__+__taxid:186536__ "Ebolavirus" _genus_
  * "cytomegalovirus" __Genus__+__taxid:10358__ "Cytomegalovirus" _genus_
* __dengue__: dengue is synonym for dengue fever (disease), annotate as  __OOS__ + __no taxid__ unless _dengue virus_ is mentioned when it should be annotated as __taxid:12637__ (__Species__)
* __smallpox__: smallpox is synonym for smallpox disease, annotate as  __OOS__ + __no taxid__ unless _smallpox virus_ is mentioned when it should be annotated as __taxid:10255__ (__Species__)
* __influenza__: influenza is synonym for the flu (disease), annotate as  __OOS__ + __no taxid__ unless _influenza X virus_ is mentioned when it should be annotated as __Species__. EXCEPTION: __standalone influenza__ may be marked when organism sense is clear from context (e.g. __influenza strains__)
* __human adenovirus__ (or similar cases): when a mention cannot be normalized in an "identified" virus species it should be annotated e.g. as __Species__+__taxid:9606__ (_Homo sapiens_) for __human__ and __OOS__+__taxid:10508__ (_Adenoviridae_) for _adenovirus_ (or no annotation for the latter)

#### Special rules for Yeasts

* All text spans including "yeast" should have an __OOS__ annotation if the taxonomy level is higher than __Genus__ or should not be annotated at all:
  * standalone _yeast_: __OOS__+__taxid:147537__ ("true yeast" subphylum) (Note: an even higher level may be included)
  * _black yeast_: __OOS__+__taxid:34395__ ("black yeast" order)
  * _budding yeast_: __OOS__+__taxid:4892__ ("budding yeasts" order)
  * _fission yeast_: __OOS__+__taxid:4894__ ("fission yeasts" family)
  * _truffle_: __Genus__ + __taxid:36048__ (_Tuber_ genus)
  
#### Special rules for Amoebae

* All __amoebae__ instances have been revised to resolve confusion of non-taxonomical expression __amoebae__ (type of cell or unicellular organism which has the ability to alter its shape), of __taxid:554915__ (__OOS__: _Clade: Amoebozoa_), and __taxid:55774__ (__Genus__: _Amoebae_). Most of the cases were non-taxonomical expressions (__OOS__ + __no taxid__)
  * __testate amoebae__: very common combination of mentions, which means _shelled amoebae_, which explains the form of microorganism(s): __OOS__ + __no taxid__ or no annotation
  * Interesting article [PMID: 21112814](https://pubmed.ncbi.nlm.nih.gov/21112814/), where both _non-taxonomical_ and _Genus_ amoebae are mentioned (only one real "amoebae" Genus in the corpus)

#### Special cases

* Four mentions of "astomes" in this document [PMID: 21398102](http://ann.turkunlp.org:8088/index.xhtml#/S800/21398102?focus=sent~9) are __OOS__
* Astome ciliates in this document [PMID: 21398102](http://ann.turkunlp.org:8088/index.xhtml#/S800/21398102?focus=T15) are also __OOS__
* FGSC should not be annotated as it refers to a complex which is __OOS__, namely Fusarium graminearum complex [PMID: 22004876](http://ann.turkunlp.org:8088/index.xhtml#/S800/22004876?focus=T2)
* Mentions of carnivores in [PMID: 21323921](http://ann.turkunlp.org:8088/index.xhtml#/S800/21323921) are __OOS__ (interpreting these to refer generally to meat-eating animals)
* __human__ and __primates__ in a context of __non-human primates__ are considered __two mentions__ [21295520](http://ann.turkunlp.org:8088/index.xhtml#/S800/21295520?focus=T9)
* [PMID: 2435057](http://ann.turkunlp.org:8088/index.xhtml#/S800-extension/2435057) is discussing retroviruses, but terminology there is quite old (published in 1987). ICTV (International Committee on Taxonomy of Viruses) was used to figure out how those viruses are called/classified in that period tracing its history.
* __GII.4__ in [PMID: 20980508](http://ann.turkunlp.org:8088/index.xhtml#/S800/20980508) has been annotated as __Species__, following the general rule about __Clade__ mentions
* __arbuscular mycorrhizal fungi__ (__AMF__) e.g. in [PMID: 20880038](http://ann.turkunlp.org:8088/index.xhtml#/S800/20880038) is __OOS__ 
* __tropical japonica rice__ (e.g. [PMID: 20946420](http://ann.turkunlp.org:8088/index.xhtml#/S800/20946420?focus=T10)): following rule about __no rank__ entries: normalization to [NCBI txid: 1736656](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=1736656) and type __Species__

### Span consistency guidelines

* The expressions _sp. nov._ and _gen. nov., sp. nov._ are not included in the __Species__ name, since these are supposedly used only the first time a _genus_ and/or a _species/subspecies_ is described to denote that it's new, so they are not part of the scientific name and shouldn't be found anywhere else other than the first paper describing them.
* An annotated span should __not__ end with _sp._ or _spp._
* Superscript T - to denote type strain - should __not__ be included in species' names
* The person's name should __not__ be included in the species name, especially when it is in parentheses. The non-parenthesized form is a bit more complex (at least in the example above _Pseudacteon tricuspis_ Borgmeier is a valid name shown as a synonym for _Pseudacteon tricuspis_ in NCBI taxonomy). For annotation consistency the suggestion is to __drop these names in all appearances__. (The confusion with subspecies can be avoided because of the capital letter at the start of the second word, e.g. _Ursus arctos arctos_ would be easy to distinguish from _Ursus arctos_ Linneaus and then drop the name for the latter.)
* Do __not__ include common head nouns such as "plants" in annotation spans
* Do __not__ include adjectival premodifiers such as "native" in annotation spans
* Model words like __SCID__ mouse should be excluded from annotations
* _"species complex"_ should __not__ be part of a species name, e.g. from [PMID: 20682355](https://pubmed.ncbi.nlm.nih.gov/20682355/)
~~~ann
The splicing activity of the PRP8 intein from the B. dermatitidis, E. parva and P. brasiliensis species complex was demonstrated in a non-native protein context in Escherichia coli.
T5	Species 50 65	B. dermatitidis
T6	Species 67 75	E. parva
T7	Species 80 95	P. brasiliensis
T8	Species 164 180	Escherichia coli
~~~
* _f. sp._ (forma specialis) should be included in the annotated mention (e.g. _Blumeria graminis f. sp. tritici_) 
* Do __not__ include nouns identifying levels of taxonomy in annotation spans. For example, the words __strain__, __serotype__, __serovar__, and __serogroup__ should be excluded from the spans of annotated _Strain_ mentions. e.g from 20154326
~~~ ann
Strain GSW-R14(T) exhibited 97.6 % 16S rRNA gene sequence similarity ...
T1 Strain 7 14 GSW-R14
~~~
* Annotate antibodies e.g __anti-HCV__ with species annotation for the organism (__HCV__) and _Note: "anti-" prefix_

For information on Annodoc, see <http://spyysalo.github.io/annodoc/>.
