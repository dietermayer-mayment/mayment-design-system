# MayMent Design System

Gemeinsame CSS-Basis für alle MayMent Web-Apps und die Homepage.

## Dateien

| Datei | Inhalt |
|---|---|
| `mayment-tokens.css` | Brand-Farben, Fonts, Spacing-Skala (hier anpassen) |
| `mayment-base.css` | Reset, alle Utility-Klassen, Animationen, Print-Styles |
| `mayment-fonts.css` | `@font-face`-Regeln für die selbst gehosteten Schriften (ab v1.4.0) |
| `fonts/` | Schriftdateien (woff2) und Lizenztexte, je Familie ein Unterordner |

## Einbindung

### Option A — über ds.mayment.de (empfohlen)

MayMent liefert das Design System selbst unter `https://ds.mayment.de/` aus (ohne Dritt-CDN,
es werden keine Besucher-IPs an Dritte übertragen):

| Adresse | Inhalt | Cache |
|---|---|---|
| `https://ds.mayment.de/<datei>` | aktueller Stand von `main` | 5 Minuten |
| `https://ds.mayment.de/vX.Y.Z/<datei>` | fester Stand eines Release-Tags | 1 Jahr, unveränderlich |

Verfügbar sind `mayment-tokens.css`, `mayment-base.css`, `mayment-fonts.css` und `fonts/`.

```html
<head>
  <!-- Schriften (optional, alternativ lokal per @fontsource) -->
  <link rel="stylesheet" href="https://ds.mayment.de/mayment-fonts.css">

  <!-- MayMent Design System -->
  <link rel="stylesheet" href="https://ds.mayment.de/mayment-tokens.css">
  <link rel="stylesheet" href="https://ds.mayment.de/mayment-base.css">

  <!-- App-eigene Styles (immer zuletzt) -->
  <link rel="stylesheet" href="style.css">
</head>
```

Fest auf eine Version: `https://ds.mayment.de/v1.4.0/mayment-base.css` usw.
Die frühere Einbindung über jsDelivr (`cdn.jsdelivr.net/gh/dietermayer-mayment/mayment-design-system@…`)
funktioniert technisch weiter, wird aber nicht mehr empfohlen (Übertragung an Dritte).

### Option B — lokale Kopie

Dateien herunterladen und direkt im Projekt einbinden:
```html
<link rel="stylesheet" href="mayment-fonts.css">
<link rel="stylesheet" href="mayment-tokens.css">
<link rel="stylesheet" href="mayment-base.css">
```

## Schriften

Die Schriften werden **selbst gehostet** und mit dem Design System mitgeliefert
(`mayment-fonts.css` + Ordner `fonts/`). Enthalten sind nur die benötigten Schnitte,
jeweils als woff2 in den Subsets latin und latin-ext (latin-ext lädt der Browser nur bei Bedarf):

| Familie | Schnitte | Token |
|---|---|---|
| Poppins | 600, 700 | `--font-heading` |
| Open Sans | 400, 600 | `--font-body` |
| Lato | 400, 700 | `--font-data` |
| Montserrat | 700 | `--font-brand` |

**Einbindung:** `mayment-fonts.css` und den Ordner `fonts/` gemeinsam in die App kopieren
(oder per Build/Submodul einbinden) und `mayment-fonts.css` vor `mayment-tokens.css` laden.
Die Pfade in `mayment-fonts.css` sind relativ (`fonts/<familie>/…`), der Ordner `fonts/`
muss also neben der CSS-Datei liegen.

> **DSGVO-Hinweis:** Schriften **nicht** von Google Fonts laden (`fonts.googleapis.com`,
> `fonts.gstatic.com`). Dabei überträgt der Browser die IP-Adresse jedes Besuchers ohne
> Einwilligung an Google; das LG München hat das 2022 als Datenschutzverstoß gewertet
> (Az. 3 O 17493/20). Auch das Laden der Schriften über ein fremdes CDN ist eine Übertragung an
> Dritte – deshalb lokal in der App oder über `https://ds.mayment.de/` einbinden.

**Lizenz:** Alle vier Familien stehen unter der SIL Open Font License 1.1; die Lizenztexte
liegen in `fonts/<familie>/OFL.txt` und müssen beim Kopieren mitgenommen werden.
Quelle der Dateien: [Fontsource](https://fontsource.org) (`@fontsource/<familie>`).

## Farben

| Token | Wert | Verwendung |
|---|---|---|
| `--color-primary` | `#1D3557` | Hauptfarbe, Überschriften, Hintergründe |
| `--color-secondary` | `#457B9D` | Akzente, Labels, Sekundärtext |
| `--color-accent` | `#E9C46A` | Buttons, Highlights, CTAs |
| `--color-light` | `#F8FAFC` | Helle Hintergründe |
| `--color-success` | `#10B981` | Erfolgsmeldungen |
| `--color-danger` | `#EF4444` | Fehlermeldungen |
| `--color-warning` | `#F59E0B` | Warnungen |

## Typografie

| Token | Font | Verwendung |
|---|---|---|
| `--font-heading` | Poppins | Alle Überschriften |
| `--font-body` | Open Sans | Fließtext |
| `--font-brand` | Montserrat | Logo, Branding |
| `--font-data` | Lato | Zahlen, Tabellen |

## Wichtige Klassen

### Layout
```html
<div class="container">          <!-- max-width: 1200px, zentriert -->
<div class="flex items-center gap-4">
<div class="grid md:grid-cols-3 gap-6">
```

### Farben & Hintergrund
```html
<div class="bg-primary text-white">
<div class="bg-gradient-primary text-white">
<span class="text-accent font-bold">
```

### Typografie
```html
<h1 class="text-4xl font-bold text-primary">
<p class="text-slate-500 leading-relaxed">
<span class="uppercase tracking-widest text-sm">
```

### Karten
```html
<div class="bg-white rounded-xl shadow-lg p-6">
<div class="bg-white rounded-2xl shadow-xl p-8">
```

### Buttons
```html
<button class="bg-accent text-primary font-bold px-8 py-3 rounded-lg hover:scale-105 transition-all">
<button class="bg-primary text-white font-semibold px-6 py-3 rounded-lg hover:bg-secondary transition-colors">
```

### Print
```html
<div class="no-print">   <!-- wird beim Drucken ausgeblendet -->
<div class="print-only"> <!-- wird nur beim Drucken angezeigt -->
```

## Projekt-Apps

Öffentlich erreichbare Apps, die das Design System nutzen:

| App | Repo | Einbindung |
|---|---|---|
| Reifegrad-Check Instandhaltung | `mayment-reifegrad-check-instandhaltung` | `ds.mayment.de` (main) |
| Feedback-Tool | `mayment-customer-feedback` | `ds.mayment.de` (main) |
| ECM (EU) 2019/779 Experte | `MayMent-ECM-EU-2019-779-Experte` | `ds.mayment.de/v1.2.0/` |

Dazu kommen interne Anwendungen (als Git-Submodul oder lokale Kopie eingebunden).
Apps auf dem Stand `main` erhalten jede Änderung auf `main` nach wenigen Minuten, daher sind Änderungen dort nur additiv.

## Brand-Werte anpassen

Nur `mayment-tokens.css` bearbeiten — alle Apps übernehmen die Änderung automatisch:

```css
:root {
    --color-primary: #1D3557;  /* ← hier anpassen */
}
```
