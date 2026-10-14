# nebrecime.cz

Studijní web pro studenty školy CreativeHill.CZ. Poznámky z maturitních předmětů
(test)

## Struktura projektu

```
nebrecime.cz/
├── content/
│   ├── dvk/          # Lekce DVK (1.md … 20.md, chronologie.md)
│   └── vma/          # Lekce VMA (1.md … 24.md)
├── data/
│   └── quizzes/
│       └── dvk/      # Kvízová data (1.yaml … N.yaml)
├── layouts/
│   ├── _default/
│   │   ├── baseof.html   # Základní šablona (head, body wrapper)
│   │   ├── single.html   # Šablona pro stránku lekce
│   │   └── list.html     # Šablona pro sekci
│   ├── partials/
│   │   └── quiz.html     # Komponenta kvízu
│   └── index.html        # Šablona homepage
├── static/
│   ├── main.css          # Styly
│   ├── main.js           # Skript (téma, lightbox, quiz, Swup)
│   ├── dvk/              # Obrázky a audio pro DVK
│   └── vma/              # Obrázky a PDF pro VMA
└── hugo.toml             # Konfigurace Hugo
```

## Lokální vývoj

```bash
hugo server
```

Server běží na `http://localhost:1313/`. Automaticky sleduje změny v `content/`, `layouts/` a `static/`.

> Po změně `static/main.css` nebo `static/main.js` je potřeba udělat hard refresh v prohlížeči (`Cmd+Shift+R`), protože Hugo tyto soubory nesleduje živě.

Pokud chceš, aby Hugo sledoval i statické soubory a plně rebuildo val:

```bash
hugo server --disableFastRender
```

## Build a deploy

Push na větev `master` automaticky spustí GitHub Actions workflow, který web sestaví a nasadí na GitHub Pages.

```bash
git add .
git commit -m "popis změny"
git push
```

## Jak přidat novou lekci

### 1. Vytvoř soubor v `content/dvk/` nebo `content/vma/`

Pojmenuj ho číslem lekce, např. `content/dvk/21.md`.

```markdown
---
markup: html
smallTitle: "Název lekce"
nav:
  - anchor: "sekce-1"
    label: "Název sekce 1"
  - anchor: "sekce-2"
    label: "Název sekce 2"
---

<h2 id="sekce-1">Název sekce 1</h2>
<p>Obsah lekce v HTML...</p>

<h2 id="sekce-2">Název sekce 2</h2>
<p>Další obsah...</p>
```

**Front matter:**
- `markup: html` — povinné (obchází Markdown parser, nutné kvůli buffer limitu)
- `smallTitle` — zobrazuje se na homepage i jako nadpis lekce
- `nav` — seznam odkazů v navigaci na stránce lekce (kotvy na `<h2 id="...">`)

### 2. Homepage se aktualizuje automaticky

Homepage prochází `range seq 1 N` a `site.GetPage` — jakmile soubor existuje, odkaz se automaticky zobrazí.

## Jak přidat kvíz k lekci

Vytvoř soubor `data/quizzes/<sekce>/<číslo>.yaml`, např. `data/quizzes/dvk/6.yaml`:

```yaml
questions:
  - q: "Text otázky?"
    options: ["Možnost A", "Možnost B", "Možnost C", "Možnost D"]
    answer: 0   # index správné odpovědi (0 = první možnost)
  - q: "Další otázka?"
    options: ["A", "B", "C", "D"]
    answer: 2
```

Kvíz se automaticky zobrazí na konci lekce a na homepage se u lekce ukáže badge `Quiz`.

## Technologie

- **Hugo** — statický generátor webu (`uglyURLs = true` zachovává URL formát `/dvk/1.html`)
- **Swup.js** — SPA přechody mezi stránkami
- **GitHub Actions** → **GitHub Pages** — automatický deploy při push
