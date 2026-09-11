# Case Study 05 — Cloud Platform Selection

## Účel případové studie

Cílem případové studie je navrhnout přehledné a udržitelné datové prostředí pro pravidelný management reporting.

Případová studie má současně ukázat schopnost:
- rozpoznat role jednotlivých cloudových služeb;
- porovnat samostatnou Azure architekturu s integrovanou platformou Microsoft Fabric;
- vybrat řešení odpovídající potřebám společnosti;
- zohlednit existující technologie a dovednosti týmu;
- pracovat s požadavky na zabezpečení, správu, výkon a náklady;
- zdůvodnit architektonické rozhodnutí z business i technického pohledu.

## Business kontext

Logistická společnost chce vytvořit cloudové analytické řešení pro pravidelné vyhodnocování zásilek, dopravců a přepravních nákladů.

Data jsou v současnosti uložená v několika zdrojových systémech a souborech. Jejich příprava není řízena v jednom společném datovém prostředí a část zpracování probíhá ručně.

Management potřebuje pravidelně aktualizovaný Power BI report, který poskytne jednotný pohled na výkonnost logistických procesů.

Společnost zvažuje dvě možnosti:
- sestavit řešení pomocí samostatných služeb Microsoft Azure;
- vytvořit integrované analytické řešení v Microsoft Fabricu.

## Datové zdroje

Společnost pracuje s následujícími zdroji:
- lokální SQL Server obsahující provozní údaje o zásilkách;
- JSON soubory obsahující GPS události zaznamenané během přepravy;
- CSV soubory obsahující ceníky dopravců;
- Excel soubor obsahující cílové hodnoty stanovené managementem.

## Business požadavky

Management potřebuje sledovat:
- celkový počet zásilek;
- počet doručených zásilek;
- počet zásilek doručených včas;
- podíl včas doručených zásilek;
- počet opožděných zásilek;
- průměrnou dobu zpoždění;
- celkové přepravní náklady;
- průměrné náklady na zásilku;
- plnění stanovených cílů;
- výsledky podle dopravce;
- výsledky podle skladu;
- výsledky podle trasy a cílové oblasti;
- vývoj výsledků v čase.

Výsledná data mají být každý den aktualizována a zpřístupněna prostřednictvím Power BI.

## Výchozí technologické prostředí

Společnost:
- již používá Power BI;
- má k dispozici Microsoft Fabric Capacity;
- využívá Microsoft Entra ID pro firemní identity;
- provozuje část zdrojových systémů v lokální firemní síti;
- zaměstnává analytiky se znalostí SQL, Power Query a Power BI;
- nemá rozsáhlý tým specializovaných cloudových data engineerů;
- chce omezit správu samostatných cloudových služeb;
- potřebuje sledovat využití výpočetní kapacity a náklady;
- požaduje řízená oprávnění k datům;
- chce oddělit vývojové, testovací a produkční prostředí.

## Technické požadavky

Navržené řešení musí umožnit:
- automatické načítání dat z lokálního SQL Serveru;
- načítání CSV, JSON a Excel souborů;
- uchování původních zdrojových dat;
- oddělení vrstev Bronze, Silver a Gold;
- čištění a validaci dat;
- vytváření business-ready tabulek;
- vytvoření Gold hvězdicového schématu;
- napojení Power BI;
- řízení identit a oprávnění;
- bezpečný přístup k lokálním datovým zdrojům;
- každodenní automatické spuštění datového procesu;
- monitoring úspěšnosti datových kroků;
- kontrolu využití cloudových prostředků a kapacity;
- oddělení prostředí DEV, TEST a PROD.

## Posuzované varianty

### Varianta A — samostatné služby Microsoft Azure

Možná architektura:

```text
lokální SQL Server a soubory
→ on-premises data gateway
→ Azure Data Factory
→ Azure Data Lake Storage Gen2
→ Azure Databricks
→ Azure Synapse Analytics
→ Power BI
```

### Varianta B — Microsoft Fabric

Možná architektura:

```text
lokální SQL Server a soubory
→ on-premises data gateway
→ Fabric Data Factory
→ Fabric Lakehouse v OneLake
→ Dataflow Gen2, SQL nebo Fabric Notebook
→ Fabric Warehouse
→ Power BI semantic model
→ Power BI report
```

## Hodnoticí kritéria

Obě varianty budou porovnány podle následujících oblastí:
- návaznost na existující technologické prostředí;
- dovednosti analytického týmu;
- složitost vytvoření řešení;
- provozní a administrativní náročnost;
- možnosti datové integrace;
- možnosti transformace dat;
- napojení na Power BI;
- škálovatelnost;
- zabezpečení a řízení přístupů;
- monitoring;
- kontrola nákladů a využití kapacity;
- možnosti budoucího rozšíření.

## Očekávaný výstup

Výstupem případové studie bude:
- návrh datového toku;
- návrh řešení pomocí samostatných služeb Azure;
- návrh řešení v Microsoft Fabricu;
- porovnání obou variant;
- doporučení vhodnější platformy;
- zdůvodnění výsledného rozhodnutí;
- přehled situací, ve kterých by mohla být vhodnější druhá varianta.

## Rozsah případové studie

Případová studie je zaměřena na koncepční návrh cloudového analytického prostředí.

Nezahrnuje:
- vytvoření skutečných cloudových prostředků;
- detailní síťovou konfiguraci;
- produkční nastavení zabezpečení;
- přesný výpočet cloudových nákladů;
- implementaci kompletní datové pipeline;
- vytvoření výsledného Power BI reportu.

Uvedené názvy tabulek, atributů a služeb slouží k návrhu architektury a mohou být při skutečné implementaci upraveny podle konkrétních zdrojových dat a firemních standardů.

---

## Úkol 1 — Kritéria výběru platformy

Urči, která kritéria musí společnost posoudit při rozhodování mezi samostatnými službami Microsoft Azure a Microsoft Fabricem.

## Úkol 1 — řešení

Rozhodnutí musí vycházet z konkrétního prostředí společnosti.

### Existující technologie

Společnost již používá Power BI, Microsoft Fabric Capacity, Microsoft Entra ID a lokální SQL Server.

Existující Fabric Capacity a používání Power BI podporují variantu Microsoft Fabric, protože není nutné zavádět úplně oddělenou analytickou platformu.

### Dovednosti týmu

Analytici znají především SQL, Power Query a Power BI. Fabric na tyto dovednosti přímo navazuje prostřednictvím Dataflow Gen2, Fabric Warehouse, SQL analytics endpointu a Power BI sémantického modelu.

Samostatná Azure architektura s ADLS Gen2, Azure Data Factory, Azure Databricks a Synapse Analytics by vyžadovala více cloudových a data engineering znalostí.

### Datové zdroje

Část dat je uložená v lokálním SQL Serveru. Obě varianty proto musí vyřešit bezpečné propojení lokálního a cloudového prostředí, například pomocí on-premises data gateway.

Souborové zdroje musí být načítány opakovatelně a uloženy tak, aby byla zachována jejich původní podoba.

### Objem a složitost dat

Před konečným rozhodnutím je nutné zjistit:

- objem zdrojových dat;
- očekávaný růst;
- frekvenci aktualizací;
- složitost transformací;
- požadovanou rychlost zpracování;
- počet uživatelů reportu.

Případová studie tyto hodnoty přesně neurčuje, proto je nelze použít pro detailní dimenzování výkonu nebo výpočet nákladů.

### Zabezpečení

Řešení musí zohlednit identity uživatelů a aplikací, nejnižší nutná oprávnění, síťový přístup k lokálním zdrojům, ochranu přihlašovacích údajů a oddělení DEV, TEST a PROD.

### Správa řešení

Společnost nemá rozsáhlý tým cloudových data engineerů. Významným kritériem je proto množství samostatných služeb, které bude nutné konfigurovat, propojovat, zabezpečovat, monitorovat, škálovat a provozně podporovat.

### Náklady

Porovnat je nutné zejména náklady na uložení dat, výpočetní výkon, dobu běhu transformačních služeb, přenos dat, Fabric Capacity a provozní správu. Bez údajů o objemu dat a využití nelze vytvořit přesný finanční výpočet.

### Kritéria podporující Microsoft Fabric

- existující Fabric Capacity;
- používání Power BI;
- znalost SQL a Power Query;
- požadavek na integrované prostředí;
- menší data engineering tým;
- snaha o omezení správy samostatných služeb.

### Kritéria podporující samostatné Azure služby

- větší technická flexibilita;
- nezávislé škálování jednotlivých služeb;
- specializované síťové řešení;
- rozsáhlé Spark transformace;
- integrace s existujícím Azure datovým prostředím;
- zkušený cloudový data engineering tým.

---

## Úkol 2 — Návrh samostatné Azure varianty

Navrhni role jednotlivých služeb v samostatné Azure architektuře.

## Úkol 2 — řešení

Navržený datový tok:

```text
zdroje
→ Azure Data Factory
→ ADLS Gen2
→ Azure Databricks
→ Azure Synapse Analytics
→ Power BI
```

- **Azure Data Factory** zajišťuje načítání, přesun dat a řízení pipeline.
- **ADLS Gen2** slouží jako data lake pro původní i zpracovaná data.
- **Azure Databricks** provádí čištění, transformace a propojení dat.
- **Azure Synapse Analytics** zpřístupňuje připravená analytická data prostřednictvím SQL.
- **Power BI** vytváří sémantický model, DAX míry a reporty.

Datové vrstvy jsou logické části architektury, nikoliv názvy služeb. V tomto návrhu může ADLS obsahovat Bronze, Silver i souborová Gold data. Synapse lze využít pro relační Gold tabulky a analytické SQL.

### Zabezpečení Azure varianty

- Microsoft Entra ID ověřuje identity uživatelů a služeb.
- Azure RBAC řídí přístup k Azure prostředkům.
- Oprávnění jednotlivých služeb řídí přístup k datům.
- Managed identities umožňují službám přistupovat k jiným prostředkům bez hesla uloženého v kódu.
- Azure Key Vault chrání tajné údaje, které nelze nahradit řízenou identitou.

---

## Úkol 3 — Návrh Microsoft Fabric varianty

Navrhni datový tok v integrovaném prostředí Microsoft Fabric.

## Úkol 3 — řešení

Navržený datový tok:

```text
zdroje
→ Fabric Data Factory
→ Fabric Lakehouse
→ Fabric Warehouse
→ Power BI
```

- **Fabric Data Factory** zajišťuje načítání dat a řízení pipeline.
- **Fabric Lakehouse** ukládá a zpracovává Bronze a Silver data.
- **Fabric Warehouse** obsahuje relační Gold tabulky připravené pro analytické SQL a reporting.
- **Power BI** vytváří sémantický model, DAX míry a reporty.
- **OneLake** představuje společnou úložnou vrstvu Fabricu.
- **Fabric Capacity** poskytuje sdílený výpočetní výkon pro Fabric položky.

Lakehouse a Warehouse jsou dva různé Fabric objekty postavené vedle sebe nad OneLake. Neznamená to automaticky, že používají stejnou tabulku bez kopírování. Data mohou být mezi položkami sdílena nebo přesouvána podle zvoleného návrhu.

---

## Úkol 4 — Výběr platformy

Vyber vhodnější variantu pro popsanou společnost.

## Úkol 4 — řešení

Pro tento scénář je doporučen Microsoft Fabric.

Rozhodnutí podporují zejména:
- existující Power BI prostředí;
- dostupná Fabric Capacity;
- znalost SQL, Power Query a Power BI;
- menší počet samostatných služeb;
- jednotné prostředí pro integraci, ukládání, transformace a reporting;
- menší provozní a administrativní náročnost.

Fabric není vybrán proto, že by byl automaticky nejlepší nebo nejlevnější pro každou situaci. Odpovídá konkrétním požadavkům a výchozímu prostředí této společnosti.

---

## Úkol 5 — Bezpečný přístup k lokálnímu SQL Serveru

Navrhni způsob, jakým bude cloudové řešení bezpečně načítat data z databáze ve firemní síti.

## Úkol 5 — řešení

Pro připojení použijeme on-premises data gateway:

```text
lokální SQL Server
→ on-premises data gateway
→ Fabric Data Factory
→ Fabric Lakehouse
```

Gateway funguje jako zabezpečený most mezi lokální sítí a cloudovou službou. SQL Server nemusí být veřejně dostupný z internetu a gateway data trvale neukládá.

Pro produkční provoz je nutné zajistit dostupnost gateway, řízené servisní identity, bezpečné přihlašovací údaje a monitoring připojení.

---

## Úkol 6 — Identity a oprávnění

Navrhni základní způsob řízení přístupu uživatelů a služeb.

## Úkol 6 — řešení

Microsoft Entra ID bude spravovat identity uživatelů, skupin a služeb. Oprávnění budou přidělována skupinám podle pracovních rolí, nikoliv prostřednictvím jednoho sdíleného účtu.

Příklad skupin:
- `Logistics_Data_Engineers` — správa pipeline, Lakehouse a Warehouse;
- `Logistics_Analysts` — práce s Warehouse a sémantickým modelem;
- `Logistics_Report_Viewers` — prohlížení publikovaných reportů.

Ve Fabricu budou použity workspace roles, oprávnění konkrétních položek a podle potřeby také datová oprávnění OneLake nebo SQL. Návrh dodržuje princip **least privilege**, tedy přidělení pouze nejnižších oprávnění nutných pro danou práci.

Stejný požadavek platí i pro Azure variantu. Rozdíl spočívá v tom, že v Azure se oprávnění nastavují napříč více samostatnými službami, zatímco ve Fabricu jsou více sjednocena v jedné platformě.

---

## Úkol 7 — Oddělení prostředí

Navrhni oddělení vývoje, testování a produkčního provozu.

## Úkol 7 — řešení

Vytvoříme samostatné Fabric workspaces:
```text
DEV
→ vývoj a úpravy

TEST
→ ověření funkčnosti a dat

PROD
→ stabilní řešení pro koncové uživatele
```

Obsah lze mezi prostředími přesouvat řízeným způsobem pomocí deployment pipeline. Připojení a konfigurace musí být nastaveny tak, aby vývojové procesy omylem nezapisovaly do produkčních cílů.

Workspaces mohou využívat společnou Fabric Capacity, pokud to odpovídá provozním pravidlům a dostupnému výkonu. Workspace a Capacity nejsou totéž: workspace organizuje obsah, Capacity poskytuje výpočetní výkon.

---

## Úkol 8 — Načítání a uchování zdrojů

Navrhni načítání všech zdrojů a zachování jejich původní podoby.

## Úkol 8 — řešení

Jednotlivé zdroje načteme pomocí Fabric Data Factory:

```text
lokální SQL Server
→ on-premises data gateway
→ Fabric Data Factory

CSV + JSON + Excel
→ Fabric Data Factory
```

Původní data uložíme beze změny do Bronze vrstvy Fabric Lakehouse:

```text
Bronze
├── provozní data o zásilkách
├── GPS události v JSON
├── ceníky dopravců v CSV
└── cílové hodnoty v Excelu
```

---

## Úkol 9 — Silver vrstva

Navrhni hlavní technické transformace a kontroly datové kvality.

## Úkol 9 — řešení

Silver vrstva bude obsahovat vyčištěná, validovaná a sjednocená data.

Chybné záznamy nebudou bez vysvětlení automaticky mazány. Budou označeny nebo odděleny pro kontrolu, aby nedošlo k nevysvětlené ztrátě zásilek.

```text
Bronze
→ původní data

Silver
→ vyčištěná a validovaná data
```

---

## Úkol 10 — Granularita faktové tabulky

Urči, co bude představovat jeden řádek hlavní Gold faktové tabulky.

## Úkol 10 — řešení

Jeden řádek tabulky `fact_shipments` bude představovat jednu zásilku:

```text
1 řádek v fact_shipments
=
1 zásilka
```

Tato granularita umožňuje správně počítat počty zásilek, včasné a opožděné zásilky, dobu zpoždění i přepravní náklady.

---

## Úkol 11 — Gold hvězdicové schéma

Navrhni hlavní tabulky Gold vrstvy.

## Úkol 11 — řešení

Gold vrstva bude obsahovat hvězdicové schéma:

```text
dim_carrier ──────┐
dim_warehouse ────┤
dim_route ─────────┤
dim_destination ───┼── fact_shipments
dim_date ──────────┘
```

### Faktová tabulka

`fact_shipments` bude obsahovat jeden řádek na zásilku, cizí klíče do dimenzí a hodnoty potřebné pro výpočet KPI, například stav doručení, dobu zpoždění, přepravní náklady a příznaky splnění cílů.

### Dimenzní tabulky

- `dim_carrier` — dopravce;
- `dim_warehouse` — výchozí sklad;
- `dim_route` — přepravní trasa;
- `dim_destination` — cílová oblast;
- `dim_date` — datum, měsíc, čtvrtletí a rok.

---

## Úkol 12 — Rozdělení přípravy KPI

Urči, která logika bude připravena v Gold vrstvě a která v Power BI.

## Úkol 12 — řešení

Gold faktová tabulka připraví hodnoty na úrovni jedné zásilky, například:

- `is_delivered` — zda byla zásilka doručena;
- `is_on_time` — zda byla doručena včas;
- `delay_minutes` — délka zpoždění;
- `transport_cost` — přepravní náklady;
- `target_met` — zda byl splněn stanovený cíl.

Power BI z těchto hodnot vytvoří dynamické DAX míry reagující na filtry dopravce, skladu, trasy, cílové oblasti a času.

```DAX
Total Shipments =
COUNTROWS(fact_shipments)
```

```DAX
Delivered Shipments =
CALCULATE(
    [Total Shipments],
    fact_shipments[is_delivered] = 1
)
```

```DAX
On-Time Delivery % =
DIVIDE(
    SUM(fact_shipments[is_on_time]),
    [Delivered Shipments]
)
```

```text
Gold
→ jednotná business logika na úrovni zásilky

Power BI
→ dynamické agregace podle filtračního kontextu
```

---

## Úkol 13 — Každodenní automatizace

Navrhni pořadí každodenního datového procesu.

## Úkol 13 — řešení

Jedna hlavní Fabric pipeline bude každý den řídit proces ve správném pořadí:
```text
1. kontrola dostupnosti zdrojů
2. načtení dat do Bronze
3. vytvoření Silver dat
4. kontroly datové kvality
5. aktualizace Gold tabulek
6. obnovení Power BI sémantického modelu
7. zaznamenání výsledku běhu
```

Jednotlivé kroky budou mít nastavené závislosti. Pokud selže Silver transformace nebo kontrola kvality, pipeline nebude pokračovat aktualizací Gold vrstvy a Power BI.

Každodenní schedule bude nastaven na dobu, kdy již mají být dostupná všechna zdrojová data.

---

## Úkol 14 — Monitoring řešení

Navrhni technické a datové kontroly po každém běhu pipeline.

## Úkol 14 — řešení

### Technický monitoring

- stav spuštění pipeline;
- úspěšnost jednotlivých kroků;
- délka jednotlivých operací;
- chyby připojení nebo gateway;
- úspěšnost obnovení Power BI modelu.

### Monitoring dat

- aktuálnost dat;
- počty načtených zásilek;
- počet duplicit;
- počet chybějících vazeb;
- počet odmítnutých nebo označených záznamů;
- odsouhlasení Gold výsledků proti zdrojům.

```text
Pipeline skončila úspěšně
≠
data jsou automaticky správná
```

---

## Úkol 15 — Kapacita a náklady

Navrhni základní způsob kontroly využití cloudových prostředků.

## Úkol 15 — řešení

Firma již Fabric Capacity používá. Nejdříve proto ověříme, zda stávající kapacita zvládne nové řešení.

Budeme sledovat:
- využití sdílené Fabric Capacity;
- délku pipeline a transformací;
- náročnost notebooků a SQL dotazů;
- obnovování Power BI modelů;
- objem uložených dat;
- zbytečné kopie a dobu uchovávání dat;
- celkové související náklady v Azure.

Doporučený postup:
```text
změřit využití
→ najít náročné operace
→ optimalizovat
→ teprve potom případně zvýšit kapacitu
```

Bez konkrétních údajů o objemu dat, době běhu a souběhu uživatelů nelze určit potřebnou velikost kapacity ani přesné náklady.

---

## Úkol 16 — Kdy zvolit samostatné Azure služby

Urči situace, ve kterých by mohla být vhodnější původně nedoporučená Azure varianta.

## Úkol 16 — řešení

Samostatné Azure služby mohou být vhodnější, pokud firma potřebuje:
- velmi individuální architekturu;
- samostatně škálovat jednotlivé komponenty;
- detailní síťovou a bezpečnostní konfiguraci;
- kombinovat více specializovaných technologií;
- provádět velmi rozsáhlé nebo specifické Spark transformace;
- navázat na již existující rozsáhlé Azure datové prostředí;
- využít zkušený cloudový a data engineering tým.

Azure řešení poskytuje větší technickou flexibilitu, ale současně přináší více integrace, konfigurace a provozní správy.

---

## Porovnání variant

### Samostatné služby Microsoft Azure

Výhody:
- vysoká technická flexibilita;
- nezávislé škálování jednotlivých služeb;
- široké možnosti síťové a bezpečnostní konfigurace;
- vhodné pro rozsáhlá nebo již existující Azure prostředí.

Nevýhody:
- více samostatných služeb k propojení;
- vyšší nároky na cloudové a data engineering znalosti;
- složitější správa oprávnění, monitoringu a nákladů;
- vyšší provozní náročnost pro malý tým.

### Microsoft Fabric

Výhody:
- integrované analytické prostředí;
- společná úložná vrstva OneLake;
- přímá návaznost na Power BI;
- využití existující Fabric Capacity;
- dobrá návaznost na SQL a Power Query;
- menší počet samostatně spravovaných komponent.

Nevýhody:
- menší volnost při návrhu velmi individuální architektury;
- sdílená kapacita musí být monitorována;
- nevhodná velikost Capacity může ovlivnit více workloadů;
- před migrací je nutné ověřit podporu konkrétních požadavků a zdrojů.

---

## Doporučené řešení

Pro popsanou logistickou společnost doporučujeme Microsoft Fabric.

Výsledná architektura:

```text
lokální SQL Server
→ on-premises data gateway
                         ↘
CSV + JSON + Excel → Fabric Data Factory
                         ↓
                Lakehouse Bronze
                         ↓
                Lakehouse Silver
                         ↓
                 Warehouse Gold
                         ↓
              Power BI semantic model
                         ↓
             management dashboard
```

Napříč řešením:

```text
Microsoft Entra ID
→ identity a skupiny

Fabric permissions
→ přístup k workspaces, položkám a datům

Fabric pipeline
→ každodenní orchestrace

Monitoring
→ technický stav a datová kvalita

Fabric Capacity
→ sdílený výpočetní výkon
```

## Zdůvodnění rozhodnutí

Fabric nejlépe odpovídá existujícím technologiím, dovednostem a velikosti týmu. Umožňuje pokrýt načítání, ukládání, transformace, analytický warehouse i reporting v jednom prostředí a omezuje množství samostatných služeb, které musí firma spravovat.

Rozhodnutí není založeno na předpokladu, že Fabric je vždy levnější nebo technicky vhodnější než Azure. Přesné náklady a potřebnou velikost kapacity nelze bez provozních údajů určit. Před produkční implementací je nutné provést pilotní ověření výkonu, datových objemů a spotřeby kapacity.

## Kontrola splnění požadavků

- SQL Server je načítán přes on-premises data gateway.
- CSV, JSON a Excel jsou načítány prostřednictvím Fabric Data Factory.
- Původní data jsou uchována v Bronze vrstvě.
- Silver vrstva zajišťuje čištění a validaci.
- Gold vrstva obsahuje business-ready hvězdicové schéma.
- Power BI je připojeno prostřednictvím sémantického modelu.
- Identity a přístupy řídí Microsoft Entra ID a Fabric permissions.
- Každodenní proces řídí plánovaná Fabric pipeline.
- Monitoring zahrnuje technický stav i datovou kvalitu.
- Využití Fabric Capacity a náklady jsou pravidelně kontrolovány.
- Prostředí jsou oddělena do DEV, TEST a PROD workspaces.

## Závěr

Microsoft Fabric představuje pro zadanou společnost praktičtější variantu, protože navazuje na stávající Power BI prostředí, dostupnou Fabric Capacity a znalosti analytického týmu.

Samostatná Azure architektura zůstává platnou alternativou pro situaci, kdy by společnost později potřebovala větší technickou flexibilitu, nezávislé škálování jednotlivých služeb nebo velmi specializované cloudové řešení.