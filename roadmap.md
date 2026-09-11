Lekce 11 — Volba a kombinace nástrojů
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