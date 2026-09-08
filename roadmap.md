1	Mapa datového prostředí	přehled datového toku a rolí
2	Jupyter workflow	profesionální analytický notebook
3	Větší datasety	strategie efektivního zpracování
4	Spark principy	rozhodnutí Spark vs. běžné nástroje
5	PySpark	praktická analýza v Spark DataFrame
6	Warehouse, lake, lakehouse	porovnání úložišť a jednoduchý model
7	ETL, ELT a vrstvy	návrh Bronze–Silver–Gold
8	Cloud basics	mapa Azure, AWS a GCP
9	Databricks	základní lakehouse notebook
10	Orchestrace	návrh pořadí datových kroků
11	Modern Data Stack	mapa kategorií a rolí
12	Kombinace nástrojů	závěrečný architektonický scénář

Upravený plán Lekce 3
1. Baseline Dataset and Memory

Nejdříve potřebujeme společný dataset pro všechna porovnání.

Prakticky:

vytvoření 300 000 prodejních záznamů;
kontrola struktury a datových typů;
uložení do CSV;
porovnání velikosti CSV s velikostí DataFrame v paměti.

Aktuální výsledek:

CSV na disku              18,70 MB
DataFrame v paměti        62,42 MB

Hlavní poznatek:

Velikost souboru na disku není stejná jako paměť potřebná pro jeho zpracování.

Tuto část máme hotovou.

2. CSV vs. Parquet

Použijeme stejný dataset, aby bylo porovnání férové.

Probereme:

co je CSV;
co je Parquet;
řádkový a sloupcový způsob uložení;
zachování datových typů;
kompresi;
velikost obou souborů;
kdy použít který formát.

Praktický mini scénář:

stejný DataFrame
→ CSV
→ Parquet
→ porovnání velikosti
→ porovnání načítání

Nebudeme zabíhat do technických detailů vnitřní struktury Parquetu.

3. Data Types and Memory Usage

Teprve po porovnání formátů se krátce vrátíme k datovým typům.

Probereme:

proč textové sloupce spotřebovávají hodně paměti;
rozdíl mezi int32 a int64;
co znamená float64;
kdy může být vhodný typ category;
proč se typy nemají měnit bez znalosti významu dat.

Praktický mini scénář:

původní DataFrame
→ změna vhodných typů
→ nové měření paměti
→ porovnání výsledku

category tedy použijeme pouze jako krátkou ukázku optimalizace opakovaných kategorií.

4. Loading Only Required Columns

Ukážeme si, že analytik často nepotřebuje celý dataset.

Praktický mini scénář:

pd.read_csv(
    CSV_PATH,
    usecols=[
        "order_date",
        "region",
        "revenue"
    ]
)

Porovnáme:

všechny sloupce
vs.
pouze potřebné sloupce

Budeme sledovat:

počet sloupců;
spotřebu paměti;
analytickou použitelnost.

Hlavní princip:

Nenačítat data jen proto, že jsou dostupná.

5. Filtering at the Source and SQL Pushdown

Data uložíme také do SQLite a použijeme známé SQL.

Vysvětlíme si pojem SQL pushdown:

filtr nebo agregaci provede databáze
→ do Pythonu nebo Power BI se přenesou pouze potřebná data

Praktický mini scénář:

SELECT
    order_date,
    region,
    revenue
FROM sales
WHERE region = 'Plzeň';

Porovnáme:

varianta A
→ načíst celou tabulku do pandas
→ filtrovat v Pythonu

varianta B
→ filtrovat pomocí SQL
→ načíst pouze výsledek
6. Aggregation Before Loading

Ukážeme si rozdíl mezi detailním datasetem a agregovaným výstupem.

SQL agregace:

SELECT
    region,
    SUM(revenue) AS total_revenue
FROM sales
GROUP BY region;

Porovnáme:

detailní data
→ 300 000 řádků

agregovaný výstup
→ 5 řádků

Vysvětlíme si, kdy agregovat před načtením do:

Pythonu;
Power BI;
analytické databáze.

Současně upozorníme na riziko příliš brzké agregace, při které můžeme ztratit potřebný detail.

7. Chunk Processing

Ukážeme si načítání CSV po částech:

pd.read_csv(
    CSV_PATH,
    chunksize=50000
)

Chunk znamená část datasetu.

Workflow:

velký CSV soubor
→ načíst 50 000 řádků
→ zpracovat
→ načíst další část
→ spojit pouze výsledky

Praktický mini scénář:

načítání CSV po částech;
výpočet tržeb podle regionu;
bez držení celého datasetu v paměti.

Vysvětlíme také omezení chunks: nejsou automatickou náhradou databáze ani Sparku.

8. Partitioning

Partitioning znamená fyzické rozdělení dat podle určitého sloupce.

Například:

sales/
├── year=2024/
│   └── sales.parquet
└── year=2025/
    └── sales.parquet

Nebo:

sales/
├── region=Praha/
├── region=Plzen/
└── region=Brno/

Hlavní přínos:

dotaz na rok 2025
→ není nutné číst data za rok 2024

Probereme:

vhodný partitioning;
nevhodné rozdělení na příliš mnoho malých souborů;
souvislost s Parquetem, data lakem a později Sparkem.
9. Pandas vs. SQL

Porovnáme role obou nástrojů.

SQL
→ výběr dat
→ filtrování
→ JOIN
→ agregace ve zdroji
→ omezení přenosu dat

pandas
→ další transformace
→ nestandardní logika
→ analýza
→ statistika
→ příprava výstupu

Praktické pravidlo:

Pokud data leží v databázi, není obvykle efektivní nejprve načíst vše do pandas a teprve potom filtrovat.

10. When One Computer Is Not Enough

Nakonec si vytvoříme rozhodovací rámec.

Nejdříve zkusíme:

načíst pouze potřebné sloupce;
filtrovat ve zdroji;
agregovat ve zdroji;
použít vhodnější formát;
upravit datové typy;
zpracovávat data po částech;
využít databázový výkon.

Teprve potom posoudíme:

zda se data nevejdou do RAM;
zda operace opakovaně selhávají;
zda je zpracování příliš pomalé;
zda potřebujeme zpracovávat data na více počítačích;
zda dává smysl Spark.
Finální rozhodovací pravidlo
Potřebuji všechny řádky?
→ Potřebuji všechny sloupce?
→ Mohu filtrovat ve zdroji?
→ Mohu agregovat před načtením?
→ Je vhodný CSV, Parquet nebo databáze?
→ Mohu použít chunks?
→ Stačí SQL a jeden počítač?
→ Teprve potom Spark
Výstupy Lekce 3

Na konci budeme mít:

large-data-efficiency-notebook.ipynb;
porovnání CSV, Parquet a SQLite;
porovnání celého a omezeného načtení;
SQL pushdown scénář;
agregovaný výstup vhodný pro Power BI;
chunk processing scénář;
vysvětlení partitioningu;
large-data-cheatsheet.md;
minitesty doplněné do společného souboru.

Teď jsme dokončili část Baseline Dataset and Memory. Dalším krokem bude CSV vs. Parquet, přičemž Parquet nejdříve vysvětlíme principem a teprve potom použijeme jeho syntaxi.


Lekce 4 — Spark: proč existuje

Nejdříve pochopíme princip, teprve potom PySpark syntax.

Témata
limity zpracování dat na jednom počítači;
distribuované zpracování;
cluster;
driver a workers;
partitions;
paralelní zpracování;
lazy evaluation;
transformations a actions;
proč Spark nepracuje stejně jako pandas;
kdy Spark dává a nedává smysl.
Zjednodušená analogie
pandas: jeden pracovník zpracovává celý úkol;
Spark: koordinátor rozdělí práci mezi více pracovníků;
partition: část datasetu přidělená ke zpracování;
lazy evaluation: Spark si nejprve připraví plán a výpočet provede až ve chvíli, kdy je výsledek potřeba.
Mini test

Výběr mezi:

Excel;
SQL;
pandas;
Power Query;
Spark.

Nebudeme vybírat podle velikosti názvu technologie, ale podle objemu dat, zdroje, cíle, infrastruktury a frekvence zpracování.

Lekce 5 — PySpark pro datového analytika

Toto bude hlavní praktická část zaměřená na Spark.

Témata
vytvoření SparkSession;
načtení datasetu;
Spark DataFrame;
kontrola schématu;
výběr sloupců;
filtrace;
tvorba vypočteného sloupce;
chybějící hodnoty;
agregace;
spojování tabulek;
řazení výsledků;
ukládání výstupu;
převod malého agregovaného výsledku do pandas.
Porovnání syntaxe
Operace	pandas	SQL	PySpark
Výběr sloupců	df[[...]]	SELECT	select()
Filtrace	maska	WHERE	filter()
Nový sloupec	přiřazení	výraz v SELECT	withColumn()
Agregace	groupby()	GROUP BY	groupBy()
Spojení	merge()	JOIN	join()
Chybějící hodnoty	fillna()	COALESCE()	fillna()
Praktický mini scénář

Prodejní data:

Načtení objednávek
→ kontrola schématu
→ validace
→ spojení s produkty a zákazníky
→ výpočet revenue a profit
→ agregace podle regionu a kategorie
→ uložení výsledku
Co úmyslně vynecháme
pokročilou správu clusterů;
detailní optimalizaci Spark execution planu;
interní fungování Spark enginu;
pokročilý streaming;
pokročilé Spark ML;
specialist-level konfiguraci výkonu.
Lekce 6 — Data warehouse, data lake a lakehouse

Tuto lekci zařadíme před Databricks, protože bez těchto pojmů by lakehouse prostředí Databricks nedávalo plný smysl.

Data warehouse

Analytické úložiště s připravenými a strukturovanými daty.

Návaznost:

SQL;
faktové a dimenzní tabulky;
hvězdicové schéma;
datamarty;
Power BI datový model.
Data lake

Úložiště různých typů dat, často v původní nebo méně zpracované podobě.

Může obsahovat:

CSV;
JSON;
Parquet;
logy;
obrázky;
další strukturovaná i nestrukturovaná data.
Lakehouse

Spojuje flexibilní ukládání data lake s některými vlastnostmi řízeného data warehouse.

Klíčové porovnání
Oblast	Warehouse	Lake	Lakehouse
Typická data	strukturovaná	různé formáty	různé formáty a řízené tabulky
Hlavní využití	BI a reporting	ukládání a další zpracování	analytika, BI a data engineering
Struktura	předem řízená	volnější	kombinovaná
Typický uživatel	analytik, BI tým	data engineer, data scientist	více datových rolí
Praktický mini scénář

Navrhnout jednoduchý prodejní datový model:

fact_sales
dim_customer
dim_product
dim_date
dim_region

Následně určit, která původní data by mohla být nejprve uložena v lake a která data už mají být připravena pro warehouse nebo reporting.

Lekce 7 — ETL, ELT a datové vrstvy
ETL
Extract → Transform → Load

Data se upraví před uložením do cílového analytického systému.

ELT
Extract → Load → Transform

Data se nejprve uloží a následně transformují uvnitř cílové platformy.

Témata
raw data;
staging vrstva;
transformovaná data;
business-ready data;
datová kvalita;
opakovatelnost transformací;
oddělení technických a business transformací;
základní princip Bronze, Silver a Gold vrstev.
Analogie s dosavadním Python workflow
Lakehouse vrstva	Dosavadní analogie
Bronze	df_raw
Silver	clean_df
Gold	agregované tabulky a KPI pro Power BI
Praktický mini scénář

Rozdělit zpracování objednávek do vrstev:

Bronze
→ původní CSV a JSON

Silver
→ validovaná a vyčištěná data

Gold
→ tržby, náklady a zisk podle regionu a kategorie
Lekce 8 — Cloud basics pro datového analytika

Cílem nebude technická správa cloudu, ale schopnost orientovat se ve firemním cloudovém prostředí.

Témata
cloud vs. lokální prostředí vs. on-premise;
cloudové úložiště;
cloudová databáze;
cloudový data warehouse;
výpočetní prostředky;
identity a oprávnění;
region;
škálování;
náklady podle využití;
základní bezpečnost;
sdílená odpovědnost.
Azure, AWS a GCP

Použijeme:

Azure jako hlavní příklad, protože tvoří přirozenou návaznost na Power BI a Microsoft prostředí;
AWS a GCP jako srovnávací alternativy;
mapu kategorií služeb, nikoli seznam desítek produktů k memorování.
Analytik musí rozumět zejména tomu:
kde jsou data uložená;
jakým způsobem k nim přistupuje;
kde probíhá SQL dotaz nebo transformace;
kdo nastavuje oprávnění;
odkud Power BI načítá data;
která operace může vytvářet náklady.
Lekce 9 — Databricks

Databricks spojí předchozí témata:

Jupyter-style notebooks
+ Spark
+ PySpark
+ SQL
+ cloud
+ lakehouse
Témata
workspace;
notebook;
compute;
Spark prostředí;
SQL a PySpark v jednom notebooku;
práce se soubory a tabulkami;
základní princip Delta tabulek;
datový katalog na orientační úrovni;
Bronze, Silver a Gold vrstvy;
jobs a pipelines pouze koncepčně.
Praktický mini scénář
CSV / JSON
→ Bronze
→ PySpark validace a čištění
→ Silver
→ agregace
→ Gold
→ výstup připravený pro Power BI
Hranice vůči Automation

V této lekci si vysvětlíme:

co je job;
co je pipeline;
proč se kroky plánují;
co znamená jejich závislost.

Nebudeme zde ještě podrobně řešit:

scheduling;
retry;
monitoring;
alerty;
produkční nasazení.

To bude součást následujícího bloku Automation.

Lekce 10 — Orchestrace jako princip

Orchestrace patří do přehledu moderního data stacku, ale její praktická implementace patří do Automation.

V tomto bloku probereme
co orchestrace znamená;
co je pipeline;
co je task;
co je dependency;
proč musí být kroky ve správném pořadí;
co se má stát při chybě;
jak orchestrace souvisí s monitoringem a logováním.
Modelový návrh
Načti data
→ validuj data
→ transformuj data
→ aktualizuj analytickou tabulku
→ obnov report
→ zkontroluj výsledek

Výstupem bude návrh procesu, nikoli jeho automatické spuštění.

Lekce 11 — Modern Data Stack a role nástrojů

Nyní spojíme všechny předchozí části do jedné mapy.

Kategorie moderního data stacku
Vrstva	Účel
Sources	vznik a poskytování dat
Ingestion	přenos dat ze zdrojů
Storage	ukládání dat
Transformation	čištění, spojování a výpočty
Orchestration	řízení pořadí a spouštění kroků
Data Quality	kontrola správnosti dat
Catalog & Governance	popis, vlastnictví a řízení dat
Analytics	analýza a tvorba datových výstupů
Reporting	prezentace výsledků
Monitoring	sledování správného fungování
Důležitá zásada

Nebudeme se učit velký seznam konkrétních produktů. Důležitější bude rozpoznat:

jaký problém daná kategorie řeší;
jaký má vstup a výstup;
kdo ji obvykle spravuje;
jak moc ji potřebuje ovládat datový analytik.
Lekce 12 — Volba a kombinace nástrojů

Toto bude závěrečná a nejdůležitější lekce bloku.

Rozhodovací otázky
Kde data vznikají?
Jak často se mění?
Jaký mají objem?
Jsou strukturovaná?
Má transformace proběhnout v SQL, Pythonu, Power Query nebo PySparku?
Má být výsledek uložen jako soubor, tabulka nebo datový model?
Bude výstup jednorázový, nebo opakovaný?
Kdo bude řešení používat a spravovat?
Je navržené řešení přiměřené business potřebě?
Závěrečný mini scénář

Firma má:

objednávky v SQL databázi;
marketingová data z API;
rozpočty v Excelu;
historické soubory v cloudovém úložišti;
Power BI reporting;
rostoucí objem dat.

Výstupem bude:

jednoduchá architektura;
popis rolí jednotlivých nástrojů;
zdůvodnění jejich výběru;
návrh datových vrstev;
označení částí, které budou později automatizovány;
upozornění na technologie, které by byly pro daný scénář zbytečné.

Nebude se ještě realizovat kompletní end-to-end řešení. To patří až do Analytical Workflow Portfolio.