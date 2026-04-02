# Story plot ontology — Knowledge Representation and Reasoning

Ontology for **story plots**, with **Narn i Chîn Húrin** (*The Children of Húrin*) as the main example. This project ties together family relations (`fo:`), Middle-earth typing (`meo:`), RCC8 space, **story-local** locations and time, events, actions, social relations, and plot structure.

---

## What is not my work

**`fo.ttl`**, **`meo.ttl`**, and **`narn.ttl`** are **not** mine.

Everything under **`http://purl.org/story/`** (including the merge **`story-ontology.ttl`** and **`example-narn.ttl`**) **is** my work for this project.

---

## Project structure

| File / folder | Description |
|---------------|-------------|
| `story-ontology.ttl` | Merged ontology for creating Widoco documentation |
| `fo.ttl` | Family ontology (`fo:`) |
| `meo.ttl` | Middle-earth ontology (`meo:`) |
| `narn.ttl` | Reference Narn KB at `https://purl.org/narn/` |
| `example-narn.ttl` | example individuals at `http://purl.org/story/narn/` — covers **`loc`**, **`ev`**, **`act`**, **`plot`**, **`soc`**, **`stime`**, **RCC8**, etc. |
| `rcc.ttl` | RCC8 classes and object properties |
| `locations.ttl` | `loc:` places, `isPartOf`, `containsPlace`, settlement hierarchy, wild areas, water bodies, `capitalOf` / `hasCapital` |
| `time-story.ttl` | `stime:` story intervals/instants; Allen-style links to W3C OWL-Time |
| `social.ttl` | `soc:` characters, houses, roles, allegiance, enmity, occupations |
| `events.ttl` | `ev:` events, participants, journey origin/destination, temporal extent |
| `actions.ttl` | `act:` Action ⊑ Event; speech / travel / combat / social act types; **`hasAgent`**, **`hasPatient`**, **`hasTheme`**, **`hasInstrument`**; **`subActionOf`** (⊑ **`ev:subEventOf`**); **`hasAgent`/`hasPatient` ⊑ `ev:hasParticipant`**; |
| `plot.ttl` | `plot:` story, narrative units, chapters, climax, tragic ending, ordering, links to events |

---

## WIDOCO documentation

Download a **WIDOCO** `jar-with-dependencies` build (e.g. **1.4.25** for **JDK 17**) from [github.com/dgarijo/Widoco/releases](https://github.com/dgarijo/Widoco/releases); the JAR is **not** stored in this repository so the clone stays light. Save it next to the files below (or fix the path) before regenerating docs.

```powershell
java -jar "widoco-1.4.25-jar-with-dependencies_JDK-17.jar" -ontFile "story-ontology.ttl" -outFolder story-doc -rewriteAll -uniteSections -confFile "config/config.properties"
```

Output is usually `story-doc/doc/index-en.html` (option `-uniteSections` merges sections into one page).

---

## Competency questions

Using the ontology, it should be possible to answer the following competency questions. Under each question, **example triples** are written as three tokens per line: *subject predicate object* (as in Turtle, using `example-narn.ttl` IRIs such as `narn:`, `ev:`, `loc:`, `fo:`, `plot:`, `rcc:`, `act:`, `soc:`).

### Who is parent of X? married to X? relative to X?

```
narn:Húrin fo:isTheFatherOf narn:Túrin
narn:Morwen fo:isTheMotherOf narn:Túrin
narn:Húrin fo:isMarriedTo narn:Morwen
```

### Where did character X go during the story?

```
narn:Túrin ev:hasParticipant narn:DepartureFromDorLómin
narn:DepartureFromDorLómin ev:hasOrigin narn:HouseOfHúrinPlace
narn:DepartureFromDorLómin ev:hasDestination narn:Doriath
narn:TúrinLeavesHouse act:subActionOf narn:DepartureFromDorLómin
```

### Who was involved in event E?

```
narn:DepartureFromDorLómin ev:hasParticipant narn:Túrin
narn:FallOfGlaurung ev:hasParticipant narn:Glaurung
narn:FarewellToMorwen act:hasAgent narn:Túrin
```

### Did event E1 happen before event E2?

```
narn:DepartureFromDorLómin ev:before narn:FallOfGlaurung
narn:FallOfGlaurung ev:before narn:DeathOfTúrin
```

### Where did event E occur?

```
narn:DepartureFromDorLómin ev:occursAt narn:HouseOfHúrinPlace
narn:AudienceWithThingol ev:occursAt narn:ThousandCavesHall
```

### Is it possible to be in location L1 and L2 at the same time?

```
narn:HouseOfHúrinPlace rcc:tpp narn:DorLómin
narn:DorLómin rcc:dc narn:Doriath
```

### What event ends story S?

```
narn:TheNarn plot:endsWithEvent narn:DeathOfTúrin
```

### What item was used as part of event E (or action A)?

```
narn:DeathOfTúrin ev:usesItem narn:Gurthang
narn:TúrinStrikesGlaurung act:hasInstrument narn:Gurthang
```

### Which smaller events or actions are part of a larger event?

```
narn:DepartureFromDorLómin ev:hasSubEvent narn:FarewellToMorwen
narn:FarewellToMorwen ev:subEventOf narn:DepartureFromDorLómin
narn:FarewellToMorwen act:subActionOf narn:DepartureFromDorLómin
narn:FallOfGlaurung ev:hasSubEvent narn:TúrinStrikesGlaurung
```

### Who is the agent or patient of action A, or what object is the theme of a give/take?

```
narn:FarewellToMorwen act:hasAgent narn:Túrin
narn:FarewellToMorwen act:hasPatient narn:Morwen
narn:MorwenGivesKeepsake act:hasTheme narn:KeepsakeFromMorwen
narn:TúrinTakesDragonHelm act:hasTheme narn:DragonHelm
```

### Which narrative unit tells event E, and what precedes what in the telling?

```
narn:Ch1_Childhood plot:tellsEvent narn:DepartureFromDorLómin
narn:Climax_Glaurung plot:tellsEvent narn:FallOfGlaurung
narn:Ch1_Childhood plot:precedesUnit narn:Climax_Glaurung
narn:Climax_Glaurung plot:precedesUnit narn:Ch2_FateOfTúrin
```

### What occupation, house, sworn enemy, or allegiance applies to character X?

```
narn:Túrin soc:belongsToHouse narn:HouseOfHador
narn:Beleg soc:hasOccupation soc:Soldier
narn:Túrin soc:hasAllegianceTo narn:Thingol
narn:Glaurung soc:swornEnemyOf narn:Túrin
```

---

## Use of AI

I used an AI chatbot (mostly ) to clarify plot structure and actions (how to model narrative units, events, and intentional acts), to understand axioms in OWL (e.g. what restrictions, property chains, and imports imply) and for validation (reviewing modeling choices, and file structure).

---
