# Git Cheatsheet

Praktický tahák pro základní práci s Gitem, GitHubem a VS Code.

---

## 1. Git vs. GitHub

```text
Git
→ verzovací systém
→ sleduje historii změn projektu

GitHub
→ online služba
→ ukládá vzdálené Git repozitáře
```

Git funguje i bez GitHubu.

GitHub je místo, kam můžeme lokální Git repozitář odesílat.

---

## 2. Základní princip

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
→ soubory, které právě upravuji

Staging area
→ změny připravené pro další commit

Local repository
→ historie commitů uložená na počítači

Remote repository
→ vzdálený repozitář, například GitHub
```

Základní workflow:

```text
změna souboru
→ git status
→ git add
→ git commit
→ git push
```

---

## 3. Otevření repozitáře ve VS Code

Je praktické otevřít celý repozitář:

```text
File
→ Open Folder
```

Například:

```text
C:\Users\frant\Documents\data-analytics-workspace\git-vs-code-practice
```

Terminál by měl být ve stejné složce:

```powershell
PS C:\Users\frant\Documents\data-analytics-workspace\git-vs-code-practice>
```

---

## 4. Kontrola instalace Gitu

```powershell
git --version
```

Příklad:

```text
git version 2.55.0.windows.1
```

Pokud Windows hlásí:

```text
git is not recognized
```

Git není nainstalovaný nebo není dostupný přes `PATH`.

---

## 5. Klonování repozitáře

Pokud repozitář už existuje na GitHubu:

```powershell
git clone URL_REPOZITARE
```

Například:

```powershell
git clone https://github.com/username/git-vs-code-practice.git
```

Potom:

```powershell
cd git-vs-code-practice
```

Princip:

```text
GitHub
→ git clone
→ lokální kopie repozitáře
```

---

## 6. `git status`

```powershell
git status
```

Je jeden z nejdůležitějších Git příkazů.

Ukazuje:

```text
aktuální branch
nové soubory
změněné soubory
smazané soubory
staged změny
nestaged změny
```

Čistý stav:

```text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

Význam:

```text
On branch main
→ pracuji na větvi main

up to date with origin/main
→ lokální a vzdálená větev jsou synchronizované

nothing to commit
→ nejsou změny pro commit

working tree clean
→ pracovní složka je čistá
```

---

## 7. Stavy souborů ve VS Code

VS Code může u souborů zobrazovat:

```text
U → Untracked
M → Modified
A → Added
D → Deleted
```

### U — Untracked

```text
vscode_cheatsheet.md  U
```

Soubor existuje, ale Git ho ještě nesleduje.

### M — Modified

```text
README.md  M
```

Soubor už Git zná, ale byl změněn.

### A — Added

```text
vscode_cheatsheet.md  A
```

Soubor byl přidán do staging area.

### D — Deleted

```text
practice.txt  D
```

Git eviduje smazání souboru.

---

## 8. `git add`

Konkrétní soubor:

```powershell
git add README.md
```

Soubor v podsložce:

```powershell
git add vs-code/vscode_cheatsheet.md
```

Tím říkáme:

```text
tuto změnu chci zahrnout do dalšího commitu
```

Workflow:

```text
Working directory
→ git add
→ Staging area
```

---

## 9. Relativní cesta k souboru

Pokud jsem v kořenu repozitáře:

```text
git-vs-code-practice/
```

a soubor je zde:

```text
vs-code/vscode_cheatsheet.md
```

nestačí:

```powershell
git add vscode_cheatsheet.md
```

Git ho v kořenové složce nenajde.

Správně:

```powershell
git add vs-code/vscode_cheatsheet.md
```

Obecný princip:

```text
aktuální složka
→ relativní cesta k souboru
```

---

## 10. Chyba `pathspec did not match any files`

Například:

```powershell
git add vscode_cheatsheet.md
```

může vrátit:

```text
pathspec 'vscode_cheatsheet.md' did not match any files
```

Typický důvod:

```text
soubor není v aktuální složce
→ je například v podsložce
```

Řešení:

```powershell
git add vs-code/vscode_cheatsheet.md
```

nebo:

```powershell
git add .
```

---

## 11. `git add .`

```powershell
git add .
```

Tečka znamená:

```text
aktuální složka
+
její podsložky
```

Hodí se například při reorganizaci projektu:

```text
git/
vs-code/
README.md
```

kdy chceme připravit více změn najednou.

Například:

```powershell
git add .
git status
```

---

## 12. `git add -A`

```powershell
git add -A
```

Přidá všechny změny:

```text
nové soubory
změněné soubory
smazané soubory
```

Na začátku je bezpečnější používat:

```powershell
git status
```

a vědět, co přesně staging area obsahuje.

---

## 13. Staging area

Staging area je přípravná oblast před commitem.

Například:

```text
README.md
git/git_cheatsheet.md
vs-code/vscode_cheatsheet.md
```

mohou být změněné, ale pomocí `git add` rozhodujeme, které změny půjdou do dalšího commitu.

Princip:

```text
Working directory
→ git add
→ Staging area
→ git commit
```

---

## 14. `git commit`

```powershell
git commit -m "Popis změny"
```

Například:

```powershell
git commit -m "Add VS Code cheatsheet and organize notes"
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

Dobrá commit message je:

```text
krátká
konkrétní
srozumitelná
```

Příklady:

```text
Add Git cheatsheet
Add VS Code cheatsheet
Update README
Remove practice file
Organize Git and VS Code notes
```

---

## 15. Co se stane po commitu

```text
git add
→ staging area

git commit
→ local repository
```

Po commitu změna ještě nemusí být na GitHubu.

Je uložená pouze lokálně.

---

## 16. `git push`

```powershell
git push
```

Odešle lokální commity na vzdálený repozitář.

Workflow:

```text
Local repository
→ git push
→ GitHub
```

---

## 17. Kompletní workflow

```text
1. upravím soubor

2. Ctrl + S

3. git status

4. git add ...

5. git status

6. git commit -m "..."

7. git push
```

Zkráceně:

```text
edit
→ save
→ status
→ add
→ status
→ commit
→ push
```

Druhé `git status` není povinné, ale je velmi užitečné před commitem.

---

## 18. `git pull`

```powershell
git pull
```

Stáhne změny ze vzdáleného repozitáře.

Workflow:

```text
GitHub
→ git pull
→ lokální repozitář
```

Používá se například pokud:

```text
změním soubor přes GitHub web
kolega odešle změnu
pracuji na jiném počítači
```

---

## 19. Remote repository a `origin`

Git běžně označuje vzdálený repozitář jako:

```text
origin
```

Kontrola:

```powershell
git remote -v
```

Výstup může být:

```text
origin  https://github.com/username/git-vs-code-practice.git (fetch)
origin  https://github.com/username/git-vs-code-practice.git (push)
```

### `fetch`

```text
odkud Git stahuje změny
```

### `push`

```text
kam Git posílá změny
```

---

## 20. Přejmenování GitHub repozitáře

Pokud přejmenuji repozitář na GitHubu například:

```text
git-practice
```

na:

```text
git-vs-code-practice
```

lokální Git může stále používat starou adresu.

Nejprve ověřím:

```powershell
git remote -v
```

Potom změním URL:

```powershell
git remote set-url origin NOVA_URL
```

Například:

```powershell
git remote set-url origin https://github.com/username/git-vs-code-practice.git
```

Znovu ověřím:

```powershell
git remote -v
```

---

## 21. Přejmenování lokální složky

Přejmenování GitHub repozitáře automaticky nepřejmenuje lokální složku.

Například:

```text
git-practice
```

může být lokálně přejmenováno na:

```text
git-vs-code-practice
```

Potom je vhodné složku znovu otevřít přes:

```text
File
→ Open Folder
```

a ověřit:

```powershell
git status
```

Git tím nezmizí.

Metadata repozitáře jsou uložená ve skryté složce:

```text
.git
```

---

## 22. Struktura složek v repozitáři

Například:

```text
git-vs-code-practice/
│
├── git/
│   └── git_cheatsheet.md
│
├── vs-code/
│   └── vscode_cheatsheet.md
│
└── README.md
```

Git pracuje se soubory i v podsložkách.

Samotné prázdné složky Git nesleduje.

Například prázdná:

```text
vs-code/
```

se na GitHubu neobjeví, dokud v ní nebude sledovaný soubor.

---

## 23. Přesun souboru do jiné složky

Například:

```text
git_cheatsheet.md
```

přesunu do:

```text
git/git_cheatsheet.md
```

Před stagingem může Git zobrazit:

```text
deleted: git_cheatsheet.md

Untracked files:
    git/
```

Po:

```powershell
git add .
```

může Git rozpoznat změnu jako:

```text
renamed:
git_cheatsheet.md -> git/git_cheatsheet.md
```

Git neukládá příkaz „přesuň soubor“.

Porovnává obsah a historii změn a může následně přesun rozpoznat.

---

## 24. Mazání souboru přes Git

```powershell
git rm practice.txt
```

Tím Git:

```text
1. smaže soubor
2. připraví jeho smazání do staging area
```

Potom:

```powershell
git status
git commit -m "Remove practice file"
git push
```

Workflow:

```text
git rm
→ staging area
→ commit
→ push
```

---

## 25. Ruční smazání ve VS Code

Soubor lze odstranit také:

```text
pravé tlačítko
→ Delete
```

Git smazání pozná.

Potom například:

```powershell
git add -A
git commit -m "Remove practice file"
git push
```

---

## 26. `git rm` vs. Delete

### Git příkaz

```powershell
git rm practice.txt
```

udělá:

```text
Delete
+
staging
```

### Ruční Delete

```text
Delete ve VS Code
```

pak je potřeba smazání ještě připravit:

```powershell
git add -A
```

---

## 27. Branch

```text
branch
→ větev projektu
```

Aktuální větev:

```powershell
git branch
```

Například:

```text
* main
```

Hvězdička znamená:

```text
aktuální větev
```

Stejnou informaci ukazuje:

```powershell
git status
```

---

## 28. `main` a `origin/main`

```text
main
→ moje lokální větev

origin/main
→ vzdálená větev main na GitHubu
```

Pokud Git hlásí:

```text
Your branch is up to date with 'origin/main'
```

znamená to:

```text
lokální main
=
GitHub main
```

---

## 29. Kontrola před commitem

Doporučený postup:

```powershell
git status
```

Díky tomu vidím:

```text
co je změněné
co není staged
co už je staged
co se skutečně commitne
```

Potom teprve:

```powershell
git commit -m "..."
```

---

## 30. Kontrola po commitu

```powershell
git status
```

Pokud vidím:

```text
nothing to commit, working tree clean
```

všechny aktuální změny jsou uložené v lokálním commitu.

Pokud ale ještě nebyl:

```powershell
git push
```

nemusí být commit na GitHubu.

---

## 31. Kontrola po pushi

Po:

```powershell
git push
```

mohu:

```text
otevřít GitHub
→ obnovit stránku
→ zkontrolovat změny
```

---

## 32. Nejčastější workflow — jeden soubor

```powershell
git status

git add README.md

git status

git commit -m "Update README"

git push
```

---

## 33. Nejčastější workflow — soubor v podsložce

```powershell
git status

git add vs-code/vscode_cheatsheet.md

git status

git commit -m "Add VS Code cheatsheet"

git push
```

---

## 34. Nejčastější workflow — více změn a složek

Například:

```text
přesunutý Git cheatsheet
nový VS Code cheatsheet
nové složky
```

Použij:

```powershell
git status

git add .

git status

git commit -m "Add VS Code cheatsheet and organize notes"

git push
```

---

## 35. Nejčastější workflow — smazání

```powershell
git rm practice.txt

git status

git commit -m "Remove practice file"

git push
```

---

## 36. Rychlý Git tahák

```powershell
# kontrola instalace
git --version

# stav repozitáře
git status

# klonování
git clone URL

# jeden soubor
git add README.md

# soubor v podsložce
git add vs-code/vscode_cheatsheet.md

# všechny změny v aktuální složce
git add .

# všechny změny včetně smazání
git add -A

# commit
git commit -m "Popis změny"

# odeslání
git push

# stažení změn
git pull

# větve
git branch

# vzdálený repozitář
git remote -v

# změna URL origin
git remote set-url origin NOVA_URL

# smazání sledovaného souboru
git rm soubor
```

---

## 37. Mentální model

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

Opačný směr:

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

## 38. Co si zatím stačí pamatovat

```text
git clone
→ stáhni repozitář

git status
→ zjisti stav

git add
→ připrav změny

git commit
→ ulož bod historie lokálně

git push
→ odešli commity na GitHub

git pull
→ stáhni změny z GitHubu

git rm
→ smaž sledovaný soubor

git remote -v
→ zobraz vzdálený repozitář

git remote set-url
→ změň adresu vzdáleného repozitáře
```

---

## 39. Praktická zásada

Před většinou důležitých kroků použij:

```powershell
git status
```

Když si nejsi jistý:

```text
nejdřív status
→ potom rozhodnutí
```

---

## 40. Další témata pro později

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

spolupráce více lidí

GitHub Issues
GitHub Actions
```