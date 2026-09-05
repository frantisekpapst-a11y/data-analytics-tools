# Git, GitHub & VS Code Reference

Praktická technická reference zaměřená na práci s **Gitem, GitHubem a Visual Studio Code** v rámci datově-analytického workflow.

Repozitář obsahuje vlastní cheatsheety a poznámky k běžným operacím používaným při správě analytických projektů, verzování souborů a práci ve VS Code.

---

## 📂 Struktura repozitáře

```text
git-and-code-practice/
│
├── git/
│   └── git_cheatsheet.md
│
├── vs-code/
│   └── vscode_cheatsheet.md
│
└── README.md
```

---

## 🎯 Zaměření

Repozitář pokrývá zejména:

- základní Git workflow,
- staging area,
- commitování změn,
- push a pull,
- clone repozitářů,
- práci s remote repozitářem,
- mazání a přesouvání souborů pomocí Gitu,
- kontrolu stavu repozitáře,
- práci s větvemi v základním rozsahu,
- základní orientaci ve VS Code,
- práci s integrovaným terminálem,
- práci se soubory a složkami,
- Git integraci ve VS Code,
- použití VS Code při práci s Pythonem a analytickými projekty.

---

## 🔄 Základní Git workflow

Typický pracovní postup:

```text
změna souboru
→ git status
→ git add
→ git commit
→ git push
```

Při práci s existujícím vzdáleným repozitářem se běžně doplňuje také:

```text
git pull
→ lokální změny
→ git add
→ git commit
→ git push
```

---

## Git

Git část obsahuje přehled příkazů a workflow používaných při správě verzí projektů.

➡️ [Git Cheatsheet](git/git_cheatsheet.md)

Obsah zahrnuje například:

```text
git status
git add
git commit
git push
git pull
git clone
git rm
git mv
```

Dále se zaměřuje na:

- staging area,
- working directory,
- repository,
- práci s remote,
- sledování změn,
- mazání souborů,
- přesouvání a přejmenování souborů,
- základní řešení běžných Git situací.

---

## Visual Studio Code

VS Code část slouží jako reference pro každodenní práci v editoru.

➡️ [VS Code Cheatsheet](vs-code/vscode_cheatsheet.md)

Obsah zahrnuje například:

- Explorer,
- práci se soubory a složkami,
- integrovaný terminál,
- otevření projektu,
- orientaci v pracovním prostoru,
- Git integraci,
- práci s Python soubory,
- základní práci s extensions,
- práci s více projekty a repozitáři.

---

## Git + VS Code workflow

VS Code je v analytických projektech používán jako centrální pracovní prostředí:

```text
VS Code
→ práce se soubory
→ Python / SQL / Markdown
→ integrovaný terminál
→ Git
→ GitHub
```

To umožňuje spojit:

- vývoj,
- dokumentaci,
- verzování,
- správu projektu,
- práci s repozitářem

do jednoho pracovního prostředí.

---

## Použití v analytických projektech

Git a VS Code jsou využívány napříč dalšími analytickými repozitáři například pro:

- verzování Python skriptů,
- správu SQL souborů,
- údržbu README dokumentace,
- organizaci case studies,
- práci s Markdown soubory,
- správu Power BI a Excel portfolio repozitářů,
- sledování změn v projektech,
- publikaci na GitHub.

---

## Hlavní principy

```text
malé a smysluplné commity

kontrola pomocí git status

jasné commit messages

necommitovat zbytečné soubory

udržovat přehlednou strukturu repozitáře

před push ověřit změny

používat verzování jako součást workflow,
ne až jako poslední krok
```

---

## Technologie

```text
Git
GitHub
Visual Studio Code
PowerShell
Markdown
```

---

## Účel repozitáře

Tento repozitář není samostatným analytickým portfolio projektem.

Slouží jako **technická reference a podpůrná znalostní báze** pro práci na datově-analytických projektech v dalších repozitářích.

Pomáhá sjednotit způsob práce s:

```text
project files
→ VS Code
→ Git
→ GitHub
```