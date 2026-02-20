# MayMent Design System

Gemeinsame CSS-Basis für alle MayMent Web-Apps und die Homepage.

## Dateien

| Datei | Inhalt |
|---|---|
| `mayment-tokens.css` | Brand-Farben, Fonts, Spacing-Skala (hier anpassen) |
| `mayment-base.css` | Reset, alle Utility-Klassen, Animationen, Print-Styles |

## Einbindung

### Option A — via jsDelivr CDN (empfohlen)

```html
<head>
  <!-- Google Fonts -->
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@700&family=Poppins:wght@600;700&family=Lato:wght@400;700&family=Open+Sans:wght@400;600&display=swap" rel="stylesheet">

  <!-- MayMent Design System -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/dietermayer-mayment/mayment-design-system@main/mayment-tokens.css">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/dietermayer-mayment/mayment-design-system@main/mayment-base.css">

  <!-- App-eigene Styles (immer zuletzt) -->
  <link rel="stylesheet" href="style.css">
</head>
```

### Option B — lokale Kopie

Dateien herunterladen und direkt im Projekt einbinden:
```html
<link rel="stylesheet" href="mayment-tokens.css">
<link rel="stylesheet" href="mayment-base.css">
```

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

| App | Repo |
|---|---|
| Reifegrad-Check Instandhaltung | `mayment-reifegrad-check-instandhaltung` |
| MayMent Homepage | *(in Planung)* |

## Brand-Werte anpassen

Nur `mayment-tokens.css` bearbeiten — alle Apps übernehmen die Änderung automatisch:

```css
:root {
    --color-primary: #1D3557;  /* ← hier anpassen */
}
```
