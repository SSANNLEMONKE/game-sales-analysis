# Analyse van populariteit, verkoop en spelersbeoordelingen van videogames

In dit project onderzoek ik hoe **verkoopcijfers**, **spelersreviews** en **game-kenmerken** (genre, prijs, releasejaar) met elkaar samenhangen.  
De volledige analyse gebeurt met **Apache Spark** in een Jupyter Notebook.

---

## Doel van het project

Dit project probeert de volgende onderzoeksvragen te beantwoorden:

- Welke genres verkopen het best (volledige verkoopdataset)?
- Welke genres krijgen de beste reviews (Steam-data)?
- Is er een verband tussen **prijs** en **reviewscore**?
- Is spelerstevredenheid een voorspeller van **verkoopcijfers**?
- Hoe verschillen commerciële toppers van best beoordeelde games?

Via datacleaning, normalisatie en joins ontstaat een geïntegreerde subset van **208 games** waarvoor zowel reviewdata, metadata als verkoopcijfers beschikbaar zijn.

---

## Gebruikte datasets

### **1. Steam Reviews - gebruikersbeoordelingen**
Miljoenen reviews van Steam, gebruikt om per game een **positive_ratio** te berekenen.  
https://www.kaggle.com/datasets/andrewmvd/steam-reviews

### **2. Steam Store Games - metadata per game**
Bevat genres, prijs, releasejaar, platformen en rating-aantallen.  
https://www.kaggle.com/datasets/nikdavis/steam-store-games

### **3. Video Game Sales - wereldwijde verkoopcijfers**
Fysieke verkoopcijfers (1980-2016), per platform en regio.  
https://www.kaggle.com/datasets/gregorut/videogamesales

> De ruwe data wordt **niet gecommit** in deze repository. Enkel links naar de originele bronnen zijn aanwezig.

---

## Dataverwerking & Methodologie

### Cleaning
- Conversie van prijzen, datums en ratingkolommen  
- Opschonen van reviews en renaming  
- Null-checks en typeconversies  

### Normalisatie
- Game-namen genormaliseerd (`lower` + `trim`)
- Releasejaar geëxtraheerd met `to_date` + `year`

### Joins
1. **Steam Reviews <-> Steam Metadata** via `appid`  
2. **Steam Metadata <-> Video Game Sales** via `name_norm + release_year`

Daarna worden verkoopcijfers over platformen samengevoegd tot één `total_global_sales`.

### Finale dataset bevat:
- totale verkoop (geaggregeerd)
- prijs
- genres
- releasejaar
- totale reviews
- positieve reviewratio

Totaal: **208 unieke games** in de geïntegreerde subset.

---

## Kernresultaten & inzichten

### Verkoop (Video Game Sales dataset)
- **Action** is veruit het best verkopende genre, gevolgd door Sports, Shooter, RPG en Adventure.
- De historische verkooppiek ligt rond **2005-2009** (Xbox 360, PS3, Wii-generation).
- Na 2010 daalt de fysieke verkoop door de shift naar **digitale distributie** (niet opgenomen in deze dataset).

### Reviews (Steam Reviews + Metadata)
- Genres zoals **Indie**, **Adventure**, **Casual** en sommige niche-genres scoren het hoogst qua **positive_ratio**.
- Reviewkwaliteit volgt een ander patroon dan commerciële verkoop:  
  **wat verkoopt is niet wat het best beoordeeld wordt.**

### Correlaties (208 games subset)
- **Bijna geen correlatie** tussen:
  - prijs <-> reviewscore  
  - reviewscore <-> verkoop  
- Zowel goedkope als dure games kunnen goede of slechte reviews hebben.
- Zeer goed verkopende games hebben niet noodzakelijk de hoogste reviewratio.

### AAA-dominantie
- De top 10 qua verkoop bestaat bijna volledig uit **grote AAA-franchises**:
  GTA, Skyrim, Call of Duty, Fallout, ...

---

## Technologieën

- Apache Spark (PySpark)
- Pandas
- Matplotlib & Seaborn
- Jupyter Notebook

---

## Limitaties

- Enkel games die in **alle drie** de datasets voorkomen worden geanalyseerd -> subset van **208 games**.
- Verkoopdataset bevat geen **digitale verkoop** -> moderne games (na 2016) ontbreken.
- Analyse is dus representatief voor **grote multiplatform titels van 2004-2016**, niet voor de volledige markt.

---

## Eindconclusie

Spelerstevredenheid, prijs en verkoop blijken **slechts zwak met elkaar verbonden**.  
Commercieel succes lijkt veel sterker gedreven door factoren zoals **marketing, franchisebekendheid en platformstrategie** dan door de prijs of Steam-reviewratio.

Dit project toont hoe dataset-integratie met Spark waardevolle inzichten kan opleveren, maar ook hoe belangrijk het is om de **beperkingen van datasets** te begrijpen bij het interpreteren van resultaten.
