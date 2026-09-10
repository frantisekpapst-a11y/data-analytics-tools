# Case Study 04 — Customer Support Lakehouse Layers

## Účel případové studie

Účelem případové studie je navrhnout opakovatelný proces zpracování dat zákaznické podpory v lakehouse architektuře pomocí vrstev Bronze, Silver a Gold.

Cílem není vytvořit skutečnou lakehouse platformu ani programovat pipeline v PySparku nebo Databricks. Výstupem bude návrh datových vrstev, transformačních pravidel a cílových analytických výstupů.

## Business kontext

Společnost poskytuje zákazníkům technickou podporu prostřednictvím několika komunikačních kanálů.

Tým podpory řeší požadavky přijaté prostřednictvím:
- e-mailu;
- telefonu;
- webového formuláře;
- online chatu.

Management potřebuje vyhodnocovat rychlost a kvalitu zákaznické podpory, vytížení jednotlivých týmů a dodržování stanovených SLA.

**SLA — Service Level Agreement** představuje stanovenou úroveň služby, například maximální dobu do první odpovědi nebo vyřešení požadavku.

## Zdrojová data

### Helpdesk systém

Každý den poskytuje CSV export zákaznických požadavků:

```text
support_tickets.csv
```

Obsahuje například:
- identifikátor ticketu;
- datum a čas vytvoření;
- datum a čas první odpovědi;
- datum a čas uzavření;
- identifikátor operátora;
- komunikační kanál;
- prioritu;
- stav;
- kategorii požadavku.

### Průzkum zákaznické spokojenosti

Výsledky průzkumu jsou poskytovány ve formátu JSON:

```text
customer_surveys.json
```

Obsahují například:
- identifikátor ticketu;
- číselné hodnocení zákazníka;
- textový komentář;
- čas odeslání odpovědi.

### Databáze zaměstnanců podpory

Informace o operátorech jsou uložené v relační databázi:

```text
support_agents
```

Obsahují například:
- identifikátor operátora;
- jméno operátora;
- tým;
- pracovní lokalitu;
- datum nástupu;
- aktivní nebo neaktivní stav.

### Textové přepisy komunikace

Online chat může poskytovat samostatné textové nebo JSON soubory s přepisy komunikace.

Tyto soubory nejsou potřebné pro hlavní management dashboard, ale společnost je chce zachovat pro případnou budoucí textovou analýzu.

## Výchozí situace

Zdrojová data používají různé formáty a nejsou připravena pro společnou analýzu.

Mohou obsahovat například:
- duplicitně exportované tickety;
- rozdílné formáty data a času;
- chybějící čas uzavření u otevřených ticketů;
- neplatné hodnocení zákazníka;
- rozdílně zapsané názvy kanálů a priorit;
- ticket odkazující na nedohledaného operátora;
- textové komentáře v různých jazycích;
- technické sloupce nepotřebné pro reporting.

Chybějící čas uzavření nemusí automaticky znamenat chybu. U otevřeného ticketu jde o očekávanou hodnotu, jejíž význam musí být posouzen podle stavu ticketu.

## Požadavky na reporting

Management potřebuje sledovat:
- počet přijatých ticketů;
- počet vyřešených a otevřených ticketů;
- aktuální backlog (tj. požadavky, které byly přijaty, ale dosud nebyly uzavřeny);
- průměrnou dobu do první odpovědi;
- průměrnou dobu vyřešení;
- podíl ticketů splňujících SLA;
- zákaznickou spokojenost;
- výsledky podle týmů;
- výsledky podle operátorů;
- výsledky podle kanálů, priorit a kategorií;
- vývoj výsledků v čase.

## Požadavky na cílové řešení

Navržený proces musí:
- zachovat původní zdrojová data;
- oddělit původní, vyčištěná a business-ready data;
- podporovat CSV, JSON, databázová i textová data;
- standardizovat datum, čas, kanály, priority a stavy;
- rozlišovat očekávané a skutečně chybějící hodnoty;
- oddělit neplatné záznamy do karantény;
- zachovat tickety s nedohledaným operátorem;
- používat jednotná pravidla pro výpočet SLA a časových KPI;
- umožnit opakované spuštění bez vzniku duplicit;
- zachovat data lineage od zdroje až k reportu;
- připravit cílová data pro Power BI.

## Navrhovaný směr zpracování

```text
helpdesk CSV
průzkumy JSON
databáze operátorů
textové přepisy
        ↓
      Bronze
        ↓
      Silver
        ↓
       Gold
        ↓
     Power BI
```

Jednotlivé vrstvy zatím nejsou detailně navrženy. Jejich obsah, transformační pravidla a kontroly kvality budou určeny v následujících úkolech.

## Úkol 1 — volba ETL nebo ELT

Urči, zda navrhovaný proces odpovídá ETL nebo ELT.

Předpokládej, že:
- zdrojová data se nejprve uloží v původní podobě do Bronze vrstvy lakehouse;
- čištění a sjednocení proběhne následně uvnitř stejné platformy;
- business-ready tabulky vzniknou v Gold vrstvě lakehouse.

Svou odpověď stručně zdůvodni.

## Úkol 1 — řešení

Navržený proces odpovídá ELT.

Zdrojová data se nejprve získají z helpdesku, průzkumu spokojenosti, databáze operátorů a úložiště textových přepisů.

Následně se v původní podobě načtou do Bronze vrstvy cílového lakehouse prostředí.

Teprve po načtení proběhnou uvnitř stejné platformy transformace:

```text
Bronze
→ technické čištění a validace
→ Silver
→ business transformace
→ Gold
```

## Úkol 2 — návrh Bronze vrstvy

Navrhni obsah Bronze vrstvy pro všechny zdrojové systémy.

Urči:

- která data mají být načtena;
- zda mají být zdrojové hodnoty upravovány;
- jaká technická metadata mají být doplněna;
- jakým způsobem Bronze umožní opakované zpracování.

## Úkol 2 — řešení

Bronze vrstva bude obsahovat původní data ze všech zdrojových systémů:

- CSV export zákaznických ticketů;
- JSON data z průzkumu spokojenosti;
- export tabulky operátorů;
- textové nebo JSON přepisy online komunikace.

Zdrojové hodnoty nebudou v Bronze opravovány.

Případné chyby budou řešeny až při přechodu do Silver vrstvy.

### Technická metadata

Ke každému načtenému záznamu mohou být doplněna např. metadata:

- čas načtení;
- zdrojový systém;
- název zdrojového souboru;
- identifikátor konkrétního běhu nebo dávky.

Metadata nemění business význam zdrojových hodnot. Umožňují dohledat původ záznamu a průběh jeho načtení.

### Role Bronze vrstvy

Bronze slouží k dlouhodobému zachování původního vstupu.

Umožňuje:
- zopakovat transformace;
- opravit chybu v pipeline;
- změnit transformační pravidla;
- zkontrolovat původní hodnotu;
- dohledat zdroj konkrétního záznamu.

Technické čištění, standardizace a validace proběhnou až při přechodu z Bronze do Silver.

## Úkol 3 — návrh vrstvy Silver

Navrhni pravidla pro vytvoření vyčištěných, validovaných a sjednocených dat ve vrstvě Silver.

Zaměř se zejména na:
- technické duplicity;
- formáty data a času;
- hodnoty kanálu, stavu a priority;
- chybějící čas uzavření;
- neplatná hodnocení;
- neznámé operátory;
- logickou návaznost časových údajů.

## Úkol 3 — řešení

Vrstva Silver obsahuje vyčištěná, validovaná a sjednocená data připravená pro další analytické zpracování.

### Navržené Silver datasety

- `silver_support_tickets`;
- `silver_customer_surveys`;
- `silver_support_agents`;
- `silver_chat_transcripts`;
- `silver_quarantine`.

### Technické transformace

Ve vrstvě Silver budou provedeny zejména tyto operace:
- sjednocení datových typů;
- převod časových údajů do jednotného formátu a časového pásma;
- odstranění pouze prokazatelně identických technických duplicit;
- standardizace textových hodnot;
- kontrola povinných identifikátorů;
- kontrola vazeb mezi datasety;
- vytvoření příznaků datové kvality.

### Otevřené tickety

Hodnota `closed_at = null` není automaticky chybou. U ticketu se stavem `Open` vyjadřuje, že ticket dosud nebyl uzavřen.

Pro otevřené tickety lze vytvořit:

```text
is_open = true
resolution_time = null
```

Doba řešení se počítá pouze pro uzavřené tickety. Pro otevřené tickety lze samostatně vypočítat jejich aktuální stáří:

```text
ticket_age = aktuální čas − created_at
```

Pokud má ticket stav `Closed`, ale `closed_at = null`, jde o nekonzistentní záznam určený k prověření.

### Validace hodnocení zákazníků

Hodnocení musí být v povoleném rozsahu `1–5`.

Hodnotu mimo rozsah, například `7`, nelze automaticky nahradit mediánem. Nejdříve je nutné určit, zda jde o překlep, jinou hodnoticí škálu nebo chybu exportu.

Ve vrstvě Silver lze použít například:

```text
rating = null
rating_valid = false
validation_error = Rating outside allowed range 1–5
```

Původní hodnota zůstává zachovaná ve vrstvě Bronze. Do výpočtu zákaznické spokojenosti vstupují pouze platná hodnocení.

### Neznámý operátor

Pokud `agent_id` nebylo nalezeno v datech operátorů, ticket se neodstraní. Přiřadí se technickému členu `Unknown`.

```text
agent_key = 0
agent_name = Unknown
agent_team = Unknown
agent_found = false
```

Ticket tak zůstane zahrnutý do celkových výsledků, aniž by byl chybně přiřazen konkrétnímu operátorovi.

### Kontrola časové návaznosti

Časové údaje musí odpovídat logickému pořadí:

```text
created_at
≤ first_response_at
≤ closed_at
```

Například první odpověď nemůže nastat před vytvořením ticketu. Nekonzistentní záznam se označí jako neplatný nebo přesune do `silver_quarantine`.

```text
data_quality_status = Invalid
validation_error = First response precedes ticket creation
```

Časy se bez ověřeného business pravidla automaticky nepřepisují.

### Karanténa

Dataset `silver_quarantine` obsahuje záznamy, které nelze bezpečně použít pro další výpočty, například:
- chybějící povinný identifikátor ticketu;
- neplatnou časovou návaznost;
- nerozpoznaný formát data;
- neřešitelný konflikt mezi verzemi stejného záznamu.

Karanténa umožní chyby oddělit, prověřit a případně znovu zpracovat, aniž by byly nenávratně odstraněny.

## Úkol 4 — návrh business transformací a vrstvy Gold

Navrhni business-ready datasety a metriky pro vyhodnocování zákaznické podpory.

## Úkol 4 — řešení

Vrstva Gold obsahuje data připravená pro konkrétní analytické využití a reporting v Power BI.

### `gold_support_tickets`

Granularita:

```text
1 řádek = 1 ticket
```

Navržené údaje:
- `ticket_id`;
- `agent_key`;
- `created_date_key`;
- `closed_date_key`;
- `channel`;
- `priority`;
- `category`;
- `status`;
- `is_open`;
- `first_response_minutes`;
- `resolution_minutes`;
- `resolution_time_band`;
- `sla_first_response_met`;
- `valid_rating`.

Doba vyřešení se počítá pouze pro uzavřené tickety:

```text
resolution_minutes = closed_at − created_at
```

U otevřených ticketů zůstává `resolution_minutes = null`. Jejich aktuální stáří lze sledovat samostatně.

### `gold_agent_performance`

Granularita:

```text
1 řádek = 1 operátor za 1 kalendářní měsíc
```

Navržené metriky:
- počet přidělených ticketů;
- počet vyřešených ticketů;
- průměrná doba první odpovědi;
- průměrná doba vyřešení;
- podíl splněných SLA;
- průměrné platné hodnocení;
- počet platných hodnocení.

Počet hodnocení je nutné uvádět společně s průměrem, aby bylo zřejmé, z jak velkého vzorku výsledek vznikl.

### `gold_support_daily`

Granularita:

```text
1 řádek = 1 kalendářní den
```

Navržené metriky:
- počet nových ticketů;
- počet uzavřených ticketů;
- počet otevřených ticketů na konci dne;
- průměrná doba první odpovědi;
- průměrná doba vyřešení;
- podíl splněných SLA;
- počet platných hodnocení;
- průměrná zákaznická spokojenost.

Denní snapshot umožní sledovat historický vývoj backlogu, nikoliv pouze jeho aktuální hodnotu.

### Výpočet SLA

```text
SLA compliance rate
=
počet ticketů splňujících SLA
/
počet ticketů, u kterých lze SLA vyhodnotit
```

Ticket s první odpovědí do stanoveného limitu získá:

```text
sla_first_response_met = true
```

Limity SLA mohou být odlišné podle priority ticketu.

### Zákaznická spokojenost

Do průměru vstupují pouze platná hodnocení v rozsahu `1–5`.

```text
Average Satisfaction
=
součet platných hodnocení
/
počet platných hodnocení
```

Chybějící a neplatná hodnocení se bez ověřeného business pravidla nenahrazují mediánem. Vedle průměru se sleduje také počet platných odpovědí nebo response rate.

### Business kategorizace doby řešení

V Gold lze vytvořit atribut:

```text
resolution_time_band
```

Například:

- `Do 4 hodin`;
- `4–24 hodin`;
- `Nad 24 hodin`.

Jde o business transformaci určenou pro reporting. Původní číselná doba řešení zůstává zachována.

### Rozdělení odpovědnosti vrstev

#### Silver — technicky připravená data

Ve vrstvě Silver probíhá:
- sjednocení datových typů;
- standardizace hodnot;
- odstranění technických duplicit;
- validace časové návaznosti;
- kontrola vazeb mezi datasety;
- označení nebo oddělení chybných záznamů.

#### Gold — business-ready data

Ve vrstvě Gold probíhá:
- výpočet KPI;
- vytvoření business kategorií;
- agregace podle času, týmu a operátora;
- výpočet splnění SLA;
- příprava dat pro Power BI;
- příprava výstupů pro management reporting.

## Úkol 5 — výsledný datový tok, kontroly kvality a data lineage

Navrhni výsledný tok dat a kontroly, které zajistí úplnost, opakovatelnost a dohledatelnost zpracování.

## Úkol 5 — řešení

### Výsledný datový tok

1. Zdrojová data se načtou do vrstvy Bronze.
2. V Bronze se zachovají původní hodnoty a metadata o načtení.
3. Při přechodu do Silver se data vyčistí, validují a sjednotí.
4. Neplatné záznamy se označí nebo přesunou do karantény.
5. V Gold se vytvoří business metriky, KPI a agregace.
6. Gold tabulky se zpřístupní pro reporting v Power BI.

### Kontroly zdroj → Bronze

Při ingestci se kontroluje zejména:
- doručení všech očekávaných souborů a tabulek;
- možnost data načíst;
- přítomnost povinných sloupců;
- počet zdrojových a načtených záznamů;
- opakované načtení stejné dávky;
- doplnění technických metadat.

### Kontroly Bronze → Silver

Při vytváření Silver vrstvy se kontroluje:
- počet zpracovaných záznamů;
- počet odstraněných technických duplicit;
- počet platných záznamů;
- počet záznamů přesunutých do karantény;
- platnost datových typů;
- časová návaznost;
- povolené rozsahy hodnot;
- vazby mezi tickety, operátory a průzkumy.

Každý rozdíl musí být vysvětlitelný:

```text
Bronze
=
Silver
+ karanténa
+ zdokumentované technické duplicity
```

### Kontroly Silver → Gold

Před publikováním Gold tabulek se ověří:
- správná granularita;
- počet ticketů zahrnutých do výpočtů;
- počet platných hodnocení;
- výpočty doby první odpovědi a vyřešení;
- jmenovatele používané při výpočtu KPI;
- pravidla pro splnění SLA;
- agregace podle data, týmu a operátora;
- shoda výsledků se zdokumentovanými business pravidly.

Samotná existence Gold tabulky není důkazem správnosti jejího obsahu.

### Reconciliation

**Reconciliation** znamená odsouhlasení dat mezi jednotlivými kroky zpracování.

Kontrolovat lze například:
- počet ticketů;
- počet platných a neplatných hodnocení;
- počet otevřených a uzavřených ticketů;
- počet záznamů v karanténě;
- počet ticketů zahrnutých do SLA;
- výsledné hodnoty KPI.

Nevysvětlený rozdíl musí být před publikováním dat prověřen.

### Idempotence

Proces musí být navržen idempotentně. Opakované spuštění stejné dávky nesmí způsobit zdvojení ticketů ani jiné nechtěné změny výsledku.

K identifikaci dávky nebo záznamu lze využít například:

```text
batch_id
source_file
ticket_id
source_updated_at
```

### Data lineage

**Data lineage** popisuje původ a cestu dat od zdroje až k výslednému KPI.

Příklad:

```text
customer_surveys.json
→ bronze_customer_surveys
→ silver_customer_surveys
→ gold_customer_satisfaction
→ Power BI
```

U výsledné metriky musí být možné dohledat:
- zdrojová data;
- použité transformační kroky;
- validační pravidla;
- vyloučené záznamy;
- čas a dávku zpracování.

### Data quality gate

**Data quality gate** je kontrolní bod, který zabrání publikování dat, pokud nejsou splněna stanovená pravidla.

Publikace do Gold nebo Power BI se zastaví například tehdy, když:
- nevysvětlitelně chybějí záznamy;
- neodpovídají kontrolní počty;
- nejsou dodrženy vazby mezi daty;
- KPI nelze správně vypočítat;
- množství neplatných záznamů překročí stanovený limit.

### Výsledné rozhodnutí

Data lze publikovat do Power BI pouze tehdy, když:
- všechny kontroly proběhly úspěšně;
- rozdíly mezi vrstvami jsou vysvětlené;
- chybné záznamy jsou dohledatelné;
- transformační a business pravidla jsou zdokumentovaná;
- výsledné KPI odpovídají datům ve vrstvě Silver.