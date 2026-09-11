# Modern Data Stack Cheatsheet

Praktická mapa datového prostředí, rolí jednotlivých vrstev, odpovědností datových profesí a znalostí potřebných pro datového analytika.

---

## 1. Co je Modern Data Stack

**Modern Data Stack** není jeden konkrétní produkt ani povinná sestava technologií. Je to soubor funkcí, které společně zajišťují cestu dat od jejich vzniku až k analytickému výstupu a business rozhodnutí.

Nejdůležitější není zapamatovat si desítky názvů služeb. Důležité je rozpoznat:

- jaký problém daná část prostředí řeší;
- jaký má vstup a výstup;
- na kterých krocích závisí;
- kdo ji obvykle spravuje;
- jak hluboce jí musí rozumět datový analytik.

Základní datový tok:

```text
Sources
→ Ingestion
→ Storage
→ Transformation
→ Analytics
→ Reporting
→ Business Decision
```

České vyjádření:

```text
datové zdroje
→ získání a přenos dat
→ uložení dat
→ čištění, spojování a výpočty
→ analýza a interpretace
→ prezentace výsledků
→ business rozhodnutí
```

Skutečné prostředí nemusí být vždy dokonale lineární. Některé funkce působí napříč celým datovým tokem.

---

## 2. Hlavní tok a průřezové funkce

### Hlavní datový tok

Hlavní tok popisuje, kudy data procházejí:

```text
Sources
→ Ingestion
→ Storage
→ Transformation
→ Analytics
→ Reporting
```

### Průřezové funkce

Tyto funkce nejsou pouze jednou zastávkou v řetězci:

- **Orchestration** řídí pořadí, spouštění a závislosti úloh;
- **Data Quality** kontroluje správnost dat v různých bodech;
- **Catalog & Governance** popisuje data, vlastníky a pravidla používání;
- **Monitoring** sleduje, zda celý proces funguje správně.

```text
Orchestration
Data Quality
Catalog & Governance
Monitoring

↓ působí napříč datovým tokem ↓

Sources → Ingestion → Storage → Transformation → Analytics → Reporting
```

---

## 3. Sources — datové zdroje

**Sources** jsou místa, kde data vznikají nebo odkud je analytický proces získává.

### Typické zdroje

- ERP;
- CRM;
- e-shop;
- provozní SQL databáze;
- REST API;
- CSV, JSON a Excel soubory;
- aplikační logy;
- externí databáze a služby.

### Vstup a výstup

- **vstup:** provozní události nebo externě poskytnutá data;
- **výstup:** data dostupná pro načtení do analytického procesu.

### Důležitý princip

Role závisí na konkrétním datovém toku. SQL Server fyzicky ukládá data, ale pokud z něj analytická pipeline přebírá objednávky, plní vůči této pipeline roli **source**.

### Typická odpovědnost

Zdroj spravuje vlastník provozního systému, aplikační tým nebo databázový administrátor. Analytik potřebuje znát význam dat, dostupné tabulky a omezení zdroje.

---

## 4. Ingestion — získání a přenos dat

**Data ingestion** znamená získání dat ze zdroje a jejich přenos do další části datového prostředí.

```text
Extract
→ Transfer
→ Load
```

- **Extract** – získání dat ze zdroje;
- **Transfer** – přenos dat;
- **Load** – zápis do cílového prostředí.

### Příklady

- kopírování dat z lokálního SQL Serveru do data lake;
- pravidelné načítání CSV souborů;
- získání dat z API;
- inkrementální načtení pouze nových záznamů;
- přenos událostí z aplikace do analytického prostředí.

### Vstup a výstup

- **vstup:** data ve zdrojovém systému;
- **výstup:** data zapsaná do cílového úložiště nebo předaná ke zpracování.

### Load versus Storage

```text
Load
= činnost, při které se data zapisují

Storage
= místo, kde jsou data uchovávána
```

### Typická odpovědnost

Produkční ingestion obvykle spravuje data engineer. Analytik běžně získává data pomocí SQL, Power Query, Pythonu nebo konektoru v BI nástroji.

---

## 5. Storage — ukládání dat

**Storage** je místo, kde jsou data uchovávána pro další zpracování, analýzu nebo reporting.

### Typy úložišť

- relační databáze;
- data warehouse;
- data lake;
- lakehouse;
- cloudové objektové úložiště;
- analytické tabulky a soubory, například Parquet.

### Vstup a výstup

- **vstup:** načtená nebo transformovaná data;
- **výstup:** trvale dostupná data pro další procesy a uživatele.

### Úroveň připravenosti dat

Jedno prostředí může uchovávat několik úrovní dat:

```text
původní data
→ vyčištěná a validovaná data
→ business-ready data
```

Samotné uložení nezaručuje správnost ani business význam dat.

### Typická odpovědnost

Architekturu úložiště obvykle navrhuje data engineer, datový architekt nebo správce platformy. Analytik musí vědět, kde se nachází správná a aktuální verze dat.

---

## 6. Transformation — transformace dat

**Transformation** převádí zdrojová data do podoby vhodné pro další použití.

### Technické transformace

- sjednocení datových typů;
- odstranění technických duplicit;
- standardizace textových hodnot;
- práce s chybějícími hodnotami;
- kontrola struktury;
- sjednocení formátů data a času.

### Business transformace

- spojování objednávek se zákazníky a produkty;
- výpočet tržeb, nákladů a zisku;
- převod měn;
- tvorba business kategorií;
- agregace;
- příprava faktových a dimenzních tabulek.

### Vstup a výstup

- **vstup:** zdrojová nebo dříve zpracovaná data;
- **výstup:** vyčištěná, spojená nebo business-ready data.

### Příklady nástrojů

- SQL;
- Power Query;
- Python a Pandas;
- PySpark;
- Dataflow Gen2;
- Databricks;
- dbt.

### Důležitý princip

Transformace má probíhat tam, kde je efektivní, kontrolovatelná a opakovatelná. Jedna logika nemá být bezdůvodně rozdělena mezi příliš mnoho nástrojů.

### Typická odpovědnost

Technické a rozsáhlé transformace často spravuje data engineer. Analytik běžně vytváří analytické transformace pomocí SQL, Power Query nebo Pandas a pomáhá definovat business pravidla.

---

## 7. Analytics — analýza a analytická vrstva

**Analytics** používá připravená data k hledání odpovědí na business otázky.

Patří sem například:

- průzkumná analýza dat;
- výpočet a interpretace ukazatelů;
- porovnávání skupin;
- hledání trendů a odchylek;
- statistická analýza;
- příprava analytického modelu;
- tvorba KPI a business metrik.

### Transformation versus Analytics

```text
Transformation
→ připraví tabulku zisku podle regionu

Analytics
→ zjistí, proč určitý region zaostává
   a jaký to má business význam
```

Výpočet ukazatele může být součástí obou oblastí. Za analytiku jej považujeme zejména tehdy, když slouží k odpovědi na otázku a interpretaci výsledku.

### Typická odpovědnost

Analytics je hlavní oblast datového analytika. Na technické podobě analytické vrstvy může spolupracovat s BI developerem a data engineerem.

---

## 8. Reporting — prezentace výsledků

**Reporting** zpřístupňuje výsledky uživatelům v přehledné a opakovaně použitelné podobě.

### Typické výstupy

- Power BI report;
- dashboard;
- pravidelný management report;
- tabulkový přehled;
- KPI karty;
- grafy a interaktivní filtry;
- management summary.

### Vstup a výstup

- **vstup:** připravený analytický nebo sémantický model;
- **výstup:** srozumitelná informace pro konkrétní uživatele.

### Analytics versus Reporting

```text
Analytics
→ hledá odpověď a interpretuje výsledek

Reporting
→ pravidelně prezentuje výsledek uživatelům
```

### Typická odpovědnost

Reporting vytváří datový analytik, BI analytik nebo BI developer. Výstup musí odpovídat konkrétnímu rozhodování, nikoliv pouze zobrazovat dostupná data.

---

## 9. Business Decision — cíl datového procesu

Dashboard není konečným cílem. Analytický proces má podporovat rozhodnutí nebo konkrétní činnost.

```text
Data
→ Information
→ Insight
→ Recommendation
→ Decision
```

Management může na základě výstupu například:

- upravit rozpočet;
- prověřit pokles marže;
- změnit cenovou strategii;
- upravit skladové zásoby;
- podpořit úspěšnou produktovou kategorii;
- změnit obchodní nebo marketingovou aktivitu.

Technicky správný proces bez použitelného business výstupu nepřináší dostatečnou hodnotu.

---

## 10. Orchestration — řízení datového procesu

**Orchestration** řídí, kdy, v jakém pořadí a za jakých podmínek se jednotlivé úlohy spustí.

Řeší například:

- pořadí úloh;
- závislosti mezi úlohami;
- paralelní zpracování nezávislých kroků;
- reakci na chybu;
- podmínky pro pokračování;
- spuštění obnovy reportu až po úspěšné přípravě dat.

```text
načtení
→ validace
→ transformace
→ aktualizace analytických tabulek
→ obnovení reportu
→ kontrola výsledku
```

Orchestrace obvykle neprovádí hlavní business transformaci. Řídí úlohy, které ji provádějí.

### Typická odpovědnost

Produkční orchestrace je obvykle odpovědností data engineera. Analytik musí chápat pipeline, task, dependency a důsledky neúspěšné aktualizace.

---

## 11. Data Quality — kontrola kvality dat

**Data Quality** ověřuje, zda jsou data správná, úplná, konzistentní, aktuální a použitelná.

### Typické kontroly

- povinné hodnoty nejsou prázdné;
- klíče jsou jedinečné tam, kde mají být;
- nevyskytují se neočekávané duplicity;
- datové typy odpovídají očekávání;
- hodnoty leží v povoleném rozsahu;
- cizí klíče odkazují na existující záznamy;
- počet řádků odpovídá očekávání;
- cílová data lze odsouhlasit proti zdroji;
- data pocházejí ze správného období.

### Transformation versus Data Quality

```text
zjištění počtu duplicit
→ Data Quality

odstranění schválených duplicit
→ Transformation
```

Kontrola může proces pouze varovat, zastavit konkrétní větev nebo zablokovat publikaci. Reakce musí vycházet z předem stanoveného pravidla.

### Typická odpovědnost

Datová kvalita je sdílená odpovědnost. Data engineer vytváří technické kontroly, analytik ověřuje business správnost a data owner schvaluje pravidla a význam dat.

---

## 12. Data Catalog — popis a dohledatelnost dat

**Datový katalog** je organizovaný přehled dostupných dat a jejich významu.

Pomáhá zjistit:

- jaká data existují;
- kde jsou uložená;
- co znamenají tabulky a sloupce;
- odkud data pocházejí;
- kdo je jejich vlastníkem;
- které procesy a reporty je používají;
- jak spolu souvisejí zdrojové a cílové objekty.

### Data lineage

**Data lineage** popisuje původ a cestu dat.

```text
zdrojová tabulka
→ transformační proces
→ analytická tabulka
→ sémantický model
→ report
```

Katalog pomáhá data najít a pochopit. Samotný katalog automaticky nezaručuje jejich správnost.

---

## 13. Data Governance — pravidla a odpovědnost

**Data governance** představuje pravidla, odpovědnosti a rozhodovací procesy spojené s daty.

Řeší například:

- kdo je vlastníkem dat;
- kdo smí data používat;
- kdo schvaluje změny definic;
- jak se zachází s osobními údaji;
- jak dlouho se data uchovávají;
- jaké názvy a definice jsou závazné;
- kdo odpovídá za řešení problémů s kvalitou.

### Catalog versus Governance

```text
Data Catalog
→ co máme, kde to je a co to znamená

Data Governance
→ kdo za data odpovídá a podle jakých pravidel se používají
```

Příklad:

```text
Catalog
→ obsahuje definici aktivního zákazníka

Governance
→ určuje vlastníka definice a způsob jejího schválení
```

### Typická odpovědnost

Governance zajišťují data owners, data stewards, bezpečnostní tým a správci platformy. Analytik musí respektovat definice, klasifikaci dat a přístupová pravidla.

---

## 14. Monitoring — sledování fungování

**Monitoring** sleduje, zda datové procesy a prostředí fungují správně.

Kontroluje například:

- zda se pipeline spustila;
- které úlohy uspěly nebo selhaly;
- jak dlouho zpracování trvalo;
- kolik řádků bylo načteno;
- zda byl report obnoven;
- zda nedošlo k neobvyklému zpomalení;
- kolik prostředků nebo kapacity proces spotřeboval.

### Log, monitoring a alert

```text
Log
→ konkrétní záznam o události

Monitoring
→ průběžné sledování stavů, logů a metrik

Alert
→ upozornění vyvolané stanovenou podmínkou
```

### Monitoring versus Data Quality

```text
Data Quality
→ jsou data správná?

Monitoring
→ proběhl proces správně?
```

### Typická odpovědnost

Technický monitoring obvykle spravuje data engineer nebo provozní tým. Analytik musí umět ověřit aktuálnost reportu a rozpoznat, že výstup pochází z neúspěšné nebo zastaralé aktualizace.

---

## 15. Relationship, Data Model a Semantic Model

### Relationship

**Relationship** je vztah mezi dvěma tabulkami. Určuje spojovací sloupce, kardinalitu, směr filtrování a stav vztahu.

```text
dim_product[product_key]
1 → N
fact_sales[product_key]
```

### Data Model

**Datový model** je celá organizovaná struktura dat. Obsahuje například:

- tabulky;
- sloupce;
- datové typy;
- vztahy;
- míry;
- hierarchie;
- výpočetní a filtrační logiku.

Vztahy jsou tedy pouze jednou součástí datového modelu.

### Semantic Model

**Sémantický model** je datový model připravený pro business analýzu a reporting. Dává technickým datům jednotný business význam.

Definuje například:

- význam tabulek a ukazatelů;
- vztahy a pravidla filtrování;
- výpočty tržeb, zisku nebo marže;
- formátování hodnot;
- hierarchie;
- technické sloupce skryté před uživatelem;
- případná pravidla přístupu k datům.

V Power BI se pojmy datový a sémantický model často používají velmi podobně. Datový model zdůrazňuje strukturu, sémantický model zdůrazňuje business význam a použití.

---

## 16. Jeden nástroj může plnit více rolí

Nástroj nezařazujeme pouze podle jeho názvu. Rozhoduje konkrétní operace.

### SQL

- čtení dat ze zdroje → ingestion;
- `WHERE`, `JOIN` a `GROUP BY` → transformation;
- příprava analytického pohledu → transformation nebo analytics;
- kontrolní dotaz → Data Quality.

SQL je jazyk. Dlouhodobé uložení zajišťuje databázový systém, například SQL Server nebo PostgreSQL.

### Python a Pandas

- `requests.get()` → ingestion;
- `pd.read_csv()` → ingestion;
- čištění a `merge()` → transformation;
- kontroly hodnot a klíčů → Data Quality;
- `groupby()` a EDA → transformation nebo analytics;
- export do souboru či databáze → load nebo delivery.

DataFrame je pracovní reprezentace dat v paměti, nikoliv dlouhodobé datové úložiště.

### Power Query

- připojení ke zdroji → ingestion;
- změna typů a čištění → transformation;
- spojování tabulek → transformation;
- načtení výsledku do modelu → load.

```text
Connect
→ Transform
→ Load
```

### Power BI

- Power Query → ingestion a transformation;
- sémantický model a vztahy → analytics;
- DAX míry → analytics;
- vizuály a dashboardy → reporting;
- informace o poslední aktualizaci → základní provozní kontrola.

### Microsoft Fabric

Microsoft Fabric spojuje více datových a analytických funkcí v jedné platformě:

- Data Factory → ingestion, transformation a orchestration;
- OneLake → centrální analytické úložiště;
- Lakehouse a Warehouse → ukládání a analytické zpracování;
- Power BI → sémantický model, analytics a reporting;
- katalogizační a monitorovací funkce → governance, dohledatelnost a provozní kontrola.

### Azure Data Factory

- kopírování dat → ingestion;
- pipelines a jejich závislosti → orchestration;
- transformační aktivity → transformation;
- historie běhů a stav úloh → monitoring.

### Databricks

- notebooky se SQL, Pythonem a Sparkem → transformation a analytics;
- lakehouse tabulky → storage a analytics;
- jobs → orchestration;
- Unity Catalog → catalog a governance.

Konkrétní funkce cloudových produktů se postupně vyvíjejí. Pro portfolio juniorního analytika je důležitější správně vysvětlit jejich hlavní roli než vyjmenovat všechny dostupné funkce.

---

## 17. Typické datové role

### Data Engineer

Zaměřuje se především na:

- ingestion;
- storage;
- rozsáhlejší transformace;
- datové pipeline;
- orchestration;
- výkon a spolehlivost;
- technický monitoring.

### Data Analyst

Zaměřuje se především na:

- pochopení business problému;
- formulaci analytických otázek;
- získání potřebných dat;
- validaci a analýzu dat;
- interpretaci výsledků;
- návrh KPI;
- reporting a doporučení.

### BI Developer

Zaměřuje se především na:

- přípravu dat pro reporting;
- návrh datového a sémantického modelu;
- vztahy mezi tabulkami;
- DAX;
- výkon modelu;
- tvorbu, publikaci a distribuci reportů.

### Data Owner

**Data owner** je business vlastník konkrétní datové oblasti. Schvaluje význam dat, pravidla, přístup a odpovědnost.

### Data Steward

**Data steward** pomáhá udržovat definice, metadata, kvalitu a dodržování pravidel při každodenní práci s daty.

### Administrátor platformy

Spravuje například:

- identity a oprávnění;
- technickou konfiguraci;
- zabezpečení;
- prostředí a kapacity;
- provozní nastavení platformy.

Hranice mezi rolemi nejsou pevné. V menší firmě může jeden člověk zastávat více rolí.

---

## 18. Co potřebuje juniorní datový analytik

### Prakticky ovládat

- SQL;
- Power Query nebo podobný transformační nástroj;
- základní práci s Pythonem a Pandas podle zaměření pozice;
- kontrolu kvality dat;
- základní datový model;
- analýzu a interpretaci;
- Power BI reporting;
- návrh a vysvětlení KPI.

### Rozumět principu

- odkud data přicházejí;
- jak probíhá ingestion;
- kde jsou data uložena;
- kde se transformují;
- proč se úlohy orchestrují;
- jak poznat neúspěšnou aktualizaci;
- co obsahuje datový katalog;
- kdo vlastní data a přiděluje přístupy;
- komu předat technický problém.

### Nemusí zatím samostatně implementovat

- produkční cloudovou infrastrukturu;
- komplexní síťové zabezpečení;
- správu Spark clusterů;
- enterprise datový katalog;
- pokročilé orchestrační pipeline;
- centrální monitoring platformy;
- detailní správu identit a oprávnění.

Znalost principu není totéž jako praktická zkušenost s produktem ani administrace platformy.

```text
rozumím principu
≠
prakticky ovládám konkrétní nástroj
≠
spravuji celou platformu
```

---

## 19. Jak zařadit neznámý nástroj z pracovního inzerátu

Při čtení pracovního inzerátu nejprve určujeme funkci technologie:

```text
Poskytuje zdrojová data?
→ Sources

Přenáší data?
→ Ingestion

Ukládá data?
→ Storage

Čistí, spojuje nebo přepočítává data?
→ Transformation

Řídí pořadí a spouštění úloh?
→ Orchestration

Kontroluje správnost dat?
→ Data Quality

Popisuje data, vlastníky a pravidla?
→ Catalog & Governance

Podporuje analýzu a interpretaci?
→ Analytics

Prezentuje výsledky uživatelům?
→ Reporting

Sleduje stav a průběh procesů?
→ Monitoring
```

Potom posuzujeme úroveň požadované znalosti:

1. Stačí orientace v účelu nástroje?
2. Očekává se jeho každodenní používání?
3. Má kandidát vytvářet řešení, nebo platformu také spravovat?
4. Lze zkušenost přenést z podobného nástroje?

Pravdivá formulace kandidáta může znít:

> Rozumím principům datových pipelines, závislostem úloh a chování procesu při selhání. S konkrétní platformou zatím nemám produkční zkušenost.

---

## 20. Praktický end-to-end scénář

### Business situace

Obchodní společnost používá:

- SQL databázi s prodejními transakcemi;
- Excel s plánovanými hodnotami;
- API s měnovými kurzy;
- Power BI pro management reporting.

### Hlavní datový tok

```text
SQL + Excel + API
→ načtení dat
→ uložení původních vstupů
→ validace a transformace
→ analytické tabulky
→ sémantický model
→ Power BI report
→ rozhodnutí managementu
```

### Průřezové řízení

- orchestrace zajistí správné pořadí a závislosti;
- Data Quality zkontroluje zdrojová i výsledná data;
- katalog popíše tabulky, ukazatele a jejich původ;
- governance určí vlastníky, definice a oprávnění;
- monitoring ověří průběh pipeline a obnovení reportu.

### Možné rozdělení odpovědností

- data engineer spravuje načítání, ukládání, pipeline a technický monitoring;
- datový analytik kontroluje business správnost, analyzuje data a interpretuje výsledky;
- BI developer připravuje sémantický model, DAX a report;
- data owner schvaluje definice ukazatelů a přístupová pravidla.

Konkrétní rozdělení závisí na velikosti firmy a dostupném týmu.

---

## 21. Hlavní principy

### Nástroj není totéž co role

Jeden produkt může plnit několik funkcí. Zařazujeme konkrétní operaci, nikoliv pouze název produktu.

### Loading není Storage

```text
Load = zápis dat
Storage = místo uchování dat
```

### Transformation není Data Quality

```text
Transformation = data mění
Data Quality = ověřuje jejich správnost
```

### Orchestration není Monitoring

```text
Orchestration = řídí průběh
Monitoring = sleduje výsledek průběhu
```

### Catalog není Governance

```text
Catalog = popisuje a pomáhá najít data
Governance = stanovuje odpovědnost a pravidla
```

### Vztahy nejsou celý datový model

Vztahy jsou pouze jednou částí struktury tabulek, výpočtů, hierarchií a pravidel.

### Sémantický model dává datům business význam

Technické sloupce a tabulky převádí do podoby srozumitelné uživatelům reportu.

### Cílem není dashboard, ale rozhodnutí

```text
Business Problem
→ Data
→ Analysis
→ Insight
→ Recommendation
→ Decision
```

### Analytik má rozumět celému toku

Nemusí spravovat všechny technologie, ale musí vědět, odkud data přicházejí, kde se mění, jak vznikají KPI a zda je výsledný report aktuální a důvěryhodný.

---

## 22. Základní terminologie

- **Data Source** – místo, kde data vznikají nebo odkud se získávají;
- **Extract** – získání dat ze zdroje;
- **Transfer** – přenos dat;
- **Load** – zápis dat do cílového prostředí;
- **Data Ingestion** – získání a přesun dat;
- **Data Storage** – místo dlouhodobého uchování dat;
- **Transformation** – změna struktury, hodnot nebo významu dat;
- **Analytics** – použití dat k hledání a interpretaci odpovědí;
- **Reporting** – prezentace analytických výsledků;
- **Data Pipeline** – posloupnost kroků pro přesun a zpracování dat;
- **Task** – samostatná úloha uvnitř procesu;
- **Dependency** – závislost jedné úlohy na jiné;
- **Orchestration** – řízení pořadí a spouštění úloh;
- **Data Quality** – kontrola správnosti a použitelnosti dat;
- **Data Catalog** – organizovaný popis dostupných dat;
- **Data Lineage** – původ a cesta dat mezi systémy;
- **Data Governance** – pravidla, odpovědnosti a řízení dat;
- **Monitoring** – sledování fungování procesů a systémů;
- **Log** – záznam konkrétní události;
- **Alert** – upozornění vyvolané stanovenou podmínkou;
- **Relationship** – propojení tabulek;
- **Data Model** – struktura tabulek, vztahů a výpočtů;
- **Semantic Model** – datový model doplněný o business význam;
- **Measure** – dynamický výpočet nad daty;
- **Business Decision** – rozhodnutí založené na analytickém výstupu.

---

## 23. Závěrečné shrnutí

```text
HLAVNÍ TOK

Sources
→ Ingestion
→ Storage
→ Transformation
→ Analytics
→ Reporting
→ Business Decision


PRŮŘEZOVÉ FUNKCE

Orchestration
→ řídí pořadí a závislosti

Data Quality
→ kontroluje správnost dat

Catalog & Governance
→ popisuje data, vlastníky a pravidla

Monitoring
→ sleduje fungování procesu
```

Hlavní zásada:

> Datový analytik nemusí spravovat celý Modern Data Stack. Musí však rozumět cestě dat, správně zařadit role nástrojů a poznat, zda pracuje s aktuálními, správnými a důvěryhodnými daty.