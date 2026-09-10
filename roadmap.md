6	Warehouse, lake, lakehouse	porovnání úložišť a jednoduchý model
7	ETL, ELT a vrstvy	návrh Bronze–Silver–Gold
8	Cloud basics	mapa Azure, AWS a GCP
9	Databricks	základní lakehouse notebook
10	Orchestrace	návrh pořadí datových kroků
11	Modern Data Stack	mapa kategorií a rolí
12	Kombinace nástrojů	závěrečný architektonický scénář

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



Složka	Příslušná lekce	Co v ní bude
tools-case-studies/lakehouse-layers	7	Case study rozdělení dat do vrstev Bronze–Silver–Gold
tools-notebooks/databricks-lakehouse	9	Jednoduchá ukázka zpracování dat v Databricks notebooku

Lekce 7 — lakehouse-layers

Tady už se zaměříme na tok dat:

zdrojová data
→ Bronze
→ Silver
→ Gold
→ Power BI

Case study ukáže například:

Bronze: původní CSV, JSON a databázové exporty;
Silver: vyčištěná, validovaná a propojená data;
Gold: faktové a dimenzní tabulky nebo agregace připravené pro reporting.

Tím prakticky propojíme:

data lake;
lakehouse;
ETL a ELT;
datové vrstvy;
warehouse design z Lekce 6.
Lekce 9 — databricks-lakehouse

Tato složka bude obsahovat notebook, ve kterém si na malém příkladu ukážeme, jak se předchozí návrh realizuje v prostředí Databricks.

Nepůjde o hlubokou výuku PySparku. Notebook bude zaměřený na pochopení pracovního postupu:

načtení dat
→ základní kontrola
→ jednoduchá transformace
→ uložení nebo příprava výsledné tabulky

Použijeme jen minimum kódu potřebné k pochopení notebooku a lakehouse workflow. Nebudeme se učit PySpark jako samostatný programovací nástroj.


Lekce 7
→ ETL, ELT a Bronze–Silver–Gold
→ case study lakehouse-layers

Lekce 9
→ prostředí Databricks
→ notebook databricks-lakehouse

Lekce 12
→ propojení všech témat
→ závěrečný architektonický scénář

Takže teď pokračujeme Lekcí 6. Jejím hlavním praktickým výstupem bude warehouse-design. Zbývající dvě složky zatím necháme připravené pro Lekce 7 a 9.