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

# Minitesty — Lekce 4: Spark — proč existuje

### 31. Hlavní účel Apache Spark

Jaký problém Apache Spark primárně pomáhá řešit?

**A.** Tvorbu kontingenčních tabulek v Excelu.

**B.** Distribuované zpracování rozsáhlých dat.

**C.** Ruční úpravu malých CSV souborů.

**D.** Vytváření vizuálů v Power BI.

#### Řešení

Správná odpověď je **B**.

Spark je výpočetní engine, který dokáže rozdělit data a výpočty mezi více výpočetních prostředků, typicky v clusteru. Pomáhá tak zpracovávat rozsáhlá data a mezivýsledky, které jeden počítač nezvládne dostatečně rychle nebo kvůli nedostatku paměti.

---

### 32. Spotřeba operační paměti

Proč může zpracování dat vyžadovat výrazně více operační paměti, než odpovídá velikosti zdrojového souboru?

**A.** Každý soubor se automaticky převádí na video.

**B.** Spark vždy ukládá celý cluster do paměti.

**C.** Vznikají pracovní kopie a mezivýsledky operací.

**D.** Velikost souboru vždy odpovídá počtu procesorů.

#### Řešení

Správná odpověď je **C**.

Během zpracování mohou vznikat:

- pracovní kopie dat;

- filtrované DataFrames;

- výsledky joinů;

- mezivýsledky agregací a řazení.

V paměti navíc mohou být data reprezentována jinak než v komprimovaném souboru na disku. Potřebná operační paměť proto nemusí odpovídat velikosti zdrojového souboru.

---

### 33. Optimalizace před použitím Sparku

Analytik nezvládne načíst velký dataset do Pandas. Co by měl udělat dříve, než automaticky zvolí Spark?

**A.** Převést dataset do Excelu.

**B.** Omezit data, datové typy a mezivýsledky.

**C.** Vytvořit co nejvíce kopií DataFrame.

**D.** Načíst dataset současně do více notebooků.

#### Řešení

Správná odpověď je **B**.

Nejdříve je vhodné ověřit, zda lze:

- vybrat pouze potřebné sloupce;

- filtrovat nebo agregovat data už ve zdroji;

- nastavit úspornější datové typy;

- použít efektivnější formát, například Parquet;

- omezit zbytečné kopie a mezivýsledky.

Spark má smysl zvažovat až tehdy, když jeden počítač nestačí ani po rozumné optimalizaci.

---

### 34. Distribuované zpracování

Co znamená distribuované zpracování dat ve Sparku?

**A.** Uložení celého datasetu do jednoho excelového souboru.

**B.** Ruční provádění každého výpočtu samostatným analytikem.

**C.** Postupné zpracování všech dat jediným procesorem.

**D.** Rozdělení dat a výpočtů mezi více propojených počítačů.

#### Řešení

Správná odpověď je **D**.

Spark rozdělí rozsáhlý dataset a jeho zpracování na menší části. Ty mohou být přiděleny více propojeným počítačům a zpracovávány paralelně. Dílčí výsledky se následně spojí do požadovaného výstupu.

---

### 35. Cluster

Co ve Sparku označuje pojem cluster?

**A.** Skupinu propojených počítačů spolupracujících na výpočtu.

**B.** Jeden sloupec obsahující seskupené hodnoty.

**C.** Formát souboru optimalizovaný pro analytiku.

**D.** Výslednou tabulku vytvořenou agregací dat.

#### Řešení

Správná odpověď je **A**.

Cluster je skupina propojených počítačů neboli uzlů, které společně poskytují procesorový výkon a operační paměť pro zpracování úlohy. Spark lze spustit také lokálně na jednom počítači, ale takové prostředí neposkytuje skutečný výkon clusteru s více počítači.

---

### 36. Driver

Jaká je hlavní úloha driveru ve Spark aplikaci?

**A.** Ukládat data jako soubory Parquet.

**B.** Plánovat, rozdělovat a koordinovat výpočet.

**C.** Vytvářet vizualizace v Power BI.

**D.** Nahrazovat všechny workers v clusteru.

#### Řešení

Správná odpověď je **B**.

Driver přijme zadaný kód, vytvoří plán výpočtu, rozdělí práci a koordinuje její provedení. Nepředstavuje všechny pracovní uzly a jeho hlavní úlohou není ukládání dat do konkrétního souborového formátu.

---

### 37. Workers

Jakou roli mají workers ve Spark clusteru?

**A.** Navrhují business požadavky analýzy.

**B.** Vytvářejí hlavní plán celé Spark aplikace.

**C.** Poskytují prostředky pro přidělené výpočty.

**D.** Ukládají všechny výsledky pouze do driveru.

#### Řešení

Správná odpověď je **C**.

Workers jsou pracovní uzly, které poskytují procesorový výkon a operační paměť pro provedení přidělených výpočtů. V praktické architektuře se na workers spouštějí výpočetní procesy označované jako executors.

---

### 38. Spark partition

Co ve Sparku představuje partition?

**A.** Část datasetu určenou k dílčímu zpracování.

**B.** Počítač koordinující celou Spark aplikaci.

**C.** Databázi používanou k ukládání výsledků.

**D.** Report vytvořený po dokončení výpočtu.

#### Řešení

Správná odpověď je **A**.

Partition je část datasetu určená pro jeden dílčí výpočet. Rozdělení dat do partitions umožňuje jejich paralelní zpracování. Partition není worker, samostatný počítač ani databáze.

---

### 39. Počet partitions a workers

Dataset má 12 partitions a Spark cluster 3 workers. Co z toho správně vyplývá?

**A.** Devět partitions se vůbec nezpracuje.

**B.** Každý worker může postupně zpracovat více partitions.

**C.** Spark automaticky vytvoří devět dalších workers.

**D.** Všechny partitions musí zpracovat pouze driver.

#### Řešení

Správná odpověď je **B**.

Počet partitions nemusí odpovídat počtu workers. Jeden worker může během výpočtu postupně zpracovat několik partitions. Dostupné výpočetní prostředky určují, kolik dílčích úloh může probíhat současně.

---

### 40. Režie distribuovaného zpracování

Proč Spark nemusí být rychlejší než Pandas při jednorázovém zpracování malého datasetu?

**A.** Spark neumí filtrovat ani agregovat data.

**B.** Pandas vždy využívá celý výpočetní cluster.

**C.** Režie distribuovaného zpracování může převýšit jeho přínos.

**D.** Spark dokáže načítat pouze velmi velké soubory.

#### Řešení

Správná odpověď je **C**.

Spark musí práci naplánovat, rozdělit a koordinovat. Při distribuovaném zpracování navíc může probíhat komunikace a přenos dat mezi uzly. U malého jednorázového datasetu může tato režie trvat déle než samotný výpočet v Pandas.

---

### 41. Parquet partitioning a Spark partition

Jaký je hlavní rozdíl mezi Parquet partitioningem a Spark partition?

**A.** Oba pojmy vždy označují stejnou strukturu složek.

**B.** První rozděluje uložená data, druhá data pro zpracování.

**C.** První používá pouze Pandas, druhou pouze Excel.

**D.** První označuje worker a druhá označuje driver.

#### Řešení

Správná odpověď je **B**.

Parquet partitioning představuje fyzické rozdělení uložených dat, například do složek podle roku nebo měsíce. Spark partition je část datasetu určená pro dílčí výpočet. Oba principy spolu mohou souviset, ale nejsou totožné.

---

# Minitesty — Lekce 5: Data warehouse, data lake a lakehouse

### 42. Provozní a analytické prostředí

Které prostředí je primárně určené pro historickou analýzu a pravidelný management reporting?

**A.** OLTP databáze provozní aplikace.

**B.** OLAP prostředí datového skladu.

**C.** Paměť spuštěného notebooku.

**D.** Samostatný soubor každého analytika.

#### Řešení

Správná odpověď je **B**.

OLAP prostředí je určené pro analytické dotazy, agregace a práci s historickými daty. Data warehouse může integrovat data z více zdrojových systémů a poskytovat jednotný podklad pro pravidelný reporting.

OLTP databáze je naproti tomu optimalizovaná především pro každodenní provoz, například vytváření objednávek, aktualizaci skladových zásob nebo ukládání plateb.

---

### 43. Faktová tabulka

Která tabulka bude v prodejním hvězdicovém schématu faktovou tabulkou?

**A.** Tabulka produktů s názvem a kategorií.

**B.** Kalendářní tabulka s měsícem a rokem.

**C.** Tabulka prodejních položek s tržbami a množstvím.

**D.** Tabulka regionů s názvem a zemí.

#### Řešení

Správná odpověď je **C**.

Faktová tabulka zachycuje měřitelné obchodní události. V prodejním modelu může jeden řádek představovat jednu položku objednávky a obsahovat například:

- prodané množství;
- skutečnou jednotkovou cenu;
- slevu;
- tržby;
- náklady.

Ostatní uvedené tabulky obsahují popisné atributy a představují dimenze.

---

### 44. Granularita faktové tabulky

Jedna objednávka může obsahovat více produktů. Jaká granularita `fact_sales` umožní analyzovat výsledky podle jednotlivých produktů?

**A.** Jeden řádek na zákazníka.

**B.** Jeden řádek na položku objednávky.

**C.** Jeden řádek na kalendářní rok.

**D.** Jeden řádek na produktovou kategorii.

#### Řešení

Správná odpověď je **B**.

Při granularitě položky objednávky platí:

```text
1 řádek
=
1 produkt na 1 objednávce
```

Každý řádek může obsahovat produkt, množství, cenu, slevu, tržbu a náklady konkrétní položky. Pokud objednávka obsahuje čtyři produktové položky, vzniknou ve `fact_sales` čtyři řádky.

---

### 45. Technický klíč dimenze

Jakou roli má `customer_key` vytvořený uvnitř datového skladu?

**A.** Představuje hodnotu zákazníkových tržeb.

**B.** Nahrazuje všechny atributy zákazníka.

**C.** Určuje pořadí měsíců v kalendáři.

**D.** Slouží jako surrogate key dimenze zákazníka.

#### Řešení

Správná odpověď je **D**.

`customer_key` je surrogate key, tedy technický klíč vytvořený v datovém skladu. Používá se jako:

- primární klíč v `dim_customer`;
- cizí klíč ve `fact_sales`;
- základ vztahu mezi dimenzí a faktovou tabulkou.

Původní `customer_id` ze CRM zůstává v dimenzi jako natural neboli business key, aby bylo možné zákazníka dohledat ve zdrojovém systému.

---

### 46. Neznámý člen dimenze

Objednávka odkazuje na zákazníka, který nebyl nalezen v `dim_customer`. Jaký postup nejlépe zachová tržby i referenční integritu?

**A.** Přiřadit objednávku technickému členu `Unknown`.

**B.** Odstranit celou objednávku z datového skladu.

**C.** Přiřadit objednávku náhodnému zákazníkovi.

**D.** Přepsat tržbu objednávky na nulu.

#### Řešení

Správná odpověď je **A**.

V `dim_customer` lze vytvořit technický záznam:

```text
customer_key = 0
customer_name = Unknown
customer_type = Unknown
```

Objednávce se následně přiřadí `customer_key = 0`. Prodej zůstane zachovaný, cizí klíč odkazuje na existující člen dimenze a neznámé zákazníky lze samostatně kontrolovat v reportu.

---

### 47. Vztah dimenze a faktové tabulky

Jaký vztah se typicky nastaví mezi `dim_product` a `fact_sales` v Power BI hvězdicovém schématu?

**A.** N:N s filtrováním oběma směry.

**B.** 1:1 bez možnosti filtrování.

**C.** 1:N s filtrem z dimenze do faktové tabulky.

**D.** N:1 s filtrem pouze z faktové tabulky do dimenze.

#### Řešení

Správná odpověď je **C**.

Jeden produkt se v `dim_product` nachází právě jednou, ale může se objevit v mnoha řádcích `fact_sales`.

```text
dim_product 1:N fact_sales
```

V Power BI se zpravidla použije jednosměrné filtrování z dimenze do faktové tabulky. Výběr produktu nebo kategorie tak omezí odpovídající prodejní řádky.

---

### 48. Sales datamart

Co nejlépe vystihuje Sales datamart?

**A.** Provozní systém pro zapisování objednávek.

**B.** Analytickou část zaměřenou na prodejní oblast.

**C.** Souborový formát pro ukládání tabulek.

**D.** Vazební tabulku mezi dvěma dimenzemi.

#### Řešení

Správná odpověď je **B**.

Sales datamart je analytická oblast připravená pro analýzu prodejů. Může obsahovat například:

- `fact_sales`;
- `dim_customer`;
- `dim_product`;
- `dim_date`;
- `dim_region`.

Datamart může být samostatná databáze, databázové schéma, sada tabulek či pohledů nebo logicky vymezená část centrálního datového skladu.

---

### 49. Uložení původních dat

Kam nejlépe uložit původní CSV, JSON, databázové exporty a aplikační logy pro budoucí zpracování?

**A.** Do jediné faktové tabulky.

**B.** Do jednotlivých DAX měr.

**C.** Do kalendářní dimenze.

**D.** Do data lake.

#### Řešení

Správná odpověď je **D**.

Data lake je vhodný pro ukládání různých typů dat v původní nebo méně zpracované podobě. Umožňuje:

- zachovat původní vstupy;
- zopakovat zpracování;
- použít data pro další analytické účely;
- ukládat strukturovaná, polostrukturovaná i nestrukturovaná data.

Samotné uložení souborů ale nezajišťuje jejich kvalitu, význam ani připravenost pro reporting.

---

### 50. Lakehouse

Které tvrzení nejlépe popisuje lakehouse?

**A.** Kombinuje data lake s řízenými analytickými tabulkami.

**B.** Slouží pouze jako provozní databáze e-shopu.

**C.** Obsahuje výhradně neupravené textové soubory.

**D.** Nahrazuje všechny vizualizace v Power BI.

#### Řešení

Správná odpověď je **A**.

Lakehouse kombinuje flexibilní ukládání různých dat s vlastnostmi řízených analytických tabulek. Může podporovat například:

- původní i připravená data;
- kontrolu datového schématu;
- historii změn;
- SQL analytiku;
- BI reporting;
- data engineering a machine learning.

Lakehouse nenahrazuje Power BI. Připravuje a zpřístupňuje data, nad kterými může Power BI vytvořit sémantický model a report.

---

### 51. Odsouhlasení dat

Co znamená reconciliation při validaci datového skladu?

**A.** Abecední seřazení názvů dimenzí.

**B.** Sloučení všech tabulek do jednoho souboru.

**C.** Odsouhlasení cílových dat proti zdroji.

**D.** Nahrazení všech chybějících hodnot nulou.

#### Řešení

Správná odpověď je **C**.

Reconciliation znamená porovnání cílových dat se zdrojem. Kontrolovat lze například:

- počet objednávek;
- počet položek;
- celkové množství;
- celkové tržby;
- celkové náklady.

Pokud se výsledky po transformaci liší, musí být rozdíl vysvětlen konkrétním pravidlem nebo chybou. Nevysvětlené rozdíly se před předáním dat do reportingu nesmí ignorovat.

---

