# Git + VS Code Cheatsheet

Praktický tahák pro základní práci s Gitem ve VS Code.

---

## 1. Základní princip

Git pracuje s několika úrovněmi:

```text
Working directory
→ Staging area
→ Local repository
→ Remote repository
```

Význam:

```text
Working directory
→ soubory, které právě upravuješ ve VS Code

Staging area
→ změny vybrané pro další commit

Local repository
→ commity uložené lokálně na počítači

Remote repository
→ vzdálený repozitář, například GitHub
```

Základní workflow:

```text
upravím soubor
→ git status
→ git add
→ git commit
→ git push
```

---

## 2. VS Code a Git

Ve VS Code je praktické otevřít celý repozitář jako složku:

```text
File
→ Open Folder
→ vyber složku repozitáře
```

Například:

```text
C:\Users\frant\Documents\data-analytics-workspace\git-practice
```

Díky tomu:

```text
VS Code
→ vidí celý projekt

Terminal
→ otevře se ve správné složce

Git
→ pracuje nad správným repozitářem
```

---

## 3. Jak poznám změny ve VS Code

Vedle souboru může VS Code zobrazovat písmena:

```text
U → Untracked
M → Modified
A → Added / staged
D → Deleted
```

### U — Untracked

Například:

```text
practice.txt  U
```

znamená:

```text
soubor existuje
→ Git ho ještě nesleduje
```

### M — Modified

Například:

```text
README.md  M
```

znamená:

```text
soubor už Git zná
→ ale byl změněn
```

### A — Added

Například:

```text
git_cheatsheet.md  A
```

znamená:

```text
soubor byl přidán přes git add
→ je připravený pro commit
```

### D — Deleted

Například:

```text
practice.txt  D
```

znamená:

```text
soubor byl odstraněn
→ Git eviduje jeho smazání
```

---

## 4. Terminál ve VS Code

Terminál otevřeš:

```text
Terminal
→ New Terminal
```

nebo klávesovou zkratkou:

```text
Ctrl + `
```

V terminálu vždy sleduj, ve které složce právě jsi.

Například:

```powershell
PS C:\Users\frant\Documents\data-analytics-workspace\git-practice>
```

To znamená, že Git příkazy budou pracovat nad repozitářem:

```text
git-practice
```

---

## 5. Kontrola instalace Gitu

```powershell
git --version
```

Příklad:

```text
git version 2.55.0.windows.1
```

Pokud příkaz nefunguje:

```text
git is not recognized
```

Git pravděpodobně není nainstalovaný nebo není v `PATH`.

---

## 6. Klonování repozitáře

Pokud repozitář existuje na GitHubu:

```powershell
git clone URL_REPOZITARE
```

Například:

```powershell
git clone https://github.com/username/git-practice.git
```

Potom:

```powershell
cd git-practice
```

Význam:

```text
git clone
→ stáhne repozitář z GitHubu do počítače

cd git-practice
→ přesune terminál do složky repozitáře
```

---

## 7. git status

```powershell
git status
```

Ukáže aktuální stav repozitáře.

Příklad čistého stavu:

```text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

Význam:

```text
On branch main
→ pracuješ na větvi main

up to date with origin/main
→ lokální verze odpovídá GitHubu

nothing to commit
→ nejsou žádné změny k uložení

working tree clean
→ pracovní složka je čistá
```

---

## 8. Untracked files

Pokud vytvoříš nový soubor:

```text
git_cheatsheet.md
```

a spustíš:

```powershell
git status
```

Git může ukázat:

```text
Untracked files:
    git_cheatsheet.md
```

To znamená:

```text
soubor existuje
→ Git o něm ví
→ ale zatím ho nesleduje
```

---

## 9. git add

```powershell
git add git_cheatsheet.md
```

Tím řekneš Gitu:

```text
tuto změnu chci zahrnout do dalšího commitu
```

Soubor se přesune do:

```text
Staging area
```

Potom:

```powershell
git status
```

může ukázat:

```text
Changes to be committed:
    new file: git_cheatsheet.md
```

---

## 10. Staging area

Staging area je přípravná oblast před commitem.

Princip:

```text
Working directory
→ git add
→ Staging area
```

Můžeš mít více změněných souborů, ale do commitu vybrat jen některé.

Například:

```text
README.md
practice.txt
git_cheatsheet.md
```

Můžeš přidat jen:

```powershell
git add git_cheatsheet.md
```

a ostatní změny zatím nechat mimo commit.

---

## 11. git commit

```powershell
git commit -m "Add Git cheatsheet"
```

Commit je uložený bod historie projektu.

Parametr:

```text
-m
```

znamená:

```text
message
```

tedy zpráva popisující změnu.

Příklad dobré commit message:

```text
Add Git cheatsheet
Update README
Fix filtering example
Add pandas lesson 7
Remove practice file
```

Commit message by měla být:

```text
krátká
konkrétní
srozumitelná
```

---

## 12. Co commit skutečně dělá

Workflow:

```text
git add
→ změna je ve staging area

git commit
→ změna se uloží do lokální historie
```

Po commitu změna ještě není automaticky na GitHubu.

Je zatím pouze v:

```text
Local repository
```

---

## 13. git push

```powershell
git push
```

Odešle lokální commity na GitHub.

Workflow:

```text
Local repository
→ git push
→ GitHub
```

Po úspěšném pushi se změny objeví ve vzdáleném repozitáři.

---

## 14. Kompletní workflow

```text
1. upravím soubor ve VS Code

2. uložím ho
Ctrl + S

3. zkontroluji změny
git status

4. přidám změnu do staging area
git add soubor

5. vytvořím commit
git commit -m "Popis změny"

6. odešlu commit na GitHub
git push
```

Zkráceně:

```text
edit
→ save
→ status
→ add
→ commit
→ push
```

---

## 15. git pull

```powershell
git pull
```

Stáhne nové změny z GitHubu do lokálního repozitáře.

Workflow:

```text
GitHub
→ git pull
→ lokální repozitář
```

Používá se hlavně tehdy, když se na vzdáleném repozitáři něco změnilo.

Například:

```text
změna přes GitHub web
změna od kolegy
změna z jiného počítače
```

---

## 16. GitHub vs. Git

```text
Git
→ nástroj pro verzování

GitHub
→ online služba pro ukládání Git repozitářů
```

Git funguje i bez GitHubu.

GitHub je pouze vzdálené místo, kam můžeš repozitář posílat.

---

## 17. Lokální vs. vzdálený repozitář

```text
lokální repozitář
→ na počítači

remote repository
→ například GitHub
```

V Gitu se často používá:

```text
origin
```

`origin` je běžný název vzdáleného repozitáře.

Například:

```text
origin/main
```

znamená:

```text
větev main na vzdáleném repozitáři origin
```

---

## 18. Branch

```text
branch
→ větev projektu
```

Aktuálně typicky pracujeme na:

```text
main
```

Kontrola:

```powershell
git status
```

nebo:

```powershell
git branch
```

Výstup například:

```text
* main
```

Hvězdička označuje aktuální větev.

---

## 19. Změna existujícího souboru

Například upravíš:

```text
README.md
```

Ve VS Code se může objevit:

```text
M
```

Potom:

```powershell
git status
```

může ukázat:

```text
modified: README.md
```

Workflow:

```powershell
git add README.md

git commit -m "Update README"

git push
```

---

## 20. Nový soubor vs. změněný soubor

Nový soubor:

```text
U
→ Untracked
```

Po:

```powershell
git add soubor
```

může být:

```text
A
→ Added
```

Existující změněný soubor:

```text
M
→ Modified
```

Po:

```powershell
git add soubor
```

se připraví do staging area.

---

## 21. Mazání souboru přes Git

Pokud chceš soubor odstranit přímo přes Git:

```powershell
git rm practice.txt
```

Tím Git udělá dvě věci najednou:

```text
1. smaže soubor z pracovní složky
2. připraví smazání do staging area
```

Potom zkontroluj:

```powershell
git status
```

Můžeš vidět například:

```text
Changes to be committed:
    deleted: practice.txt
```

Následně:

```powershell
git commit -m "Remove practice file"
```

a:

```powershell
git push
```

Tím se smazání projeví i na GitHubu.

Workflow:

```text
git rm practice.txt
→ git status
→ git commit -m "Remove practice file"
→ git push
```

---

## 22. Smazání souboru ručně ve VS Code

Soubor můžeš odstranit i běžně ve VS Code.

Například:

```text
pravé tlačítko na soubor
→ Delete
```

Git si všimne, že sledovaný soubor zmizel.

Potom:

```powershell
git status
```

může ukázat:

```text
deleted: practice.txt
```

Smazání je ale ještě potřeba přidat do staging area:

```powershell
git add practice.txt
```

nebo obecně:

```powershell
git add -A
```

Potom:

```powershell
git commit -m "Remove practice file"
git push
```

---

## 23. `git rm` vs. ruční smazání

### `git rm`

```powershell
git rm practice.txt
```

Výhoda:

```text
smaže soubor
+
rovnou připraví smazání do staging area
```

### Ruční smazání

```text
Delete ve VS Code
```

pak ještě:

```powershell
git add practice.txt
```

nebo:

```powershell
git add -A
```

Prakticky je `git rm` čistší, pokud už předem víš, že chceš sledovaný soubor odstranit.

---

## 24. git add více souborů

Konkrétní soubor:

```powershell
git add README.md
```

Více konkrétních souborů:

```powershell
git add README.md git_cheatsheet.md
```

Všechny aktuální změny:

```powershell
git add .
```

Tečka:

```text
.
```

znamená:

```text
aktuální složka a její změny
```

### `git add -A`

```powershell
git add -A
```

přidá všechny změny, včetně:

```text
nových souborů
změněných souborů
smazaných souborů
```

Na začátku je ale často lepší používat konkrétní názvy souborů, protože přesně víš, co dáváš do commitu.

---

## 25. Kontrola před commitem

Dobrá praxe:

```powershell
git status
```

před:

```powershell
git commit
```

Díky tomu vidíš:

```text
co je změněné
co je staged
co není staged
co se skutečně commitne
```

---

## 26. Kontrola po commitu

Po:

```powershell
git commit -m "Update README"
```

spusť:

```powershell
git status
```

Pokud vidíš:

```text
nothing to commit, working tree clean
```

znamená to:

```text
všechny aktuální změny jsou uložené v commitu
```

---

## 27. Kontrola po pushi

Po:

```powershell
git push
```

můžeš:

```text
otevřít GitHub
→ obnovit stránku repozitáře
→ ověřit nový soubor, změnu nebo smazání
```

---

## 28. VS Code Source Control

Ve VS Code je vlevo ikona:

```text
Source Control
```

Git stav můžeš sledovat i graficky.

VS Code umí zobrazit například:

```text
Changes
Staged Changes
Commit
Sync Changes
```

Pro výuku je ale dobré nejdřív používat terminál, protože přesně vidíš, co Git skutečně dělá.

---

## 29. Uložení souboru před Git příkazy

Před:

```powershell
git status
```

je dobré soubor uložit:

```text
Ctrl + S
```

Pokud máš ve VS Code neuložené změny, Git nemusí vidět nejnovější obsah souboru.

Ve VS Code může neuložený soubor indikovat tečka na záložce.

---

## 30. Otevřená správná složka

V Exploreru VS Code by měla být nahoře vidět složka repozitáře, například:

```text
git-practice
```

Terminál by měl být ve stejné složce:

```powershell
PS C:\Users\frant\Documents\data-analytics-workspace\git-practice>
```

To pomáhá předejít situaci, kdy Git příkazy spouštíš ve špatném adresáři.

---

## 31. Rychlý Git tahák

```powershell
# verze Gitu
git --version

# stav repozitáře
git status

# přidání jednoho souboru
git add README.md

# přidání všech změn v aktuální složce
git add .

# přidání všech změn včetně smazání
git add -A

# commit
git commit -m "Popis změny"

# odeslání na GitHub
git push

# stažení změn
git pull

# klonování repozitáře
git clone URL

# zobrazení větví
git branch

# smazání sledovaného souboru
git rm practice.txt
```

---

## 32. Nejčastější workflow

### Nový nebo upravený soubor

```powershell
git status

git add git_cheatsheet.md

git commit -m "Add Git cheatsheet"

git push
```

### Smazání souboru

```powershell
git rm practice.txt

git status

git commit -m "Remove practice file"

git push
```

Význam:

```text
git status
→ co se změnilo?

git add / git rm
→ co chci zahrnout do commitu?

git commit
→ vytvoř bod historie

git push
→ odešli ho na GitHub
```

---

## 33. Mentální model

Nejdůležitější je držet si tento obraz:

```text
VS Code
↓
Working directory
↓
git add / git rm
↓
Staging area
↓
git commit
↓
Local repository
↓
git push
↓
GitHub
```

A opačným směrem:

```text
GitHub
↓
git pull
↓
Local repository
↓
Working directory
```

---

## 34. Co si zatím stačí pamatovat

```text
git clone
→ stáhni repozitář

git status
→ ukaž stav

git add
→ připrav změnu

git rm
→ smaž sledovaný soubor a připrav smazání

git commit
→ ulož změnu do historie

git push
→ pošli změnu na GitHub

git pull
→ stáhni změnu z GitHubu
```

---

## 35. Další témata pro později

```text
.gitignore
git log
git diff
git restore
git reset

branches
git switch
git merge

merge conflicts

rebase

pull requests

práce více lidí na jednom projektu
```
