# Jupyter Notebook Workflow Cheatsheet

Praktický tahák pro vytváření přehledných, reprodukovatelných a businessově srozumitelných analytických notebooků.

---

# 1. Jupyter Notebook

Jupyter Notebook propojuje:

- spustitelný Python kód;
- textovou dokumentaci;
- tabulkové výstupy;
- grafy;
- analytickou interpretaci.

Notebook tak může fungovat jako analytický dokument, který vysvětluje:

```text
business problém
→ zdroj dat
→ kontrolu dat
→ transformace
→ analýzu
→ zjištění
→ doporučení
```

Soubor notebooku používá příponu:

```text
.ipynb
```

---

# 2. Notebook vs. Python Script

| Vlastnost | `.ipynb` notebook | `.py` skript |
|---|---|---|
| Způsob práce | Interaktivní práce po buňkách | Spuštění programu jako celku |
| Dokumentace | Markdown přímo mezi kódem | Komentáře nebo samostatné README |
| Výstupy | Tabulky a grafy přímo v souboru | Obvykle terminál nebo uložené soubory |
| Vhodné použití | Analýza, EDA, prezentace postupu | Automatizace, opakované zpracování |
| Riziko | Skrytý stav kernelu | Menší riziko nesprávného pořadí |
| Git kontrola změn | Složitější | Přehlednější |

Praktické pravidlo:

```text
zkoumání dat a vysvětlení analýzy
→ notebook

pravidelné automatické zpracování
→ Python script
```

Častý pracovní postup:

```text
notebook
→ vytvoření a ověření analytického postupu
→ stabilizace řešení
→ případný přesun opakované logiky do .py skriptu
```

---

# 3. Markdown and Code Cells

## Markdown buňka

Markdown buňka obsahuje:

- nadpisy;
- popis business kontextu;
- vysvětlení postupu;
- interpretaci výsledků;
- omezení analýzy;
- doporučení.

Příklad:

```markdown
## Data Validation

V této části ověříme strukturu, datové typy a kvalitu vstupních dat.
```

Markdown buňka se nespouští jako Python. Po potvrzení se zobrazí jako formátovaný text.

## Code buňka

Code buňka obsahuje spustitelný Python kód:

```python
df = pd.read_csv(DATA_PATH)

df.head()
```

Code buňka může vytvořit:

- proměnné;
- DataFrame;
- tabulkový výstup;
- graf;
- soubor;
- chybovou zprávu.

---

# 4. Zobrazení výstupu

Jupyter automaticky zobrazí poslední výraz v Code buňce.

```python
df.head()
```

Není nutné psát:

```python
print(df.head())
```

Pro přehledné zobrazení DataFrame je vhodné použít přímo:

```python
df
```

`print()` je praktický hlavně pro vlastní textové zprávy:

```python
print("Počet řádků:", df.shape[0])
print("Počet sloupců:", df.shape[1])
```

Pokud potřebujeme zobrazit více tabulek v jedné buňce:

```python
display(region_summary)
display(product_summary)
```

Prakticky:

```text
poslední výraz
→ automatické zobrazení výsledku

print()
→ textové zprávy a kombinace popisu s hodnotou

display()
→ přehledné zobrazení více tabulek nebo objektů
```

---

# 5. Kernel

Kernel je běžící Python prostředí notebooku.

Uchovává v paměti například:

- importované knihovny;
- vytvořené proměnné;
- načtené DataFrame;
- výsledky předchozích buněk.

Příklad:

```python
revenue = 1000
```

V další buňce lze použít:

```python
revenue_with_vat = revenue * 1.21

revenue_with_vat
```

Druhá buňka funguje pouze tehdy, pokud byla první buňka spuštěna a proměnná je stále uložená v kernelu.

---

# 6. Restart

Restart vymaže stav uložený v paměti kernelu.

Po restartu přestanou existovat:

- proměnné;
- DataFrame;
- importy;
- pomocné objekty.

Kód v notebooku ani uložené soubory se nesmažou.

Po restartu je potřeba buňky znovu spustit.

```text
Restart
→ vymazání paměti kernelu

Run All
→ spuštění všech buněk shora dolů
```

---

# 7. Run All

**Run All** spustí všechny Code buňky v pořadí od začátku notebooku.

Používá se k ověření, že notebook:

- obsahuje všechny potřebné importy;
- používá správné cesty;
- spouští buňky ve správném pořadí;
- není závislý na skrytém stavu kernelu;
- vytvoří všechny očekávané výstupy.

Před dokončením notebooku:

```text
uložit notebook
→ Restart
→ Run All
→ zkontrolovat chyby
→ uložit výsledný stav
```

---

# 8. Správné pořadí buněk

Notebook musí být možné spustit shora dolů.

Správné pořadí:

```text
importy
→ cesty
→ načtení dat
→ validace
→ pracovní kopie
→ transformace
→ kontrola po transformaci
→ analýza
→ grafy
→ interpretace
→ export
```

Nesprávný příklad:

```python
region_summary = df.groupby("Region")["Revenue"].sum()
```

Pokud ještě nebylo vytvořeno `df`, vznikne:

```text
NameError
```

Notebook může při ručním spouštění fungovat, ale po restartu selhat. Proto je důležitý závěrečný test přes **Restart + Run All**.

---

# 9. Execution Order

Číslo vedle Code buňky ukazuje pořadí jejího spuštění:

```text
[1]
[2]
[3]
```

Během práce mohou být čísla zpřeházená:

```text
[4]
[9]
[6]
```

To znamená, že buňky nebyly spuštěny postupně shora dolů.

Po závěrečném **Restart + Run All** by mělo být pořadí spuštění souvislé.

---

# 10. Clear All Outputs

**Clear All Outputs** odstraní zobrazené výstupy notebooku:

- tabulky;
- grafy;
- textové výpisy;
- chybová hlášení.

Neodstraní:

- kód;
- Markdown;
- proměnné z paměti kernelu;
- soubory uložené na disku.

Rozdíl:

```text
Clear All Outputs
→ odstraní pouze zobrazené výsledky

Restart
→ vymaže paměť kernelu
```

Pro portfolio notebook je možné ponechat důležité tabulky a grafy, aby byly viditelné přímo na GitHubu.

---

# 11. Working Directory

Working directory je složka, ze které Python aktuálně pracuje.

Kontrola:

```python
from pathlib import Path

Path.cwd()
```

Výsledek může být například:

```text
data-analytics-tools/notebooks/notebook-workflow
```

Relativní cesty se vyhodnocují vůči této složce.

---

# 12. Relativní cesty

Nevhodná absolutní cesta:

```python
pd.read_csv(
    "C:/Users/username/Documents/data-analytics-tools/datasets/raw/sales.csv"
)
```

Taková cesta obvykle nebude fungovat na jiném počítači.

Vhodnější řešení pomocí `pathlib`:

```python
from pathlib import Path

PROJECT_ROOT = Path.cwd().parents[1]

DATA_PATH = (
    PROJECT_ROOT
    / "datasets"
    / "raw"
    / "sales.csv"
)
```

Kontrola:

```python
print("Datový soubor:", DATA_PATH.name)
print("Soubor existuje:", DATA_PATH.exists())
```

Načtení:

```python
df_raw = pd.read_csv(DATA_PATH)
```

Výhoda:

```text
repozitář lze přesunout
→ relativní struktura zůstane zachována
→ notebook může fungovat na jiném počítači
```

---

# 13. Importy a nastavení

Importy patří na začátek notebooku:

```python
from pathlib import Path

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import plotly.express as px
```

Importujeme pouze knihovny, které notebook skutečně používá.

---

# 14. Raw Data and Working Copy

Načtení původních dat:

```python
df_raw = pd.read_csv(DATA_PATH)
```

Vytvoření pracovní kopie:

```python
df = df_raw.copy()
```

Význam:

```text
df_raw
→ původní načtená data v paměti

df
→ pracovní DataFrame určený pro úpravy
```

Původní CSV ve složce `raw` se při úpravách DataFrame automaticky nemění.

---

# 15. Profesionální struktura notebooku

Doporučená struktura analytického notebooku:

```text
Business Context
→ Data Sources
→ Setup and Paths
→ Data Loading
→ Data Validation
→ Data Preparation and Transformation
→ Analysis
→ Visualizations
→ Findings
→ Limitations
→ Recommendations
→ Processed Data Export
```

## Business Context

Vysvětluje:

- proč analýza vzniká;
- jaký problém řeší;
- komu má výsledek pomoci;
- jaké rozhodnutí může podpořit.

## Data Sources

Popisuje:

- odkud data pocházejí;
- jaký mají formát;
- jaké období obsahují;
- co představuje jeden řádek.

## Setup and Paths

Obsahuje:

- importy;
- nastavení cest;
- kontrolu dostupnosti souborů.

## Data Loading

Obsahuje samotné načtení dat:

```python
df_raw = pd.read_csv(DATA_PATH)

df_raw
```

## Data Validation

Ověřuje zejména:

```python
df_raw.shape
df_raw.info()
df_raw.dtypes
df_raw.isna().sum()
df_raw.duplicated().sum()
```

Dále kontroluje:

- názvy sloupců;
- unikátní hodnoty;
- neplatné hodnoty;
- business pravidla.

## Data Preparation and Transformation

Obsahuje například:

- odstranění nadbytečných mezer;
- sjednocení velikosti písmen;
- opravu známých chyb;
- převod datových typů;
- tvorbu nových sloupců.

## Analysis

Obsahuje:

- KPI;
- agregace;
- porovnání kategorií;
- regionální nebo produktové pohledy.

## Visualizations

Graf má podporovat konkrétní analytické zjištění.

```text
tabulka
→ přesné hodnoty

graf
→ rychlé porovnání a rozpoznání rozdílů
```

## Findings

Obsahuje pouze zjištění podložená výsledky analýzy.

## Limitations

Uvádí, co nelze z dostupných dat spolehlivě určit.

## Recommendations

Převádí analytická zjištění do opatrných a realizovatelných dalších kroků.

## Processed Data Export

Ukládá vyčištěný dataset odděleně od původních dat.

---

# 16. Oddělení částí analytického workflow

Jednotlivé fáze nemícháme bez důvodu do jedné velké buňky.

```text
Data Loading
→ získání dat do notebooku

Data Validation
→ zjištění stavu a kvality dat

Data Preparation and Transformation
→ oprava a úprava dat

Analysis
→ výpočty a porovnání

Findings
→ interpretace výsledků

Recommendations
→ návrh dalších kroků
```

Výhoda oddělení:

- lepší čitelnost;
- jednodušší kontrola;
- snadnější hledání chyby;
- srozumitelnější prezentace na GitHubu.

---

# 17. Processed Data Export

Výstupní cesta:

```python
OUTPUT_PATH = (
    PROJECT_ROOT
    / "datasets"
    / "processed"
    / "sales_clean.csv"
)
```

Vytvoření složky, pokud neexistuje:

```python
OUTPUT_PATH.parent.mkdir(
    parents=True,
    exist_ok=True
)
```

Export:

```python
df.to_csv(
    OUTPUT_PATH,
    index=False,
    encoding="utf-8"
)
```

Kontrola:

```python
print("Soubor byl uložen:", OUTPUT_PATH.name)
print("Soubor existuje:", OUTPUT_PATH.exists())
```

Doporučená struktura:

```text
datasets/
├── raw/
│   └── sales.csv
└── processed/
    └── sales_clean.csv
```

Princip:

```text
raw
→ původní data beze změny

processed
→ vyčištěná a validovaná data
```

---

# 18. Výhody notebooků

Notebook je vhodný, protože umožňuje:

- postupovat po menších krocích;
- okamžitě kontrolovat výsledky;
- propojit kód s vysvětlením;
- zobrazit tabulky a grafy;
- dokumentovat analytické rozhodování;
- prezentovat analýzu na GitHubu.

---

# 19. Rizika notebooků

## Skrytý stav kernelu

Proměnná může existovat v paměti, i když příslušná buňka nebyla spuštěna ve správném pořadí.

Řešení:

```text
Restart + Run All
```

## Nesprávné pořadí buněk

Notebook může fungovat autorovi, ale selhat jinému uživateli.

Řešení:

```text
spouštět notebook shora dolů
```

## Příliš mnoho výstupů

Velké tabulky a nepotřebné výpisy zhoršují čitelnost.

Řešení:

```python
df.head()
```

místo zobrazování celého velkého datasetu.

## Příliš dlouhé Code buňky

Jedna rozsáhlá buňka komplikuje hledání chyby.

Řešení:

```text
jedna logická část
→ jedna nebo několik krátkých buněk
```

## Absolutní cesty

Notebook funguje pouze na počítači autora.

Řešení:

```text
relativní cesty + pathlib
```

## Citlivá data

Notebook může obsahovat:

- hesla;
- API klíče;
- osobní údaje;
- interní obchodní data.

Takové údaje nepatří do veřejného GitHub repozitáře.

---

# 20. Časté chyby

## NameError

```text
NameError: name 'df' is not defined
```

Příčina:

- nebyla spuštěna buňka, která vytváří `df`;
- kernel byl restartován;
- buňky byly spuštěny ve špatném pořadí.

Řešení:

```text
Restart + Run All
```

## FileNotFoundError

```text
FileNotFoundError
```

Příčina:

- nesprávná cesta;
- jiný working directory;
- soubor neexistuje.

Kontrola:

```python
Path.cwd()
DATA_PATH.exists()
```

## Tuple is not callable

Nesprávně:

```python
df.shape()
```

Správně:

```python
df.shape
```

`shape` je atribut, nikoliv metoda.

## Starý výstup

Zobrazený výsledek nemusí odpovídat aktuálnímu kódu.

Řešení:

```text
Restart
→ Run All
→ zkontrolovat nové výstupy
```

---

# 21. Notebook na GitHubu

Do repozitáře obvykle patří:

- `.ipynb` notebook;
- použitý ukázkový dataset;
- vyčištěný výstup, pokud má smysl;
- README s vysvětlením projektu;
- relevantní tabulky a grafy uložené ve výstupu notebooku.

Do repozitáře nepatří:

```text
.venv/
.ipynb_checkpoints/
__pycache__/
API klíče
hesla
citlivá data
```

Příklad `.gitignore`:

```gitignore
.venv/
.ipynb_checkpoints/
__pycache__/
*.pyc
```

---

# 22. Kontrola před publikováním

Před uložením na GitHub zkontrolovat:

```text
[ ] Notebook má jasný název.
[ ] Business Context vysvětluje účel analýzy.
[ ] Zdroj a struktura dat jsou popsané.
[ ] Importy jsou na začátku.
[ ] Cesty nejsou závislé na konkrétním počítači.
[ ] Raw data zůstávají beze změny.
[ ] Validace probíhá před transformacemi.
[ ] Transformace jsou vysvětlené.
[ ] Analýza odpovídá business otázkám.
[ ] Grafy mají název a popsané osy.
[ ] Findings vycházejí z výsledků.
[ ] Limitations otevřeně popisují omezení.
[ ] Recommendations nepřekračují možnosti dat.
[ ] Processed dataset je uložen odděleně.
[ ] Notebook úspěšně prošel Restart + Run All.
[ ] V notebooku nejsou chyby ani citlivé údaje.
```

---

# 23. Co si pamatovat

```text
.ipynb
→ interaktivní analytický dokument

.py
→ skript vhodný pro opakované spuštění a automatizaci

Markdown
→ vysvětlení a interpretace

Code
→ spustitelný Python

kernel
→ paměť běžícího Python prostředí

Restart
→ vymaže paměť kernelu

Run All
→ spustí notebook shora dolů

Clear All Outputs
→ odstraní pouze zobrazené výstupy

Path.cwd()
→ aktuální pracovní složka

relativní cesty
→ přenositelnější projekt

raw
→ původní data

processed
→ vyčištěná data

Restart + Run All
→ základní test reprodukovatelnosti
```