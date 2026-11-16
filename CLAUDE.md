# CLAUDE.md – MayMent Design System

## Zweck
Gemeinsame CSS-Basis (Brand-Tokens + Utility-Klassen im Tailwind-Stil) für alle MayMent-Web-Apps und die geplante Homepage.
Reines Stylesheet-Repo: keine Laufzeit, kein Build, keine Logos (nur Farben/Fonts/Abstände).
Das Repo ist **öffentlich** (Voraussetzung für die Auslieferung über jsDelivr und für Submodule in privaten Repos).

## Wer nutzt es (Stand: November 2026)
| Projekt | Einbindung | Version |
|---|---|---|
| mayment-duktus | Git-Submodul `resources/design-system`, `@import` in `resources/css/app.css` | Submodul-Commit 85f89bf (v1.3.2) |
| mayment-firmenwissen | Git-Submodul `resources/design-system`, `@import` in `resources/css/app.css` | Submodul-Commit 85f89bf (v1.3.2) |
| mayment-reisekosten | lokale Kopie `public/css/mayment-*.css` (aktuell inhaltsgleich mit v1.3.2) | – |
| mayment-customer-feedback | jsDelivr `@main` in `index.html` | main |
| mayment-reifegrad-check-instandhaltung | jsDelivr `@main` in `index.html` | main |
| MayMent-ECM-EU-2019-779-Experte | jsDelivr `@v1.2.0` in `index.html` | v1.2.0 |

Zusätzlich lokale Kopie in `mayment-duktus/docs/entwurf/design-system/` (Entwurf).
Folge: Jede Änderung auf `main` wirkt **sofort** in den `@main`-Apps (nach jsDelivr-Cache). **Das ist so gewollt** (Entscheidung Dieter Mayer, 15.11.2026): Design-Änderungen sollen ohne Einzelfreigabe direkt in diesen Apps ankommen. Deshalb gilt: Breaking Changes vermeiden, vor dem Push auf `main` die Auswirkung auf die `@main`-Apps bedenken.

## Tech-Stack & Struktur
Nur plain CSS (Custom Properties), kein package.json, kein Node, keine Tests, kein Linter, keine CI.
```
mayment-tokens.css   :root-Variablen: Farben, Slate-Skala, Fonts, Spacing, Radius, Schatten, Transitions
mayment-base.css     Reset/Base, Utility-Klassen, Animationen, Hover/Group-Hover,
                     @media print (no-print/print-only), danach Responsive-Blöcke sm/md/lg
mayment-fonts.css    @font-face für die selbst gehosteten Schriften (relative Pfade auf fonts/)
fonts/<familie>/     woff2-Dateien + OFL.txt je Schriftfamilie
README.md            Einbindung, Farb-/Font-Tabellen, Klassenbeispiele, Liste der Apps
```
- `mayment-base.css` setzt `mayment-tokens.css` voraus (Reihenfolge: tokens, base, App-CSS).
- Fonts (Poppins, Open Sans, Lato, Montserrat) werden ab v1.4.0 **selbst gehostet** mitgeliefert: `mayment-fonts.css` + `fonts/<familie>/` (woff2, latin/latin-ext, OFL.txt). Kein Google Fonts (DSGVO).
- Breakpoints: sm 640px, md 768px, lg 1024px (nur als Kommentar in tokens; CSS-Variablen gehen in `@media` nicht).

## Befehle
Es gibt nichts zu installieren oder zu bauen. Prüfen = eine Test-HTML-Seite mit beiden Dateien im Browser öffnen,
oder in einer Consumer-App gegentesten (z. B. Submodul in Duktus/Firmenwissen auf den neuen Commit setzen).

## Versionierung / Release
- SemVer über **Git-Tags** `vX.Y.Z` auf `main` (bisher v1.1.0 … v1.3.2; Tags mit Nachricht, z. B. `git tag -a v1.3.3 -m "..."`).
- Die Versionsnummer steht sonst nur im Header-Kommentar von `mayment-base.css` (CDN-Beispiel `@vX.Y.Z`) – beim Release mit anpassen.
- Commit-Stil: `feat: ... , bump to vX.Y.Z` / `fix: ...`.
- „Veröffentlichen" = Commit + Tag auf GitHub pushen; jsDelivr liefert dann `@vX.Y.Z` bzw. `@main` aus. Kein npm-Paket.
- Consumer aktualisieren ist Sache der jeweiligen App (Submodul-Pointer, Kopie, CDN-Tag).

## Konventionen & Stolperfallen
- **Kaskadenregel:** Alle `@media`-Breakpoint-Blöcke (sm:, md:, lg:) stehen am **Ende** von `mayment-base.css`, nach allen Base-Utilities –
  sonst überschreiben z. B. `.lg:flex` die Basisklasse `.hidden` nicht (war Fix v1.2.0). Neue Responsive-Utilities in den passenden Endblock.
- Klassennamen mit Doppelpunkt sind im CSS escaped (`.md\:grid-cols-3`).
- Brand-Werte nur in `mayment-tokens.css` ändern; Utilities referenzieren `var(--…)`.
- Nur **additive** Änderungen in Minor/Patch; Umbenennen/Entfernen von Klassen oder Tokens bricht `@main`-Consumer.
- README-Tabelle „Projekt-Apps“ nennt nur die öffentlich erreichbaren Apps; interne Projekte nicht namentlich ins öffentliche README.
- Umgebungsvariablen/Secrets: keine im Projekt; Repo ist öffentlich, also niemals Zugangsdaten oder interne Infos committen.

## Anforderungen & Dokumentation
Anforderungen kommen von der **MayMent IT-Strategie** und liegen dort. `Nextcloud-dev` ist für Entwickler **nur lesbar** – dort nichts ablegen oder ändern.
Ein eigener Baubrief/Auftrag für das Design System existiert nicht; relevante Fundstellen:
- `Nextcloud-dev/00_Gesamt/MayMent_IT-Gesamtarchitektur_V1.6.html` – Grundsatz „Ein Design, ein Betrieb“ (nennt noch v1.2.0)
- `Nextcloud-dev/00_Gesamt/MayMent_Technologie-Register_V1.0.html` – Eintrag „Design-System (quer)“
- `Nextcloud-dev/02_Dokumentationen/01_Konzepte/MayMent_Code-Ablage-Konvention_V1.0.html` – Code nur in Git/GitHub-Org `dietermayer-mayment`, nie in Nextcloud; keine Secrets im Repo
- `Nextcloud-dev/02_Dokumentationen/01_Konzepte/Baubrief_Firmenwissen_V2.6.md` – v1.3.2 als Submodul; HTML-Templates auf Basis des Design Systems
- `Nextcloud-dev/02_Dokumentationen/01_Konzepte/konzept-sprachassistent.md` – Duktus, Design System v1.3.2 verbindlich
- `Nextcloud-dev/02_Dokumentationen/01_Konzepte/Baubrief_Buchaltungs_app.md` und `Kickoff_Buchhaltung_V1.0.md` – v1.2.0, Dokument-Rendering mit Design System
- `Nextcloud-dev/02_Dokumentationen/01_Konzepte/Baukonzept_Nextcloud_V1.0.md` – Nextcloud-Theming nach Design-System-Farben; Logos liegen **nicht** im Repo
## Rollen & Zusammenarbeit

- Anforderungen kommen von der **MayMent IT-Strategie** und liegen in der internen Dokumentenablage (nur lesbar für Entwickler).
- Ein **Entwicklungsleiter** koordiniert alle MayMent-Projekte; dieser **Projekt-Entwickler** setzt in diesem Repo um.
- Änderungen auf `main` wirken sofort in mehreren Apps: vorher mit dem Entwicklungsleiter abstimmen, wenn sie mehr als additiv sind.
- Widersprüche zwischen Code und Doku nicht selbst in der Doku beheben, sondern melden.
- **Dieses Repo ist öffentlich:** keine internen Hostnamen, Adressen, Zugangsdaten oder vertraulichen Inhalte einchecken.
- **Fehler sammeln:** Eigene Fehler ehrlich und zeitnah im zentralen Fehlerlog der Entwicklungsleitung eintragen (internes Repo, siehe Entwicklungsleiter). Keine Interna in diesem öffentlichen Repo.
