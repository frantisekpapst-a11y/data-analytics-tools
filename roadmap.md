Blok 1 — Automation

A2 — Převod notebooku na Python skript
Témata
rozdíl mezi .ipynb a .py;
spuštění bez otevřeného VS Code;
odstranění závislosti na stavu kernelu;
rozdělení procesu do funkcí;
funkce main();
if __name__ == "__main__":;
vstupní a výstupní složky;
relativní cesty;
konfigurace oddělená od kódu;
návratový stav procesu;
spuštění z terminálu.
Základní struktura
def load_data():
    pass


def validate_data():
    pass


def transform_data():
    pass


def save_output():
    pass


def main():
    pass


if __name__ == "__main__":
    main()

A3 — API automatizace
Témata
pravidelné stahování dat;
HTTP status;
timeout;
pagination;
rate limit;
kontrola struktury odpovědi;
neúplná odpověď;
jednoduchý retry;
řízené ukončení;
uložení původní odpovědi;
timestamp načtení;
validace výsledku.
Praktický scénář
API
→ Python
→ validace
→ CSV nebo JSON

A4 — SQL Server LocalDB a Python
Hlavní technologie
SQL Server LocalDB;
SQL Server Management Studio;
T-SQL;
Python;
pyodbc;
Pandas.

SQLite zůstane záložní variantou pro případ nepřiměřených technických problémů.

Témata
instalace a spuštění LocalDB;
vytvoření lokální databáze;
připojení přes SSMS;
tabulky a datové typy;
primární a cizí klíče;
připojení Pythonu;
načtení SQL dat do Pandas;
zápis dat z Pythonu do SQL;
parametrizované dotazy;
transakce na základní úrovni;
správné uzavření spojení;
rozdělení transformací mezi SQL a Python.
Praktické scénáře
SQL LocalDB
→ Python
→ Excel
API
→ Python
→ SQL LocalDB
→ Power BI Desktop

A5 — Validace a error handling
Validace vstupů
existence souboru;
dostupnost API;
dostupnost databáze;
povinné sloupce;
datové typy;
prázdné klíče;
duplicity;
neplatné hodnoty;
aktuálnost dat;
očekávaný počet řádků.
Error handling
try a except;
konkrétní typy chyb;
kritická chyba versus varování;
kdy proces zastavit;
kdy lze pokračovat;
srozumitelná chybová zpráva;
zachování posledního správného výstupu;
ukončení s odpovídajícím statusem.
Bezpečná publikace
načtení úspěšné
+ validace úspěšná
+ transformace úspěšná
+ výstup zkontrolovaný
=
nový výstup lze publikovat

A6 — Logging a monitoring
Témata
modul logging;
log soubor;
timestamp;
status;
úrovně INFO, WARNING a ERROR;
počet vstupních a výstupních řádků;
čas zahájení a dokončení;
délka zpracování;
výsledek validace;
vytvořený výstup;
záznam chyby.
Příklad logu
2026-09-12 06:00:01 | INFO | Proces zahájen
2026-09-12 06:00:03 | INFO | Načteno 1 250 řádků
2026-09-12 06:00:04 | INFO | Validace úspěšná
2026-09-12 06:00:06 | INFO | Výstup vytvořen
2026-09-12 06:00:06 | INFO | Proces dokončen

Při chybě:

2026-09-12 06:00:03 | ERROR | Kurzovní API není dostupné

A7 — Secrets a environment variables
Témata
API key;
heslo;
přístupový token;
connection string;
environment variables;
soubor .env;
soubor .env.example;
.gitignore;
GitHub Secrets;
proč citlivé údaje nepatří do kódu ani repozitáře.
Struktura
.env
→ obsahuje skutečné hodnoty
→ neukládat na GitHub

.env.example
→ obsahuje pouze názvy proměnných
→ uložit na GitHub

Příklad:

API_KEY=
DATABASE_SERVER=
DATABASE_NAME=

A8 — Windows Task Scheduler a .bat

Toto bude hlavní praktický způsob automatického spouštění.

Témata
vytvoření .bat;
aktivace správného .venv;
cesta k Python interpreteru;
cesta ke skriptu;
pracovní adresář;
spuštění bez otevřeného VS Code;
časové plánování;
spuštění při přihlášení;
ruční test úlohy;
historie spuštění;
návratový kód;
řešení rozdílu mezi ručním a naplánovaným během.
Datový tok
Windows Task Scheduler
→ run_pipeline.bat
→ Python z .venv
→ pipeline.py
→ výstupní data
→ log

A9 — GitHub Actions
Témata
workflow;
job;
step;
runner;
YAML;
schedule;
workflow_dispatch;
instalace Pythonu;
instalace dependencies;
GitHub Secrets;
spuštění Python skriptu;
workflow artifacts;
kontrola úspěchu a selhání.
Praktická hranice

GitHubem hostovaný runner se běžně nepřipojí k LocalDB na uživatelově počítači.

Proto:

GitHub Actions
→ API-only automatizace

Windows Task Scheduler
→ LocalDB a lokální soubory
Praktický scénář
GitHub Actions
→ plánované stažení z API
→ validace
→ CSV nebo JSON
→ workflow artifact

A10 — SQL scheduling
Témata
princip SQL Server Agentu;
job;
job step;
schedule;
historie spuštění;
úspěch a chyba;
kdy plánovat proces v databázi;
kdy použít Python;
kdy použít orchestrační platformu.
Omezení LocalDB

LocalDB nemá SQL Server Agent. Praktickou variantou proto bude:

Windows Task Scheduler
→ Python nebo sqlcmd
→ SQL LocalDB
→ SQL skript nebo uložená procedura
SQL scheduling je vhodný, když
celý proces zůstává v SQL Serveru;
transformaci lze provést uloženou procedurou;
výstup zůstává v databázi;
nejsou potřeba API ani soubory;
databázový tým job spravuje.
Python je vhodnější, když
získáváme data z API;
zpracováváme JSON, CSV nebo Excel;
kombinujeme různé zdroje;
potřebujeme nestandardní logiku;
vytváříme souborové výstupy;
Python řídí validaci a logování.

A11 — Power Query a Power BI refresh
Power Query refresh

Power Query definuje:

připojení ke zdroji;
načítání;
transformační kroky;
načtení výsledku.

Prostředí, ve kterém Power Query běží, určuje způsob obnovy.

Power Query
→ jak data načíst a transformovat

Excel nebo Power BI
→ kdy obnovu spustit
Kdy Power Query stačí
proces má málo zdrojů;
transformace jsou jednoduché;
zdroje jsou přímo dostupné;
nejsou složité závislosti;
není potřeba pokročilé logování;
případná chyba nevyžaduje speciální reakci.
Kdy potřebujeme další automatizaci
nejprve se musí stáhnout API;
musí proběhnout Python;
několik zdrojů je dostupných v různých časech;
publikace závisí na validaci;
je potřeba detailní log;
chyba musí zastavit navazující kroky.
Power BI
scheduled refresh;
data source credentials;
refresh history;
gateway;
závislost na dostupnosti zdroje;
poslední úspěšná aktualizace;
Power BI Desktop versus Power BI Service.
Praktický rozsah
SQL LocalDB nebo exportní soubor
→ Power BI Desktop
Gateway a plánovanou obnovu v Power BI Service probereme koncepčně.

A12 — Závěrečná case study Automation
Daily Pricing Control Automation
Vstupy
SQL Server LocalDB s produkty a nákupními cenami;
API s měnovými kurzy;
Excel s cenovými limity.
Datový tok
Windows Task Scheduler
→ .bat
→ Python
→ SQL LocalDB + API + Excel
→ validace
→ převod cen do CZK
→ kontrola cenových limitů
→ zápis výsledku
→ export pro Power BI
→ log
GitHub Actions větev
GitHub Actions
→ plánované stažení kurzů
→ validace
→ export
→ workflow artifact
Výstupy
funkční Python skript;
SQL skripty;
.bat;
.env.example;
log;
ukázkový export;
naplánovaná úloha;
GitHub Actions workflow;
README;
Automation cheatsheet;
minitesty.
Časový odhad bloku

Přibližně 14–19 hodin.

Blok 2 — Volba nástrojů a návrh datového řešení
Hlavní cíl

Na základě business požadavků navrhnout přiměřené analytické řešení a zdůvodnit každé technologické rozhodnutí.

Tento blok nebude zaměřený na další programování. Bude ověřovat schopnost rozhodnout:

Co použít?
Kde to použít?
Proč to použít?
Kdo to bude spravovat?
Co naopak vůbec nepotřebujeme?
B1 — Business potřeba

Určit:

jaký problém firma řeší;
jaké rozhodnutí má výstup podporovat;
kdo bude výstup používat;
jaké KPI jsou potřebné;
jak často se mají výsledky aktualizovat;
jaká přesnost a aktuálnost je požadována.
B2 — Posouzení datových zdrojů

Prověřit:

kde data vznikají;
kdo je vlastní;
jaké mají formáty;
zda jsou strukturovaná;
jaký mají objem;
jak rychle přibývají;
jakou historii potřebujeme;
zda jsou lokální nebo cloudová;
zda obsahují citlivé údaje;
jak spolehlivě jsou dostupná.
B3 — Volba ingestion

Rozhodnout mezi:

SQL dotazem;
Pythonem;
Power Query;
API integrací;
souborovým importem;
datovou pipeline;
inkrementálním načítáním.
B4 — Volba úložiště

Rozhodnout mezi:

původním zdrojem;
CSV, Excel nebo Parquet;
SQL databází;
data warehouse;
data lake;
lakehouse;
analytickým modelem.
B5 — Volba transformačního nástroje

Posoudit, co patří do:

SQL;
Pythonu;
Power Query;
PySparku;
analytické databáze;
Power BI modelu;
DAX.
Hlavní zásada
Transformaci provést tam,
kde bude efektivní,
kontrolovatelná
a opakovatelná.
B6 — Návrh datových vrstev

Navrhnout:

raw nebo Bronze;
staging;
Silver;
Gold;
faktové tabulky;
dimenzní tabulky;
analytické agregace;
sémantický model;
KPI;
reporting.
B7 — Data Quality

Určit:

validační pravidla;
povinné hodnoty;
duplicity;
klíče a vazby;
rozsahy hodnot;
reconciliation;
varování;
kritické chyby;
podmínky publikace.
B8 — Automatizace a monitoring

Navrhnout:

trigger;
scheduler;
pořadí úloh;
závislosti;
paralelní kroky;
retry;
error handling;
logování;
secrets;
monitoring;
podmínky obnovení Power BI.
B9 — Governance a odpovědnosti

Určit:

data ownera;
vlastníka zdrojového systému;
správce pipeline;
odpovědnost za kvalitu;
správce sémantického modelu;
správce reportu;
přístupová oprávnění;
odpovědnost za řešení chyby.
B10 — Přiměřenost architektury

Ověřit:

zda není použito příliš mnoho nástrojů;
zda lze filtrovat a agregovat ve zdroji;
zda je potřeba cloud;
zda je potřeba Spark;
zda je potřeba lakehouse;
zda řešení zvládne dostupný tým;
zda provozní složitost odpovídá business hodnotě;
zda lze řešení jednodušeji udržovat.
B11 — Zamítnuté alternativy

Zdokumentovat:

které varianty byly posouzeny;
proč byla zvolena výsledná varianta;
proč nebyl použit jednodušší nástroj;
proč nebyla použita pokročilejší technologie;
za jakých okolností by se doporučení změnilo.
B12 — Závěrečná case study
Energy Consumption Architecture
Zdroje
SQL databáze s odečty;
API s cenami energií;
API s počasím;
Excel s rozpočty;
historické CSV a Parquet soubory;
Power BI.
Business výstupy
spotřeba energie;
celkové náklady;
náklady podle budovy;
odchylka od rozpočtu;
spotřeba podle počasí;
vývoj v čase;
neobvyklé zvýšení spotřeby;
plnění úsporných cílů.
Úkol

Navrhnout:

architekturu;
datový tok;
úložiště;
transformační nástroje;
datové vrstvy;
datovou kvalitu;
analytické tabulky;
sémantický model;
automatizaci;
monitoring;
odpovědnosti;
budoucí škálování;
zamítnuté technologie.
Výstupy bloku
tools-case-studies/
└── 07_energy-data-solution-design.md

tools-mini-tests/
└── minitesty-volba-nastroju.md

Nový cheatsheet nebude potřeba. Teorii již pokryjí:

modern-data-stack-cheatsheet.md;
automation-cheatsheet.md.
Časový odhad bloku

Přibližně 3–5 hodin.

Blok 3 — Analytical Workflow Portfolio
Hlavní cíl

Samostatně navrhnout a realizovat analytické řešení od business problému až po automatizovaný a zdokumentovaný výstup.

Toto nebude pouze další lekce. Půjde o závěrečnou portfolio fázi.

Business Understanding
→ Architecture Decision
→ Data Acquisition
→ Data Quality
→ Data Preparation
→ Analysis
→ Data Model
→ Reporting
→ Interpretation
→ Automation
→ Delivery
→ Documentation
C1 — Business Understanding
Témata
business kontext;
cílový uživatel;
rozhodnutí, které má analýza podpořit;
analytické otázky;
definice KPI;
rozsah projektu;
předpoklady;
omezení;
kritéria úspěchu.
Výstup
business-requirements.md
C2 — Data Source Assessment
Témata
dostupné zdroje;
význam jednotlivých tabulek;
granularita;
datové typy;
objem dat;
historie;
frekvence změn;
kvalita;
přístupová omezení;
osobní a citlivé údaje.
Výstup
data-sources.md
data-dictionary.md
C3 — Architecture Decision
Témata
výběr nástrojů;
role SQL;
role Pythonu;
role Power Query;
role Power BI;
forma úložiště;
datové vrstvy;
automatizace;
zamítnuté alternativy;
zdůvodnění přiměřenosti řešení.
Výstup
architecture.md
C4 — Data Acquisition a Raw Layer
Témata
SQL extraction;
API;
CSV, JSON a Excel;
uchování původních dat;
timestamp načtení;
oddělení raw dat;
reprodukovatelnost;
dokumentace původu dat.
Výstup
data/raw/
C5 — Data Quality, Cleaning a Validation
Témata
missing values;
duplicity;
datové typy;
neplatné hodnoty;
klíče;
referenční integrita;
časová návaznost;
business pravidla;
reconciliation;
audit změn;
validace před publikací.
Výstup
data-quality-report.md
C6 — Transformation a Business Logic
Témata
filtrování;
joiny;
agregace;
výpočty;
business kategorizace;
rozdělení práce mezi SQL a Python;
příprava faktů a dimenzí;
Gold tabulky;
dokumentace transformačních pravidel.
C7 — Exploratory Data Analysis
Témata
distribuce;
trendy;
porovnání skupin;
odchylky;
outliers;
vztahy mezi proměnnými;
segmentace;
formulace a ověřování hypotéz;
hledání relevantních business zjištění.
C8 — Statistická analýza

Použije se pouze tehdy, když odpovídá business otázce.

Možná témata:

deskriptivní statistika;
korelace;
testování rozdílů;
intervaly spolehlivosti;
jednoduchá regrese;
interpretace statistického výsledku;
omezení a riziko nesprávného závěru.

Statistiku nebudeme přidávat pouze proto, aby projekt vypadal složitěji.

C9 — Datový a sémantický model
Témata
granularita faktové tabulky;
faktové a dimenzní tabulky;
surrogate keys;
vztahy;
kardinalita;
kalendářní dimenze;
jednosměrné filtrování;
measures;
hierarchie;
formátování;
skrytí technických sloupců.
C10 — KPI a DAX
Témata
základní míry;
poměrové ukazatele;
časové porovnání;
plán versus skutečnost;
dynamické filtrování;
správný kontext výpočtu;
popis business významu každé míry.
C11 — Dashboard
Témata
cílová skupina;
informační hierarchie;
KPI karty;
trendy;
porovnání kategorií;
tabulkové detaily;
filtry a slicery;
tooltipy;
navigace;
čitelnost;
omezení počtu vizuálů;
podpora rozhodování.
C12 — Interpretace a doporučení
Témata
hlavní zjištění;
business význam;
oddělení faktu od domněnky;
omezení analýzy;
rizika;
doporučení;
očekávaný přínos;
navržený další krok.
Hlavní zásada
Výsledek
≠ pouze číslo

Výsledek
= číslo + kontext + význam + doporučení
C13 — Automation a Monitoring
Témata
převod procesu do .py;
scheduler;
pipeline;
validace vstupů;
error handling;
logging;
secrets;
bezpečná publikace;
Power BI refresh;
kontrola poslední aktualizace;
zachování posledního správného výstupu.

Automatizace se použije pouze u projektu, kde dává smysl opakované zpracování.

C14 — Delivery a distribuce
Témata
Power BI;
Excel export;
CSV nebo Parquet;
databázová tabulka;
sdílení výsledku;
cílový uživatel;
frekvence distribuce;
oprávnění;
verze výstupu;
archivace.
C15 — Dokumentace a GitHub
Povinné části README
business problém;
cílový uživatel;
datové zdroje;
použitá architektura;
role nástrojů;
datová kvalita;
transformační proces;
KPI;
analýza;
dashboard;
zjištění;
doporučení;
automatizace;
omezení;
návod ke spuštění;
struktura repozitáře.
Další dokumentace
requirements.txt;
.env.example;
.gitignore;
SQL skripty;
Python skripty;
datový slovník;
screenshoty dashboardu;
ukázkové výstupy.
C16 — Finální kontrola projektu

Projekt zkontrolujeme z pohledu:

datového analytika;
BI specialisty;
hiring managera;
recruitera;
technické reprodukovatelnosti;
pravdivosti prezentovaných dovedností;
relevance pro juniorní pozice.

Prověříme:

zda každý nástroj má jasný účel;
zda projekt není zbytečně komplikovaný;
zda jsou KPI správně definována;
zda závěry vycházejí z dat;
zda lze projekt vysvětlit při pohovoru;
zda README odpovídá skutečné realizaci.
Portfolio projekty

Doporučuji vytvořit tři až čtyři větší end-to-end projekty. Každý nemusí používat všechny technologie.

Projekt 1 — SQL a Power BI

Hlavní důraz:

business analýza;
SQL;
datový model;
DAX;
management dashboard.
Projekt 2 — Python, API a automatizace

Hlavní důraz:

API;
Pandas;
validace;
automatické spuštění;
logging;
Power BI výstup.
Projekt 3 — Kombinovaný analytický projekt

Hlavní důraz:

více datových zdrojů;
SQL;
Python;
Power Query;
Power BI;
architektonické rozhodování;
automatizace.
Volitelný projekt 4 — Excel nebo business analýza

Hlavní důraz:

Excel;
Power Query;
analytická interpretace;
management reporting;
rychlé ad-hoc řešení bez zbytečné infrastruktury.
Konečný harmonogram
Modern Data Stack
→ dokončení současného bloku

Automation
→ 14 až 19 hodin

Volba nástrojů a návrh řešení
→ 3 až 5 hodin

Analytical Workflow Portfolio
→ metodika a následně jednotlivé projekty

Jeden větší portfolio projekt může podle rozsahu zabrat přibližně 20–40 hodin. Důležitější než počet projektů bude jejich dokončenost, dokumentace a schopnost vysvětlit použitá rozhodnutí.

Hranice mezi bloky
AUTOMATION

Jak proces automaticky a spolehlivě spustit?


VOLBA NÁSTROJŮ

Jaké řešení navrhnout a proč?


ANALYTICAL WORKFLOW

Jak navržené řešení skutečně vytvořit,
analyzovat, automatizovat a předat?

Tím se obsah nebude zbytečně opakovat. Stejné pojmy se objeví vícekrát, ale pokaždé v jiné úrovni: nejprve se je naučíš, potom podle nich navrhneš architekturu a nakonec je použiješ v reálném projektu.







1. Automation

Zůstane v repozitáři da-tools-portfolio.

Prakticky projdeme:

převod notebooku na Python skript;
API automatizaci;
SQL Server LocalDB;
validaci a error handling;
logging;
environment variables a secrets;
.bat;
Windows Task Scheduler;
GitHub Actions;
princip SQL Server Agentu;
Power Query refresh;
Power BI scheduled refresh a gateway;
závěrečnou automatizační case study.

Výsledkem bude funkční, ale přiměřeně jednoduchý automatizovaný proces.

2. Volba a kombinace nástrojů

Také zůstane v da-tools-portfolio.

Půjde především o závěrečnou architektonickou case study:

business požadavky;
vlastnosti dat;
volba ingestion;
volba úložiště;
rozdělení transformací mezi SQL, Python a Power Query;
návrh datových vrstev;
Data Quality;
automatizace a monitoring;
Power BI;
odpovědnosti;
škálovatelnost;
náklady a složitost;
zamítnuté alternativy;
vysvětlení, proč některé technologie nepotřebujeme.

Nebudeme zde celé řešení implementovat.

3. Analytical Workflow

Dostane samostatné repo:

da-workflow
Úvodní teoretická část

Bude krátká a prakticky zaměřená:

postup od business otázky k řešení;
definice KPI;
posouzení zdrojů;
granularita;
výběr nástrojů;
datová kvalita;
analýza;
modelování;
reporting;
interpretace;
automatizace;
dokumentace;
kontrolní seznam dokončeného projektu.

Teorie nebude znovu podrobně vysvětlovat SQL, Pandas, DAX ani automatizaci. Bude fungovat jako metodika:

Jak správně vést celý analytický projekt?
Praktická část

Po metodice už budeme pracovat na jednotlivých projektech:

Projekt 1
→ zadání
→ řešení
→ review
→ dokončení

Projekt 2
→ zadání
→ řešení
→ review
→ dokončení

Projekt 3
→ zadání
→ řešení
→ review
→ dokončení

Každý projekt použije pouze technologie, které mají jasný účel.

Výsledné rozdělení repozitářů
da-tools-portfolio
│
├── nástroje
├── Modern Data Stack
├── Automation
└── Volba a kombinace nástrojů

da-workflow
│
├── metodika Analytical Workflow
├── projektová šablona
└── praktické end-to-end projekty