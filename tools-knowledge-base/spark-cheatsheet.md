# Spark a PySpark Cheatsheet

Praktický přehled základních principů Apache Spark před zahájením práce s PySparkem.

---

# 1. Proč Spark existuje

Pandas zpracovává data především v paměti jednoho počítače.

To funguje, dokud má počítač dostatek:

- operační paměti;
- výkonu procesoru;
- místa na disku;
- času na dokončení výpočtu.

Při zpracování navíc mohou vznikat:

- pracovní kopie dat;
- filtrované DataFrames;
- výsledky spojování tabulek;
- mezivýsledky agregací;
- mezivýsledky řazení.

Výpočet proto může potřebovat více paměti, než kolik zabírá samotný zdrojový soubor.

```text
pandas
→ jeden počítač
→ jeho procesor a paměť
→ omezení výkonem jednoho počítače
```

Spark umožňuje rozdělit data a jejich zpracování mezi více počítačů.

```text
Spark
→ rozdělení dat
→ více pracovníků
→ paralelní zpracování
→ spojení dílčích výsledků
```

---

# 2. Nejdříve optimalizace, potom Spark

Velký dataset automaticky neznamená, že potřebujeme Spark.

Nejdříve ověříme, zda lze:

- vybrat pouze potřebné sloupce;
- filtrovat data už ve zdroji;
- provést agregaci v databázi;
- použít vhodné datové typy;
- použít Parquet;
- využít partitioning;
- zpracovat CSV po částech pomocí chunks;
- odstranit zbytečné mezikroky a kopie dat.

Teprve pokud jeden počítač nestačí ani po optimalizaci, uvažujeme o distribuovaném zpracování.

```text
optimalizace množství dat
→ optimalizace formátu
→ optimalizace způsobu načítání
→ posouzení výkonu jednoho počítače
→ případně Spark
```

---

# 3. Distribuované zpracování

Distribuované zpracování znamená, že se data a výpočty rozdělí mezi více počítačů.

Každý počítač zpracuje přidělenou část dat.

```text
velký dataset
→ rozdělení na části
→ zpracování na více počítačích
→ spojení výsledků
```

Výhodou může být:

- možnost zpracovat data, která nezvládne jeden počítač;
- paralelní provádění výpočtů;
- rozdělení požadavků na paměť a procesory;
- rychlejší pravidelné zpracování velkých dat.

Distribuované zpracování má ale také režii:

- plánování práce;
- rozdělování úloh;
- komunikaci mezi počítači;
- přenos dat;
- spojování dílčích výsledků.

Proto Spark nemusí být výhodný pro malé datasety.

---

# 4. Cluster

**Cluster** je skupina propojených počítačů, které spolupracují na zpracování dat.

Jednotlivé počítače v clusteru se označují jako **nodes** neboli uzly.

```text
cluster
→ více propojených počítačů
→ společné zpracování úlohy
```

Spark lze spustit také lokálně na jednom počítači. Lokální režim je vhodný například pro:

- učení;
- vývoj;
- testování;
- menší úlohy.

Lokální Spark ale neposkytuje skutečný výkon clusteru složeného z více počítačů.

---

# 5. Driver a workers

## Driver

**Driver** koordinuje Spark aplikaci.

Jeho úlohou je především:

- přijmout náš kód;
- vytvořit plán výpočtu;
- rozdělit práci;
- koordinovat její provedení;
- sledovat průběh výpočtu.

```text
driver
→ plánuje
→ rozděluje práci
→ koordinuje zpracování
```

## Workers

**Workers** jsou pracovní uzly, které poskytují prostředky pro provedení přidělených výpočtů.

Zjednodušená analogie:

```text
driver
→ vedoucí, který připraví a rozdělí práci

workers
→ pracovníci, kteří provedou přidělené úkoly
```

V praktické architektuře Sparku se na workers spouštějí výpočetní procesy nazývané **executors**. Podrobněji se jim budeme věnovat při práci s PySparkem.

---

# 6. Spark partitions

Aby bylo možné data zpracovávat paralelně, Spark rozdělí dataset na menší části nazývané **partitions**.

```text
dataset
├── partition 1 → worker A
├── partition 2 → worker B
├── partition 3 → worker C
└── partition 4 → worker D
```

Partition představuje:

```text
část datasetu určenou k dílčímu zpracování
```

Partition není:

- celý dataset;
- worker;
- samostatný počítač;
- výsledný DataFrame.

Jeden worker může postupně zpracovat více partitions. Počet partitions proto nemusí být stejný jako počet workers.

---

# 7. Paralelní zpracování

Pokud jsou k dispozici volné výpočetní prostředky, může Spark zpracovávat více partitions současně.

Tomu říkáme **paralelní zpracování**.

```text
worker A → partition 1
worker B → partition 2
worker C → partition 3
worker D → partition 4
```

Více workers ale neznamená automaticky rychlejší výpočet.

Výkon ovlivňuje například:

- počet a velikost partitions;
- množství přenášených dat;
- složitost transformací;
- výkon clusteru;
- způsob uložení dat;
- rozsáhlé joiny a agregace;
- rozdělení dat mezi workers.

Při některých operacích se data musí přesouvat mezi workers. Tento přesun se označuje jako **shuffle** a může být časově i výpočetně náročný.

---

# 8. Dva významy partition

Partitioning jsme používali také při ukládání Parquetu. Nejde však přesně o totéž jako Spark partition.

## Parquet partitioning

Představuje fyzické rozdělení uložených dat, například podle roku.

```text
sales_partitioned/
├── year=2024/
└── year=2025/
```

Díky tomu lze při filtrování přeskočit celé nepotřebné části datasetu.

## Spark partition

Představuje část dat určenou pro jeden dílčí výpočet.

| Parquet partitioning | Spark partition |
|---|---|
| Rozdělení dat při uložení | Rozdělení dat při zpracování |
| Často podle roku, měsíce nebo regionu | Část dat přidělená výpočetní úloze |
| Může být viditelné ve struktuře složek | Nemusí odpovídat fyzickým složkám |
| Pomáhá přeskočit nepotřebné soubory | Umožňuje paralelní zpracování |

Oba principy spolu mohou souviset, ale nejsou totožné.

---

# 9. Lazy evaluation

**Lazy evaluation** znamená odložené vyhodnocení.

Spark transformace obvykle neprovede okamžitě. Nejdříve si uloží požadované kroky a vytvoří plán výpočtu.

Výpočet provede až ve chvíli, kdy je požadovaný konkrétní výsledek.

```text
zadání transformací
→ vytvoření plánu
→ optimalizace plánu
→ požadavek na výsledek
→ provedení výpočtu
```

Příklad:

```python
filtered_df = (
    df
    .filter(df["year"] == 2025)
    .select("region", "revenue")
)
```

Spark si při tomto zápisu připraví plán, ale výpočet ještě nemusí provést.

Výhody lazy evaluation:

- Spark vidí více kroků současně;
- může optimalizovat celý plán;
- nemusí provádět nepotřebné operace;
- může efektivněji omezit načítaná data.

---

# 10. Transformations a actions

Operace ve Sparku rozdělujeme na:

- **transformations**;
- **actions**.

## Transformations

Transformations popisují, jak chceme data upravit.

Obvykle vytvoří nový Spark DataFrame nebo plán, ale samy výpočet nespustí.

Příklady:

```python
df.select(...)
df.filter(...)
df.withColumn(...)
df.groupBy(...)
df.orderBy(...)
```

Příklad:

```python
filtered_df = df.filter(
    df["year"] == 2025
)
```

`df` zůstává původní a `filtered_df` představuje nový plán zpracování.

## Actions

Actions požadují konkrétní výsledek, a proto spustí připravený výpočet.

Příklady:

```python
df.show()
df.count()
df.collect()
df.take(5)
df.write.parquet(...)
```

Celý princip:

```text
transformation
→ transformation
→ transformation
→ action
→ provedení výpočtu
```

---

# 11. Pozor na `collect()`

Metoda:

```python
df.collect()
```

přenese výsledná data z workers do paměti driveru.

Pokud je výsledek příliš velký, může driveru dojít operační paměť.

```text
workers
→ výsledná data
→ driver
→ paměť jednoho počítače
```

`collect()` proto používáme pouze tehdy, když víme, že je výsledný dataset dostatečně malý.

Pro rychlou kontrolu dat je bezpečnější například:

```python
df.show(5)
```

nebo:

```python
df.take(5)
```

---

# 12. Spark není databáze ani formát souboru

Spark je především výpočetní engine pro zpracování dat.

Spark není:

- databáze;
- datový sklad;
- formát souboru;
- automaticky cloudová služba.

Může pracovat s daty uloženými například v:

- databázích;
- CSV;
- Parquetu;
- datovém jezeře;
- distribuovaném úložišti.

```text
úložiště
→ obsahuje data

Spark
→ data zpracovává
```

---

# 13. Pandas vs. Spark

| Pandas | Spark |
|---|---|
| Především jeden počítač | Lokální počítač nebo cluster |
| Data převážně v paměti jednoho počítače | Data rozdělená do partitions |
| Operace se obvykle provedou ihned | Transformace používají lazy evaluation |
| Vhodný pro malá a středně velká data | Vhodný pro distribuované zpracování |
| Jednodušší použití | Vyšší technická a provozní složitost |
| `groupby()` | `groupBy()` |
| `head()` | `show()` nebo `limit()` |
| `df["revenue"].sum()` | `df.agg(...)` |

Zjednodušená analogie:

```text
Pandas
→ jeden pracovník zpracovává celý úkol

Spark
→ koordinátor rozdělí práci mezi více pracovníků
```

Spark není „lepší Pandas“. Jde o jiný způsob zpracování dat určený pro jiné situace.

---

# 14. Kdy Spark dává smysl

Spark může být vhodný, když:

- data nebo mezivýsledky nezvládne jeden počítač;
- máme k dispozici cluster;
- potřebujeme zpracovávat velké množství souborů;
- provádíme rozsáhlé joiny a agregace;
- výpočet se pravidelně opakuje;
- potřebujeme paralelní datovou pipeline;
- přínos distribuovaného zpracování převýší jeho režii.

Příklad:

```text
několik terabajtů dat
→ distribuované úložiště
→ pravidelné rozsáhlé joiny
→ dostupný cluster
→ Spark může být vhodný
```

---

# 15. Kdy Spark obvykle nedává smysl

Spark zpravidla není první volbou, když:

- dataset pohodlně zvládne Pandas;
- potřebujeme jednoduchý SQL dotaz;
- analyzujeme malý soubor jednorázově;
- nemáme vhodnou infrastrukturu;
- potřebujeme rychlou ruční analýzu v Excelu;
- lze problém vyřešit filtrováním nebo agregací ve zdroji;
- by zavedení Sparku přidalo více složitosti než užitku.

Neexistuje univerzální hranice:

```text
dataset větší než určitý počet GB
→ vždy Spark
```

Rozhodnutí závisí na:

- velikosti zdrojových dat;
- velikosti dat po načtení;
- velikosti mezivýsledků;
- složitosti výpočtu;
- dostupné paměti;
- požadované rychlosti;
- frekvenci zpracování;
- dostupné infrastruktuře.

---

# 16. Výběr vhodného nástroje

| Nástroj | Typické použití |
|---|---|
| Excel | Menší jednorázová analýza, vzorce, kontingenční tabulky |
| SQL | Výběr, filtrování, joiny a agregace dat v databázi |
| Pandas | Flexibilní transformace, API, statistika a analýza na jednom počítači |
| Power Query | Opakovatelné načítání a běžná příprava dat pro Excel nebo Power BI |
| Spark | Distribuované zpracování dat pomocí clusteru |

## Příklad použití Excelu

```text
2 000 řádků
→ jednorázový výpočet
→ kontingenční tabulka
→ Excel
```

## Příklad použití SQL

```text
50 milionů řádků v SQL Serveru
→ poslední rok
→ měsíční tržby podle regionu
→ SQL agregace
→ Power BI
```

## Příklad použití Power Query

```text
pravidelné excelové soubory
→ spojení souborů
→ úprava názvů a datových typů
→ Power Query
→ Power BI
```

## Příklad použití Pandas

```text
REST API
→ vnořený JSON
→ nestandardní transformace
→ statistická analýza
→ Pandas
```

## Příklad použití Sparku

```text
několik TB denně
→ distribuované úložiště
→ rozsáhlé joiny a agregace
→ dostupný cluster
→ Spark
```

---

# 17. Praktické rozhodovací otázky

Před výběrem nástroje se ptáme:

1. Kde jsou data uložená?
2. Kolik dat skutečně potřebujeme?
3. Lze filtrovat nebo agregovat už ve zdroji?
4. Vejdou se data a mezivýsledky do paměti jednoho počítače?
5. Je úloha jednorázová, nebo pravidelná?
6. Jak složité transformace potřebujeme?
7. Kam má výsledek směřovat?
8. Máme dostupný cluster?
9. Převýší přínos Sparku jeho režii a složitost?

---

# 18. Hlavní rozhodovací pravidlo

```text
Nejdříve
→ snížit množství dat
→ vybrat potřebné sloupce
→ filtrovat a agregovat u zdroje
→ zvolit vhodný formát
→ optimalizovat datové typy
→ posoudit možnosti jednoho počítače

Teprve potom
→ uvažovat o Sparku
```

Spark vybíráme podle skutečného požadavku, nikoliv podle toho, že jde o známější nebo technicky pokročilejší technologii.

---

# 19. Co si pamatovat

```text
cluster
→ skupina spolupracujících počítačů

driver
→ plánuje a koordinuje výpočet

workers
→ poskytují prostředky pro provedení práce

partition
→ část datasetu určená ke zpracování

paralelní zpracování
→ více částí dat se zpracovává současně

lazy evaluation
→ výpočet se odloží do okamžiku, kdy je potřeba výsledek

transformation
→ popisuje změnu dat a obvykle výpočet nespustí

action
→ požaduje výsledek a spustí výpočet

collect()
→ přenese data do paměti driveru

shuffle
→ přesun dat mezi workers

Spark
→ výpočetní engine, nikoliv databáze nebo formát souboru
```

Hlavní princip:

> Spark dává smysl tehdy, když distribuované zpracování řeší skutečný problém, který již nelze rozumně vyřešit jednodušším nástrojem.