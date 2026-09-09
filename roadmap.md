4	Spark principy	rozhodnutí Spark vs. běžné nástroje
5	PySpark	praktická analýza v Spark DataFrame
6	Warehouse, lake, lakehouse	porovnání úložišť a jednoduchý model
7	ETL, ELT a vrstvy	návrh Bronze–Silver–Gold
8	Cloud basics	mapa Azure, AWS a GCP
9	Databricks	základní lakehouse notebook
10	Orchestrace	návrh pořadí datových kroků
11	Modern Data Stack	mapa kategorií a rolí
12	Kombinace nástrojů	závěrečný architektonický scénář


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