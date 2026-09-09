# Case study 01 - Large Data Strategy

## Účel případové studie



## Business konext

Obchodní společnost ukládá údaje o prodejích do databáze Microsoft SQL Server.

Tabulka `sales` obsahuje:

- 80 milionů řádků;
- 30 sloupců;
- přibližně 5 milionů nových řádků měsíčně;
- detail jednotlivých prodejních transakcí;
- historii za několik let.

Datový analytik pracuje na počítači s 16 GB RAM.

## Výchozí situace

Současný proces vypadá následovně:

```text
SQL Server
→ export celé tabulky do CSV
→ načtení CSV do Pandas
→ základní filtrování a agregace
→ export výsledku
→ načtení do Power BI
```

Při zpracování se objevují tyto problémy:

- CSV soubor je velmi velký;
- načítání trvá dlouho;
- DataFrame se nevejde do dostupné paměti;
- Power BI refresh je pomalý;
- každý měsíc je nutné zpracovat více dat.

## Požadavky na reporting

Management potřebuje v Power BI sledovat:

- měsíční tržby;
- počet objednávek;
- tržby podle regionu;
- tržby podle produktu;
- vývoj za posledních 24 měsíců.

Pro hlavní management dashboard nejsou potřeba všechny detailní transakce ani všech 30 sloupců.

Analytici však mohou příležitostně potřebovat detail objednávek za posledních 90 dní.

## Úkol 1 — zhodnocení současného procesu

Urči hlavní slabiny současného procesu.

Odpověz vlastními slovy na následující otázky:

1. Proč je problematický export celé tabulky do CSV?
2. Proč není vhodné načítat všech 30 sloupců?
3. Proč není správné dělat veškeré filtrování až v Pandas?
4. Proč bychom neměli automaticky přejít rovnou na Spark?

## Task 1 — Řešení

### 1. Export celé tabulky do CSV

Tabulka obsahuje 80 milionů řádků a 30 sloupců. Export celé tabulky proto vytváří velmi velký soubor, jehož přenos, uložení a následné načítání jsou náročné na čas a paměť.

CSV je navíc textový formát, který:
- obvykle zabírá více místa než komprimovaný Parquet;
- neuchovává spolehlivě původní datové typy;
- není optimalizovaný pro výběr jednotlivých sloupců;
- vytváří zbytečný mezikrok mezi databází a analytickým nástrojem.

### 2. Načítání všech sloupců

Pro management dashboard nejsou všechny sloupce potřebné. Načítání všech 30 sloupců zvyšuje:
- množství přenášených dat;
- spotřebu operační paměti;
- dobu načítání;
- velikost Power BI modelu.

Pomocí SQL `SELECT` by měly být vybrány pouze sloupce potřebné pro konkrétní výstup.

### 3. Filtrování a agregace až v Pandas

Pokud jsou data uložena v databázi, není efektivní nejprve exportovat celou tabulku do CSV a potom ji filtrovat v Pandas.

Základní operace je vhodné provést již v databázi:
- výběr sloupců;
- filtrování požadovaného období;
- spojení databázových tabulek;
- opakované standardní čištění;
- agregace pro reporting.

Pandas lze použít až pro složitější nebo nestandardní transformace vhodné pro Python.

Pokud další transformace v Pythonu nejsou potřeba, může databáze předat připravený výsledek přímo do Power BI.

```text
Varianta A

SQL Server
→ SQL filtr a agregace
→ Power BI
```

```text
Varianta B

SQL Server
→ SQL filtr a základní transformace
→ Pandas pro komplexnější analýzu
→ Power BI
```

### 4. Předčasný přechod na Spark

Spark přidává další infrastrukturu, režii a složitost. Nejdříve je proto vhodné:
- snížit počet řádků;
- vybrat pouze potřebné sloupce;
- filtrovat a agregovat v databázi;
- zvolit vhodné datové typy;
- odstranit zbytečný export do CSV.

Teprve pokud optimalizovaný proces stále nelze spolehlivě nebo dostatečně rychle zpracovat dostupnými prostředky, dává smysl uvažovat o distribuovaném zpracování.

## Úkol 2 — návrh cílového řešení

Navrhni efektivní datovou cestu pro:

1. management dashboard s měsíčními výsledky za posledních 24 měsíců;
2. analytika, který příležitostně potřebuje detail objednávek za posledních 90 dní.

Při návrhu zvaž:

- požadovanou granularitu dat;
- sloupce potřebné pro reporting;
- rozdělení práce mezi SQL Server, Power Query, DAX a případně Python;
- způsob aktualizace dat;
- vhodnost SQL view;
- důvody, proč v tomto řešení není nutné použít Spark.

## Úkol 2 — řešení

### 1. Management dashboard

Management potřebuje sledovat měsíční výsledky podle regionů a produktů. Pro hlavní dashboard proto není nutná granularita jednotlivých objednávek.

Výsledná granularita bude:

```text
1 řádek
=
1 měsíc
×
1 region
×
1 produkt
```

Tato granularita umožní v Power BI zobrazit:

- celkové tržby;
- tržby podle regionu;
- tržby podle produktu;
- tržby podle kombinace regionu a produktu;
- celkový počet objednávek;
- počet objednávek podle regionu;
- počet objednávek podle produktu;
- počet objednávek podle kombinace regionu a produktu.

### 2. Výstupní sloupce

SQL výstup pro Power BI bude obsahovat pouze potřebné sloupce:

| Sloupec | Význam |
|---|---|
| `month_start` | první den příslušného měsíce |
| `region` | region prodeje |
| `product` | produkt |
| `total_revenue` | součet tržeb |
| `order_count` | počet objednávek |

Měsíc je vhodné uložit jako datum, například `2026-09-01`, nikoliv jako text `2026-09`.

Datum usnadní:

- chronologické řazení;
- filtrování období;
- propojení s kalendářní tabulkou;
- časové výpočty v DAX.

### 3. Kontrola objednávek

Ve scénáři předpokládáme:

```text
1 order_id
=
1 objednávka
=
1 produkt
=
1 zdrojový řádek
```

Unikátnost `order_id` musí být zkontrolována v transformační vrstvě.

```sql
SELECT
    order_id,
    COUNT(*) AS record_count
FROM sales
GROUP BY order_id
HAVING COUNT(*) > 1;
```

Pokud dotaz vrátí záznamy, musí být duplicity před agregací prověřeny a vyřešeny.

Použití:

```sql
COUNT(DISTINCT order_id)
```

vrátí počet unikátních objednávek, ale samo neodstraní duplicitní řádky z ostatních výpočtů. Duplicitní řádky by například stále mohly navýšit `SUM(revenue)`.

Po vyčištění dat budou `COUNT(*)` a `COUNT(DISTINCT order_id)` při stanovené granularitě vracet stejný počet objednávek.

### 4. Časové období

Dashboard se aktualizuje jednou měsíčně po uzavření předchozího měsíce.

Obsahuje posledních 24 dokončených měsíců. Díky tomu nejsou kompletní měsíce porovnávány s neúplným aktuálním měsícem.

Časový filtr se nastaví dynamicky v SQL, aby nebylo nutné ručně měnit konkrétní datum při každém refreshi.

### 5. Rozdělení práce mezi nástroje

#### SQL Server

SQL Server provede:

- kontrolu a standardní čištění dat;
- potřebné `JOIN`;
- výběr požadovaných sloupců;
- dynamický filtr posledních 24 dokončených měsíců;
- agregaci podle měsíce, regionu a produktu;
- výpočet `total_revenue`;
- výpočet `order_count`.

Zpracování proběhne přímo v databázi ještě před předáním výsledku do Power BI.

#### SQL view

Agregovaný výstup bude zpřístupněn prostřednictvím SQL view.

```text
SQL view
=
uložený SQL dotaz
```

Běžné view fyzicky neukládá výsledná data. Jeho dotaz se vyhodnotí ve chvíli, kdy jej Power BI při refreshi načte.

```text
Power BI refresh
→ dotaz na SQL view
→ SQL Server provede filtr a agregaci
→ výsledek se načte do Power BI
```

Nejdříve se použije běžné view a změří se jeho výkon.

Pokud by opakované zpracování 80 milionů řádků bylo příliš pomalé, lze později vytvořit fyzickou agregovanou tabulku. Tu by pravidelně aktualizoval ETL proces, například stored procedure spouštěná pomocí SQL Server Agentu.

#### Power Query

Power Query bude mít v tomto řešení pouze omezenou roli:

- připojení k SQL view;
- kontrola názvů sloupců;
- kontrola datových typů;
- kontrola neočekávaných `null` nebo chybových hodnot;
- případné přejmenování sloupců pro report;
- jednoduché úpravy specifické pouze pro tento report.

Transformace již provedené v SQL se nebudou zbytečně opakovat v Power Query.

#### Power BI model

Power BI model zajistí:

- propojení výsledku s kalendářní tabulkou;
- případné vztahy s dimenzemi produktů a regionů;
- správné formáty hodnot;
- použití dat ve vizualizacích a slicerech.

#### DAX

DAX bude použit pro dynamické metriky reagující na filtry a slicery.

```DAX
Total Revenue =
SUM(SalesSummary[total_revenue])
```

```DAX
Order Count =
SUM(SalesSummary[order_count])
```

Další možné DAX míry:

- meziroční změna tržeb;
- podíl regionu na celkových tržbách;
- podíl produktu na celkových tržbách;
- průměrná hodnota objednávky;
- porovnání skutečnosti s plánem.

### 6. Cílová cesta management dashboardu

```text
SQL Server
→ centrální čištění a kontrola dat
→ potřebné JOIN
→ výběr potřebných sloupců
→ posledních 24 dokončených měsíců
→ agregace měsíc × region × produkt
→ SQL view
→ Power Query
→ Power BI Import model
→ DAX
→ management dashboard
```

Pro tento výstup není Python potřeba, protože požadované filtrování, spojování a agregace lze efektivně provést v SQL.

### 7. Detail objednávek za posledních 90 dní

Analytik může příležitostně potřebovat detail objednávek za posledních 90 dní.

Také v tomto případě není vhodné exportovat celou tabulku do CSV.

SQL Server nejprve provede:

- filtr posledních 90 dní;
- výběr pouze potřebných sloupců;
- potřebné spojení s dalšími tabulkami;
- základní centrální čištění.

Výsledek může analytik podle účelu načíst:

- přímo do Power BI;
- do Excelu nebo Power Query;
- do Pandas pro složitější analýzu.

```text
SQL Server
→ filtr posledních 90 dní
→ výběr potřebných sloupců
→ analytický nástroj
```

Python má smysl pouze tehdy, pokud analytik potřebuje například:

- nestandardní transformaci;
- statistickou analýzu;
- zpracování textu;
- propojení s API;
- automatizovaný analytický výpočet.

### 8. Proč nepoužít CSV jako mezikrok

CSV by v navrženém procesu vytvářelo zbytečnou mezivrstvu:

```text
SQL Server
→ CSV
→ Pandas nebo Power BI
```

Nevýhody této cesty:

- další export a uložení dat;
- delší doba zpracování;
- vyšší nároky na disk;
- nutnost znovu odvozovat datové typy;
- riziko práce s neaktuální kopií;
- složitější aktualizace celého procesu.

Pokud cílový nástroj podporuje připojení k SQL Serveru, je vhodnější načíst připravený výsledek přímo z databáze.

### 9. Proč zatím nepoužít Spark

Původní problém nevzniká pouze kvůli objemu dat. Hlavní příčinou je neefektivní proces, který exportuje a načítá celou databázovou tabulku.

Nejdříve proto provedeme:

- omezení počtu řádků;
- omezení počtu sloupců;
- filtrování v databázi;
- agregaci v databázi;
- odstranění zbytečného CSV mezikroku;
- oddělení management souhrnu a analytického detailu.

Spark bychom zvažovali teprve tehdy, pokud by optimalizované zpracování v SQL Serveru a dostupných nástrojích stále nesplňovalo požadavky na výkon, objem nebo frekvenci zpracování.

## Výsledné doporučení

Pro management dashboard:

```text
SQL Server
→ SQL view s agregovanými daty
→ Power BI Import
→ DAX metriky
```

Pro detailní analýzu posledních 90 dní:

```text
SQL Server
→ filtrovaný detail
→ Power BI, Excel nebo Pandas podle účelu
```

Hlavním přínosem návrhu je odstranění zbytečného exportu celé tabulky do CSV a zpracování dat co nejblíže jejich zdroji.