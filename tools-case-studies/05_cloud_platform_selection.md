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

### Lokální SQL Server

Obsahuje provozní údaje o zásilkách, například:
- identifikátor zásilky;
- datum odeslání;
- plánované datum doručení;
- skutečné datum doručení;
- identifikátor dopravce;
- výchozí sklad;
- cílovou oblast;
- stav zásilky.

### JSON soubory

Obsahují GPS události zaznamenané během přepravy, například:
- identifikátor zásilky;
- čas události;
- typ události;
- geografickou polohu;
- stav přepravy.

### CSV soubory

Obsahují ceníky dopravců, například:
- identifikátor dopravce;
- typ přepravy;
- výchozí a cílovou oblast;
- cenu přepravy;
- datum platnosti ceníku.

### Excel soubor

Obsahuje cílové hodnoty stanovené managementem, například:
- požadovaný podíl včasných doručení;
- maximální přijatelnou dobu zpoždění;
- cílové přepravní náklady;
- cíle podle dopravce nebo oblasti.

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
→ OneLake / Fabric Lakehouse
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

