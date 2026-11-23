# CLAUDE.md – MayMent Design System

## Zweck
Gemeinsame CSS-Basis (Brand-Tokens + Utility-Klassen im Tailwind-Stil) für alle MayMent-Web-Apps und die geplante Homepage.
Reines Stylesheet-Repo: keine Laufzeit, kein Build, keine Logos (nur Farben/Fonts/Abstände).
Das Repo ist **öffentlich** (Voraussetzung für die Auslieferung über `https://ds.mayment.de/`, früher jsDelivr, und für Submodule in privaten Repos).
Auslieferung: `ds.mayment.de` holt alle 5 min `main` und alle Tags aus diesem Repo (nur die drei CSS-Dateien und `fonts/`);
`/` = Stand `main` (5 min Cache), `/vX.Y.Z/` = Tag (1 Jahr, immutable).

## Wer nutzt es
- **Öffentliche Web-Apps** binden das Design System über `https://ds.mayment.de/` ein: teils mit Stand `main`, teils fest auf
  eine Version (`/vX.Y.Z/`).
- **Interne Apps** nutzen es als Git-Submodul oder als lokale Kopie.
- Die vollständige Liste mit Versionen steht nur in der internen Dokumentation (nicht in diesem öffentlichen Repo).

Folge: Jede Änderung auf `main` wirkt **sofort** in den `main`-Apps (nach ca. 5 min Abholung + 5 min Cache). **Das ist so gewollt** (Entscheidung Dieter Mayer, 15.11.2026): Design-Änderungen sollen ohne Einzelfreigabe direkt in diesen Apps ankommen. Deshalb gilt: Breaking Changes vermeiden, vor dem Push auf `main` die Auswirkung auf die `@main`-Apps bedenken.

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
oder in einer nutzenden App gegentesten (z. B. Submodul auf den neuen Commit setzen bzw. die `main`-Apps ansehen).

## Versionierung / Release
- SemVer über **Git-Tags** `vX.Y.Z` auf `main` (bisher v1.1.0 … v1.3.2; Tags mit Nachricht, z. B. `git tag -a v1.3.3 -m "..."`).
- Die Versionsnummer steht sonst nur im Header-Kommentar von `mayment-base.css` (Einbindungsbeispiel `ds.mayment.de/vX.Y.Z/`) – beim Release mit anpassen.
- Commit-Stil: `feat: ... , bump to vX.Y.Z` / `fix: ...`.
- „Veröffentlichen" = Commit + Tag auf GitHub pushen; `ds.mayment.de` liefert dann `/vX.Y.Z/` bzw. `main` aus (jsDelivr technisch weiter, aber nicht mehr empfohlen). Kein npm-Paket.
- Consumer aktualisieren ist Sache der jeweiligen App (Submodul-Pointer, Kopie, Versionspfad).

## Konventionen & Stolperfallen
- **Kaskadenregel:** Alle `@media`-Breakpoint-Blöcke (sm:, md:, lg:) stehen am **Ende** von `mayment-base.css`, nach allen Base-Utilities –
  sonst überschreiben z. B. `.lg:flex` die Basisklasse `.hidden` nicht (war Fix v1.2.0). Neue Responsive-Utilities in den passenden Endblock.
- Klassennamen mit Doppelpunkt sind im CSS escaped (`.md\:grid-cols-3`).
- Brand-Werte nur in `mayment-tokens.css` ändern; Utilities referenzieren `var(--…)`.
- Nur **additive** Änderungen in Minor/Patch; Umbenennen/Entfernen von Klassen oder Tokens bricht `@main`-Consumer.
- README-Tabelle „Projekt-Apps“ nennt nur die öffentlich erreichbaren Apps; interne Projekte nicht namentlich ins öffentliche README.
- Umgebungsvariablen/Secrets: keine im Projekt; Repo ist öffentlich, also niemals Zugangsdaten oder interne Infos committen.

## Anforderungen & Dokumentation
Anforderungen und die interne Dokumentation (Steckbrief, Einbindungen, Technik & Betrieb, Änderungsprotokoll) liegen in der
internen Dokumentenablage von MayMent, nicht in diesem öffentlichen Repo. Für Entwickler nur lesbar.
## Rollen & Zusammenarbeit

- Anforderungen kommen von der **MayMent IT-Strategie** und liegen in der internen Dokumentenablage (nur lesbar für Entwickler).
- Ein **Entwicklungsleiter** koordiniert alle MayMent-Projekte; dieser **Projekt-Entwickler** setzt in diesem Repo um.
- Änderungen auf `main` wirken sofort in mehreren Apps: vorher mit dem Entwicklungsleiter abstimmen, wenn sie mehr als additiv sind.
- Widersprüche zwischen Code und Doku nicht selbst in der Doku beheben, sondern melden.
- **Dieses Repo ist öffentlich:** keine internen Hostnamen, Adressen, Zugangsdaten oder vertraulichen Inhalte einchecken.
- **Fehler sammeln:** Eigene Fehler ehrlich und zeitnah im zentralen Fehlerlog der Entwicklungsleitung eintragen (internes Repo, siehe Entwicklungsleiter). Keine Interna in diesem öffentlichen Repo.
