# Cloud Platforms Cheatsheet

Praktický přehled cloudových principů a analytických služeb v Microsoft Azure, Microsoft Fabric, AWS a Google Cloud. Hlavní důraz je kladen na Azure a Fabric.

---

# 0. Obecný základ cloudového prostředí

## Co je cloudová služba

Cloudová služba je výpočetní prostředek poskytovaný prostřednictvím sítě. Může jít například o úložiště, databázi, výpočetní výkon, datovou pipeline nebo hotovou analytickou aplikaci.

Uživatel obvykle pracuje s logickým prostředkem vytvořeným v cloudové platformě. Fyzické servery, disky a datová centra provozuje cloudový poskytovatel.

```text
cloudový poskytovatel
→ provozuje fyzickou infrastrukturu

zákaznická organizace
→ vytváří a konfiguruje cloudové služby
→ spravuje data, uživatele a oprávnění
```

## Základní role v cloudové datové architektuře

Jednotlivé cloudové služby mají v datovém toku odlišné role.

### Datový zdroj

Místo, kde data původně vznikají:

- provozní databáze;
- podniková aplikace;
- soubor;
- API;
- externí služba;
- zařízení nebo aplikační log.

### Ingestce

**Ingestce** znamená získání a načtení dat ze zdroje do datové platformy.

Může probíhat:

- dávkově podle plánu;
- průběžně pomocí streamingu;
- úplným načtením;
- přírůstkově pouze pro nová nebo změněná data.

### Úložiště

Uchovává data. Může jít například o:

- objektové úložiště;
- data lake;
- relační databázi;
- analytický warehouse;
- lakehouse.

### Výpočetní a transformační vrstva

Provádí nad daty výpočty:

- čištění;
- validaci;
- joiny;
- agregace;
- technické transformace;
- business transformace;
- machine learning.

### Orchestrace

Řídí pořadí, časování a závislosti jednotlivých datových kroků.

### Serving layer

**Serving layer** je vrstva, která zpřístupňuje připravená data analytickým nástrojům a uživatelům. Může ji tvořit například Gold tabulka, SQL endpoint, warehouse nebo datamart.

### Sémantická a reportovací vrstva

Převádí technická data do business podoby pomocí vztahů, metrik, KPI a uživatelsky srozumitelných názvů. Výsledkem jsou reporty a dashboardy.

Celý tok:

```text
datový zdroj
→ ingestce
→ úložiště
→ transformace
→ serving layer
→ sémantický model
→ report a rozhodnutí
```

Jedna služba může pokrývat více rolí, ale při návrhu je stále důležité vědět, kterou roli právě plní.

## Typy cloudového nasazení

### Public cloud

Služby poskytuje veřejný cloudový poskytovatel, například Microsoft Azure, AWS nebo Google Cloud. Zákazník používá logicky oddělené prostředky na infrastruktuře poskytovatele.

### Private cloud

Cloudové principy jsou použity v prostředí vyhrazeném jedné organizaci.

### Hybrid cloud

Kombinuje vlastní infrastrukturu a veřejný cloud.

Příklad:

```text
lokální SQL Server
→ cloudová datová pipeline
→ cloudový lakehouse nebo warehouse
→ Power BI Service
```

### Multi-cloud

Organizace používá služby více cloudových poskytovatelů, například Azure i AWS. To může přinést flexibilitu, ale také složitější integraci, zabezpečení a řízení nákladů.

## Managed service

**Managed service** je služba, u které cloudový poskytovatel přebírá část technické správy.

U spravované databáze poskytovatel typicky zajišťuje větší část provozu platformy než u databáze nainstalované na vlastním virtuálním počítači.

Zákazník však stále odpovídá například za:

- data a jejich kvalitu;
- uživatele a oprávnění;
- způsob použití databáze;
- část konfigurace;
- kontrolu nákladů.

## Serverless

**Serverless** neznamená, že služba nepoužívá servery. Znamená, že zákazník servery přímo nezřizuje a nespravuje.

Platforma podle konkrétní služby zajišťuje například:

- přidělení výpočetních prostředků;
- škálování;
- provoz infrastruktury;
- účtování podle kapacity, času nebo provedených operací.

Serverless může zjednodušit správu, ale stále je nutné sledovat výkon a náklady.

## Scalability a elasticity

### Scalability

**Scalability** neboli škálovatelnost znamená schopnost zvýšit nebo snížit dostupný výkon.

```text
scale up
→ výkonnější jeden prostředek

scale out
→ více výpočetních jednotek
```

### Elasticity

**Elasticity** neboli pružnost znamená schopnost přizpůsobovat kapacitu měnící se zátěži, často automaticky.

```text
vyšší zátěž
→ dočasné navýšení kapacity

nižší zátěž
→ snížení kapacity
```

Škálování řeší velikost výkonu. Elasticita zdůrazňuje jeho pružné přizpůsobování v čase.

## Region, availability zone a datová rezidence

### Region

Geografická oblast, ve které cloudový poskytovatel provozuje služby.

### Availability zone

**Availability zone** je fyzicky oddělená část regionu s vlastní podpůrnou infrastrukturou. Podpora zón závisí na konkrétní službě a regionu.

### Data residency

**Data residency** označuje požadavek, ve které geografické oblasti mají být data uložena nebo zpracovávána.

Volba umístění může ovlivnit:

- odezvu;
- dostupnost;
- regulatorní požadavky;
- možnosti obnovy;
- náklady na přenos dat.

## High availability a disaster recovery

### High availability

**High availability** znamená návrh služby tak, aby zůstala dostupná i při části technických výpadků.

### Disaster recovery

**Disaster recovery** řeší obnovení služby a dat při závažném výpadku.

### Replikace a záloha

```text
replikace
→ další kopie pro dostupnost a odolnost

záloha
→ možnost vrátit se k dřívějšímu stavu
```

Replikace není totéž jako záloha. Nechtěná změna nebo odstranění dat může být přeneseno také do repliky.

## Identity, authentication a authorization

### Identity

Určuje uživatele, skupinu, aplikaci nebo automatizovaný proces.

### Authentication

**Authentication** neboli autentizace ověřuje:

```text
Kdo jsi?
```

### Authorization

**Authorization** neboli autorizace určuje:

```text
Co smíš dělat
a nad kterým prostředkem?
```

Správné přihlášení tedy ještě neznamená, že uživatel může číst nebo měnit všechna data.

## Základní principy zabezpečení

### Least privilege

Uživatel nebo aplikace získá pouze nejnižší oprávnění potřebné pro svou práci.

### Role-based access control

Oprávnění jsou přidělována prostřednictvím rolí, ideálně skupinám nebo pracovním identitám místo jednotlivých uživatelů.

### Separation of duties

**Separation of duties** znamená rozdělení citlivých odpovědností mezi více rolí. Jeden uživatel by neměl bez kontroly současně vytvářet, schvalovat a publikovat kritické změny.

### Secrets management

Hesla, API klíče a další tajné údaje se neukládají přímo do zdrojového kódu nebo veřejného repozitáře.

## Síťová vrstva

Přístup ke cloudové službě vyžaduje:

```text
správné oprávnění
+ povolenou síťovou cestu
```

Základní pojmy:

- **public endpoint** — přístup přes veřejnou síťovou adresu podle nastavených pravidel;
- **private endpoint** — privátní přístup uvnitř řízené sítě;
- **firewall rule** — pravidlo povolující nebo blokující síťový provoz;
- **VPN** — zabezpečené propojení uživatele nebo sítě s cloudovým prostředím;
- **gateway** — prostředník mezi lokálním a cloudovým prostředím.

Správná uživatelská role sama o sobě nevyřeší chybějící síťové připojení.

## Základní model nákladů

Cloudové náklady mohou vznikat za:

- množství uložených dat;
- výpočetní výkon;
- dobu běhu služby;
- počet dotazů nebo operací;
- přenos dat;
- zálohování a replikaci;
- licence;
- rezervovanou nebo sdílenou kapacitu.

### Pay-as-you-go

Platba podle skutečného používání nebo doby běhu služby.

### Reserved capacity

Kapacita rezervovaná na delší období může být vhodná pro stabilní a předvídatelnou zátěž.

### Egress

**Egress** znamená přenos dat z cloudového prostředí ven. U některých scénářů může být zpoplatněn.

Praktické pravidlo:

```text
úložiště uchovává data
→ výpočet je zpracovává
→ přenos je přesouvá
→ každá část může mít vlastní způsob účtování
```

## Prostředí DEV, TEST a PROD

### DEV

Vývoj a první ověřování změn.

### TEST

Kontrola funkčnosti, datové kvality a dopadu před nasazením.

### PROD

Produkční prostředí používané uživateli a navazujícími procesy.

```text
DEV
→ TEST
→ schválení
→ PROD
```

Změny se nemají bez ověření provádět přímo v produkčním prostředí.

## Co má umět datový analytik

Datový analytik nemusí cloudovou infrastrukturu kompletně administrovat. Měl by však rozumět:

- kde jsou data uložena;
- jak se k nim připojí;
- zda používá Import, DirectQuery nebo jiný režim;
- kde probíhá SQL dotaz nebo transformace;
- která služba data přesouvá;
- kdo nastavuje oprávnění;
- proč může být potřebná gateway nebo VPN;
- která operace může vytvářet náklady;
- zda pracuje ve vývojovém nebo produkčním prostředí;
- komu oznámit problém s přístupem, kvalitou nebo výkonem.

---

# 1. Co znamená cloud computing

**Cloud computing** znamená využívání výpočetních prostředků poskytovatele prostřednictvím sítě místo provozování veškeré infrastruktury ve vlastní organizaci.

Cloud může poskytovat například:

- datová úložiště;
- databáze;
- virtuální počítače;
- analytické platformy;
- datové pipeline;
- nástroje pro reporting;
- správu identit a zabezpečení.

## On-premises

**On-premises** prostředí provozuje organizace ve vlastní infrastruktuře.

Organizace typicky zajišťuje:

- nákup a provoz serverů;
- fyzické zabezpečení;
- operační systémy a aktualizace;
- dostupnost a zálohování;
- výkon a kapacitu;
- síťovou infrastrukturu.

## Cloud

V cloudovém prostředí fyzickou infrastrukturu provozuje poskytovatel. Zákazník si vytváří a konfiguruje konkrétní cloudové služby.

Výhody mohou zahrnovat:

- rychlejší vytvoření prostředí;
- pružné škálování;
- platbu podle zvoleného modelu a spotřeby;
- menší potřebu správy fyzického hardwaru;
- dostupnost spravovaných služeb.

Cloud ale není automaticky levnější ani bezpečný bez správné konfigurace.

---

# 2. IaaS, PaaS a SaaS

## IaaS — Infrastructure as a Service

Poskytovatel spravuje fyzickou infrastrukturu, ale zákazník spravuje například operační systém, aplikace a data.

Příklad:

```text
Azure Virtual Machine
Amazon EC2
Google Compute Engine
```

## PaaS — Platform as a Service

Poskytovatel spravuje také operační systém a platformu. Zákazník se soustředí hlavně na konfiguraci, aplikace, data a oprávnění.

Příklad:

```text
Azure SQL Database
Amazon RDS
Google Cloud SQL
```

## SaaS — Software as a Service

Uživatel používá hotovou aplikaci poskytovanou jako službu.

Příklad:

```text
Power BI Service
Microsoft Fabric
```

Zjednodušené pravidlo:

```text
IaaS
→ zákazník spravuje větší část prostředí

PaaS
→ poskytovatel přebírá více technického provozu

SaaS
→ uživatel pracuje s hotovou cloudovou platformou nebo aplikací
```

---

# 3. Čtyři porovnávaná prostředí

## Microsoft Azure

Azure je obecná cloudová platforma. Nabízí samostatné služby pro úložiště, databáze, integraci, výpočty, sítě, zabezpečení, analytiku a další oblasti.

## Microsoft Fabric

Fabric je integrovaná SaaS analytická platforma Microsoftu. Sdružuje datovou integraci, data engineering, lakehouse, warehouse, data science a Power BI.

Fabric není čtvrtý samostatný globální cloudový poskytovatel na stejné úrovni jako Azure, AWS a Google Cloud. Je to analytická platforma ekosystému Microsoft, jejíž kapacita může být spravována a účtována prostřednictvím Azure.

## Amazon Web Services — AWS

AWS je obecná cloudová platforma společnosti Amazon. Poskytuje samostatné služby pro infrastrukturu, ukládání dat, databáze, analytiku, integraci a zabezpečení.

## Google Cloud Platform — GCP

Google Cloud je obecná cloudová platforma společnosti Google. V analytice je známá zejména službami Cloud Storage, BigQuery, Dataflow a Looker.

---

# 4. Mapa analytických služeb

Jednotlivé služby nejsou vždy přesnými technickými náhradami. Následující přehled je porovnává podle jejich hlavní role v datovém řešení.

## Objektové úložiště a data lake

- **Microsoft Azure:** Blob Storage nebo ADLS Gen2;
- **Microsoft Fabric:** OneLake;
- **AWS:** Amazon S3;
- **Google Cloud:** Cloud Storage.

## Spravovaná relační databáze

- **Microsoft Azure:** Azure SQL Database;
- **Microsoft Fabric:** SQL Database in Fabric;
- **AWS:** Amazon RDS;
- **Google Cloud:** Cloud SQL.

## Analytický warehouse

- **Microsoft Azure:** Azure Synapse Analytics;
- **Microsoft Fabric:** Fabric Warehouse;
- **AWS:** Amazon Redshift;
- **Google Cloud:** BigQuery.

## Datová integrace a pipeline

- **Microsoft Azure:** Azure Data Factory;
- **Microsoft Fabric:** Fabric Data Factory;
- **AWS:** AWS Glue;
- **Google Cloud:** Dataflow nebo Cloud Data Fusion.

## Spark a lakehouse

- **Microsoft Azure:** Azure Databricks;
- **Microsoft Fabric:** Fabric Lakehouse a Fabric Notebook;
- **AWS:** Databricks on AWS nebo Amazon EMR;
- **Google Cloud:** Databricks on Google Cloud nebo Dataproc.

## Identity a oprávnění

- **Microsoft Azure:** Microsoft Entra ID a Azure RBAC;
- **Microsoft Fabric:** Microsoft Entra ID a Fabric permissions;
- **AWS:** AWS IAM;
- **Google Cloud:** Cloud IAM.

## Správa tajných údajů

- **Microsoft Azure:** Azure Key Vault;
- **Microsoft Fabric:** správa připojení, pracovních identit a integrace s Azure službami;
- **AWS:** AWS Secrets Manager;
- **Google Cloud:** Secret Manager.

## Business intelligence

- **Microsoft Azure:** Power BI;
- **Microsoft Fabric:** Power BI jako součást platformy;
- **AWS:** Amazon QuickSight;
- **Google Cloud:** Looker.

## Monitoring nákladů a kapacity

- **Microsoft Azure:** Azure Cost Management;
- **Microsoft Fabric:** Fabric Capacity Metrics a Azure Cost Management;
- **AWS:** AWS Cost Explorer;
- **Google Cloud:** Cloud Billing.

---

# 5. Organizace prostředků v Azure

Azure používá několik úrovní správy.

```text
management group
→ subscription
→ resource group
→ resource
```

Identity organizace jsou spravovány v Microsoft Entra tenantovi. Struktura identit a struktura Azure prostředků spolu souvisejí, ale nejsou totožné.

## Microsoft Entra tenant

Obsahuje například:

- uživatele;
- skupiny;
- aplikační identity;
- pravidla přihlašování;
- identity používané pro přístup ke cloudovým službám.

## Management group

Seskupuje více subscriptions a umožňuje nad nimi společně uplatňovat pravidla a správu.

## Subscription

Subscription představuje základní rámec pro:

- používání Azure služeb;
- účtování spotřeby;
- přístupová oprávnění;
- limity prostředků;
- oddělení prostředí nebo organizačních částí.

## Resource group

Resource group je logický kontejner pro související Azure resources.

Příklad:

```text
rg-logistics-analytics-prod

- Storage Account
- Azure Data Factory
- Azure SQL Database
- Azure Key Vault
```

## Resource

Resource je konkrétní vytvořená služba, například databáze, Storage Account nebo Data Factory.

---

# 6. Azure regiony

**Region** je geografická oblast, ve které Azure provozuje své služby.

Volba regionu může ovlivnit:

- síťovou odezvu;
- umístění dat;
- dostupnost služeb;
- regulatorní požadavky;
- možnosti obnovy při výpadku;
- ceny a náklady na přenos dat.

Hlavní datové a výpočetní služby jednoho řešení bývá praktické umístit blízko sebe, pokud jiné požadavky neurčí odlišný návrh.

```text
data ve vzdáleném regionu
a výpočet v jiném regionu
→ vyšší latence
→ možné náklady na přenos dat
```

Resource group má vlastní evidované umístění, ale resources uvnitř ní mohou být vytvořeny v různých regionech.

---

# 7. Azure Storage Account, Blob Storage a ADLS Gen2

## Storage Account

Storage Account je Azure resource zastřešující data uložená ve službách Azure Storage.

Zjednodušená struktura pro objektová data:

```text
Storage Account
→ container
→ blob neboli uložený objekt
```

## Azure Blob Storage

Objektové úložiště pro textová a binární data, například:

- CSV;
- JSON;
- Parquet;
- logy;
- obrázky;
- zálohy.

## Azure Data Lake Storage Gen2

ADLS Gen2 využívá schopnosti Azure Storage Accountu s aktivovaným **hierarchical namespace**.

Hierarchical namespace poskytuje adresáře a soubory vhodné pro analytické enginy a rozsáhlé zpracování dat.

Příklad:

```text
logistics-data/
├── bronze/
├── silver/
└── gold/
```

ADLS Gen2 data ukládá. Samo o sobě neurčuje kvalitu dat ani neprovádí všechny transformace.

---

# 8. Azure SQL Database

Azure SQL Database je spravovaná relační databázová služba typu PaaS.

Je vhodná, když potřebujeme:

- relační tabulky;
- SQL dotazy;
- primární a cizí klíče;
- transakční zpracování;
- databázi bez správy vlastního serveru a operačního systému;
- menší nebo běžné analytické řešení nad strukturovanými daty.

```text
Azure SQL Database
→ relační databáze

ADLS Gen2
→ souborové a objektové úložiště
```

---

# 9. Azure Data Factory

Azure Data Factory, zkráceně **ADF**, slouží pro datovou integraci a řízení datových pipeline.

Základní pojmy:

- **Pipeline** — celý datový proces;
- **Activity** — jeden konkrétní krok;
- **Copy Activity** — kopírování dat;
- **Trigger** — určuje, kdy se pipeline spustí;
- **Linked Service** — připojení ke zdroji, cíli nebo výpočetní službě;
- **Dataset** — popis dat používaných aktivitou.

Příklad:

```text
lokální SQL Server
→ Azure Data Factory
→ ADLS Bronze
→ spuštění Databricks notebooku
→ ADLS Silver a Gold
```

ADF není primárně datové úložiště. Přesouvá data, spouští kroky a koordinuje proces.

---

# 10. Azure Synapse Analytics

Azure Synapse Analytics je platforma pro datové sklady a rozsáhlejší analytické zpracování.

Může zahrnovat:

- analytické SQL;
- distribuované zpracování dotazů;
- dotazování nad daty v Azure Storage;
- datové pipeline;
- Spark zpracování.

Zjednodušeně:

```text
Azure SQL Database
→ obecná relační databáze

Azure Synapse Analytics
→ rozsáhlejší analytická platforma a datový sklad
```

---

# 11. Azure Databricks

Azure Databricks je analytická platforma používaná pro:

- data engineering;
- rozsáhlé transformace;
- Spark a PySpark;
- SQL analytiku;
- lakehouse architekturu;
- data science a machine learning.

V jednoduché Azure architektuře:

```text
ADLS Gen2
→ ukládá data

Azure Databricks
→ data načítá a transformuje

Azure Data Factory
→ proces spouští a koordinuje
```

Pokud běžné čištění spolehlivě zvládne Power Query nebo SQL, není nutné automaticky zavádět Spark či Databricks.

---

# 12. Microsoft Fabric

Microsoft Fabric sjednocuje více analytických workloadů do jedné SaaS platformy.

Mezi hlavní části patří:

- Data Factory;
- Data Engineering;
- Lakehouse;
- Data Warehouse;
- Data Science;
- Real-Time Intelligence;
- Power BI.

Fabric je vhodný zejména pro organizace, které chtějí úzce propojit datovou přípravu, analytiku a Power BI v jednom prostředí.

---

# 13. OneLake

**OneLake** je jednotné datové úložiště organizace v Microsoft Fabric.

Zjednodušeně:

```text
Azure architektura
→ samostatně vytvořený ADLS Gen2

Fabric architektura
→ OneLake jako společná datová vrstva platformy
```

OneLake poskytuje společný základ pro Fabric Lakehouse, Warehouse a další analytické položky.

---

# 14. Fabric workspace

**Workspace** je pracovní prostor pro související Fabric položky.

Příklad:

```text
Logistics Analytics workspace

- Lakehouse
- Data Pipeline
- Dataflow Gen2
- Notebook
- Warehouse
- Semantic Model
- Power BI Report
```

Workspace pomáhá řídit:

- organizaci řešení;
- spolupráci;
- oprávnění;
- přiřazení ke kapacitě;
- oddělení prostředí.

Doporučené oddělení:

```text
DEV
→ vývoj

TEST
→ ověření

PROD
→ produkční používání
```

---

# 15. Fabric Data Factory

Fabric Data Factory slouží pro načítání, transformaci a orchestraci dat ve Fabricu.

## Data Pipeline

Řídí pořadí jednotlivých datových kroků.

## Copy Activity

Kopíruje data ze zdroje do cíle.

## Dataflow Gen2

Používá Power Query Online a je vhodný pro low-code transformace:

- přejmenování sloupců;
- změny datových typů;
- filtrování;
- spojování tabulek;
- nahrazování a standardizaci hodnot;
- běžná business pravidla.

---

# 16. Fabric Notebook a SQL

## Fabric Notebook

Notebook je vhodný pro:

- Spark úlohy;
- programové transformace;
- PySpark;
- složitější nebo rozsáhlé zpracování;
- data science a machine learning.

## SQL

T-SQL ve Fabric Warehouse nebo SQL analytics endpointu je vhodný pro:

- joiny;
- agregace;
- relační transformace;
- business logiku;
- vytváření analytických tabulek.

Doporučená volba:

```text
jednoduché čištění známé z Power Query
→ Dataflow Gen2

relační transformace a agregace
→ SQL

složitá programová nebo Spark logika
→ Notebook
```

---

# 17. Fabric Lakehouse a Warehouse

## Fabric Lakehouse

Kombinuje data lake s možnostmi řízených tabulek, SQL a Sparku.

Lze v něm uchovávat například:

- původní soubory;
- Bronze, Silver a Gold data;
- strukturovaná i nestrukturovaná data;
- Delta tabulky.

## Fabric Warehouse

Je zaměřený na strukturovaná analytická data a práci pomocí T-SQL.

Je vhodný například pro Gold hvězdicové schéma:

```text
fact_deliveries
dim_carrier
dim_warehouse
dim_route
dim_date
```

Možné návrhy:

```text
Lakehouse Bronze
→ Lakehouse Silver
→ Lakehouse Gold
→ Power BI
```

nebo:

```text
Lakehouse Bronze
→ Lakehouse Silver
→ Fabric Warehouse Gold
→ Power BI
```

Gold vrstva nemusí být vždy ve Warehouse. Rozhoduje způsob práce a potřeby analytického týmu.

---

# 18. Power BI ve Fabricu

Power BI je součástí Microsoft Fabric a zajišťuje analytickou a reportovací vrstvu.

## Sémantický model

Obsahuje například:

- vztahy mezi tabulkami;
- DAX míry;
- hierarchie;
- formátování;
- business názvy;
- row-level security.

```text
Lakehouse nebo Warehouse
→ data

sémantický model
→ vztahy, DAX a business význam

Power BI report
→ vizualizace a rozhodování
```

## Režimy připojení

### Import

Data se při refreshi načtou do Power BI sémantického modelu.

### DirectQuery

Power BI posílá dotazy do zdroje při práci s reportem.

### Direct Lake

Power BI pracuje s Delta tabulkami uloženými v OneLake bez klasického importu celého datasetu.

---

# 19. Azure vs. Microsoft Fabric

## Samostatné Azure služby

```text
Azure Data Factory
ADLS Gen2
Azure Databricks
Azure Synapse Analytics
Power BI
```

Výhody:

- větší volnost při výběru technologií;
- samostatné škálování komponent;
- vhodné pro složité a rozsáhlé architektury;
- možnost kombinovat více specializovaných služeb.

Nevýhody:

- více služeb k propojení;
- složitější oprávnění a síťová konfigurace;
- náklady se sledují napříč více resources;
- vyšší technická a provozní náročnost.

## Microsoft Fabric

```text
Fabric Data Factory
OneLake
Lakehouse
Warehouse
Power BI
```

Výhody:

- integrované SaaS prostředí;
- těsné propojení s Power BI;
- společná datová vrstva OneLake;
- jednodušší spolupráce analytického a BI týmu;
- menší potřeba skládat samostatné Azure služby.

Nevýhody:

- jednotlivé workloads sdílejí Fabric Capacity;
- je nutné sledovat spotřebu společné kapacity;
- specializované požadavky mohou vyžadovat další Azure služby;
- organizace je více vázaná na integrovaný Fabric způsob práce.

## Kdy preferovat Fabric

- organizace intenzivně používá Power BI;
- tým zná SQL a Power Query;
- cílem je jednotná analytická SaaS platforma;
- organizace má nebo plánuje Fabric Capacity;
- jednodušší správa je důležitější než maximální technická volnost.

## Kdy preferovat samostatné Azure služby

- řešení vyžaduje specializovanou infrastrukturu;
- jednotlivé komponenty se musí nezávisle škálovat;
- organizace již provozuje rozsáhlé Azure datové prostředí;
- existují pokročilé síťové nebo integrační požadavky;
- tým potřebuje kombinovat více technologií mimo Fabric.

---

# 20. Identity a oprávnění

## Microsoft Entra ID

Odpovídá na otázku:

```text
Kdo se přihlašuje?
```

Identitou může být uživatel, skupina, aplikace nebo automatizovaný proces.

## Azure RBAC

Odpovídá na otázku:

```text
Co smí identita dělat
a nad kterým prostředkem?
```

## Service principal

Identita aplikace nebo automatizovaného procesu v Entra tenantovi.

## Managed identity

Identita Azure služby, jejíž přihlašovací údaje spravuje Azure.

Příklad:

```text
Azure Data Factory
→ managed identity
→ Azure RBAC
→ zápis do ADLS Bronze
```

## Least privilege

Identita dostane pouze oprávnění, která skutečně potřebuje.

```text
analytik
→ čtení Gold dat

pipeline
→ zápis do Bronze

data engineer
→ transformace Silver a Gold
```

---

# 21. Azure Key Vault a tajné údaje

Azure Key Vault slouží k bezpečnému ukládání:

- hesel;
- API klíčů;
- connection secrets;
- šifrovacích klíčů;
- certifikátů.

Nevhodně:

```python
password = "MojeTajneHeslo123"
```

Vhodněji:

```text
secret
→ Azure Key Vault
→ oprávněná aplikace jej načte při spuštění
```

Pokud je to možné, managed identity umožní nepoužívat uložené heslo vůbec.

---

# 22. Síťový přístup

Oprávnění a síťový přístup jsou dvě samostatné podmínky.

```text
autorizace
→ smím operaci provést?

síťový přístup
→ mohu se ke službě připojit?
```

Azure služba může být podle konfigurace dostupná:

- přes veřejný endpoint;
- pouze z povolených sítí;
- přes private endpoint uvnitř virtuální sítě.

Uživatel může mít správnou Azure RBAC roli, ale spojení nemusí fungovat, pokud nemá přístup k požadované síti nebo VPN.

---

# 23. On-premises data gateway

Gateway funguje jako bezpečný most mezi lokálními zdroji a cloudovými službami Microsoftu.

Příklad:

```text
lokální SQL Server
→ on-premises data gateway
→ Power BI Service nebo Microsoft Fabric
```

Počítač nebo server s gateway musí:

- mít přístup k lokálnímu zdroji;
- být dostupný během aktualizace;
- mít potřebné ovladače a konfiguraci;
- používat správně nastavené přihlašovací údaje.

Gateway není režim uložení dat a běžně neslouží jako jejich trvalé úložiště.

---

# 24. Shared responsibility model

**Shared responsibility model** znamená sdílenou odpovědnost mezi poskytovatelem a zákazníkem.

Poskytovatel odpovídá například za:

- fyzická datová centra;
- fyzické servery;
- základní cloudovou infrastrukturu;
- fyzické zabezpečení platformy.

Zákazník nadále odpovídá například za:

- uživatelské účty;
- přidělená oprávnění;
- klasifikaci a ochranu dat;
- bezpečnou konfiguraci;
- transformační logiku;
- kvalitu dat;
- řízení nákladů.

Ani u SaaS poskytovatel nerozhoduje, kterým zaměstnancům má organizace povolit přístup ke konkrétním obchodním datům.

---

# 25. Náklady v Azure a Fabricu

## Azure

Náklady mohou vznikat za:

- uložená data;
- výpočetní výkon;
- dobu běhu služby;
- počet některých operací;
- přenos dat mezi regiony nebo ven z cloudu;
- databázový výkonový tier;
- licence a rezervovanou kapacitu;
- zálohy a replikaci.

## Fabric

Fabric workloads sdílejí výpočetní kapacitu vyjádřenou pomocí **Capacity Units — CUs**.

Kapacitu mohou současně spotřebovávat například:

- pipeline;
- Dataflow Gen2;
- notebooky;
- Warehouse;
- Power BI dotazy.

OneLake storage se sleduje odděleně od výpočetní spotřeby kapacity.

Při přetížení může dojít k **throttlingu**, tedy dočasnému omezení výkonu.

Praktické optimalizace:

- plánovat náročné transformace mimo špičku;
- vypínat nebo pozastavovat nepotřebný výpočet;
- nastavit automatické ukončení neaktivních clusterů;
- vybírat pouze potřebné sloupce a řádky;
- sledovat Fabric Capacity Metrics;
- používat rozpočty a upozornění;
- kontrolovat přenosy mezi regiony.

---

# 26. Tags

Azure tags jsou štítky ve formátu klíč–hodnota.

Příklad:

```text
environment = production
project = logistics-analytics
department = analytics
owner = data-team
cost-center = CC-120
```

Tags pomáhají:

- identifikovat vlastníka;
- rozlišit DEV, TEST a PROD;
- seskupovat náklady;
- dohledat účel služby;
- organizovat cloudové prostředky.

Tag sám o sobě nezabezpečuje data ani automaticky nezastavuje náklady.

---

# 27. Škálování

## Scale up

Zvýšení výkonu jednoho prostředku:

```text
více CPU
více paměti
výkonnější databázová úroveň
```

## Scale out

Rozdělení zátěže mezi více výpočetních jednotek:

```text
více workers
více uzlů
více paralelních instancí
```

## Autoscaling

Platforma automaticky upravuje množství prostředků podle nastavených pravidel.

Vyšší výkon může zrychlit zpracování, ale obvykle zvyšuje náklady.

---

# 28. Dostupnost, replikace a zálohování

## Dostupnost

Schopnost služby zůstat použitelná nebo se rychle obnovit při technickém problému.

## Replikace

Vytváření dalších kopií dat kvůli dostupnosti nebo odolnosti.

## Záloha

Umožňuje obnovit dřívější stav dat.

Replikace není totéž jako záloha. Nechtěná změna může být replikována také do další kopie.

---

# 29. Organizace AWS a GCP

## AWS

Zjednodušená organizační struktura:

```text
AWS Organization
→ AWS Account
→ Region
→ Resource
```

Klíčové služby pro analytiku:

- Amazon S3 — objektové úložiště;
- Amazon RDS — spravované relační databáze;
- AWS Glue — datová integrace, ETL a Data Catalog;
- Amazon Redshift — cloudový datový sklad;
- AWS IAM — identity, role a oprávnění;
- Amazon QuickSight — BI a vizualizace.

## Google Cloud

Zjednodušená organizační struktura:

```text
Organization
→ Folder
→ Project
→ Resource
```

Klíčové služby pro analytiku:

- Cloud Storage — objektové úložiště;
- Cloud SQL — spravované relační databáze;
- BigQuery — serverless analytická platforma a datový sklad;
- Dataflow — dávkové a streamové datové zpracování;
- Cloud Data Fusion — vizuální datová integrace;
- Cloud IAM — identity a oprávnění;
- Looker — BI a analytika.

---

# 30. Překlad pracovních inzerátů

## Příklad Azure

```text
Data stored in ADLS Gen2
→ souborová data jsou v Azure data lake

Pipelines in Azure Data Factory
→ ADF načítá data a řídí kroky

Transformations in Azure Databricks
→ výpočty probíhají v Databricks, často pomocí Sparku

Serving layer in Synapse
→ analytická SQL nebo warehouse vrstva

Reporting in Power BI
→ sémantický model, DAX a report
```

## Příklad Fabric

```text
Data in OneLake
→ společná datová vrstva Fabricu

Fabric Data Factory
→ pipeline, kopírování a Dataflow Gen2

Lakehouse and notebooks
→ soubory, Delta tabulky a Spark transformace

Fabric Warehouse
→ relační Gold vrstva a T-SQL

Power BI semantic model
→ vztahy, DAX a reporting
```

## Příklad AWS

```text
Data lake on Amazon S3
→ objektové úložiště

ETL with AWS Glue
→ integrace a transformační úlohy

Warehouse in Amazon Redshift
→ analytické SQL a datový sklad

Access controlled by IAM
→ identity, role a oprávnění
```

## Příklad GCP

```text
Files in Cloud Storage
→ objektové úložiště

Transformations in Dataflow
→ dávkové nebo streamové zpracování

Analytics in BigQuery
→ serverless SQL analytika a warehouse

Reports in Looker
→ BI a vizualizace
```

---

# 31. End-to-end Azure scénář

```text
lokální SQL Server a soubory
→ on-premises data gateway
→ Azure Data Factory
→ ADLS Gen2 Bronze
→ Azure Databricks Silver a Gold
→ Synapse Analytics nebo Azure SQL
→ Power BI semantic model
→ Power BI report
```

Podpůrné oblasti:

- Entra ID — identity;
- Azure RBAC — oprávnění;
- Key Vault — tajné údaje;
- private endpoints — privátní síťový přístup;
- Cost Management a tags — náklady;
- DEV, TEST a PROD — bezpečné nasazování.

---

# 32. End-to-end Fabric scénář

```text
lokální SQL Server a soubory
→ on-premises data gateway
→ Fabric Data Factory
→ OneLake / Lakehouse Bronze
→ Dataflow Gen2, SQL nebo Notebook
→ Lakehouse Silver
→ Fabric Warehouse nebo Lakehouse Gold
→ Power BI semantic model
→ Power BI report
```

Podpůrné oblasti:

- Entra ID — identity;
- workspace a datová oprávnění — přístup;
- Fabric Capacity — výpočetní výkon;
- Capacity Metrics — sledování spotřeby;
- DEV, TEST a PROD workspaces — nasazování změn.

---

# 33. Jak vybrat platformu

Před výběrem se ptáme:

1. Kde jsou zdrojová data?
2. Jaký objem dat zpracováváme?
3. Používá firma Power BI a Microsoft ekosystém?
4. Má firma Fabric Capacity?
5. Potřebujeme integrované SaaS prostředí, nebo samostatné služby?
6. Jaké dovednosti má tým — Power Query, SQL, Python nebo Spark?
7. Jaké jsou požadavky na sítě a zabezpečení?
8. Kde musí být data geograficky umístěna?
9. Jak často se data načítají a transformují?
10. Které operace vytvářejí náklady?
11. Odkud bude Power BI nebo jiný BI nástroj data číst?
12. Kdo bude prostředí provozovat a podporovat?

---

# 34. Nejčastější chyby

- výběr služby pouze podle známého názvu;
- zaměňování úložiště a výpočetního enginu;
- považování Data Factory za databázi;
- ukládání hesel přímo do kódu;
- příliš široká oprávnění;
- ignorování síťových omezení;
- umístění dat a výpočtů do vzdálených regionů bez důvodu;
- ponechání nepotřebného výpočetního prostředí v provozu;
- spouštění náročných Fabric workloadů současně bez kontroly kapacity;
- úpravy přímo v produkčním workspace;
- připojení Power BI přímo k raw datům bez řízené transformační vrstvy;
- automatické použití Databricks nebo Sparku pro malý a jednoduchý problém;
- předpoklad, že všechny cloudové služby jsou mezi platformami přesně ekvivalentní.

---

# 35. Co si pamatovat

```text
Azure
→ obecná cloudová platforma se samostatnými službami

Microsoft Fabric
→ integrovaná SaaS analytická platforma s Power BI

AWS
→ obecná cloudová platforma Amazonu

Google Cloud
→ obecná cloudová platforma Googlu

ADLS Gen2 / S3 / Cloud Storage / OneLake
→ ukládání dat

Azure Data Factory / Fabric Data Factory / AWS Glue / Dataflow
→ datová integrace a zpracování

Synapse / Fabric Warehouse / Redshift / BigQuery
→ analytický warehouse a SQL

Entra ID / AWS IAM / Cloud IAM
→ identity a oprávnění

Power BI / QuickSight / Looker
→ business intelligence a reporting

gateway
→ most mezi lokálním zdrojem a cloudovou službou

managed identity
→ přístup Azure služby bez ručně uloženého hesla

Fabric Capacity
→ sdílený výpočetní výkon Fabric workloadů
```

Hlavní princip:

> Nejdříve určujeme roli služby v datovém toku. Teprve potom vybíráme konkrétní technologii podle prostředí firmy, dovedností týmu, zabezpečení, výkonu a nákladů.