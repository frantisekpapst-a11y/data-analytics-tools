9	Databricks	základní lakehouse notebook
10	Orchestrace	návrh pořadí datových kroků
11	Modern Data Stack	mapa kategorií a rolí
12	Kombinace nástrojů	závěrečný architektonický scénář



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



tools-notebooks/databricks-lakehouse	9	Jednoduchá ukázka zpracování dat v Databricks notebooku

Lekce 9 — databricks-lakehouse

Tato složka bude obsahovat notebook, ve kterém si na malém příkladu ukážeme, jak se předchozí návrh realizuje v prostředí Databricks.

Nepůjde o hlubokou výuku PySparku. Notebook bude zaměřený na pochopení pracovního postupu:

načtení dat
→ základní kontrola
→ jednoduchá transformace
→ uložení nebo příprava výsledné tabulky

Použijeme jen minimum kódu potřebné k pochopení notebooku a lakehouse workflow. Nebudeme se učit PySpark jako samostatný programovací nástroj.