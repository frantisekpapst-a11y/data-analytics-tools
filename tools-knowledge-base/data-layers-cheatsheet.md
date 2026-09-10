# Data Layers Cheatsheet

Praktický přehled základních principů datových toků, transformačních kroků a architektury Bronze–Silver–Gold.

---

# 1. Datový tok

Datový tok popisuje cestu dat od zdroje až k jejich analytickému využití.

```text
zdrojové systémy
→ načtení dat
→ transformace a validace
→ business-ready data
→ analýza a reporting
```

Typickými zdroji mohou být:

- databáze;
- CSV, JSON a další soubory;
- REST API;
- aplikační logy;
- textové přepisy;
- externí datové služby.

---

# 2. ETL

**ETL** znamená:

```text
Extract → Transform → Load
```

1. **Extract** — získání dat ze zdroje.
2. **Transform** — vyčištění, validace a úprava dat před uložením do cíle.
3. **Load** — načtení připravených dat do cílového systému.

ETL je vhodné například tehdy, když:

- cílový systém má přijímat pouze připravená data;
- transformace probíhají mimo cílovou platformu;
- je nutné před uložením omezit citlivá nebo nepotřebná data;
- cílový systém nemá sloužit k uchování původních vstupů.

Příklad:

```text
CSV soubor
→ čištění v Pythonu
→ načtení vyčištěné tabulky do data warehouse
```

---

# 3. ELT

**ELT** znamená:

```text
Extract → Load → Transform
```

1. **Extract** — získání dat ze zdroje.
2. **Load** — uložení původních dat do cílové datové platformy.
3. **Transform** — zpracování dat uvnitř cílové platformy.

ELT se často používá v cloudových datových skladech a lakehouse platformách, které poskytují dostatečný výkon pro následné transformace.

Výhody:

- zachování původních dat;
- možnost zpracování zopakovat;
- oddělení načtení od dalších transformací;
- využití výkonu cílové platformy;
- podpora více analytických účelů nad stejnými vstupy.

Příklad:

```text
CSV, JSON, databáze a textové soubory
→ načtení do Bronze
→ transformace do Silver
→ příprava Gold tabulek
```

---

# 4. ETL vs. ELT

| Oblast | ETL | ELT |
|---|---|---|
| Pořadí | Extract → Transform → Load | Extract → Load → Transform |
| Místo hlavních transformací | Před cílovým systémem | Uvnitř cílové platformy |
| Původní data v cíli | Nemusí být zachována | Obvykle jsou zachována |
| Typické prostředí | Tradiční datový sklad | Cloudový warehouse nebo lakehouse |
| Opakování zpracování | Může vyžadovat nový přístup ke zdroji | Lze vycházet z uložených raw dat |

Použitý nástroj sám o sobě neurčuje, zda jde o ETL, nebo ELT. Rozhoduje pořadí a místo jednotlivých kroků.

---

# 5. Základní stavy dat

## Raw data

**Raw data** jsou původní nebo minimálně upravená data získaná ze zdroje.

Mohou obsahovat:

- duplicity;
- chybějící hodnoty;
- rozdílné formáty;
- neplatné hodnoty;
- technická metadata;
- historii zdrojových změn.

## Staging

**Staging** je dočasná pracovní oblast používaná při načítání a transformaci dat.

Slouží například pro:

- dočasné uložení dávky;
- kontrolu formátu a struktury;
- přípravu dat před načtením do cílových tabulek;
- oddělení zdrojového systému od transformačního procesu.

Staging nemusí být dlouhodobě uchovávaná vrstva.

## Transformovaná data

Transformovaná data již prošla technickými úpravami, například:

- sjednocením datových typů;
- odstraněním technických duplicit;
- standardizací hodnot;
- validací rozsahů a vazeb;
- řešením chybných záznamů.

## Business-ready data

**Business-ready data** jsou připravena pro konkrétní analýzu, KPI nebo reporting.

Mohou obsahovat:

- faktové a dimenzní tabulky;
- business metriky;
- předpočítané agregace;
- kategorizace;
- pravidla SLA;
- datamarty pro jednotlivé oblasti.

---

# 6. Technické a business transformace

## Technické transformace

Technické transformace zajišťují, aby byla data konzistentní a použitelná.

Příklady:

- převod datových typů;
- sjednocení formátu data a času;
- odstranění technických duplicit;
- standardizace hodnot `email`, `E-MAIL` a `Email` na `Email`;
- kontrola povinných identifikátorů;
- validace časové návaznosti;
- kontrola referenčních vazeb.

## Business transformace

Business transformace převádějí validní data do podoby odpovídající obchodním pravidlům.

Příklady:

- výpočet tržeb nebo zisku;
- vyhodnocení splnění SLA;
- rozdělení doby řešení do kategorií;
- výpočet zákaznické spokojenosti;
- agregace podle týmu, regionu nebo období;
- příprava KPI pro management.

---

# 7. Bronze–Silver–Gold

Bronze, Silver a Gold představují logické vrstvy zpracování dat. Tento přístup se označuje také jako **Medallion Architecture**.

## Bronze — původní data

Bronze obsahuje data v původní nebo minimálně změněné podobě.

Účel:

- zachovat původní vstupy;
- umožnit audit a dohledání hodnot;
- podpořit opakované zpracování;
- evidovat zdroj a datovou dávku.

Typická metadata:

```text
loaded_at
source_system
source_file
batch_id
```

Bronze není určena jako přímý zdroj pro management reporting.

## Silver — čistá a validovaná data

Silver obsahuje technicky připravená, sjednocená a validovaná data.

Ve vrstvě Silver probíhá:

- sjednocení datových typů;
- standardizace hodnot;
- odstranění prokazatelných technických duplicit;
- validace časové návaznosti;
- kontrola vazeb mezi datasety;
- označení nebo oddělení chybných záznamů.

Silver má být znovupoužitelným základem pro více analytických výstupů.

## Gold — business-ready data

Gold obsahuje data připravená pro konkrétní business využití.

Ve vrstvě Gold probíhá:

- výpočet KPI;
- vytvoření business kategorií;
- agregace podle času, týmu nebo zákazníka;
- příprava faktových a dimenzních tabulek;
- vytvoření datamartů;
- příprava dat pro Power BI a management reporting.

---

# 8. Analogie s Pandas workflow

| Datová vrstva | Analogie v Pandas |
|---|---|
| Bronze | `df_raw` |
| Silver | `clean_df` |
| Gold | Souhrnné tabulky, KPI a exporty pro Power BI |

Jde pouze o myšlenkovou analogii. Samotné názvy proměnných z notebooku ještě nevytvářejí skutečnou lakehouse architekturu.

---

# 9. Pravidla pro chybějící a neplatné hodnoty

Chybějící hodnota není automaticky chyba a neměla by být automaticky nahrazena nulou nebo mediánem.

Příklad otevřeného ticketu:

```text
status = Open
closed_at = null
```

Hodnota `null` je zde správná, protože ticket dosud nebyl uzavřen.

Naopak kombinace:

```text
status = Closed
closed_at = null
```

je nekonzistentní a vyžaduje kontrolu.

Neplatná hodnota také není totéž jako chybějící hodnota. Hodnocení `7` na škále `1–5` se bez ověřeného pravidla nenahrazuje mediánem.

Doporučený postup:

```text
rating = null
rating_valid = false
validation_error = Rating outside allowed range 1–5
```

Původní hodnota zůstává zachovaná v Bronze.

---

# 10. Unknown člen

Pokud faktový záznam odkazuje na chybějící dimenzní záznam, neměl by být automaticky odstraněn.

Lze použít technického člena `Unknown`:

```text
agent_key = 0
agent_name = Unknown
agent_team = Unknown
agent_found = false
```

Výhody:

- obchodní událost zůstane zachována;
- vazba na dimenzi zůstane platná;
- problém lze samostatně sledovat;
- záznam není nesprávně přiřazen konkrétnímu členovi.

---

# 11. Karanténa

**Karanténa** je oddělené úložiště záznamů, které nelze bezpečně použít pro další výpočty.

Patří sem například:

- chybějící povinný identifikátor;
- neplatná časová návaznost;
- nerozpoznaný formát data;
- konflikt mezi verzemi záznamu;
- hodnota mimo povolený rozsah.

Karanténa umožňuje chyby prověřit a případně znovu zpracovat. Záznamy se tak nenávratně neztratí.

---

# 12. Reconciliation

**Reconciliation** znamená odsouhlasení dat mezi zdrojem a cílem nebo mezi jednotlivými vrstvami.

Kontrolovat lze například:

- počet záznamů;
- počet technických duplicit;
- počet platných a neplatných záznamů;
- počet záznamů v karanténě;
- kontrolní součty;
- výsledné hodnoty KPI.

Příklad:

```text
10 000 záznamů v Bronze
= 9 950 platných záznamů v Silver
+ 50 záznamů v karanténě
```

Každý rozdíl musí odpovídat zdokumentovanému transformačnímu pravidlu.

---

# 13. Idempotence

**Idempotence** znamená, že opakované spuštění stejné datové dávky nezpůsobí zdvojení ani jinou nechtěnou změnu výsledku.

K identifikaci dávky a záznamů lze použít například:

```text
batch_id
source_file
business_key
source_updated_at
```

Proces musí určit, zda již načtený záznam:

- přeskočí;
- aktualizuje;
- bezpečně nahradí;
- uloží jako novou historickou verzi.

---

# 14. Data lineage

**Data lineage** neboli datový původ zachycuje cestu dat od zdroje až k výslednému reportu.

Příklad:

```text
customer_surveys.json
→ bronze_customer_surveys
→ silver_customer_surveys
→ gold_customer_satisfaction
→ Power BI
```

U výsledné metriky má být možné dohledat:

- zdrojová data;
- použité transformační kroky;
- validační pravidla;
- vyloučené záznamy;
- čas a dávku zpracování.

---

# 15. Data quality gate

**Data quality gate** je kontrolní bod, který zabrání publikování nespolehlivých dat.

Proces lze zastavit například tehdy, když:

- chybějí očekávané soubory nebo sloupce;
- nevysvětlitelně zmizely záznamy;
- neodpovídají kontrolní počty;
- jsou porušeny klíčové vazby;
- množství chybných záznamů překročilo limit;
- KPI nelze správně vypočítat.

---

# 16. Praktický příklad — zákaznická podpora

## Zdroje

- helpdesk CSV s tickety;
- JSON se zákaznickými průzkumy;
- databáze operátorů;
- textové nebo JSON přepisy komunikace.

## Bronze

```text
bronze_support_tickets
bronze_customer_surveys
bronze_support_agents
bronze_chat_transcripts
```

Bronze zachová původní hodnoty a metadata o načtení.

## Silver

```text
silver_support_tickets
silver_customer_surveys
silver_support_agents
silver_chat_transcripts
silver_quarantine
```

Silver sjednotí datové typy, kanály, stavy a priority, odstraní technické duplicity, ověří časovou návaznost a oddělí neplatné záznamy.

## Gold

```text
gold_support_tickets
gold_agent_performance
gold_support_daily
gold_customer_satisfaction
```

Gold připraví KPI, agregace a datasety pro Power BI.

---

# 17. Příklady KPI zákaznické podpory

## Doba první odpovědi

```text
first_response_time
= first_response_at − created_at
```

Musí platit:

```text
created_at ≤ first_response_at
```

## Doba vyřešení

Počítá se pouze u uzavřených ticketů:

```text
resolution_time
= closed_at − created_at
```

U otevřeného ticketu zůstává `resolution_time = null`.

## Splnění SLA

```text
SLA compliance rate
= počet ticketů splňujících SLA
/ počet ticketů, u kterých lze SLA vyhodnotit
```

## Zákaznická spokojenost

Do průměru vstupují pouze platná hodnocení.

Společně s průměrem je vhodné uvést:

- počet platných hodnocení;
- response rate;
- povolený rozsah hodnocení.

## Backlog

Backlog představuje počet ticketů, které k určitému okamžiku ještě nebyly uzavřeny.

Pro sledování vývoje v čase jsou vhodné pravidelné snapshoty, například stav na konci každého dne.

---

# 18. Doporučený postup návrhu pipeline

1. Určit zdrojové systémy a jejich vlastníky.
2. Popsat cílové analytické využití.
3. Rozhodnout mezi ETL a ELT.
4. Navrhnout granularitu cílových tabulek.
5. Oddělit technické a business transformace.
6. Stanovit pravidla pro chyby, `null` hodnoty a duplicity.
7. Navrhnout reconciliation a kontrolní součty.
8. Zajistit idempotentní zpracování.
9. Zdokumentovat data lineage.
10. Nastavit data quality gates před publikováním.

---

# 19. Nejčastější chyby

- přepisování původních dat v Bronze;
- automatické nahrazování každé hodnoty `null` nulou nebo mediánem;
- odstranění chybných záznamů bez evidence;
- směšování technických a business transformací;
- výpočet průměru bez uvedení velikosti vzorku;
- nezohlednění granularit tabulek;
- opakované načtení stejné dávky bez kontroly;
- publikování výsledků bez reconciliation;
- nezdokumentovaný původ KPI;
- používání Gold vrstvy jako úložiště původních souborů.

---

# 20. Co si pamatovat

```text
ETL
→ transformace před načtením do cíle

ELT
→ načtení původních dat a transformace v cílové platformě

Bronze
→ původní data a metadata

Silver
→ čistá, sjednocená a validovaná data

Gold
→ business-ready data, KPI a agregace

reconciliation
→ odsouhlasení dat mezi kroky

idempotence
→ opakované spuštění nezdvojí výsledek

data lineage
→ dohledatelná cesta dat od zdroje k reportu

data quality gate
→ zastavení publikace nespolehlivých dat
```

Hlavní princip:

> Původní data zachováme, technickou kvalitu řešíme odděleně od business logiky a do reportingu publikujeme pouze ověřená a dohledatelná data.
