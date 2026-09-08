# Data Analytics Tools — Mini Tests

## Modern Data Environment

### 1. Vrstvy datového prostředí

Firma používá následující postup:

1. Objednávky vznikají v e-shopové SQL databázi.
2. Python pomocí SQL dotazu načte objednávky za poslední měsíc.
3. Načtená data se uloží do analytické databáze.
4. SQL spojí objednávky se zákazníky a produkty a vypočítá tržby.
5. Power BI nad připravenými tabulkami vytvoří vztahy a DAX míry.
6. Dashboard zobrazí tržby, zisk a výsledky regionů.
7. Obchodní ředitel podle výsledků upraví regionální rozpočty.

Přiřaď ke každému kroku hlavní vrstvu:

- Data Source
- Data Ingestion
- Data Storage
- Data Transformation
- Analytical Layer
- Reporting
- Business Decision

#### Řešení

1. **Data Source** – provozní SQL databáze je zdrojem objednávek.
2. **Data Ingestion** – Python spustí SQL dotaz a načte data.
3. **Data Storage** – analytická databáze uchovává načtená data.
4. **Data Transformation** – SQL data spojí a vypočítá tržby.
5. **Analytical Layer** – Power BI obsahuje vztahy, datový model a DAX míry.
6. **Reporting** – dashboard vizuálně prezentuje výsledky.
7. **Business Decision** – obchodní ředitel podle výsledků upraví rozpočty.

---

### 2. Role Pythonu v datovém procesu

Python:

1. získá data z API;
2. odstraní duplicity a vypočítá tržby;
3. zapíše výsledek do SQL databáze.

Jaké hlavní operace Python postupně provádí?

#### Řešení

1. **Extract / Data Ingestion** – Python získá data z API.
2. **Data Transformation** – Python data vyčistí a vytvoří nový výpočet.
3. **Load** – Python zapíše výsledek do cílové databáze.

SQL databáze následně představuje **Data Storage**, protože uložená data uchovává.

```text
Python → Extract
Python → Transform
Python → Load
SQL databáze → Storage
```

---

### 3. Extract, Transform and Load v Power Query

Firma má objednávky v SQL databázi. Power Query:

1. připojí se k databázi;
2. načte tabulku objednávek;
3. odstraní nepotřebné sloupce;
4. nahradí chybějící region hodnotou `"Unknown"`;
5. načte výsledek do Power BI datového modelu.

Přiřaď ke každému kroku hlavní operaci:

- Extract
- Transform
- Load

#### Řešení

1. **Extract** – Power Query vytvoří připojení ke zdroji.
2. **Extract** – Power Query získá tabulku z databáze.
3. **Transform** – odstraní nepotřebné sloupce.
4. **Transform** – nahradí chybějící hodnoty.
5. **Load** – načte připravená data do Power BI modelu.

Celý proces:

```text
SQL Database
→ Extract
→ Transform
→ Load
→ Power BI Model
```

Samotné vytvoření připojení ještě data nepřenáší, ale je součástí fáze Extract, která pokračuje načtením tabulky.

---

### 4. Role v datovém týmu

Přiřaď ke každé činnosti nejvhodnější hlavní roli:

- **DE** – Data Engineer
- **DA** – Data Analyst
- **BI** – BI Developer

1. Zjistí, jaká rozhodnutí má report podporovat.
2. Vytvoří pravidelný přenos dat z provozního systému do datového skladu.
3. Navrhne vztahy mezi faktovou a dimenzní tabulkou.
4. Analyzuje příčinu poklesu tržeb.
5. Sleduje, zda datová pipeline proběhla bez chyby.
6. Vytvoří složitější DAX míry.
7. Interpretuje výsledky a připraví business doporučení.

#### Řešení

1. **DA** – datový analytik vyjasňuje business potřebu.
2. **DE** – data engineer zajišťuje pravidelný přenos dat.
3. **BI** – BI developer navrhuje Power BI datový model.
4. **DA** – datový analytik hledá příčinu business výsledku.
5. **DE** – data engineer sleduje fungování datových pipeline.
6. **BI** – tvorba složitějších DAX měr je typická BI činnost.
7. **DA** – interpretace a doporučení jsou klíčovou odpovědností analytika.

Hranice mezi rolemi nejsou pevné. V praxi mohou na některých činnostech spolupracovat různí členové datového týmu.

---

### 5. Pořadí vrstev datového prostředí

Seřaď následující vrstvy od vzniku dat po jejich business využití:

- Reporting
- Data Source
- Business Decision
- Data Transformation
- Analytical Layer
- Data Ingestion
- Data Storage

#### Řešení

```text
Data Source
→ Data Ingestion
→ Data Storage
→ Data Transformation
→ Analytical Layer
→ Reporting
→ Business Decision
```

Jde o orientační mapu datových vrstev. Skutečné pořadí uložení a transformace se může lišit podle toho, zda proces využívá ETL nebo ELT.

---

### 6. Ingestion, Load and Storage

Python získá data z API a zapíše je do databáze.

Urči, co představuje:

1. Python při získání dat z API.
2. Python při zápisu dat.
3. Databáze po uložení dat.

#### Řešení

1. **Extract / Data Ingestion** – Python získává data ze zdroje.
2. **Load** – Python zapisuje data do cílového prostředí.
3. **Data Storage** – databáze uložená data uchovává.

Hlavní rozdíl:

```text
Load = zápis dat
Storage = místo uchování dat
```

---

### 7. Role SQL

SQL dotaz:

- spojí objednávky se zákazníky;
- odfiltruje zrušené objednávky;
- vypočítá tržby podle regionu.

Jakou hlavní roli zde SQL plní?

- Data Ingestion
- Data Storage
- Data Transformation
- Reporting

#### Řešení

Správná odpověď je:

```text
Data Transformation
```

SQL zde data spojuje, filtruje, počítá a agreguje. Neprovádí reporting ani neposkytuje samotné úložiště.

---

### 8. Součásti Power BI

Přiřaď každou součást Power BI k její hlavní roli:

| Součást | Role |
|---|---|
| Power Query | ? |
| Vztahy mezi tabulkami | ? |
| DAX míry | ? |
| Sloupcový graf | ? |

Použij následující možnosti:

- Data Ingestion / Data Transformation
- Analytical Layer
- Reporting

Jednu možnost můžeš použít vícekrát.

#### Řešení

| Součást | Hlavní role |
|---|---|
| Power Query | Data Ingestion / Data Transformation |
| Vztahy mezi tabulkami | Analytical Layer |
| DAX míry | Analytical Layer |
| Sloupcový graf | Reporting |

Power BI obsahuje více částí datového procesu:

```text
Power Query
→ příprava dat

Semantic Model
→ tabulky, vztahy a business logika

DAX
→ dynamické výpočty

Report
→ vizuální prezentace výsledků
```

---

### 9. Datový a sémantický model

Doplň následující věty:

1. Vztahy jsou __________ datového modelu.
2. Sémantický model dává technickým datům __________ význam.
3. Power BI report používá sémantický model pro __________ výsledků.

#### Řešení

1. Vztahy jsou **součástí** datového modelu.
2. Sémantický model dává technickým datům **business** význam.
3. Power BI report používá sémantický model pro **zobrazování** výsledků.

Základní rozlišení:

```text
Relationship
→ propojení tabulek

Data Model
→ tabulky, sloupce, vztahy a výpočty

Semantic Model
→ datový model doplněný o business význam

Report
→ vizuální prezentace dat ze sémantického modelu
```

---

### 10. Odpovědnost datového analytika

Které tvrzení je nejpřesnější?

**A.** Datový analytik potřebuje ovládat správu celé cloudové infrastruktury.

**B.** Datový analytik se má orientovat v celém datovém toku, ale jeho hlavní odpovědností je práce s business otázkou, daty, analýzou a interpretací.

**C.** Datový analytik pracuje pouze s hotovým dashboardem.

**D.** Datový analytik nesmí provádět transformace dat.

#### Řešení

Správná odpověď je **B**.

Datový analytik by měl rozumět celému toku dat:

```text
Data Source
→ Ingestion
→ Storage
→ Transformation
→ Analytical Layer
→ Reporting
→ Business Decision
```

Nemusí však spravovat veškerou datovou nebo cloudovou infrastrukturu.

Jeho hlavní odpovědností je:

- pochopení business problému;
- získání potřebných dat;
- kontrola jejich kvality;
- analýza;
- interpretace;
- reporting;
- formulace doporučení.