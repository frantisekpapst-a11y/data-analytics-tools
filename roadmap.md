9	Databricks	základní lakehouse notebook
10	Orchestrace	návrh pořadí datových kroků
11	Modern Data Stack	mapa kategorií a rolí
12	Kombinace nástrojů	závěrečný architektonický scénář



Lekce 9 — Databricks

Databricks spojí předchozí témata:
Jupyter-style notebooks
+ Spark
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



tools-notebooks/databricks-lakehouse	9	Jednoduchá ukázka zpracování dat v Databricks notebooku

Lekce 9 — databricks-lakehouse

Tato složka bude obsahovat notebook, ve kterém si na malém příkladu ukážeme, jak se předchozí návrh realizuje v prostředí Databricks.

Nepůjde o hlubokou výuku PySparku. Notebook bude zaměřený na pochopení pracovního postupu:

načtení dat
→ základní kontrola
→ jednoduchá transformace
→ uložení nebo příprava výsledné tabulky

Použijeme jen minimum kódu potřebné k pochopení notebooku a lakehouse workflow. Nebudeme se učit PySpark jako samostatný programovací nástroj.


1. Automation

Tady by hlavní otázka byla:

Jak udělat z analytického procesu opakovatelný proces bez ručního spouštění?

Obsah bych postavil kolem konkrétních situací:

Windows Task Scheduler
spuštění Python skriptu podle času
spuštění .bat
logování úspěchu/chyby
GitHub Actions
spuštění Python skriptu podle schedule
práce se secrets
uložení/export výsledků
SQL scheduling
princip SQL Agent / scheduled jobs
kdy plánovat SQL místo Pythonu
Power BI refresh
scheduled refresh
gateway princip
závislost na zdroji
Power Query refresh
kdy stačí obnovit dotaz
kdy už je potřeba jiná automatizační vrstva
API automatizace
pravidelné stahování dat
pagination
timeout
retry / error handling
logging a monitoring
log soubor
timestamp
status
jednoduché error messages
secrets
environment variables
GitHub Secrets
proč nedávat API key do repa

A hlavně praktické scénáře:

API
→ Python
→ CSV
→ Power BI

SQL
→ Python
→ Excel

API
→ Python
→ SQLite
→ Power BI

Python
→ Task Scheduler

Python
→ GitHub Actions

SQL
→ scheduled job
→ Power BI refresh

Cíl automatizace by nebyl „naučit se deset schedulerů“, ale pochopit:

co spouštím
→ kdy
→ čím
→ co se stane při chybě
→ kde je výsledek
2. Analytical Workflow

Tohle bych dal opravdu až úplně na konec.

Tady už hlavní otázka nebude:

Jak něco naprogramovat?

ale:

Jak navrhnout celé analytické řešení od business problému až po finální reporting?

Obsah:

business question
definice KPI
výběr datových zdrojů
granularita dat
ingestion
raw / staging / clean vrstva
data quality
cleaning
validation
joins
transformations
feature / business logic
EDA
statistická analýza, pokud dává smysl
agregace
datový model
DAX
dashboard
business interpretace
doporučení
export / distribuce
automatizace
monitoring
dokumentace

Hlavní princip:

Business Question
→ Data Source
→ Extraction
→ Cleaning
→ Validation
→ Analysis
→ Data Model
→ KPI
→ Visualization
→ Interpretation
→ Recommendation
→ Automation
→ Delivery

Ale nejdůležitější část bude rozhodování který nástroj kam patří.

Například:

SQL
→ extraction
→ joins
→ jednoduché transformace

Python
→ cleaning
→ validation
→ API
→ statistika
→ komplexnější transformace

Power Query
→ lehká příprava / refresh

Power BI
→ model
→ DAX
→ dashboard

Excel
→ ad-hoc kontrola / business analýza

GitHub
→ versioning / dokumentace

Automation
→ opakované spouštění

Tohle je přesně místo, kde se propojí úplně všechno, co ses učil.

Jak bych to postavil jako portfolio
Automation repo

Spíš menší scénáře:

01 Python + Task Scheduler
02 API + Python + Scheduled Export
03 GitHub Actions
04 SQL Scheduling
05 Power BI Refresh Architecture
06 End-to-End Automated Mini Pipeline
Analytical Workflow repo

Méně projektů, ale větších:

01 Sales Performance Workflow
02 Customer / Support Analytics Workflow
03 Financial / Fund Analytics Workflow
04 Final Capstone End-to-End Project

A ten poslední capstone by mohl vypadat třeba:

API / SQL / Files
→ SQL
→ Python
→ Validation
→ Clean DB
→ Power BI
→ DAX
→ Dashboard
→ Scheduled Refresh
→ GitHub Documentation

Takže stručně:

Automation
= jak proces spouštět opakovaně

Workflow
= jak celé řešení správně navrhnout