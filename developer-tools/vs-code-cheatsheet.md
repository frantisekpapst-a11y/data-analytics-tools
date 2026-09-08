# VS Code Cheatsheet

Praktický tahák pro základní práci ve Visual Studio Code při práci s Pythonem, Gitem a datovou analýzou.

---

## 1. Otevření pracovní složky

Ve VS Code je lepší otevřít celý projekt jako složku než jednotlivé soubory.

Použij:

```text
File
→ Open Folder
```

Například:

```text
C:\Users\frant\Documents\data-analytics-workspace\python
```

nebo:

```text
C:\Users\frant\Documents\data-analytics-workspace\git-vs-code-practice
```

Výhody:

```text
Explorer
→ zobrazí všechny soubory projektu

Terminal
→ otevře se ve správné pracovní složce

Git
→ pracuje nad správným repozitářem
```

---

## 2. Explorer

Explorer je levý panel pro práci se soubory a složkami.

Můžeš zde:

```text
vytvářet nové soubory
vytvářet nové složky
přejmenovávat soubory
mazat soubory
přesouvat soubory mezi složkami
otevírat soubory
```

Pravé tlačítko na soubor nebo složku nabízí například:

```text
New File
New Folder
Rename
Delete
```

---

## 3. Vytvoření nového souboru

V Exploreru:

```text
pravé tlačítko
→ New File
```

Například:

```text
lesson_08.py
vscode_cheatsheet.md
README.md
```

Přípona určuje typ souboru:

```text
.py
→ Python

.md
→ Markdown

.csv
→ CSV data

.json
→ JSON data
```

---

## 4. Vytvoření nové složky

V Exploreru:

```text
pravé tlačítko
→ New Folder
```

Například:

```text
git/
vs-code/
lessons/
mini-tests/
cheatsheets/
case-studies/
```

Git samotné prázdné složky nesleduje.

Složka se v Git repozitáři projeví až tehdy, když obsahuje alespoň jeden sledovaný soubor.

---

## 5. Uložení souboru

Klávesová zkratka:

```text
Ctrl + S
```

Neuložený soubor může mít na záložce tečku.

Před spuštěním Python skriptu nebo Git příkazy je dobré soubor vždy uložit.

Princip:

```text
upravím soubor
→ Ctrl + S
→ změna je skutečně uložena na disku
```

---

## 6. Otevření terminálu

Terminál otevřeš:

```text
Terminal
→ New Terminal
```

Klávesová zkratka:

```text
Ctrl + `
```

V terminálu můžeš spouštět například:

```powershell
python lesson_07.py
git status
git add .
git commit -m "Update notes"
git push
```

---

## 7. Aktuální pracovní složka

V terminálu vždy sleduj aktuální cestu.

Například:

```powershell
PS C:\Users\frant\Documents\data-analytics-workspace\python>
```

To znamená, že příkazy se spouštějí ve složce:

```text
python
```

Pokud spustíš:

```powershell
python lesson_07.py
```

Python bude hledat soubor právě v této složce.

---

## 8. Změna pracovní složky

Použij:

```powershell
cd cesta
```

Například:

```powershell
cd "C:\Users\frant\Documents\data-analytics-workspace\python"
```

O úroveň výš:

```powershell
cd ..
```

Přechod do podsložky:

```powershell
cd git-vs-code-practice
```

---

## 9. Kontrola obsahu složky

V PowerShellu:

```powershell
dir
```

Zobrazí soubory a složky v aktuálním adresáři.

Hodí se například při kontrole, zda se ve složce nachází:

```text
lesson_07.py
README.md
git_cheatsheet.md
```

---

## 10. Spuštění Python skriptu

Pokud je terminál ve správné složce:

```powershell
python lesson_07.py
```

Pokud dostaneš chybu:

```text
can't open file
No such file or directory
```

většinou to znamená:

```text
terminál je v jiné složce než soubor
```

Řešení:

```text
zkontrolovat cestu
→ přejít přes cd do správné složky
→ znovu spustit python soubor
```

---

## 11. Open Folder vs. otevřený soubor

Pokud otevřeš pouze jeden soubor, Explorer může zobrazovat:

```text
No Folder Opened
```

V takovém případě je lepší použít:

```text
File
→ Open Folder
```

a otevřít celý projekt.

To usnadní:

```text
práci se soubory
spouštění Pythonu
práci s Gitem
orientaci ve složkách
```

---

## 12. Rychlé otevření souboru

Klávesová zkratka:

```text
Ctrl + P
```

Potom napiš například:

```text
lesson_07.py
```

a VS Code soubor rychle otevře.

To je rychlejší než hledání v Exploreru u větších projektů.

---

## 13. Command Palette

Klávesová zkratka:

```text
Ctrl + Shift + P
```

Otevře Command Palette.

Přes ni lze rychle spustit mnoho funkcí VS Code.

Například:

```text
Python: Select Interpreter
Format Document
Git: Clone
Preferences
```

---

## 14. Python interpreter

VS Code potřebuje vědět, jakou instalaci Pythonu má používat.

Aktuální interpreter je obvykle vidět dole ve stavovém řádku.

Například:

```text
Python 3.14.7
```

Interpreter lze změnit přes:

```text
Ctrl + Shift + P
→ Python: Select Interpreter
```

---

## 15. Spuštění Pythonu přes tlačítko Run

Vpravo nahoře u Python souboru může být tlačítko:

```text
▶
```

To umožňuje spustit aktuální Python skript.

Při výuce je ale užitečné znát i terminálový způsob:

```powershell
python lesson_07.py
```

Díky tomu přesně víš, co se spouští.

---

## 16. Source Control

V levém panelu VS Code je ikona:

```text
Source Control
```

Slouží jako grafické rozhraní pro Git.

Může zobrazovat:

```text
Changes
Staged Changes
Commit
Sync Changes
```

Pro začátek je vhodné kombinovat Source Control s terminálem.

Terminál pomáhá pochopit, co Git skutečně dělá.

---

## 17. Git značky u souborů

VS Code může vedle názvu souboru zobrazovat:

```text
U
M
A
D
```

Význam:

```text
U
→ Untracked
→ nový soubor, který Git ještě nesleduje

M
→ Modified
→ existující soubor byl změněn

A
→ Added
→ soubor je připravený ve staging area

D
→ Deleted
→ soubor byl odstraněn
```

---

## 18. Git stav ve VS Code

Příklad:

```text
vscode_cheatsheet.md  U
```

znamená:

```text
soubor existuje
→ Git ho ještě nesleduje
```

Po:

```powershell
git add vscode_cheatsheet.md
```

se stav změní a soubor je připravený pro commit.

---

## 19. Přesun souboru

Soubor můžeš přesunout v Exploreru například:

```text
git_cheatsheet.md
```

do:

```text
git/git_cheatsheet.md
```

Git může před `git add` zobrazit:

```text
deleted: git_cheatsheet.md

Untracked:
git/
```

Po:

```powershell
git add .
```

může Git přesun rozpoznat jako:

```text
renamed:
git_cheatsheet.md
→ git/git_cheatsheet.md
```

---

## 20. Mazání souboru

Ve VS Code můžeš:

```text
pravé tlačítko
→ Delete
```

Git pak smazání uvidí jako změnu.

Pokud pracuješ přes Git, lze použít také:

```powershell
git rm soubor
```

Například:

```powershell
git rm practice.txt
```

---

## 21. Přejmenování souboru

V Exploreru:

```text
pravé tlačítko
→ Rename
```

Git změnu následně rozpozná.

Po přejmenování je vhodné zkontrolovat:

```powershell
git status
```

---

## 22. Přepínání mezi soubory

Otevřené soubory se zobrazují v horních záložkách.

Můžeš mezi nimi přepínat kliknutím.

Při větším počtu otevřených souborů pomůže:

```text
Ctrl + P
```

---

## 23. Zavření souboru

Záložku zavřeš:

```text
X
```

nebo klávesovou zkratkou:

```text
Ctrl + W
```

Zavřením souboru se soubor nesmaže.

Pouze se zavře jeho editor.

---

## 24. Hledání v souboru

Klávesová zkratka:

```text
Ctrl + F
```

Použije se pro hledání textu v aktuálním souboru.

Například:

```text
iloc
total
category
git status
```

---

## 25. Hledání v celém projektu

Klávesová zkratka:

```text
Ctrl + Shift + F
```

Vyhledává text ve všech souborech otevřené pracovní složky.

Hodí se například při hledání:

```text
average_order
git push
Furniture
```

ve větším projektu.

---

## 26. Automatické doplňování

VS Code při psaní nabízí návrhy.

Například u Pythonu:

```python
df.
```

může nabídnout:

```text
head
info
sort_values
loc
iloc
```

Stejně tak může nabídnout možné hodnoty parametrů.

Například:

```python
inclusive=
```

může nabídnout:

```text
both
left
right
neither
```

---

## 27. Chybové podtržení

VS Code může zvýrazňovat:

```text
syntaktické chyby
neexistující názvy
problémy v kódu
```

Panel:

```text
Problems
```

zobrazuje nalezené problémy.

Ne každé zvýraznění znamená, že se program určitě nespustí, ale je dobré ho zkontrolovat.

---

## 28. Terminálový výstup

Výstup Pythonu se zobrazuje v terminálu.

Například:

```powershell
python lesson_07.py
```

může vypsat:

```text
order_id product category total
...
```

Stejně tak se v terminálu zobrazují chyby:

```text
KeyError
IndexingError
FileNotFoundError
```

Při chybě je důležité číst hlavně:

```text
poslední řádky tracebacku
název chyby
řádek kódu, kde chyba vznikla
```

---

## 29. Traceback

Python při chybě zobrazí traceback.

Například:

```text
KeyError: 'total'
```

To znamená, že Python nebo pandas hledal:

```text
total
```

ale v daném objektu ho nenašel.

Traceback pomáhá zjistit:

```text
ve kterém souboru chyba vznikla
na kterém řádku
jaký typ chyby nastal
```

---

## 30. Klávesové zkratky

```text
Ctrl + S
→ uložit soubor

Ctrl + P
→ rychle otevřít soubor

Ctrl + Shift + P
→ Command Palette

Ctrl + F
→ hledání v souboru

Ctrl + Shift + F
→ hledání v celém projektu

Ctrl + W
→ zavřít aktuální záložku

Ctrl + `
→ otevřít / zavřít terminál
```

---

## 31. VS Code + Python workflow

Typický postup:

```text
Open Folder
→ otevřít projekt

Explorer
→ otevřít Python soubor

upravit kód

Ctrl + S
→ uložit

Terminal
→ spustit skript

python lesson_07.py

zkontrolovat výstup
→ případně opravit chybu
```

---

## 32. VS Code + Git workflow

Typický postup:

```text
otevřít Git repozitář přes Open Folder
→ upravit soubor
→ Ctrl + S
→ git status
→ git add
→ git commit
→ git push
```

Například:

```powershell
git status

git add vscode_cheatsheet.md

git commit -m "Add VS Code cheatsheet"

git push
```

---

## 33. Správná složka při práci s Gitem

Explorer může zobrazovat:

```text
git-vs-code-practice
```

Terminál by měl být například:

```powershell
PS C:\Users\frant\Documents\data-analytics-workspace\git-vs-code-practice>
```

Tím máš jistotu, že Git příkazy pracují nad správným repozitářem.

---

## 34. Praktický mentální model

```text
VS Code Explorer
→ pracuji se soubory

Editor
→ píšu a upravuji obsah

Terminal
→ spouštím Python, Git a další příkazy

Source Control
→ vizuálně sleduji Git změny

GitHub
→ vzdálený repozitář
```

---

## 35. Co si zatím stačí pamatovat

```text
Open Folder
→ otevři celý projekt

Explorer
→ správa souborů

Ctrl + S
→ ulož změny

Terminal
→ příkazy

Ctrl + `
→ terminál

Ctrl + P
→ rychlé otevření souboru

Ctrl + Shift + P
→ Command Palette

Source Control
→ Git ve VS Code

U / M / A / D
→ Git stav souboru
```

---

## 36. Další témata pro později

```text
debugging
breakpoints
Run and Debug

formatování kódu
Python formatter

extensions
správa rozšíření

více panelů editoru
split editor

workspace settings

keyboard shortcuts

Git branch ve VS Code

merge conflicts

Jupyter notebooks

virtual environments

Python package management
```
