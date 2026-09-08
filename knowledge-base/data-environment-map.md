# Modern Data Environment Map

Praktický přehled základních vrstev moderního datového prostředí, rolí analytických nástrojů a odpovědností datových profesí.

---

## 1. Základní datový tok

Moderní datové prostředí lze zjednodušeně znázornit takto:

```text
Data Sources
→ Data Ingestion
→ Data Storage
→ Data Transformation
→ Analytical Layer
→ Reporting
→ Business Decision
```

České označení:

```text
Datové zdroje
→ získání a přenos dat
→ uložení dat
→ transformace dat
→ analytická vrstva
→ reporting
→ business rozhodnutí
```

Jedná se o orientační mapu jednotlivých vrstev. Skutečné pořadí některých operací se může lišit například podle použití ETL nebo ELT.

---

## 2. Data Sources

**Data sources** jsou místa, kde data vznikají nebo odkud je získáváme.

Typické datové zdroje:

- SQL databáze
- Excel
- CSV
- JSON
- externí API
- CRM
- ERP
- e-shop
- účetní systém
- cloudové úložiště
- webové nebo interní aplikace

Datový zdroj nemusí být připravený přímo pro analýzu. Provozní databáze například slouží primárně k zaznamenávání transakcí, nikoli k management reportingu.

### Příklad

```text
SQL databáze → objednávky a zákazníci
Excel → regionální rozpočty
API → měnové kurzy
CSV → historická marketingová data
```

---

## 3. Data Ingestion

**Data ingestion** znamená získání a přesunutí dat ze zdrojového systému do prostředí, kde budou uložena nebo dále zpracována.

Může zahrnovat:

```text
Extract
→ Transfer
→ Load
```

- **Extract** – získání dat ze zdroje
- **Transfer** – přenos dat
- **Load** – zápis dat do cílového prostředí

V praxi se někdy pojem ingestion používá pouze pro získání dat a jindy pro celý proces jejich přenosu do cílového úložiště.

### Příklady

```python
df = pd.read_csv("sales.csv")
```

Načtení dat ze souboru do DataFrame.

```python
response = requests.get(url)
```

Získání dat z API.

```sql
SELECT *
FROM sales;
```

Získání dat z databázové tabulky.

Power Query může získat data například z:

- SQL databáze
- Excelu
- CSV
- webu
- cloudového úložiště

### Hlavní otázka

> Jak dostaneme data ze zdroje do další části datového procesu?

---

## 4. Load vs. Storage

Loading a storage nejsou totéž.

### Load

**Load** je činnost, při které se data zapisují do cílového prostředí.

Například:

```text
Python zapíše data do SQL databáze.
```

Python v tomto případě provádí loading.

### Storage

**Storage** je místo, kde jsou data uchovávána.

Například:

```text
SQL databáze uchovává uložená data.
```

SQL databáze v tomto případě představuje storage.

### Základní rozdíl

```text
Load = zápis dat
Storage = místo uchování dat
```

---

## 5. Data Storage

**Data storage** označuje systémy nebo formáty, ve kterých jsou data uložena.

Příklady:

- relační databáze
- data warehouse
- data lake
- lakehouse
- cloudové objektové úložiště
- CSV
- JSON
- Parquet
- Excel

Volba úložiště závisí například na:

- objemu dat
- typu dat
- frekvenci aktualizace
- počtu uživatelů
- požadavku na uchování historie
- výkonu
- bezpečnosti
- navazujících analytických nástrojích

### Hlavní otázka

> Kde budou data bezpečně, systematicky a dlouhodobě uložena?

---

## 6. Data Transformation

**Data transformation** znamená úpravu dat do podoby vhodné pro další analýzu nebo reporting.

Typické transformace:

- změna datových typů
- odstranění duplicit
- práce s chybějícími hodnotami
- standardizace textu
- validace hodnot
- filtrace
- spojování tabulek
- výpočet nových sloupců
- převod měn
- agregace
- tvorba business kategorií
- příprava faktových a dimenzních tabulek

Transformace mohou probíhat v různých nástrojích:

| Nástroj | Typické transformační využití |
|---|---|
| SQL | filtrace, JOIN, agregace a výpočty v databázi |
| Power Query | načtení a příprava dat pro Excel nebo Power BI |
| Python/pandas | komplexnější čištění, validace a business logika |
| PySpark | distribuované transformace větších datasetů |

### Důležitá zásada

> Transformace má proběhnout tam, kde je efektivní, kontrolovatelná a opakovatelná.

Jedna transformační logika by neměla být bezdůvodně rozdělena mezi příliš mnoho nástrojů.

---

## 7. Analytical Layer

**Analytical layer** neboli analytická vrstva obsahuje data, výpočty a pravidla připravená pro analýzu a reporting.

Může zahrnovat:

- faktové tabulky
- dimenzní tabulky
- vztahy mezi tabulkami
- hvězdicové schéma
- hierarchie
- agregace
- KPI
- business metriky
- DAX míry
- sémantický model

### Příklad v Power BI

```text
fact_sales
dim_customer
dim_product
dim_date
```

Analytická vrstva dále obsahuje:

- vztahy mezi tabulkami
- správné datové typy
- připravené hierarchie
- DAX míry
- pravidla filtrování
- formátování business ukazatelů

---

## 8. Relationships, Data Model and Semantic Model

### Relationships

**Relationships** jsou vztahy mezi tabulkami.

Například:

```text
dim_product[product_id] 1 ─── * fact_sales[product_id]
```

Vztah určuje například:

- spojovací sloupce
- kardinalitu
- směr filtrování
- zda je vztah aktivní

Vztahy umožňují, aby výběr hodnoty z jedné tabulky správně filtroval data v jiné tabulce.

### Data Model

**Data model** je celá organizovaná struktura dat.

Obsahuje například:

```text
Data Model
├── tables
├── columns
├── data types
├── relationships
├── measures
├── calculated columns
└── hierarchies
```

Vztahy jsou tedy pouze jednou součástí datového modelu.

### Semantic Model

**Semantic model** neboli sémantický model je datový model připravený pro business analýzu a reporting.

Slovo **semantic** znamená významový.

Sémantický model dává technickým datům business význam a definuje například:

- co představují jednotlivé tabulky
- jak spolu tabulky souvisejí
- co znamenají jednotlivé ukazatele
- jak se počítají tržby, zisk nebo marže
- jak se mají hodnoty formátovat
- které technické sloupce mají být skryté
- kdo může vidět určitá data

### Příklad

Technické názvy:

```text
qty
unit_prc
cost_amt
cust_id
```

Business názvy:

```text
Quantity
Unit Price
Revenue
Cost
Customer
```

Připravené DAX míry:

```DAX
Total Revenue =
SUM(Sales[Revenue])
```

```DAX
Profit Margin =
DIVIDE(
    [Total Profit],
    [Total Revenue]
)
```

### Praktické rozlišení

| Pojem | Význam |
|---|---|
| Relationship | propojení dvou tabulek |
| Data model | celá struktura tabulek, vztahů a výpočtů |
| Semantic model | model doplněný o business význam a připravený pro reporting |
| Report | vizuální prezentace dat ze sémantického modelu |

V Power BI se pojmy **data model** a **semantic model** často používají téměř zaměnitelně.

Rozdíl je především v důrazu:

- datový model zdůrazňuje strukturu
- sémantický model zdůrazňuje business význam a způsob použití

---

## 9. Reporting

**Reporting** je prezentační vrstva určená uživatelům analytických výstupů.

Výstupem může být:

- Power BI dashboard
- Power BI report
- Excel report
- tabulka
- graf
- management summary
- prezentace
- pravidelně distribuovaný analytický soubor

Reporting odpovídá na otázku:

> Jak uživateli předáme srozumitelnou a použitelnou informaci?

Dobrý report nemá pouze zobrazovat data. Má podporovat konkrétní rozhodování.

---

## 10. Business Decision

Poslední částí analytického procesu není dashboard, ale business rozhodnutí.

Management může na základě reportu například:

- upravit regionální rozpočty
- zvýšit investice do úspěšného produktu
- prověřit pokles marže
- změnit cenovou strategii
- omezit ztrátovou kategorii
- změnit marketingovou kampaň

Celý datový proces směřuje k výsledku:

```text
Data
→ Information
→ Insight
→ Decision
```

Technicky správný datový proces bez použitelného business výstupu nepřináší dostatečnou hodnotu.

---

## 11. Role jednotlivých nástrojů

Stejný nástroj může v datovém procesu plnit více rolí.

Rozhodující není pouze název nástroje, ale konkrétní operace, kterou provádí.

### SQL

| Použití SQL | Role |
|---|---|
| Čtení dat z databáze | Extract / ingestion |
| `WHERE`, `JOIN`, `GROUP BY` | Transformation |
| Příprava analytického pohledu | Transformation / analytical layer |
| Výpočet agregovaných výsledků | Transformation / analytical layer |

SQL je dotazovací jazyk. Samotné úložiště poskytuje databázový systém, například SQL Server, PostgreSQL nebo SQLite.

### Python and pandas

| Použití | Role |
|---|---|
| `requests.get()` | Extract / ingestion |
| `pd.read_csv()` | Extract / ingestion |
| čištění a validace | Transformation |
| `merge()` a `groupby()` | Transformation |
| zápis do databáze | Load |
| export do CSV nebo Excelu | Load / delivery |
| EDA a statistická analýza | Analytics |

Python může propojit více částí procesu, ale obvykle není dlouhodobým datovým úložištěm.

```text
DataFrame = pracovní reprezentace dat v paměti
Databáze nebo soubor = trvalejší storage
```

### Power Query

| Použití | Role |
|---|---|
| připojení k datovému zdroji | Extract / ingestion |
| změna datových typů | Transformation |
| odstranění chyb a duplicit | Transformation |
| spojení tabulek | Transformation |
| načtení výsledku do modelu | Load |

Power Query lze zjednodušeně popsat jako:

```text
Connect
→ Transform
→ Load
```

### Power BI

| Součást Power BI | Hlavní role |
|---|---|
| Power Query | ingestion a transformation |
| Datový model | analytical layer |
| Vztahy | analytical layer |
| DAX míry | analytical layer |
| Vizualizace | reporting |
| Publikovaný report | reporting / delivery |

Přesnější popis práce v Power BI:

```text
Power Query
→ příprava dat

Semantic Model
→ tabulky, vztahy a business logika

DAX
→ dynamické ukazatele

Report
→ vizuální prezentace výsledků
```

### Excel

Excel může sloužit jako:

- datový zdroj
- jednoduché úložiště
- transformační nástroj
- analytický nástroj
- reportingový nástroj

U menších procesů to může být praktické. Riziko vzniká, pokud jeden soubor současně obsahuje:

```text
raw data
+ ruční úpravy
+ výpočty
+ analýzu
+ finální report
```

Takový proces bývá obtížněji kontrolovatelný, sdílený a automatizovaný.

---

## 12. Typický analytický proces

### Business situace

Obchodní firma používá:

- SQL databázi s objednávkami
- Excel s regionálními rozpočty
- API s měnovými kurzy
- Power BI pro management reporting

### Možný datový tok

```text
SQL + Excel + API
→ načtení dat
→ uložení
→ validace a transformace
→ analytický model
→ Power BI report
→ rozhodnutí managementu
```

### Rozdělení rolí

| Vrstva | Příklad |
|---|---|
| Data Sources | SQL, Excel a API |
| Ingestion | SQL dotaz, Power Query a Python `requests` |
| Storage | analytická databáze nebo datové soubory |
| Transformation | SQL, Python nebo Power Query |
| Analytical Layer | Power BI model, vztahy a DAX |
| Reporting | Power BI report |
| Business Decision | úprava rozpočtu nebo obchodní strategie |

Nejde o jedinou správnou kombinaci. Konkrétní řešení závisí na business potřebě, objemu dat, frekvenci aktualizace a dostupné infrastruktuře.

---

## 13. Data Roles

### Data Engineer

Data engineer se zaměřuje především na:

- ingestion
- storage
- datové pipeline
- rozsáhlejší transformace
- orchestration
- výkon
- monitoring
- spolehlivost datových procesů

### Data Analyst

Datový analytik se zaměřuje především na:

- pochopení business problému
- formulaci analytických otázek
- získání potřebných dat
- validaci dat
- analýzu
- interpretaci
- reporting
- formulaci doporučení

Analytik může pracovat také s ingestion a transformacemi pomocí SQL, Power Query nebo Pythonu. Obvykle však nespravuje celou datovou infrastrukturu.

### BI Developer

BI developer se zaměřuje především na:

- přípravu dat pro reporting
- návrh datového modelu
- vztahy mezi tabulkami
- DAX
- výkon modelu
- tvorbu Power BI reportů
- publikování a distribuci reportingu

### Překryv rolí

| Činnost | Data Engineer | Data Analyst | BI Developer |
|---|:---:|:---:|:---:|
| SQL | ✓ | ✓ | ✓ |
| Kontrola dat | ✓ | ✓ | ✓ |
| Python/pandas | ✓ | ✓ | někdy |
| Datové pipeline | ✓ | někdy | někdy |
| Power BI model | někdy | někdy | ✓ |
| DAX | méně často | ✓ | ✓ |
| Business interpretace | někdy | ✓ | ✓ |
| Cloudová infrastruktura | ✓ | základní orientace | základní orientace |

Hranice mezi rolemi nejsou pevné.

V menší firmě může jeden člověk zastávat více rolí. Ve větší organizaci bývají odpovědnosti více oddělené.

---

## 14. Key Principles

### 1. Nástroj není totéž co role

Jeden nástroj může provádět více operací.

```text
Python
→ ingestion
→ transformation
→ load
→ analytics
```

### 2. Loading není storage

```text
Load = zápis dat
Storage = místo uchování dat
```

### 3. Vztahy nejsou celý datový model

Vztahy jsou pouze jednou ze součástí modelu.

### 4. Sémantický model dává datům business význam

Obsahuje tabulky, vztahy, metriky, názvy a pravidla připravená pro uživatele reportu.

### 5. Ne všechny transformace mají probíhat všude

Je vhodné určit, která logika patří do:

- SQL
- Pythonu
- Power Query
- datového modelu
- DAX

### 6. Datový analytik má rozumět celému toku

Nemusí spravovat všechny technologie, ale měl by rozumět tomu:

- odkud data přicházejí
- kde jsou uložena
- kde se transformují
- jak vznikají KPI
- jak se dostávají do reportu
- jaké rozhodnutí mají podporovat

### 7. Cílem není dashboard, ale rozhodnutí

```text
Business Problem
→ Data
→ Analysis
→ Insight
→ Recommendation
→ Decision
```

---

## 15. Essential Terminology

| Termín | Význam |
|---|---|
| Data Source | místo, kde data vznikají |
| Extract | získání dat ze zdroje |
| Transfer | přenos dat |
| Load | zápis dat do cílového prostředí |
| Data Ingestion | získání a přesun dat |
| Data Storage | systém nebo místo uchování dat |
| Transformation | úprava dat |
| Analytical Layer | data a logika připravené pro analýzu |
| Relationship | propojení tabulek |
| Data Model | struktura tabulek, vztahů a výpočtů |
| Semantic Model | datový model doplněný o business význam |
| Measure | dynamický výpočet nad daty |
| Reporting | prezentace analytických výsledků |
| Business Decision | rozhodnutí založené na výsledku analýzy |
| Data Pipeline | posloupnost kroků pro přesun a zpracování dat |
| Orchestration | řízení pořadí a spouštění datových kroků |

---

## Summary

Základní mapa moderního datového prostředí:

```text
Data Sources
→ Data Ingestion
→ Data Storage
→ Data Transformation
→ Analytical Layer
→ Reporting
→ Business Decision
```

Přesnější pohled na přesun dat:

```text
Extract
→ Transfer
→ Load
→ Storage
```

Typické rozdělení nástrojů:

```text
SQL
→ získání, filtrace, spojování a agregace dat

Python/pandas
→ ingestion, validace, transformace a analýza

Power Query
→ připojení, transformace a načtení dat

Power BI Semantic Model
→ vztahy, business logika a DAX

Power BI Report
→ vizualizace a reporting
```

Hlavní princip:

> Správný nástroj se nevybírá podle toho, kolik funkcí nabízí, ale podle role, kterou má v konkrétním analytickém procesu plnit.