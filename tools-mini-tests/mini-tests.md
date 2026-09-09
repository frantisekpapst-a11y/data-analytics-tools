# Data Analytics Tools — Mini Tests

## Minitesty — Lekce 1: Data Analytics Environment Map

### 1. Vrstvy datového prostředí

Firma používá následující postup:

1. Objednávky vznikají v e-shopové SQL databázi.
2. Python pomocí SQL dotazu načte objednávky za poslední měsíc.
3. Načtená data se uloží do analytické databáze.
4. SQL spojí objednávky se zákazníky a produkty a vypočítá tržby.
5. Power BI nad připravenými tabulkami vytvoří vztahy a DAX míry.
6. Dashboard zobrazí tržby, zisk a výsledky regionů.
7. Obchodní ředitel podle výsledků upraví regionální rozpočty.

Přiřaď ke každému kroku hlavní vrstvu:

- Data Source
- Data Ingestion
- Data Storage
- Data Transformation
- Analytical Layer
- Reporting
- Business Decision

#### Řešení

1. **Data Source** – provozní SQL databáze je zdrojem objednávek.
2. **Data Ingestion** – Python spustí SQL dotaz a načte data.
3. **Data Storage** – analytická databáze uchovává načtená data.
4. **Data Transformation** – SQL data spojí a vypočítá tržby.
5. **Analytical Layer** – Power BI obsahuje vztahy, datový model a DAX míry.
6. **Reporting** – dashboard vizuálně prezentuje výsledky.
7. **Business Decision** – obchodní ředitel podle výsledků upraví rozpočty.

---

### 2. Role Pythonu v datovém procesu

Python:

1. získá data z API;
2. odstraní duplicity a vypočítá tržby;
3. zapíše výsledek do SQL databáze.

Jaké hlavní operace Python postupně provádí?

#### Řešení

1. **Extract / Data Ingestion** – Python získá data z API.
2. **Data Transformation** – Python data vyčistí a vytvoří nový výpočet.
3. **Load** – Python zapíše výsledek do cílové databáze.

SQL databáze následně představuje **Data Storage**, protože uložená data uchovává.

```text
Python → Extract
Python → Transform
Python → Load
SQL databáze → Storage
```

---

### 3. Extract, Transform and Load v Power Query

Firma má objednávky v SQL databázi. Power Query:

1. připojí se k databázi;
2. načte tabulku objednávek;
3. odstraní nepotřebné sloupce;
4. nahradí chybějící region hodnotou `"Unknown"`;
5. načte výsledek do Power BI datového modelu.

Přiřaď ke každému kroku hlavní operaci:

- Extract
- Transform
- Load

#### Řešení

1. **Extract** – Power Query vytvoří připojení ke zdroji.
2. **Extract** – Power Query získá tabulku z databáze.
3. **Transform** – odstraní nepotřebné sloupce.
4. **Transform** – nahradí chybějící hodnoty.
5. **Load** – načte připravená data do Power BI modelu.

Celý proces:

```text
SQL Database
→ Extract
→ Transform
→ Load
→ Power BI Model
```

Samotné vytvoření připojení ještě data nepřenáší, ale je součástí fáze Extract, která pokračuje načtením tabulky.

---

### 4. Role v datovém týmu

Přiřaď ke každé činnosti nejvhodnější hlavní roli:

- **DE** – Data Engineer
- **DA** – Data Analyst
- **BI** – BI Developer

1. Zjistí, jaká rozhodnutí má report podporovat.
2. Vytvoří pravidelný přenos dat z provozního systému do datového skladu.
3. Navrhne vztahy mezi faktovou a dimenzní tabulkou.
4. Analyzuje příčinu poklesu tržeb.
5. Sleduje, zda datová pipeline proběhla bez chyby.
6. Vytvoří složitější DAX míry.
7. Interpretuje výsledky a připraví business doporučení.

#### Řešení

1. **DA** – datový analytik vyjasňuje business potřebu.
2. **DE** – data engineer zajišťuje pravidelný přenos dat.
3. **BI** – BI developer navrhuje Power BI datový model.
4. **DA** – datový analytik hledá příčinu business výsledku.
5. **DE** – data engineer sleduje fungování datových pipeline.
6. **BI** – tvorba složitějších DAX měr je typická BI činnost.
7. **DA** – interpretace a doporučení jsou klíčovou odpovědností analytika.

Hranice mezi rolemi nejsou pevné. V praxi mohou na některých činnostech spolupracovat různí členové datového týmu.

---

### 5. Pořadí vrstev datového prostředí

Seřaď následující vrstvy od vzniku dat po jejich business využití:

- Reporting
- Data Source
- Business Decision
- Data Transformation
- Analytical Layer
- Data Ingestion
- Data Storage

#### Řešení

```text
Data Source
→ Data Ingestion
→ Data Storage
→ Data Transformation
→ Analytical Layer
→ Reporting
→ Business Decision
```

Jde o orientační mapu datových vrstev. Skutečné pořadí uložení a transformace se může lišit podle toho, zda proces využívá ETL nebo ELT.

---

### 6. Ingestion, Load and Storage

Python získá data z API a zapíše je do databáze.

Urči, co představuje:

1. Python při získání dat z API.
2. Python při zápisu dat.
3. Databáze po uložení dat.

#### Řešení

1. **Extract / Data Ingestion** – Python získává data ze zdroje.
2. **Load** – Python zapisuje data do cílového prostředí.
3. **Data Storage** – databáze uložená data uchovává.

Hlavní rozdíl:

```text
Load = zápis dat
Storage = místo uchování dat
```

---

### 7. Role SQL

SQL dotaz:

- spojí objednávky se zákazníky;
- odfiltruje zrušené objednávky;
- vypočítá tržby podle regionu.

Jakou hlavní roli zde SQL plní?

- Data Ingestion
- Data Storage
- Data Transformation
- Reporting

#### Řešení

Správná odpověď je:

```text
Data Transformation
```

SQL zde data spojuje, filtruje, počítá a agreguje. Neprovádí reporting ani neposkytuje samotné úložiště.

---

### 8. Součásti Power BI

Přiřaď každou součást Power BI k její hlavní roli:

| Součást | Role |
|---|---|
| Power Query | ? |
| Vztahy mezi tabulkami | ? |
| DAX míry | ? |
| Sloupcový graf | ? |

Použij následující možnosti:

- Data Ingestion / Data Transformation
- Analytical Layer
- Reporting

Jednu možnost můžeš použít vícekrát.

#### Řešení

| Součást | Hlavní role |
|---|---|
| Power Query | Data Ingestion / Data Transformation |
| Vztahy mezi tabulkami | Analytical Layer |
| DAX míry | Analytical Layer |
| Sloupcový graf | Reporting |

Power BI obsahuje více částí datového procesu:

```text
Power Query
→ příprava dat

Semantic Model
→ tabulky, vztahy a business logika

DAX
→ dynamické výpočty

Report
→ vizuální prezentace výsledků
```

---

### 9. Datový a sémantický model

Doplň následující věty:

1. Vztahy jsou __________ datového modelu.
2. Sémantický model dává technickým datům __________ význam.
3. Power BI report používá sémantický model pro __________ výsledků.

#### Řešení

1. Vztahy jsou **součástí** datového modelu.
2. Sémantický model dává technickým datům **business** význam.
3. Power BI report používá sémantický model pro **zobrazování** výsledků.

Základní rozlišení:

```text
Relationship
→ propojení tabulek

Data Model
→ tabulky, sloupce, vztahy a výpočty

Semantic Model
→ datový model doplněný o business význam

Report
→ vizuální prezentace dat ze sémantického modelu
```

---

### 10. Odpovědnost datového analytika

Které tvrzení je nejpřesnější?

**A.** Datový analytik potřebuje ovládat správu celé cloudové infrastruktury.

**B.** Datový analytik se má orientovat v celém datovém toku, ale jeho hlavní odpovědností je práce s business otázkou, daty, analýzou a interpretací.

**C.** Datový analytik pracuje pouze s hotovým dashboardem.

**D.** Datový analytik nesmí provádět transformace dat.

#### Řešení

Správná odpověď je **B**.

Datový analytik by měl rozumět celému toku dat:

```text
Data Source
→ Ingestion
→ Storage
→ Transformation
→ Analytical Layer
→ Reporting
→ Business Decision
```

Nemusí však spravovat veškerou datovou nebo cloudovou infrastrukturu.

Jeho hlavní odpovědností je:

- pochopení business problému;
- získání potřebných dat;
- kontrola jejich kvality;
- analýza;
- interpretace;
- reporting;
- formulace doporučení.

---

# Minitesty — Lekce 2: Jupyter Notebook Workflow

### 11. Jupyter Notebook a Python skript

Jaký je hlavní rozdíl mezi souborem `.ipynb` a běžným Python skriptem `.py`?

**A.** `.ipynb` kombinuje kód, Markdown a výstupy.

**B.** `.py` nelze spouštět ve VS Code.

**C.** `.py` automaticky ukládá všechny výstupy.

**D.** `.ipynb` neumožňuje spouštět Python.

#### Řešení

Správná odpověď je **A**.

Jupyter Notebook může v jednom souboru kombinovat:

- spustitelný Python kód;
- Markdown dokumentaci;
- tabulky, grafy a další výstupy buněk.

Python skript `.py` obsahuje především zdrojový kód a výsledky jednotlivých příkazů automaticky neukládá jako součást souboru.

---

### 12. Markdown buňka

Který typ buňky je vhodný pro nadpis, business kontext a interpretaci výsledků?

**A.** Code

**B.** Raw

**C.** Markdown

**D.** Output

#### Řešení

Správná odpověď je **C**.

Markdown buňka slouží k dokumentaci notebooku. Může obsahovat například:

- nadpisy;
- odstavce;
- seznamy;
- popis analytického postupu;
- interpretaci výsledků;
- omezení a doporučení.

Code buňka je naopak určená především ke spouštění programového kódu.

---

### 13. Paměť kernelu

Kde jsou během práce uložené vytvořené proměnné a načtené DataFrame?

**A.** V paměti kernelu

**B.** Automaticky v CSV

**C.** Pouze v Markdown buňkách

**D.** Automaticky v Git repozitáři

#### Řešení

Správná odpověď je **A**.

Kernel je běžící výpočetní prostředí notebooku. Ve své paměti uchovává například:

- vytvořené proměnné;
- importované knihovny;
- načtené DataFrame;
- výsledky výpočtů potřebné pro další buňky.

Proměnné nejsou automaticky ukládány do CSV ani do Git repozitáře.

---

### 14. Restart kernelu

Co se stane po restartování kernelu?

**A.** Smaže se celý notebook.

**B.** Smažou se zdrojové soubory.

**C.** Proměnné zůstanou dostupné.

**D.** Vymaže se aktuální stav paměti kernelu.

#### Řešení

Správná odpověď je **D**.

Restart kernelu vymaže aktuální stav paměti, tedy například proměnné, DataFrame a provedené importy. Buňky notebooku ani zdrojové soubory na disku se nesmažou.

Po restartu je nutné potřebný stav znovu vytvořit spuštěním buněk.

---

### 15. Reprodukovatelnost notebooku

Jak nejlépe ověřit, že je notebook reprodukovatelný?

**A.** Spustit pouze poslední buňku.

**B.** Restartovat kernel a spustit všechny buňky shora dolů.

**C.** Notebook pouze uložit.

**D.** Zkontrolovat poslední zobrazený výstup.

#### Řešení

Správná odpověď je **B**.

Nejvhodnější kontrola je:

```text
Restart Kernel
→ Run All
→ kontrola všech výstupů a chyb
```

Tím se odstraní skrytý stav paměti a ověří se, že notebook funguje ve správném pořadí od první do poslední buňky.

---

### 16. Skrytý stav a pořadí buněk

Buňka používá proměnnou `df`, ale buňka s načtením dat je umístěna až pod ní. Proč může notebook přesto dočasně fungovat?

**A.** Pandas automaticky spustí následující buňku.

**B.** Markdown vytvoří chybějící DataFrame.

**C.** `df` zůstalo v paměti kernelu z dřívějšího spuštění.

**D.** Jupyter vždy ignoruje pořadí spuštění kódu.

#### Řešení

Správná odpověď je **C**.

Kernel může obsahovat proměnnou vytvořenou při dřívějším spuštění buněk v jiném pořadí. Notebook potom zdánlivě funguje, ale po restartu kernelu selže, protože `df` nebylo před použitím vytvořeno.

Tomuto problému se říká skrytý stav notebooku.

---

### 17. Atribut `shape`

Který zápis správně zjistí počet řádků a sloupců DataFrame `df`?

**A.** `df.shape`

**B.** `df.shape()`

**C.** `pd.shape(df)`

**D.** `df.info.shape`

#### Řešení

Správná odpověď je **A**.

`shape` je atribut DataFrame, a proto se zapisuje bez kulatých závorek:

```python
df.shape
```

Vrací tuple ve tvaru:

```text
(počet řádků, počet sloupců)
```

Jednotlivé hodnoty lze získat pomocí jejich pozice:

```python
df.shape[0]  # počet řádků
df.shape[1]  # počet sloupců
```

---

### 18. Více výstupů v jedné buňce

Jak v jedné Code buňce spolehlivě zobrazit `df.shape` i tabulku `df.head()`?

**A.** Napsat oba výrazy pod sebe bez dalších funkcí.

**B.** Převést buňku na Markdown.

**C.** Za oba výrazy přidat středník.

**D.** Použít `print()` pro `shape` a `display()` pro tabulku.

#### Řešení

Správná odpověď je **D**.

Jupyter automaticky zobrazí zpravidla pouze poslední samostatný výraz v Code buňce. Více výstupů proto zobrazíme explicitně:

```python
print("Shape:", df.shape)
display(df.head())
```

`print()` je vhodný pro textové a jednoduché výstupy. `display()` poskytuje přehledné tabulkové zobrazení DataFrame.

---

### 19. Relativní cesta

Jaká je hlavní výhoda relativní cesty k datasetu v portfolio projektu?

**A.** Datový soubor se automaticky zmenší.

**B.** Projekt je přenositelnější mezi různými počítači.

**C.** DataFrame vždy zabere méně paměti.

**D.** Git automaticky opraví obsah datasetu.

#### Řešení

Správná odpověď je **B**.

Relativní cesta vychází ze struktury projektu a není pevně svázána s konkrétní uživatelskou složkou nebo počítačem.

Například:

```python
DATA_PATH = (
    PROJECT_ROOT
    / "datasets"
    / "raw"
    / "sales.csv"
)
```

Takový projekt lze snáze přenést, sdílet nebo naklonovat z GitHubu.

---

### 20. Profesionální struktura notebooku

Které pořadí nejlépe odpovídá profesionální struktuře analytického notebooku?

**A.** Analysis → Business Context → Data Sources → Findings

**B.** Data Preparation → Recommendations → Data Validation → Analysis

**C.** Business Context → Data Sources → Data Validation → Data Preparation → Analysis → Findings

**D.** Findings → Analysis → Data Sources → Business Context

#### Řešení

Správná odpověď je **C**.

Profesionální analytický notebook má logickou strukturu od zadání až po závěry:

```text
Business Context
→ Data Sources
→ Data Validation
→ Data Preparation
→ Analysis
→ Findings
→ Limitations
→ Recommendations
```

Taková struktura pomáhá čtenáři pochopit účel analýzy, použitá data, provedený postup i výslednou business interpretaci.

---

# Minitesty — Lekce 3: Větší datasety a efektivní práce s daty

### 21. Velikost dat na disku a v paměti

Které tvrzení nejpřesněji popisuje vztah mezi velikostí souboru na disku a velikostí DataFrame v paměti?

**A.** Obě velikosti jsou vždy stejné.

**B.** DataFrame je vždy menší než původní soubor.

**C.** Velikosti se mohou lišit podle formátu, komprese a datových typů.

**D.** Rozdíl vzniká pouze při načítání z databáze.

#### Řešení

Správná odpověď je **C**.

Soubor na disku a DataFrame používají rozdílnou reprezentaci dat. Výslednou velikost ovlivňuje například:

- komprese;
- textový nebo binární formát;
- datové typy;
- způsob uložení textových hodnot v paměti.

V našem testu měl například Parquet soubor na disku `3,36 MB`, ale načtený DataFrame zabíral přibližně `26,04 MB`.

---

### 22. Datový typ `category`

Pro který sloupec je datový typ `category` nejvhodnější?

**A.** `order_id`, který je pro každý řádek unikátní.

**B.** `region`, který obsahuje pět opakujících se hodnot.

**C.** `revenue`, který obsahuje číselné částky.

**D.** `customer_comment`, který obsahuje různé dlouhé komentáře.

#### Řešení

Správná odpověď je **B**.

Datový typ `category` je vhodný pro sloupce s malým počtem často opakovaných hodnot. Pandas může místo opakovaného ukládání textů používat seznam kategorií a číselné kódy.

V našem testu převod sloupců `region`, `product` a `channel` na `category` snížil spotřebu paměti přibližně o `43,95 %`.

---

### 23. Načítání vybraných sloupců

Jaký je hlavní rozdíl mezi načtením vybraných sloupců z CSV a z Parquetu?

**A.** CSV uchovává datové typy lépe než Parquet.

**B.** Parquet může díky sloupcovému uložení nepotřebné sloupce vůbec nenačíst.

**C.** Parametr `usecols` způsobí, že CSV nemusí přečíst textové řádky.

**D.** Parquet musí vždy načíst všechny sloupce.

#### Řešení

Správná odpověď je **B**.

Parquet ukládá data po sloupcích. Parquet engine proto může načíst pouze sloupce uvedené v parametru `columns`.

U CSV parametr `usecols` zajistí, že se do výsledného DataFrame uloží jen vybrané sloupce. CSV parser však stále musí projít textové řádky a rozpoznat jejich hodnoty.

---

### 24. SQL pushdown

Co znamená SQL pushdown?

**A.** Celá databázová tabulka se načte do Pandas a tam se vyfiltruje.

**B.** SQL dotaz se uloží jako CSV soubor.

**C.** Power BI odešle všechna data do Pythonu.

**D.** Výběr sloupců, filtrování nebo agregaci provede databáze před odesláním výsledku.

#### Řešení

Správná odpověď je **D**.

SQL pushdown znamená, že databáze provede operaci co nejblíže uloženým datům. Do Pandas nebo Power BI následně odešle pouze potřebný výsledek.

```text
Databáze
→ SELECT / WHERE / JOIN / GROUP BY
→ menší výsledek
→ Python nebo Power BI
```

---

### 25. Načítání pomocí chunks

Jaký je hlavní účel parametru `chunksize` při načítání CSV?

**A.** Automaticky odstraní duplicitní řádky.

**B.** Postupně načítá části souboru a omezuje množství dat držených současně v paměti.

**C.** Převede CSV do Parquetu.

**D.** Zajistí, že se načte jen jeden vybraný sloupec.

#### Řešení

Správná odpověď je **B**.

Například `chunksize=50_000` znamená, že Pandas postupně zpracovává části po 50 000 řádcích. Celý CSV soubor proto nemusí být současně uložený v paměti.

CSV se však stále postupně projde celé. Výběr sloupců zajišťuje parametr `usecols`, nikoliv `chunksize`.

---

### 26. Agregace při zpracování chunks

Proč jsme při zpracování CSV pomocí chunks provedli `groupby()` dvakrát?

**A.** První `groupby()` odstranil duplicity a druhý opravil datové typy.

**B.** První `groupby()` filtroval rok a druhý vybíral sloupce.

**C.** Nejprve jsme agregovali každý chunk a potom spojili a znovu agregovali dílčí výsledky.

**D.** Druhá agregace byla zbytečná a výsledek nijak nezměnila.

#### Řešení

Správná odpověď je **C**.

Každý chunk obsahoval vlastní řádky pro stejné regiony. První `groupby()` vytvořil souhrn uvnitř každého chunku.

Po spojení dílčích výsledků existovalo několik součtů například pro Prahu. Druhý `groupby()` je spojil do jednoho celkového výsledku za region.

```text
1. agregace
→ souhrn každého chunku

2. agregace
→ celkový souhrn všech chunků
```

---

### 27. Partitioning

Co nejpřesněji znamená partitioning datasetu?

**A.** Rozdělení zobrazení DataFrame na několik obrazovek.

**B.** Fyzické rozdělení jednoho logického datasetu do souborů nebo složek podle vybraného sloupce.

**C.** Postupné čtení jednoho CSV souboru po blocích.

**D.** Převod všech textových sloupců na `category`.

#### Řešení

Správná odpověď je **B**.

Partitioning fyzicky rozděluje jeden logický dataset například podle roku:

```text
sales_partitioned
├── year=2024
└── year=2025
```

Při požadavku na rok 2025 může nástroj přeskočit celou partition `year=2024`.

Chunks naproti tomu pouze řídí postupné čtení částí jednoho souboru.

---

### 28. Partitioning a Parquet

Proč se partitioning často kombinuje právě s Parquetem?

**A.** Parquet umožňuje mít několik excelových listů v jednom souboru.

**B.** Parquet je jediný formát, který lze fyzicky rozdělit.

**C.** Lze přeskočit nepotřebné partition a z potřebných souborů načíst jen vybrané sloupce a části dat.

**D.** Partitionovaný Parquet je vždy rychlejší bez ohledu na velikost a strukturu dat.

#### Řešení

Správná odpověď je **C**.

Partitioning umožňuje přeskočit celé nepotřebné složky nebo soubory. Sloupcový formát Parquet navíc umožňuje z potřebných souborů načíst jen vybrané sloupce a podle metadat přeskočit některé části dat.

Partitioning lze použít také s CSV, JSON nebo databázovými tabulkami. Parquet je však pro partitionované analytické datasety obvykle efektivnější.

Partitioning nezaručuje automatické zrychlení každého dotazu. U malého datasetu může být režie práce s více soubory větší než dosažená úspora.

---

### 29. Agregace před načtením do Power BI

Kdy dává smysl agregovat data v SQL ještě před načtením do Power BI?

**A.** Vždy, protože detailní data se do Power BI nikdy nenačítají.

**B.** Když report potřebuje pouze souhrnnou granularitu a detailní řádky by nevyužil.

**C.** Pouze tehdy, když databáze obsahuje méně než 1 000 řádků.

**D.** Když chceme všechny výpočty později provést nad jednotlivými objednávkami.

#### Řešení

Správná odpověď je **B**.

Pokud report potřebuje například pouze měsíční tržby podle regionu, může databáze provést agregaci a předat do Power BI jen výsledný souhrn.

V našem testu SQL agregace snížila 300 000 detailních řádků na 120 agregovaných řádků.

Pokud by však report potřeboval analýzu jednotlivých objednávek, taková agregace by odstranila potřebný detail. Granularitu proto volíme podle účelu výsledného reportu.

---

### 30. Kdy použít Spark

Kdy je nejrozumnější začít uvažovat o Sparku?

**A.** Jakmile dataset obsahuje více než 10 000 řádků.

**B.** Vždy, když používáme Parquet.

**C.** Když ani po rozumné optimalizaci nestačí paměť nebo výkon jednoho počítače.

**D.** Když chceme z DataFrame vytvořit jednoduchý graf.

#### Řešení

Správná odpověď je **C**.

Neexistuje univerzální počet řádků, od kterého je nutné použít Spark. Nejdříve vyzkoušíme:

- načítání potřebných sloupců;
- filtrování ve zdroji;
- SQL pushdown;
- agregaci před načtením;
- vhodné datové typy;
- Parquet;
- partitioning;
- zpracování pomocí chunks.

Spark začíná dávat smysl tehdy, když ani po těchto optimalizacích jeden počítač neposkytuje dostatek paměti nebo výkonu.

---

