# Databricks Cheatsheet

Praktický přehled prostředí Databricks, notebooků, compute, Spark workflow, Delta tabulek, Unity Catalogu a vrstev Bronze–Silver–Gold.

Cheatsheet je určený pro základní orientaci datového analytika. Nejde o kompletní výuku PySparku ani administrace Databricks.

---

## 1. Co je Databricks

Databricks je cloudová datová a analytická platforma, která propojuje:

```text
notebooky
+ compute
+ Spark
+ SQL
+ lakehouse
+ datový katalog
+ jobs a pipelines
```

Databricks není pouze notebook, databáze ani cloudové úložiště.

Typické využití:

- načítání a transformace velkých dat;
- datové pipeline;
- SQL analytika a data warehousing;
- lakehouse architektura;
- datová věda a machine learning;
- příprava dat pro Power BI a další BI nástroje.

---

## 2. Databricks a cloud

Databricks je samostatná společnost a její platformu lze provozovat u hlavních cloudových poskytovatelů:

```text
Databricks
├── Azure Databricks
├── Databricks on AWS
└── Databricks on Google Cloud
```

**Azure Databricks** je platforma Databricks integrovaná do prostředí Microsoft Azure.

Může využívat například:

- Microsoft Entra ID;
- Azure Data Lake Storage Gen2;
- Azure Data Factory;
- Azure Key Vault;
- Power BI.

Microsoft Fabric a Azure Databricks nejsou stejná platforma. Obě prostředí mohou používat Spark, notebooky a lakehouse principy, ale jde o odlišné produkty.

---

## 3. Workspace

**Workspace** je společné pracovní prostředí pro uživatele a týmy.

Může obsahovat například:

- notebooky;
- SQL dotazy;
- jobs;
- pipelines;
- dashboards;
- Git folders;
- přístup k datům a compute.

```text
Databricks workspace
├── notebooks
├── SQL queries
├── jobs a pipelines
├── Git folders
└── přístup k datům a compute
```

Workspace není:

- jeden fyzický server;
- jeden notebook;
- jedna tabulka;
- samotné úložiště všech dat.

---

## 4. Notebook

Databricks notebook je podobný Jupyter Notebooku.

Obsahuje:

- Code buňky;
- Markdown buňky;
- výstupy jednotlivých buněk;
- dokumentaci a komentáře;
- připojení ke compute.

Notebook podporuje například:

- Python a PySpark;
- SQL;
- Scala;
- R.

### Magic commands

Jazyk konkrétní buňky lze určit pomocí magic command:

```sql
%sql

SELECT *
FROM logistics.silver.shipments;
```

```python
%python

silver_df = spark.table(
    "logistics.silver.shipments"
)
```

`%sql` znamená, že se obsah buňky spustí jako SQL. `%python` přepne buňku do Pythonu.

Notebook může kombinovat více jazyků, ale každá jednotlivá buňka se provádí v jednom zvoleném jazyce.

---

## 5. Compute

**Compute** představuje výpočetní prostředky potřebné ke spuštění kódu:

- procesory;
- operační paměť;
- výpočetní instance;
- runtime a potřebné knihovny.

```text
notebook
→ obsahuje příkazy

compute
→ poskytuje výpočetní výkon

Databricks Runtime
→ obsahuje Spark, Python a knihovny
```

Notebook bez dostupného compute obsahuje kód, ale nemá kde ho vykonat.

### Základní druhy compute

- **general-purpose compute** — interaktivní práce v notebooku;
- **job compute** — spuštění automatizované úlohy;
- **SQL warehouse** — compute optimalizovaný pro SQL analytiku;
- **serverless compute** — výpočetní prostředí spravované Databricks.

Pro datového analytika je důležité rozlišovat notebook, uložená data a compute. Detailní nastavení clusterů není cílem této lekce.

---

## 6. SQL warehouse

**SQL warehouse** je výpočetní prostředí optimalizované pro SQL dotazy.

Používá se například pro:

- Databricks SQL;
- analytické SQL dotazy;
- SQL notebooky;
- připojení Power BI;
- souběžné dotazy více uživatelů.

```text
Delta tabulky
→ obsahují data

SQL warehouse
→ provede SQL dotaz

analytik nebo Power BI
→ získá výsledek
```

SQL warehouse není samotná tabulka ani místo, kde musí být všechna data trvale uložena.

---

## 7. Spark prostředí

Databricks poskytuje připravené prostředí pro Apache Spark.

V PySpark notebooku je dostupný objekt `spark`:

```python
shipments_df = (
    spark.read
    .option("header", True)
    .option("inferSchema", True)
    .csv("/Volumes/logistics/bronze/source_files/shipments.csv")
)
```

Výsledkem je Spark DataFrame.

```text
Pandas DataFrame
→ zpracování především na jednom počítači

Spark DataFrame
→ data mohou být rozdělena do partitions
→ výpočet může probíhat distribuovaně
```

Základní Spark principy:

- transformations používají lazy evaluation;
- action spustí připravený výpočet;
- data mohou být rozdělena do partitions;
- velký `collect()` může přetížit driver.

Pro základní orientaci stačí znát operace:

```python
df.select(...)
df.filter(...)
df.withColumn(...)
df.groupBy(...)
```

---

## 8. Soubory, volumes a tabulky

Databricks může pracovat se soubory i registrovanými tabulkami.

### Soubor

Původní CSV nebo JSON lze uchovat v cloudovém úložišti. Pro řízenou práci se soubory se používají například Unity Catalog volumes.

```text
/Volumes/logistics/bronze/source_files/shipments.csv
```

K souboru přistupujeme pomocí cesty:

```python
bronze_df = (
    spark.read
    .option("header", True)
    .csv("/Volumes/logistics/bronze/source_files/shipments.csv")
)
```

### Tabulka

Tabulka má definovanou strukturu a je registrovaná v katalogu:

```text
logistics.silver.shipments
```

Načtení v PySparku:

```python
silver_df = spark.table(
    "logistics.silver.shipments"
)
```

Dotaz v SQL:

```sql
SELECT *
FROM logistics.silver.shipments;
```

Praktický rozdíl:

```text
soubor
→ přístup pomocí cesty
→ vhodný například pro původní CSV a JSON

tabulka
→ přístup pomocí názvu
→ definované sloupce a datové typy
→ SQL dotazy a řízená oprávnění
```

Také tabulka je pod povrchem fyzicky uložená jako datové soubory v cloudovém úložišti.

---

## 9. Delta Lake a Delta tabulky

Databricks používá především Delta Lake tabulky.

```text
Delta tabulka
├── datové soubory Parquet
└── transakční log
```

- Parquet soubory obsahují data.
- Transakční log zaznamenává změny a verze tabulky.

Delta Lake přidává například:

- spolehlivé zápisy a změny dat;
- kontrolu datového schématu;
- podporu `UPDATE`, `DELETE` a `MERGE`;
- historii verzí;
- time travel;
- bezpečnější souběžnou práci více procesů.

Samotná složka s Parquet soubory nemá automaticky stejné databázové vlastnosti.

### Uložení Delta tabulky

```python
(
    silver_df.write
    .format("delta")
    .mode("overwrite")
    .saveAsTable("logistics.silver.shipments")
)
```

- `write` — zápis DataFrame;
- `format("delta")` — použití Delta Lake;
- `mode("overwrite")` — nahrazení existujícího obsahu;
- `saveAsTable()` — uložení a registrace tabulky pod názvem.

Zápis je Spark action a spustí připravené transformace.

### Základní režimy změn

- `overwrite` — kompletní nahrazení obsahu;
- `append` — přidání nových řádků;
- `MERGE` — aktualizace existujících a vložení nových řádků.

Konkrétní režim se volí podle chování zdrojových dat. `overwrite` je vhodný pro jednoduchou výukovou ukázku, ale nemusí být správnou volbou pro každou produkční pipeline.

---

## 10. Unity Catalog

**Unity Catalog** je centrální governance vrstva Databricks.

Pomáhá řídit:

- organizaci datových objektů;
- vyhledávání a popis dat;
- přístupová oprávnění;
- audit používání dat;
- datovou lineage neboli původ a návaznost dat;
- sdílení řízených dat mezi workspaces.

Unity Catalog není compute ani nový datový formát.

### Hierarchie názvů

```text
catalog
└── schema
    └── table nebo volume
```

Celý název tabulky:

```text
catalog.schema.table
```

Příklad:

```text
logistics.gold.fact_shipments
```

- `logistics` — catalog;
- `gold` — schema;
- `fact_shipments` — table.

Další příklady:

```text
logistics.bronze.shipments_raw
logistics.silver.shipments
logistics.gold.fact_shipments
logistics.gold.dim_carrier
```

### Dva významy slova schema

**Datové schéma** popisuje sloupce a datové typy tabulky.

```text
shipment_id → integer
delivery_at → timestamp
transport_cost → decimal
```

**Databázové schema** je organizační prostor uvnitř katalogu.

```text
catalog: logistics
├── schema: bronze
├── schema: silver
└── schema: gold
```

Bronze, Silver a Gold jsou logické vrstvy. Jejich realizace pomocí stejně pojmenovaných schemas je praktická konvence, nikoliv povinnost platformy.

---

## 11. Bronze, Silver a Gold

### Bronze

Obsahuje původní nebo téměř neupravená data.

```text
CSV + JSON
→ logistics.bronze
```

Účel:

- zachovat původní vstup;
- umožnit opakované zpracování;
- dohledat chyby;
- porovnat výsledky se zdrojem.

### Silver

Obsahuje vyčištěná, validovaná a sjednocená data.

Typické operace:

- sjednocení datových typů;
- standardizace hodnot;
- kontrola identifikátorů;
- odstranění technických duplicit;
- kontrola chybějících hodnot;
- validace časové návaznosti;
- označení chybných záznamů.

### Gold

Obsahuje business-ready tabulky pro analytiku a reporting.

Může obsahovat:

- faktové a dimenzní tabulky;
- hvězdicové schéma;
- business příznaky;
- připravené agregace;
- KPI podklady pro Power BI.

Celý tok:

```text
CSV / JSON
→ Bronze
→ PySpark validace a čištění
→ Silver Delta tabulky
→ SQL nebo PySpark agregace
→ Gold Delta tabulky
→ Power BI
```

---

## 12. Jednoduchá Silver transformace

Import Spark funkcí:

```python
from pyspark.sql import functions as F
```

`F` je krátký alias modulu se Spark funkcemi.

Příklad sjednocení typů:

```python
silver_df = (
    shipments_df
    .withColumn(
        "planned_delivery_at",
        F.to_timestamp("planned_delivery_at")
    )
    .withColumn(
        "actual_delivery_at",
        F.to_timestamp("actual_delivery_at")
    )
    .withColumn(
        "transport_cost",
        F.col("transport_cost").cast("decimal(18,2)")
    )
)
```

Příklad validačního příznaku:

```python
silver_df = silver_df.withColumn(
    "is_valid",
    F.col("shipment_id").isNotNull()
    & (F.col("transport_cost") >= 0)
)
```

Oddělení validních a chybných záznamů:

```python
valid_df = silver_df.filter(
    F.col("is_valid") == True
)

invalid_df = silver_df.filter(
    F.col("is_valid") == False
)
```

Chybné řádky nemažeme bez vysvětlení. Uchováme je pro kontrolu nebo opravu.

### Business význam `null`

`null` nemusí být automaticky chyba.

```text
actual_delivery_at = null
+ status = In Transit
→ zásilka ještě nebyla doručena
```

Doplnění mediánu nebo aktuálního data by vytvořilo nepravdivou informaci.

---

## 13. Jednoduchá Gold agregace

Silver data lze zpracovat pomocí PySparku a Gold agregaci vytvořit pomocí SQL ve stejném notebooku:

```sql
%sql

CREATE OR REPLACE TABLE logistics.gold.carrier_performance AS

SELECT
    carrier_id,
    COUNT(*) AS total_shipments,
    SUM(CASE WHEN is_delivered = true THEN 1 ELSE 0 END)
        AS delivered_shipments,
    SUM(CASE WHEN is_on_time = true THEN 1 ELSE 0 END)
        AS on_time_shipments,
    AVG(delay_minutes) AS average_delay_minutes,
    SUM(transport_cost) AS total_transport_cost
FROM logistics.silver.shipments
GROUP BY carrier_id;
```

Praktické rozdělení:

```text
PySpark
→ technické čištění a validace

SQL
→ čitelné business agregace

Gold
→ řízený výstup pro analytiku
```

SQL není automaticky lepší než PySpark. Nástroj volíme podle charakteru transformace a dovedností týmu.

Agregovaná tabulka nenahrazuje automaticky celé hvězdicové schéma. Gold vrstva může obsahovat například:

```text
logistics.gold.fact_shipments
logistics.gold.dim_carrier
logistics.gold.dim_date
logistics.gold.dim_warehouse
logistics.gold.dim_route
logistics.gold.carrier_performance
```

---

## 14. Základní kontrola dat

Po načtení dat nejdříve ověříme jejich obsah a strukturu:

```python
display(shipments_df)
```

```python
shipments_df.printSchema()
```

```python
shipments_df.count()
```

```python
shipments_df.select("shipment_id").distinct().count()
```

- `display()` zobrazí ukázku dat;
- `printSchema()` zobrazí sloupce a datové typy;
- `count()` zjistí počet řádků;
- `distinct().count()` pomůže ověřit počet unikátních hodnot.

Doporučený postup:

```text
načíst
→ prohlédnout
→ ověřit strukturu
→ zkontrolovat kvalitu
→ transformovat
→ uložit
```

---

## 15. Power BI a Databricks

Power BI by měl primárně používat připravené Gold tabulky, nikoliv přímo původní CSV a JSON soubory.

```text
Gold Delta tabulky
→ Databricks SQL warehouse
→ Power BI semantic model
→ Power BI report
```

Základní režimy připojení:

- **Import** — data se načtou do Power BI sémantického modelu;
- **DirectQuery** — Power BI posílá dotazy do Databricks SQL warehouse.

Import bývá praktický pro pravidelný reporting s přiměřeným objemem Gold dat. DirectQuery lze zvážit při velmi velkém objemu nebo požadavku na aktuálnější výsledky. Volba závisí na objemu, výkonu, souběhu uživatelů a požadované aktuálnosti.

---

## 16. Notebook, job a pipeline

### Notebook

Obsahuje konkrétní transformační nebo analytický kód.

```text
načíst
→ zkontrolovat
→ transformovat
→ uložit
```

### Job

Spouští a koordinuje jeden nebo více úkolů.

Úkolem může být například:

- notebook;
- SQL dotaz;
- Python skript;
- pipeline.

Job může určit pořadí a závislosti:

```text
Bronze úspěšně
→ spustit Silver

Silver úspěšně
→ spustit Gold
```

### Pipeline

Pipeline představuje opakovatelný datový tok od zdrojů k cílovým tabulkám a vztahy mezi jeho kroky nebo datasety.

```text
zdroje
→ Bronze
→ Silver
→ Gold
```

Zjednodušeně:

```text
notebook
→ obsahuje kód

job
→ spouští a koordinuje úkoly

pipeline
→ popisuje řízený datový tok
```

V aktuální terminologii Databricks se používají názvy **Lakeflow Jobs** a **Lakeflow Spark Declarative Pipelines**. Pro základní orientaci stačí rozumět jejich rolím.

Scheduling, retry, monitoring, alerty a produkční nasazení patří do navazujícího bloku Automation.

---

## 17. Databricks versus Microsoft Fabric

### Databricks

Typicky vyniká v oblastech:

- rozsáhlé Spark zpracování;
- flexibilní data engineering;
- pokročilé notebooky;
- machine learning a data science;
- multi-cloud prostředí;
- otevřená lakehouse architektura.

### Microsoft Fabric

Typicky vyniká v oblastech:

- přímá návaznost na Power BI;
- integrované prostředí Microsoftu;
- společná úložná vrstva OneLake;
- SQL, Data Factory, Lakehouse a Power BI v jedné platformě;
- nižší počet samostatně propojovaných služeb.

Databricks ani Fabric nejsou automaticky lepší pro každou situaci. Rozhoduje:

- existující technologické prostředí;
- objem a složitost dat;
- dovednosti týmu;
- požadovaná flexibilita;
- způsob reportingu;
- správa a náklady.

---

## 18. Kdy Databricks dává smysl

Databricks může být vhodný, když:

- firma zpracovává velká data pomocí Sparku;
- potřebuje společné prostředí pro data engineering a analytiku;
- používá data lake nebo lakehouse;
- kombinuje SQL, Python a Spark;
- potřebuje řízené Delta tabulky;
- má více datových rolí a vyžaduje centrální governance;
- pravidelně spouští rozsáhlé datové workflow.

Databricks obvykle není první volbou, když:

- malý dataset pohodlně zvládne Pandas;
- stačí jednoduchý SQL dotaz v existující databázi;
- jde o jednorázovou analýzu malého souboru;
- firma již efektivně používá jednodušší integrované řešení;
- provozní složitost převýší skutečný přínos.

---

## 19. Co má znát juniorní datový analytik

Pro juniorní analytickou pozici je užitečné umět vysvětlit:

- co je Databricks;
- rozdíl mezi workspace, notebookem a compute;
- proč notebook potřebuje compute;
- k čemu slouží SQL warehouse;
- rozdíl mezi souborem a registrovanou tabulkou;
- vztah Parquetu a Delta Lake;
- význam Unity Catalogu;
- hierarchii `catalog.schema.table`;
- princip Bronze, Silver a Gold;
- proč kombinovat PySpark a SQL;
- jak Gold data zpřístupnit Power BI;
- rozdíl mezi notebookem, jobem a pipeline.

Není nutné okamžitě ovládat:

- detailní správu clusterů;
- pokročilou optimalizaci Sparku;
- streaming;
- pokročilý PySpark;
- produkční CI/CD;
- detailní nastavení sítí a zabezpečení.

---

## 20. Hlavní workflow

```text
CSV / JSON
→ načtení do Bronze
→ kontrola struktury a kvality
→ PySpark validace a čištění
→ Silver Delta tabulky
→ SQL nebo PySpark agregace
→ Gold Delta tabulky
→ SQL warehouse
→ Power BI
```

Hlavní role:

```text
workspace
→ organizuje práci

notebook
→ obsahuje kód a dokumentaci

compute
→ poskytuje výkon

Spark
→ zpracovává data

Delta Lake
→ přidává řízení tabulek nad Parquetem

Unity Catalog
→ organizuje data a řídí přístup

job
→ spouští a koordinuje úkoly

pipeline
→ vytváří opakovatelný datový tok
```

Hlavní princip:

> Databricks propojuje notebooky, výpočetní prostředky, Spark, SQL a řízené lakehouse tabulky do jednoho prostředí pro opakovatelné zpracování dat.

---

## Oficiální dokumentace

- [Azure Databricks — přehled](https://learn.microsoft.com/azure/databricks/introduction/)
- [Databricks notebooks](https://learn.microsoft.com/azure/databricks/notebooks/)
- [Notebook compute](https://learn.microsoft.com/azure/databricks/notebooks/notebook-compute)
- [Unity Catalog](https://learn.microsoft.com/azure/databricks/data-governance/unity-catalog/)
- [Databricks tables](https://learn.microsoft.com/azure/databricks/tables/)
- [Unity Catalog volumes](https://learn.microsoft.com/azure/databricks/volumes/)
- [Power BI a Azure Databricks](https://learn.microsoft.com/azure/databricks/partners/bi/power-bi)