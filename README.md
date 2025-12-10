
# Analyse van populariteit, verkoop en spelersbeoordelingen van videogames

In dit project onderzoek ik hoe **verkoopcijfers**, **spelersreviews** en **game-kenmerken** (genre, prijs, releasejaar) met elkaar samenhangen.  
De volledige analyse gebeurt met **Apache Spark** in een Jupyter Notebook.

---

## Doel van het project

Dit project probeert de volgende onderzoeksvragen te beantwoorden:

- Welke genres verkopen het best, en welke krijgen de beste reviews?  
- Is er een verband tussen **prijs** en **reviewscore**?  
- Is spelerstevredenheid een voorspeller van **verkoopcijfers**?  
- Hoe verschillen commerciële toppers van best beoordeelde games?

Door datacleaning, normalisatie en joins ontstaat een geïntegreerde subset van **208 games** met zowel reviewdata, metadata als verkoopcijfers.

---

## Gebruikte datasets

### **1. Steam Reviews - gebruikersbeoordelingen**  
Miljoenen reviews van Steam, gebruikt om de **positive_ratio** per game te berekenen.  
https://www.kaggle.com/datasets/andrewmvd/steam-reviews

### **2. Steam Store Games - metadata per game**  
Bevat genres, prijs, releasejaar, rating-aantallen en platforminformatie.  
https://www.kaggle.com/datasets/nikdavis/steam-store-games

### **3. Video Game Sales - wereldwijde verkoopcijfers**  
Bevat fysieke verkoopcijfers (1980-2016), per platform en regio.  
https://www.kaggle.com/datasets/gregorut/videogamesales

> De ruwe data wordt **niet gecommit** in deze repository. Enkel links naar de originele bronnen worden opgenomen.

---

## Dataverwerking & Methodologie

Belangrijkste stappen:

### Cleaning
- Conversie van prijzen, datums en ratings  
- Opschonen van reviews + renaming  
- Null-checks en typeconversies  

### Normalisatie
- Namen genormaliseerd (lowercase + trim)  
- Releasejaar geëxtraheerd via `to_date` + `year`

### Joins
1. **Steam Reviews <-> Steam Metadata** via `appid`  
2. **Steam Metadata <-> Video Game Sales** via `name_norm + release_year`  

Verkoop per platform wordt daarna samengevoegd tot één `total_global_sales` per game.

### Finale dataset bevat:
- totale verkoop  
- prijs  
- genres  
- releasejaar  
- totale reviews  
- positieve review-ratio  

Totaal: **208 unieke games** in de analyse-subset.

---

## Kernresultaten & inzichten

- **Action** domineert globale verkoopcijfers, gevolgd door Sports, RPG en Adventure.  
- Genres zoals **Indie**, **Adventure** en **Casual** scoren het hoogst qua **positive_ratio**.  
- Er is **nauwelijks correlatie** tussen:
  - prijs <-> reviewscore  
  - reviewscore <-> verkoop  
- De top 10 best verkopende games bestaat grotendeels uit **grote AAA-franchises**.  
- De Video Game Sales dataset toont een historische piek rond 2005-2009, gevolgd door een daling door de overgang naar digitale verkoop (niet opgenomen in die dataset).

---


## Technologieën

- Apache Spark (PySpark)
- Pandas
- Matplotlib & Seaborn
- Jupyter Notebook

---

## Limitaties

- Enkel games die in **alle drie** de datasets voorkomen worden geanalyseerd -> subset van **208** games.  
- Verkoopdataset bevat geen moderne digitale verkoop -> recente releases (na 2016) ontbreken.  
- Daardoor is de analyse representatief voor **grote multiplatform titels (2004-2016)**, niet voor de volledige gamesmarkt.

---

## Eindconclusie

Spelerstevredenheid, prijs en verkoop blijken **slechts zwak met elkaar verbonden**.  
Wat het meest verkoopt is niet noodzakelijk wat spelers het best beoordelen.

Commercieel succes wordt waarschijnlijk bepaald door factoren die buiten deze datasets vallen, zoals **marketingbudgetten, franchisebekendheid en platformstrategie**.

Dit project toont hoe waardevol dataset-integratie is, maar ook hoe cruciaal het is om de **beperkingen van datasets** te begrijpen bij het trekken van conclusies.
