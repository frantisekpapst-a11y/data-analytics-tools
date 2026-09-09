# Large Data Cheatsheet

Praktický přehled optimalizace načítání a zpracování větších datasetů v Pythonu, Pandas, SQL a Power BI.

---

## 1. Hlavní rozhodovací pravidlo

```text
Nejdříve optimalizujeme:

1. množství dat;
2. datové typy;
3. formát uložení;
4. způsob načítání;
5. místo filtrování a agregace.

Teprve potom uvažujeme o Sparku.
```

Větší dataset automaticky neznamená, že potřebujeme distribuovaný systém.

---

## 2. Velikost na disku vs. velikost v paměti

### Velikost souboru na disku

```python
csv_size_mb = (
    CSV_PATH.stat().st_size
    / 1024 ** 2
)

print(
    "Velikost CSV na disku:",
    round(csv_size_mb, 2),
    "MB"
)
```

### Velikost DataFrame v paměti

```python
memory_size_mb = (
    df
    .memory_usage(deep=True)
    .sum()
    / 1024 ** 2
)

print(
    "Velikost DataFrame v paměti:",
    round(memory_size_mb, 2),
    "MB"
)
```

```text
.stat().st_size
→ velikost souboru na disku v bytech

.memory_usage(deep=True)
→ paměť jednotlivých sloupců včetně textových hodnot

.sum()
→ celková spotřeba DataFrame

/ 1024 ** 2
→ převod bytů na MB
```

Soubor o velikosti 20 MB nemusí po načtení zabírat 20 MB RAM. Formát na disku a reprezentace dat v paměti jsou dvě různé věci.

---

## 3. Paměť podle sloupců

```python
memory_by_column_mb = (
    df
    .memory_usage(deep=True)
    / 1024 ** 2
)

memory_by_column_mb = (
    memory_by_column_mb
    .sort_values(ascending=False)
)

display(memory_by_column_mb)
```

Tímto způsobem zjistíme, které sloupce spotřebovávají nejvíce paměti. Často jsou to textové sloupce s opakujícími se hodnotami.

---

## 4. Datové typy a spotřeba paměti

### Běžné datové typy

```text
int64      → celé číslo, 64 bitů
int32      → celé číslo, 32 bitů
float64    → desetinné číslo
bool       → True / False
str        → text
datetime64 → datum a čas
category   → opakující se kategorie uložené pomocí kódů
```

Menší číselný typ může spotřebovat méně paměti, ale musí bezpečně pojmout všechny hodnoty ve sloupci.

### Category

Datový typ `category` je vhodný pro sloupce s malým počtem často opakovaných hodnot, například region, produkt nebo prodejní kanál.

```python
df_optimized = df.copy()

category_columns = [
    "region",
    "product",
    "channel"
]

df_optimized[category_columns] = (
    df_optimized[category_columns]
    .astype("category")
)
```

`category` typicky ukládá seznam kategorií pouze jednou a v řádcích používá menší číselné kódy.

Nevhodné použití:

- unikátní identifikátory;
- e-mailové adresy;
- komentáře;
- téměř unikátní text.

### Porovnání datových typů

```python
dtype_comparison = pd.DataFrame()

dtype_comparison["Before"] = df.dtypes
dtype_comparison["After"] = df_optimized.dtypes

display(dtype_comparison)
```

### Úspora paměti

```python
memory_before_mb = (
    df.memory_usage(deep=True).sum()
    / 1024 ** 2
)

memory_after_mb = (
    df_optimized.memory_usage(deep=True).sum()
    / 1024 ** 2
)

memory_saving_pct = (
    (memory_before_mb - memory_after_mb)
    / memory_before_mb
    * 100
)
```

V našem testu klesla paměť z `26,04 MB` na `14,59 MB`, tedy přibližně o `43,95 %`.

---

## 5. CSV vs. Parquet vs. SQLite

| Vlastnost | CSV | Parquet | SQLite |
|---|---|---|---|
| Způsob uložení | textový | binární sloupcový | databáze |
| Datové typy | neuchovává spolehlivě | uchovává | uchovává databázové typy |
| Komprese | obvykle ne | ano | záleží na databázi |
| Výběr sloupců | parser musí projít řádky | může načíst jen vybrané sloupce | provede `SELECT` |
| Filtrování před DataFrame | běžně ne | ano přes engine | ano pomocí `WHERE` |
| SQL dotazy | ne | ne přímo v Pandas | ano |
| Vhodné použití | přenos a jednoduchá výměna | analytické datasety | strukturovaná data a dotazy |

### Důležité

```text
Jeden CSV soubor
→ jedna plochá tabulka

Jeden Parquet soubor
→ jedna tabulková datová struktura

SQLite databáze
→ může obsahovat více tabulek

Partitionovaný Parquet dataset
→ jedna logická tabulka uložená ve více souborech a složkách
```

### Výsledek našeho testu na disku

| Formát | Velikost |
|---|---:|
| CSV | 18,70 MB |
| Parquet | 3,36 MB |
| SQLite | 20,64 MB |

Tyto výsledky platí pouze pro náš dataset a testovací prostředí.

---

## 6. Měření času načítání

```python
import time

start_time = time.perf_counter()

df_csv = pd.read_csv(CSV_PATH)

load_time = (
    time.perf_counter()
    - start_time
)

print(
    "Čas načtení:",
    round(load_time, 3),
    "sekund"
)
```

První načtení může být pomalejší kvůli inicializaci knihoven a studené cache. Pro srovnání je vhodné měření několikrát zopakovat.

```text
Cold run
→ první spuštění bez připravené cache

Warm run
→ opakované spuštění, při kterém mohou být data nebo knihovny v cache
```

Naměřený čas není univerzální vlastností formátu. Ovlivňuje ho hardware, cache, komprese, datové typy a struktura datasetu.

---

## 7. Načítání pouze potřebných sloupců

```python
selected_columns = [
    "order_date",
    "region",
    "product",
    "quantity",
    "revenue"
]
```

### CSV

```python
df_csv_selected = pd.read_csv(
    CSV_PATH,
    usecols=selected_columns
)
```

CSV parser musí projít textové řádky, ale do výsledného DataFrame uloží pouze vybrané sloupce.

### Parquet

```python
df_parquet_selected = pd.read_parquet(
    PARQUET_PATH,
    columns=selected_columns
)
```

Parquet je sloupcový formát, takže nepotřebné sloupce nemusí načítat.

### SQLite

```python
query = """
SELECT
    order_date,
    region,
    product,
    quantity,
    revenue
FROM sales
"""

connection = sqlite3.connect(DATABASE_PATH)

df_sql_selected = pd.read_sql(
    query,
    connection
)

connection.close()
```

V SQL nepoužíváme `SELECT *`, pokud všechny sloupce nepotřebujeme.

---

## 8. Filtrování dat

### CSV — filtrování po načtení

```python
df_csv = pd.read_csv(
    CSV_PATH,
    usecols=selected_columns,
    parse_dates=["order_date"]
)

df_csv_2025 = df_csv[
    df_csv["order_date"].between(
        "2025-01-01",
        "2025-12-31"
    )
].reset_index(drop=True)
```

Pandas nejprve načte požadované sloupce z CSV a teprve potom vytvoří filtrovaný DataFrame.

### Parquet — filtr při načítání

```python
df_parquet_2025 = pd.read_parquet(
    PARQUET_PATH,
    columns=selected_columns,
    filters=[
        (
            "order_date",
            ">=",
            "2025-01-01"
        ),
        (
            "order_date",
            "<",
            "2026-01-01"
        )
    ]
)
```

Pandas předá filtr Parquet enginu, například PyArrow. Ten může nepotřebné části dat přeskočit ještě před vytvořením DataFrame.

### SQL — WHERE ve zdroji

```python
query = """
SELECT
    order_date,
    region,
    product,
    quantity,
    revenue
FROM sales
WHERE order_date BETWEEN
    '2025-01-01'
    AND '2025-12-31 23:59:59'
"""

connection = sqlite3.connect(DATABASE_PATH)

df_sql_2025 = pd.read_sql(
    query,
    connection,
    parse_dates=["order_date"]
)

connection.close()
```

Databáze provede filtr a do Pandas odešle pouze výsledek.

---

## 9. SQL pushdown

SQL pushdown znamená, že databáze provede výběr sloupců, filtrování, spojování nebo agregaci před odesláním výsledku do dalšího nástroje.

```text
Databáze
→ SELECT / WHERE / JOIN / GROUP BY
→ menší výsledek
→ Python nebo Power BI
```

Výhody:

- menší přenos dat;
- nižší spotřeba paměti klienta;
- využití databázového optimalizátoru;
- možnost využít indexy a databázový server.

---

## 10. Agregace před načtením do Power BI

Pokud report nepotřebuje detailní řádky, může agregaci provést databáze.

```sql
SELECT
    strftime('%Y-%m', order_date) AS year_month,
    region,
    SUM(revenue) AS total_revenue,
    COUNT(*) AS order_count
FROM sales
GROUP BY
    strftime('%Y-%m', order_date),
    region
ORDER BY
    year_month,
    region;
```

V našem testu:

```text
300 000 detailních řádků
→ SQL agregace
→ 120 řádků
→ přibližně 0,0053 MB
```

Pro běžný reporting může databáze výsledek předat přímo do Power BI:

```text
Databáze
→ SQL filtr a agregace
→ Power BI
```

Pandas mezi databází a Power BI není povinný. Přidáváme ho pouze pro transformace nebo analýzu vhodnou pro Python.

Pozor: neagregujeme dříve, než známe požadovanou granularitu reportu. Předčasnou agregací bychom mohli ztratit potřebný detail.

---

## 11. Chunks — zpracování CSV po částech

`chunksize` určuje, kolik řádků Pandas načte v jednom kroku.

```python
chunk_results = []
chunk_count = 0

for chunk in pd.read_csv(
    CSV_PATH,
    usecols=["region", "revenue"],
    chunksize=50_000
):
    chunk_summary = (
        chunk.groupby(
            "region",
            as_index=False
        )["revenue"]
        .sum()
    )

    chunk_results.append(chunk_summary)
    chunk_count += 1

region_revenue_chunks = (
    pd.concat(
        chunk_results,
        ignore_index=True
    )
    .groupby(
        "region",
        as_index=False
    )["revenue"]
    .sum()
    .sort_values(
        "revenue",
        ascending=False,
        ignore_index=True
    )
)
```

Proč se agreguje dvakrát:

```text
1. agregace
→ souhrn uvnitř každého chunku

2. agregace
→ spojení souhrnů ze všech chunků
```

### Kontrola výsledku

```python
comparison_ok = np.allclose(
    region_revenue_chunks["revenue"],
    region_revenue_full["revenue"]
)

print(comparison_ok)
```

`np.allclose()` ověřuje, zda jsou číselné výsledky shodné s malou tolerancí pro desetinné výpočty.

Chunks snižují maximální spotřebu paměti, ale stále postupně projdou celý CSV soubor.

---

## 12. Partitioning

Partitioning znamená fyzické rozdělení jednoho logického datasetu do více složek nebo souborů podle hodnot vybraného sloupce.

```text
sales_partitioned
├── year=2024
│   └── parquet soubor
└── year=2025
    └── parquet soubor
```

### Příprava partition sloupce

```python
df_partitioned = df.copy()

df_partitioned["year"] = (
    df_partitioned["order_date"]
    .dt.year
)
```

### Uložení partitionovaného Parquet datasetu

```python
import shutil

PARTITIONED_PARQUET_PATH = (
    PARQUET_PATH.parent
    / "sales_partitioned"
)

if PARTITIONED_PARQUET_PATH.exists():
    shutil.rmtree(
        PARTITIONED_PARQUET_PATH
    )

df_partitioned.to_parquet(
    PARTITIONED_PARQUET_PATH,
    partition_cols=["year"],
    index=False
)
```

Starý generovaný výstup odstraňujeme, aby při opakovaném spuštění notebooku nevznikly duplicitní soubory.

### Kontrola partition

```python
for folder in sorted(
    PARTITIONED_PARQUET_PATH.iterdir()
):
    print(folder.name)
```

### Načtení jedné partition

```python
df_partitioned_2025 = pd.read_parquet(
    PARTITIONED_PARQUET_PATH,
    columns=selected_columns,
    filters=[
        ("year", "==", 2025)
    ]
)
```

### Chunks vs. partitioning

| Chunks | Partitioning |
|---|---|
| Postupně čte části jednoho souboru | Dataset je fyzicky rozdělen |
| Vhodné, když se celý soubor nevejde do RAM | Vhodné pro časté filtry podle partition sloupce |
| Typicky projde celý zdroj | Může přeskočit celé složky nebo soubory |

### Lze partitionovat i jiné formáty?

| Formát nebo zdroj | Partitioning |
|---|---|
| Parquet | Ano, nejběžnější analytická kombinace |
| CSV | Ano, rozdělením do více souborů a složek |
| JSON | Ano, ale obvykle méně efektivní pro analytiku |
| XML | Technicky ano, prakticky se používá málo |
| Excel | Technicky lze rozdělit, ale není vhodný pro velký partitionovaný dataset |
| Podniková databáze | Ano, pomocí partitionovaných tabulek |
| SQLite | Nemá klasický table partitioning velkých databází |

### Proč partitioning často používáme s Parquetem

```text
Partitioning
→ přeskočí celé nepotřebné složky nebo soubory

Parquet
→ z potřebných souborů načte jen vybrané sloupce a části dat
```

Vhodné partition sloupce mají omezený počet hodnot a často se používají ve filtrech, například rok, měsíc nebo region.

Nevhodný partition sloupec je například unikátní `order_id`, protože by vytvořil obrovské množství malých částí.

Příliš mnoho malých souborů se označuje jako **small files problem** a může výkon zhoršit.

---

## 13. SQL vs. Pandas

### SQL je vhodné pro

- výběr sloupců;
- filtrování velkých databázových tabulek;
- spojování tabulek;
- agregace;
- opakované centrální transformace;
- práci více uživatelů nad společnými daty.

### Pandas je vhodný pro

- průzkum dat;
- čištění a nestandardní transformace;
- statistickou analýzu;
- práci s API a soubory;
- automatizaci;
- přípravu dat pro analýzu nebo vizualizaci.

Nejde o volbu pouze jednoho nástroje. SQL a Pandas se mohou doplňovat:

```text
Databáze
→ SQL vybere a zmenší data
→ Pandas provede další analýzu
```

Pokud Python nemá jasný účel, není nutné ho mezi databázi a Power BI přidávat.

---

## 14. Cleaning a transformační vrstva

Původní raw data zachováváme a čištění provádíme v samostatné transformační vrstvě.

```text
Zdrojová data
→ Raw vrstva beze změn
→ Cleaning a transformace
→ Analytická data
→ Agregace podle potřeby
→ Reporting
```

| Situace | Vhodné místo |
|---|---|
| Oprava používaná více reporty | SQL nebo centrální transformační vrstva |
| Jednoduchá úprava jednoho reportu | Power Query |
| CSV, JSON, API nebo složitější logika | Python |
| Statistika a nestandardní transformace | Python |

Opakované čištění provádíme v jedné zvolené transformační vrstvě. Nemusíme stejnou transformaci opakovat v SQL, Pythonu i Power Query.

### ETL

```text
Extract
→ získání dat

Transform
→ čištění a transformace

Load
→ uložení nebo načtení výsledku
```

ETL není konkrétní nástroj. Proces lze vytvořit pomocí SQL, Power Query, Pythonu nebo specializované datové platformy.

---

## 15. Kdy jeden počítač nemusí stačit

Neexistuje univerzální počet řádků, od kterého je nutné použít Spark.

Varovné signály:

- dataset se nevejde do dostupné RAM;
- systém intenzivně přesouvá data mezi RAM a diskem;
- vzniká `MemoryError`;
- joiny, agregace nebo transformace trvají nepřijatelně dlouho;
- mezivýsledky jsou výrazně větší než vstupní data;
- data rychle rostou;
- výpočty musí běžet často nebo pro více uživatelů současně.

Před přechodem na Spark vyzkoušíme:

1. potřebné sloupce;
2. filtrování ve zdroji;
3. SQL pushdown;
4. agregaci před načtením;
5. vhodné datové typy;
6. Parquet;
7. partitioning;
8. chunks.

```text
Jeden počítač
→ omezená RAM a výpočetní výkon

Spark cluster
→ data a výpočty rozdělené mezi více počítačů
```

Spark přidává infrastrukturu, režii a složitost. Pro menší data proto nemusí být rychlejší ani jednodušší.

---

## 16. Rychlý rozhodovací přehled

| Potřeba | Vhodný postup |
|---|---|
| Jednoduchá výměna menších dat | CSV |
| Efektivní analytické uložení | Parquet |
| Dotazy a více tabulek | Databáze + SQL |
| Jen několik sloupců | `usecols`, `columns` nebo SQL `SELECT` |
| Jen některé řádky z databáze | SQL `WHERE` |
| Jen některé části Parquetu | `filters` a vhodný partitioning |
| CSV se nevejde do paměti | `chunksize` |
| Opakující se textové hodnoty | `category` |
| Power BI nepotřebuje detail | Agregace v SQL před načtením |
| Data po optimalizaci stále nestačí jednomu počítači | Zvážit Spark |

---

## 17. Kontrolní otázky před zpracováním velkých dat

```text
Potřebuji všechny řádky?

Potřebuji všechny sloupce?

Mohu filtrovat už ve zdroji?

Mohu agregovat před přenosem?

Jsou datové typy vhodné?

Je CSV správný formát?

Filtruji často podle roku nebo regionu?

Vejde se výsledek bezpečně do paměti?

Má Python v datovém toku jasný účel?

Potřebuji opravdu distribuované zpracování?
```

---

## 18. Závěrečný mentální model

```text
Raw data zachovat

→ omezit řádky a sloupce

→ zvolit správné datové typy

→ zvolit vhodný formát

→ filtrovat a transformovat ve vhodné vrstvě

→ agregovat podle požadované granularity

→ načíst pouze potřebný výsledek

→ Spark použít až při skutečné potřebě
```