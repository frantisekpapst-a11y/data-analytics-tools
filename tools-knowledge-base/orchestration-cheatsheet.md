# Orchestration Cheatsheet

Praktický přehled návrhu datových procesů, jejich kroků, pořadí, závislostí a chování při úspěchu nebo chybě.

Cheatsheet je zaměřený na koncepční návrh. Detailní scheduling, retry, alerty a produkční nasazení budou součástí navazujícího bloku Automation.

---

## 1. Co je orchestrace

**Orchestrace** znamená řízení datového procesu.

Určuje:

- které kroky se mají provést;
- v jakém pořadí;
- které kroky mohou běžet současně;
- jaké jsou mezi nimi závislosti;
- za jakých podmínek lze pokračovat;
- co se má stát při úspěchu nebo chybě;
- jak ověřit výsledek procesu.

```text
načtení dat
→ validace
→ transformace
→ aktualizace cílové tabulky
→ obnovení reportu
→ kontrola výsledku
```

Orchestrace sama nemusí data transformovat. Koordinuje nástroje a úkoly, které transformace skutečně provádějí.

Může řídit například:

- SQL dotaz;
- Python skript;
- notebook;
- API volání;
- načtení souboru;
- aktualizaci datové tabulky;
- obnovení Power BI sémantického modelu.

Analogie:

```text
jednotlivé úkoly
→ hudebníci

orchestrace
→ dirigent určující pořadí a koordinaci
```

---

## 2. Orchestrace a transformace

Tyto pojmy nejsou totožné.

### Transformace

Mění obsah nebo strukturu dat:

```text
odstranění duplicit
sjednocení datových typů
výpočet nového sloupce
spojení tabulek
agregace
```

### Orchestrace

Řídí spuštění transformačních a dalších kroků:

```text
nejdříve načti data
→ potom je validuj
→ pouze při úspěchu spusť transformaci
→ nakonec obnov report
```

```text
transformace
→ co se s daty provede

orchestrace
→ kdy, v jakém pořadí a za jakých podmínek se to provede
```

---

## 3. Task

**Task** neboli úkol je jeden konkrétní krok procesu.

Příklady:

- načíst CSV;
- získat data z API;
- spustit SQL dotaz;
- spustit Python skript;
- ověřit počet řádků;
- vytvořit Gold tabulku;
- obnovit Power BI model.

Každý task by měl mít:

- jasný účel;
- definovaný vstup;
- konkrétní činnost;
- očekávaný výstup;
- pravidlo pro určení úspěchu nebo chyby.

Příklad:

```text
Task: Load Supplier File

vstup
→ dodavatelský CSV soubor

činnost
→ načtení souboru do staging tabulky

výstup
→ supplier_stock_staging

úspěch
→ soubor existuje, má očekávané sloupce a obsahuje data
```

---

## 4. Workflow

**Workflow** je celý řízený proces složený z více tasks.

```text
Task 1: načtení
→ Task 2: validace
→ Task 3: transformace
→ Task 4: uložení
→ Task 5: obnovení reportu
```

Zjednodušeně:

```text
task
→ jeden krok

workflow
→ celý proces z více kroků

orchestrace
→ řízení workflow
```

V praxi se pojmy workflow a pipeline někdy částečně překrývají. Důležitější než konkrétní název je správně popsat kroky, vstupy, výstupy a závislosti.

---

## 5. Pipeline

**Pipeline** představuje řízený datový tok od zdrojů k cílovým datům.

```text
zdroje
→ ingestion
→ Bronze nebo staging
→ Silver transformace
→ Gold tabulky
→ reporting
```

Pipeline může obsahovat:

- načítací úkoly;
- transformační úkoly;
- datové kontroly;
- větvení;
- závislosti;
- předání výsledků dalším systémům.

Pojem pipeline se často používá pro technickou realizaci workflow zaměřeného na pohyb a zpracování dat.

---

## 6. Závislost

**Dependency** neboli závislost určuje, že jeden task může začít až po splnění podmínky související s jiným taskem.

```text
načtení dat úspěšně
→ spustit validaci

validace úspěšně
→ spustit transformaci
```

Pokud načtení selže, navazující transformace se nemá spustit nad neúplnými nebo neexistujícími daty.

Závislost může vyjadřovat například:

- předchozí task musí skončit úspěšně;
- všechny vstupní větve musí být dokončeny;
- konkrétní validační podmínka musí být splněna;
- úkol se spustí pouze při chybě předchozího kroku.

---

## 7. Sekvenční kroky

Sekvenční úkoly běží jeden po druhém, protože každý potřebuje výsledek předchozího.

```text
načtení
→ validace
→ transformace
→ publikace
```

Použití:

- transformace potřebuje načtená data;
- Gold tabulka potřebuje dokončená Silver data;
- Power BI refresh potřebuje aktualizované cílové tabulky.

Sekvenční kroky zajišťují správné pořadí, ale prodlužují dobu procesu, pokud mezi nimi ve skutečnosti závislost není.

---

## 8. Paralelní kroky

Nezávislé úkoly mohou běžet současně.

```text
načtení ERP databáze ──────┐
načtení dodavatelského CSV ├──→ spojení dat
načtení kurzů z API ───────┘
```

Výhody:

- kratší celková doba procesu;
- lepší využití výpočetních prostředků;
- oddělení nezávislých zdrojů.

Možné omezení:

- vyšší současné zatížení;
- limity zdrojových systémů nebo API;
- omezený výpočetní výkon;
- složitější vyhodnocení více souběžných výsledků.

Neplatí tedy, že všechny technicky nezávislé tasks musí vždy běžet paralelně. Návrh musí zohlednit dostupné prostředky a omezení zdrojů.

---

## 9. Spojení paralelních větví

Navazující task se může spustit až po dokončení všech vstupů, které potřebuje.

```text
ERP zásoby úspěšně ─────────┐
dodavatelské CSV úspěšně ───┼──→ vytvořit společný dataset
kurzovní API úspěšně ───────┘
```

Pokud je každý ze tří vstupů povinný, selhání jedné větve zastaví závislý společný výstup.

Pokud je některý vstup pouze volitelný, musí být předem definováno, zda a v jaké omezené podobě může workflow pokračovat.

---

## 10. DAG

DAG znamená **Directed Acyclic Graph**, tedy orientovaný graf bez cyklů.

V orchestrace:

```text
uzel
→ task

šipka
→ závislost a směr procesu

bez cyklu
→ proces se nevrací nekonečně sám do sebe
```

DAG pomáhá zobrazit:

- pořadí úkolů;
- závislosti;
- paralelní větve;
- spojení více větví;
- cestu od zdrojů k výsledku.

Příklad:

```text
Source A ─→ Load A ─┐
                    ├→ Transform → Validate → Publish
Source B ─→ Load B ─┘
```

---

## 11. Běžné stavy tasku

### Succeeded

Task skončil úspěšně a splnil definovanou technickou podmínku.

### Failed

Task skončil chybou.

### Running

Task právě probíhá.

### Skipped

Task nebyl spuštěn, protože nebyla splněna jeho závislost.

Příklad:

```text
Load API = Failed
→ Transform Prices = Skipped
→ Refresh Report = Skipped
```

Stav `Skipped` tedy nemusí znamenat chybu samotného tasku. Může být důsledkem selhání předchozího povinného kroku.

---

## 12. Úspěšná a chybová větev

### Úspěšná větev

```text
načtení úspěšně
→ validace úspěšně
→ transformace
→ aktualizace cíle
→ obnovení reportu
```

### Chybová větev

```text
načtení selže
→ závislá transformace se nespustí
→ chyba se zaznamená
→ neúplný výsledek se nepublikuje
```

Zásadní pravidlo:

> Nový neúplný nebo chybný výstup nemá bez vysvětlení nahradit poslední ověřená data.

Přesná pravidla pro opakování tasku a odesílání upozornění budou řešena v bloku Automation.

---

## 13. Povinné a volitelné vstupy

### Povinný vstup

Bez vstupu nelze vytvořit věcně správný závislý výstup.

```text
chybí stav skladových zásob z ERP
→ nelze vytvořit aktuální skladové KPI
→ závislý Gold výstup se neaktualizuje
```

### Volitelný vstup

Základní výstup může vzniknout, ale některá doplňková část bude chybět.

```text
chybí nepovinný komentář managementu
→ základní KPI lze vytvořit
→ omezení se zaznamená
```

Kritičnost zdroje neurčuje:

- velikost souboru;
- typ technologie;
- pořadí v seznamu zdrojů.

Určuje ji business význam pro konkrétní výstup.

---

## 14. Technický a datový úspěch

Technicky úspěšný task nemusí znamenat, že jsou data správná.

```text
soubor se načetl bez chyby
→ technický úspěch

soubor obsahuje očekávané a kvalitní údaje
→ datový úspěch
```

Příklad:

```text
načítací task = Succeeded
počet načtených řádků = 0
→ výsledek vyžaduje datovou kontrolu
```

Prázdný výstup může být:

- platný, pokud opravdu nenastala žádná událost;
- chybný, pokud zdroj neposkytl očekávaná data.

Rozhodnutí musí vycházet z business pravidla, nikoliv pouze z technického stavu tasku.

---

## 15. Validační body

Workflow může obsahovat samostatné validační tasks neboli **quality gates**.

Kontrolovat lze například:

- zda jsou dostupné všechny povinné zdroje;
- počet načtených řádků;
- očekávané názvy sloupců;
- datové typy;
- duplicity;
- chybějící primární nebo cizí klíče;
- neplatné hodnoty;
- časovou návaznost;
- důležité součty proti zdroji;
- aktuálnost dat.

Příklad:

```text
Transformace dokončena
→ Quality Gate
    ├── počet řádků v očekávaném rozsahu
    ├── žádné nepovolené duplicity
    └── součet hodnot odsouhlasen se zdrojem
→ publikace Gold dat
```

Pokud validační podmínka není splněna, závislý výstup se nemá automaticky publikovat.

---

## 16. Správné pořadí reportovacího procesu

Doporučený základní tok:

```text
1. načíst zdrojová data
2. ověřit technické načtení
3. provést datové kontroly
4. transformovat data
5. ověřit transformační výsledek
6. aktualizovat cílové tabulky
7. obnovit Power BI sémantický model
8. ověřit finální stav procesu
```

Power BI report se nemá obnovit před aktualizací a validací jeho zdrojových tabulek.

```text
zdrojové tabulky nejsou připravené
→ report se neobnovuje
```

---

## 17. Vstup a výstup tasku

Výstup jednoho tasku se často stává vstupem dalšího.

```text
Task A
vstup: CSV
výstup: staging tabulka

Task B
vstup: staging tabulka
výstup: validovaná tabulka

Task C
vstup: validovaná tabulka
výstup: Gold tabulka
```

Při návrhu je vhodné u každého tasku zaznamenat:

- název;
- účel;
- vstup;
- výstup;
- závislost;
- podmínku úspěchu;
- reakci na chybu;
- vlastníka nebo odpovědnou roli.

---

## 18. Návrh workflow krok za krokem

### Krok 1 — Určit business výstup

```text
Co musí uživatel nebo report získat?
```

### Krok 2 — Sepsat zdroje

```text
Kde jsou potřebná data?
```

### Krok 3 — Rozdělit proces na tasks

```text
Jaké konkrétní kroky vytvoří požadovaný výstup?
```

### Krok 4 — Určit vstupy a výstupy

```text
Co každý task potřebuje a co vytvoří?
```

### Krok 5 — Určit závislosti

```text
Který task musí čekat na jiný?
```

### Krok 6 — Najít paralelní větve

```text
Které kroky mohou běžet současně?
```

### Krok 7 — Určit kritičnost

```text
Které vstupy a kontroly jsou povinné?
```

### Krok 8 — Navrhnout chybové větve

```text
Co se nesmí spustit, když některý krok selže?
```

### Krok 9 — Přidat quality gates

```text
Jak ověříme, že technicky dokončený výstup je také datově správný?
```

### Krok 10 — Ověřit publikaci

```text
Za jakých podmínek lze aktualizovat report nebo předat data uživatelům?
```

---

## 19. Časté chyby v návrhu

### Obnovení reportu příliš brzy

```text
report se obnoví
→ cílová data ještě nejsou připravena
→ uživatel vidí nekonzistentní výsledek
```

### Chybějící datové kontroly

```text
task skončil bez chyby
→ výstup je automaticky považován za správný
```

### Zbytečně sekvenční proces

Nezávislé úkoly čekají jeden na druhý a zbytečně prodlužují celkovou dobu workflow.

### Nekontrolované paralelní spuštění

Příliš mnoho souběžných tasks přetíží zdrojový systém, API nebo compute.

### Nejasné povinné vstupy

Workflow neví, zda může při výpadku některého zdroje pokračovat.

### Tiché publikování neúplných dat

Report se aktualizuje, přestože chybí důležitý vstup, a uživatel o omezení neví.

### Jeden příliš velký task

Celý proces je schovaný v jednom kroku. Hůře se potom určuje, která část selhala, a obtížněji se znovu spouští pouze potřebná část.

---

## 20. Orchestrace v různých nástrojích

Stejný princip se může realizovat různými technologiemi.

Příklady kategorií nástrojů:

- cloudové datové pipeline;
- workflow orchestrátory;
- Databricks jobs;
- databázové joby;
- CI/CD workflow;
- plánované skripty.

Technologie se liší, ale základní otázky zůstávají stejné:

```text
Co se má spustit?
V jakém pořadí?
Na čem krok závisí?
Co znamená úspěch?
Co se stane při chybě?
Kdy lze publikovat výsledek?
```

---

## 21. Hranice vůči automation

### Tato lekce — návrh orchestrace

- seznam tasks;
- pořadí;
- závislosti;
- paralelní větve;
- povinné a volitelné vstupy;
- chování při úspěchu a chybě;
- základní validační body;
- podmínky publikace.

### Navazující blok Automation

- skutečný schedule;
- časové a událostní spouštění;
- retry;
- timeout;
- monitoring běhů;
- alerty;
- technické logování;
- produkční nasazení;
- správa provozních parametrů.

Výstupem této lekce je návrh procesu, nikoliv jeho automatické produkční spuštění.

---

## 22. Co si pamatovat

```text
task
→ jeden konkrétní krok

workflow
→ celý proces z více tasks

orchestrace
→ řízení pořadí, závislostí a výsledků

dependency
→ podmínka mezi úkoly

sekvenční kroky
→ musí běžet v určeném pořadí

paralelní kroky
→ mohou běžet současně

DAG
→ graf úkolů a jejich závislostí bez cyklů

quality gate
→ datová kontrola před pokračováním

povinný zdroj
→ bez něj nelze vytvořit správný závislý výstup

volitelný zdroj
→ jeho výpadek nemusí zastavit nezávislé výstupy
```

Hlavní princip:

> Dobrá orchestrace nezajišťuje pouze spuštění kroků. Zajišťuje, že se správné kroky provedou ve správném pořadí a že neověřený nebo neúplný výsledek nebude bez vysvětlení publikován.