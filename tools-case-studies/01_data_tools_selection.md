# Case Study 01 — Modern Data Environment and Tools Selection

## Účel případové studie

Cílem případové studie je navrhnout přehledné a udržitelné datové prostředí pro pravidelný management reporting.

Nejde o použití co největšího množství technologií. Každý nástroj má být použit pouze tehdy, pokud má v navrženém datovém toku jasný účel.

## Business kontext

Středně velká obchodní společnost prodává produkty firemním i koncovým zákazníkům.

Management chce v Power BI pravidelně sledovat:
- tržby;
- náklady;
- zisk;
- počet objednávek;
- plnění obchodního plánu;
- výsledky podle období;
- výsledky podle regionu;
- výsledky podle produktu;
- výsledky podle zákaznického segmentu.

Dashboard se má automaticky aktualizovat každý pracovní den ráno.

## Datové zdroje

### 1. ERP systém — Microsoft SQL Server

ERP databáze obsahuje:
- objednávky;
- zákazníky;
- produkty;
- prodejní ceny;
- náklady;
- regiony;
- data vytvoření objednávek.

Data se v průběhu dne průběžně aktualizují.

### 2. Obchodní plán — Excel

Soubor `sales_plan.xlsx` obsahuje:
- měsíc;
- region;
- produkt;
- plánované tržby.

Soubor jednou měsíčně aktualizuje obchodní oddělení.

### 3. CRM systém — REST API

CRM API poskytuje:
- identifikátor zákazníka;
- zákaznický segment;
- stav zákazníka;
- datum poslední aktivity.

Data v CRM se mohou měnit každý den.

## Současný proces

Jednotliví analytici si data připravují samostatně.

```text
SQL Server
→ ruční export do CSV

CRM
→ ruční export do CSV

Excel obchodního plánu
→ ruční kopírování

CSV a Excel soubory
→ samostatné úpravy jednotlivých analytiků
→ různé Power BI reporty
```

## Současné problémy

- proces obsahuje mnoho ručních kroků;
- reporty nemusí používat stejná aktuální data;
- stejné KPI mohou mít v různých reportech rozdílné výsledky;
- transformační logika se opakuje na více místech;
- není jasné, která verze dat je správná;
- změna zdroje nebo business pravidla vyžaduje úpravu více reportů;
- CRM data se do reportů dostávají nepravidelně;
- není jednoznačně určená odpovědnost jednotlivých nástrojů.

## Dostupné nástroje

Společnost již používá:
- Microsoft SQL Server;
- Excel;
- Power Query;
- Power BI;
- DAX.

Analytický tým může používat také:
- Python;
- Pandas;
- REST API;
- CSV;
- Parquet.

Společnost momentálně nepoužívá:
- Spark;
- Databricks;
- cloudový lakehouse.

Novou technologii lze navrhnout pouze tehdy, pokud přinese jasný přínos a stávající nástroje požadavek nedokážou rozumně splnit.

## Požadavky na cílové řešení

Navržené řešení musí:
- omezit ruční exporty a kopírování;
- zajistit pravidelnou aktualizaci dat;
- zachovat původní zdrojová data;
- sjednotit transformační a business logiku;
- umožnit kontrolu kvality dat;
- vytvořit důvěryhodný zdroj pro Power BI;
- jasně rozdělit odpovědnost mezi jednotlivé nástroje;
- zůstat přiměřené velikosti společnosti a dostupnému týmu.

## Modern Data Environment Map

Při návrhu použijeme následující základní mapu datového prostředí:

```text
Data Sources
→ Ingestion
→ Storage
→ Transformation
→ Analytical Layer
→ Reporting
→ Business Decision
```

### Data Sources

Systémy a soubory, ve kterých data původně vznikají.

### Ingestion

Proces získání dat ze zdrojových systémů.

### Storage

Místo, kde jsou získaná data ukládána.

### Transformation

Čištění, spojování, validace a úprava dat.

### Analytical Layer

Vrstva připravená pro analýzu a jednotné business metriky.

### Reporting

Dashboardy, reporty a další analytické výstupy.

### Business Decision

Rozhodnutí provedená na základě výsledků analýzy.

## Úkol 1 — zhodnocení současného prostředí

Odpověz vlastními slovy na následující otázky:
1. Jaké datové zdroje společnost používá?
2. Které části současného procesu jsou ruční?
3. Proč mohou různé reporty zobrazovat rozdílné hodnoty stejných KPI?
4. Kde podle tebe vzniká největší riziko chyb?
5. Proč nestačí pouze vytvořit další samostatný Power BI report?

## Úkol 1 — řešení

### 1. Datové zdroje

Společnost používá tři hlavní datové zdroje:
- Microsoft SQL Server - objednávky, zákazníci, produkty, ceny, náklady a regiony;
- Excel - měsíční obchodní plán podle regionu a produktu;
- CRM REST API - zákaznické segmenty, stav zákazníků a poslední aktivita.

Každý zdroj má jiný formát, způsob aktualizace a frekvenci změn.

### 2. Ruční části procesu

Současný proces obsahuje několik ručních kroků:
- export dat z SQL Serveru do CSV;
- export CRM dat do CSV;
- kopírování dat z Excelu;
- ruční úpravy souborů;
- samostatné vytváření transformační logiky v jednotlivých Power BI reportech;
- nepravidelné aktualizace vstupních dat.

Tyto kroky zvyšují časovou náročnost a riziko lidské chyby.

### 3. Rozdílné hodnoty KPI

Různé reporty mohou zobrazovat rozdílné hodnoty stejných KPI, protože mohou používat:
- jinou verzi zdrojových dat;
- jiný čas poslední aktualizace;
- rozdílné filtry;
- rozdílné transformační postupy;
- rozdílnou definici stejné metriky;
- ručně upravené soubory.

### 4. Největší rizika chyb

Největší riziko vzniká při ruční práci a při opakování stejné logiky na více místech.

Není jednoznačně určeno:
- která verze dat je správná;
- zda byla data správně aktualizována;
- zda všechny reporty používají stejná pravidla;
- kde probíhá validace dat;
- kdo odpovídá za opravu chyby.

Výsledkem může být několik rozdílných verzí "pravdy".

### 5. Proč nestačí další Power BI report

Vytvoření dalšího samostatného reportu by nevyřešilo příčinu problému.

Nový report by pravděpodobně znovu obsahoval:
- vlastní kopii dat;
- vlastní transformační logiku;
- vlastní definice KPI;
- vlastní proces aktualizace.

Je proto nutné nejdříve navrhnout společný datový proces a důvěryhodnou analytickou vrstvu. Teprve nad ní mají vznikat jednotlivé Power BI reporty.

## Úkol 2 — volba základní architektury

Byla zvolena centrální příprava dat.

```text
SQL Server ─┐
Excel ──────┼→ centrální uložení a transformace
CRM API ────┘
             → společná analytická vrstva
             → Power BI
```

Samostatné zpracování zdrojů v každém Power BI reportu by vedlo k opakování transformační logiky a k rozdílným výsledkům napříč společností.

Centrální řešení umožní:
- sjednotit transformační pravidla;
- definovat společné KPI;
- zavést pravidelnou validaci dat;
- určit odpovědnost za datový proces;
- vytvořit důvěryhodný zdroj pro všechny reporty;
- snížit množství ručních zásahů.

Součástí řešení musí být také data governance, tedy jasné určení odpovědnosti za business definice, technické zpracování, kvalitu dat a reporting.

### Centrální uložení dat

Data z Excelu a CRM API budou načítána do staging tabulek v existujícím SQL Serveru.

```text
Excel
→ staging.sales_plan

CRM API
→ staging.crm_customers
```

Staging tabulky představují vstupní databázovou vrstvu. Uchovávají data získaná z externích zdrojů před jejich čištěním, validací a propojením s ostatními daty.

Následné zpracování proběhne v jednom SQL prostředí:

```text
Staging
→ kontrola kvality
→ cleaning
→ joiny
→ business transformace
→ agregace
→ reporting vrstva
```

Navržené SQL vrstvy:

```text
staging
→ vstupní data z externích zdrojů

core
→ vyčištěná a propojená data

reporting
→ data připravená pro Power BI
```

Centrální uložení omezí množství samostatných souborů, sjednotí transformační logiku a vytvoří jeden důvěryhodný zdroj pro reporting.

### Ingestion externích zdrojů

Centrálním místem pro uložení a transformaci dat zůstane Microsoft SQL Server.

ERP data jsou již v SQL Serveru. Data z Excelu a CRM API je však nejdříve nutné do centrálního prostředí přenést.

Python bude použit pouze jako ingestion nástroj:
```text
Excel
→ Python
→ staging.sales_plan
```

```text
CRM REST API
→ Python
→ staging.crm_customers
```

Python zajistí:
- načtení Excel souboru;
- získání dat z CRM API;
- kontrolu úspěšnosti API requestu;
- základní kontrolu očekávané struktury;
- zápis získaných dat do staging tabulek.

Po uložení dat do SQL Serveru role Pythonu končí.

Business cleaning, joiny, transformace a agregace budou provedeny centrálně v SQL Serveru.

Navržený proces odpovídá principu ELT:
```text
Extract
→ Python získá externí data

Load
→ Python uloží data do SQL staging

Transform
→ SQL Server data vyčistí, propojí a připraví
```

Python lze v budoucnu nahradit specializovaným ingestion nástrojem, aniž by se změnila centrální role SQL Serveru.

### Rozdělení výpočtů mezi SQL Server a DAX

Stabilní business pravidla a základní hodnoty budou připraveny centrálně v SQL Serveru.

SQL Server zajistí například:
- výpočet čisté tržby;
- výpočet nákladů;
- správné přiřazení zákazníka, produktu a regionu;
- propojení skutečných výsledků s obchodním plánem;
- jednotnou granularitu dat.

DAX bude použit pro dynamické metriky reagující na filtry a slicery:
- celkové tržby;
- celkové náklady;
- celkový zisk;
- ziskovou marži;
- obchodní plán;
- absolutní odchylku od plánu;
- procentní plnění plánu;
- meziroční změnu.

```text
SQL Server
→ připravená a konzistentní data

Power BI semantic model + DAX
→ dynamické business metriky

Power BI report
→ vizualizace a interakce
```

Výpočty se nebudou opakovat samostatně v každém reportu. Společné DAX míry budou součástí centrálního Power BI sémantického modelu.

### Společný Power BI sémantický model

Power BI reporty budou používat jeden společný sémantický model.

Sémantický model bude obsahovat:
- společné tabulky;
- vztahy mezi tabulkami;
- kalendářní tabulku;
- jednotné DAX míry;
- business názvy;
- formáty hodnot;
- případná přístupová pravidla.

Jednotlivé reporty budou vytvořeny jako thin reports.

```text
Společný sémantický model
├── Management dashboard
├── Sales report
├── Regional report
└── Product report
```

Thin report obsahuje především vizualizace a používá existující centrální model. Nevytváří vlastní kopii transformační logiky a DAX metrik.

Tento přístup zajistí:
- konzistentní KPI;
- jednotné vztahy;
- jednodušší údržbu;
- menší množství duplicitní logiky;
- centrální řízení změn;
- důvěryhodnější reporting.

Změny společného modelu musí být řízené a otestované, protože mohou ovlivnit více reportů současně.

### Orchestrace a pořadí aktualizace

Automatický datový proces musí probíhat v přesně určeném pořadí:

```text
1. Ingestion

Excel + CRM API
→ načtení do staging tabulek
```

```text
2. Transformation

SQL Server
→ kontrola kvality
→ cleaning
→ joiny
→ business transformace
→ příprava reporting vrstvy
```

```text
3. Reporting refresh

Power BI
→ aktualizace společného sémantického modelu
→ zpřístupnění aktuálních dat reportům
```

Řízení pořadí a závislostí jednotlivých kroků se označuje jako orchestrace.

Power BI refresh se nesmí spustit dříve, než jsou dokončeny ingestion a SQL transformace. Jinak by mohl načíst stará, neúplná nebo nekonzistentní data.

### Zpracování chyb a monitoring

Pokud některý zdroj nebo krok datového procesu selže, navazující kroky se nesmí automaticky považovat za úspěšné.

Například při nedostupnosti CRM API proces:
1. zachová poslední validní CRM data;
2. nesmaže staging tabulku;
3. označí aktuální načítání jako neúspěšné;
4. zapíše informace o chybě do logu;
5. zastaví navazující SQL transformace a Power BI refresh;
6. odešle upozornění odpovědné osobě;
7. umožní opakované spuštění procesu.

```text
CRM API failure
→ zachování posledních validních dat
→ zápis chyby
→ zastavení závislých kroků
→ upozornění
→ oprava a nové spuštění
```

Součástí reportingu by měla být také informace o aktuálnosti dat, například datum a čas posledního úspěšného načtení.

Tím se zabrání tomu, aby uživatelé považovali stará nebo neúplná data za aktuální.

## Cílová mapa datového prostředí

### Data Sources

```text
ERP v Microsoft SQL Serveru
+
Excel s obchodním plánem
+
CRM REST API
```

Zdroje obsahují provozní, plánovací a zákaznická data potřebná pro management reporting.

### Ingestion

```text
Python
```

Python zajišťuje:
- načtení Excel souboru;
- získání dat z CRM REST API;
- základní technickou kontrolu vstupu;
- zápis externích dat do SQL staging tabulek.

ERP data již v SQL Serveru jsou, a proto pro ně není nutný export do CSV ani další ingestion přes Python.

### Storage

```text
Microsoft SQL Server
```

SQL Server představuje centrální úložiště.

Data jsou rozdělena do vrstev:

```text
staging
→ vstupní data z externích zdrojů

core
→ vyčištěná a propojená data

reporting
→ data připravená pro analytickou vrstvu
```

### Transformation

Hlavní centrální transformace probíhá v SQL Serveru.

```text
SQL Server
→ validace
→ cleaning
→ joiny
→ business pravidla
→ příprava reporting vrstvy
```

Power Query zajišťuje pouze závěrečnou přípravu specifickou pro Power BI:
- kontrolu datových typů;
- kontrolu názvů sloupců;
- kontrolu chyb a neočekávaných `null`;
- případné přejmenování sloupců;
- jednoduché úpravy potřebné pouze pro konkrétní model.

Centrální SQL transformace se v Power Query znovu neopakují.

### Analytical Layer

```text
Společný Power BI sémantický model
+
DAX
```

Sémantický model obsahuje:
- tabulky;
- vztahy;
- kalendářní tabulku;
- společné business názvy;
- jednotné DAX míry;
- formáty hodnot;
- případná přístupová pravidla.

DAX zajišťuje dynamické metriky reagující na filtry a slicery.

### Reporting

```text
Power BI thin reports
```

Jednotlivé reporty používají stejný sémantický model, ale mohou poskytovat různé pohledy pro management, obchodní oddělení, regionální manažery nebo produktové týmy.

### Business Decision

```text
Management
+
oprávnění interní business uživatelé
```

Interní zákazníci používají reporty pro:
- sledování obchodních výsledků;
- kontrolu plnění plánu;
- porovnávání regionů a produktů;
- identifikaci problémů a příležitostí;
- přijímání obchodních rozhodnutí.

### Kompletní cílový datový tok

```text
Data Sources
→ ERP SQL Server + Excel + CRM API

Ingestion
→ Python

Storage
→ SQL Server staging + core + reporting

Transformation
→ SQL Server + minimální Power Query

Analytical Layer
→ společný Power BI sémantický model + DAX

Reporting
→ Power BI thin reports

Business Decision
→ management a interní business uživatelé
```

## Nástroje, které nebyly vybrány

Součástí tool selection není pouze výběr používaných nástrojů. Důležité je také zdůvodnit, proč některé dostupné technologie nejsou pro konkrétní řešení potřeba.

### CSV

CSV nebude používáno jako mezikrok mezi SQL Serverem a Power BI.

```text
SQL Server
→ CSV
→ Power BI
```

Tato cesta by přidávala:
- ruční nebo dodatečný export;
- další kopii dat;
- riziko neaktuálních souborů;
- ztrátu přesných datových typů;
- další krok při aktualizaci;
- zbytečné nároky na disk a přenos dat.

Power BI se může připojit přímo k připravené reporting vrstvě v SQL Serveru.

### Parquet

Parquet je efektivní analytický formát, ale v tomto řešení pro něj není jasná potřeba.

Společnost již používá SQL Server jako centrální úložiště a Power BI se k němu může přímo připojit.

Přidání Parquetu by vytvořilo další souborovou vrstvu, kterou by bylo nutné:
- vytvářet;
- aktualizovat;
- ukládat;
- zabezpečit;
- spravovat.

Parquet by mohl být vhodný například pro datový lake, archivaci, předávání větších analytických datasetů nebo lakehouse. Tyto požadavky ale v současném zadání nejsou.

### Pandas

Pandas nebude použit pro hlavní business transformace, joiny ani výpočty KPI.

Jeho role je omezená na podporu ingestion procesu:
- načtení Excel souboru;
- převod vstupních dat do tabulkové struktury;
- základní technická kontrola;
- příprava dat pro zápis do SQL staging.

CRM API získává Python například pomocí knihovny `requests`. Pandas může následně převést získaný JSON do DataFrame.

```text
Excel + CRM API
→ Python / Pandas ingestion
→ SQL staging
```

Po zápisu do staging vrstvy probíhá centrální cleaning, spojování a business transformace v SQL Serveru.

### Spark

Spark nebude použit, protože současné požadavky lze splnit pomocí dostupného SQL Serveru a Power BI.

Před zavedením distribuovaného zpracování je vhodné nejdříve:
- omezit ruční exporty;
- centralizovat data;
- optimalizovat SQL dotazy;
- načítat pouze potřebná data;
- odstranit duplicitní transformační logiku.

Spark by přidal infrastrukturu a složitost bez jasného přínosu.

### Databricks

V současném řešení není potřeba, protože:
- společnost již má centrální SQL Server;
- nevytváří lakehouse;
- požadavky nevyžadují distribuované zpracování;
- současné nástroje zvládnou požadovaný objem a způsob zpracování;
- zavedení nové platformy by zvýšilo náklady a složitost.

Databricks by se zvažoval až při vzniku požadavků, které SQL Server a současná architektura nedokážou rozumně splnit.

## Finální doporučení

Navržené řešení používá pouze nástroje, které mají jasnou odpovědnost:

```text
ERP SQL Server
+
Excel
+
CRM REST API
→ datové zdroje

Python
→ ingestion Excelu a CRM API

SQL Server
→ centrální storage
→ staging, core a reporting vrstvy
→ cleaning, validace, joiny a business transformace

Power Query
→ minimální závěrečná kontrola pro Power BI

Power BI sémantický model + DAX
→ společná analytická vrstva a dynamické KPI

Power BI thin reports
→ reporting pro různé skupiny uživatelů

Management a business uživatelé
→ rozhodování
```

CSV, Parquet, Spark a Databricks nebyly použity, protože by v současném řešení nepřinesly dostatečný přínos oproti přidané složitosti.