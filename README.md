# Stammtischfahrten

Eine einfache Webseite zur Chronik unserer jährlichen Stammtischfahrten – *est. 09*.
Wer hat wann wo ausgerichtet, welches **Ausrichterspiel** hat den nächsten Gastgeber bestimmt, wo liegt das **Programm-PDF** – und auf einer stilisierten Deutschlandkarte, wo wir schon überall waren.

---

## Überblick

Die Anwendung ist eine schlanke statische Webseite aus wenigen Dateien – kein Server, kein Build, keine Frameworks.
Die Chronik wird zentral aus einer **`data.json`** geladen, sodass alle Besucher denselben Stand sehen.
Änderungen lassen sich direkt im Browser vornehmen und als `data.json` exportieren, um sie ins Repo zu übernehmen.

## Features

- **Chronik-Tabelle** mit den Spalten *Wann · Wer · Wo · Ausrichterspiel*
- **Deutschlandkarte** mit den besuchten Orten – Punkte in der Farbe der Rotation, Hover/Tap zeigt Jahr, Ort, Ausrichter und Spiel
- **Zentrale Daten** aus `data.json` – alle sehen denselben Stand (ideal für GitHub Pages)
- **Direkt editierbar** im Browser: Einträge hinzufügen, bearbeiten und löschen
- **Programm-PDFs**: Jahre mit hinterlegtem PDF werden als Link dargestellt und öffnen `Programme/JJJJ.pdf`
- **Statusanzeige**: zeigt an, ob der zentrale Stand aktiv ist oder ungespeicherte lokale Änderungen bestehen
- **Export / Import** der kompletten Liste als `data.json`
- **Rotationen farblich hervorgehoben**
  - 1. Rotation · 2011–2016
  - 2. Rotation · 2017–2024
  - 3. Rotation · seit 2025
- Dunkelblaues Design mit dem Gold des Logos, reduzierte, stilisierte SVG-Icons
- Responsiv – auf dem Smartphone im Hochformat ist die Tabelle horizontal scrollbar

## Nutzung

1. Repository klonen
2. Die Dateien über einen Webserver ausliefern (nötig, damit `data.json` per `fetch` geladen werden kann)

```bash
git clone <repo-url>
cd stammtischfahrten
python3 -m http.server 8000
# danach http://localhost:8000 im Browser aufrufen
```

> **Hinweis:** Beim reinen Doppelklick (Öffnen per `file://`) kann der Browser aus Sicherheitsgründen die `data.json` nicht laden.
> Die Seite fällt dann auf einen eingebauten Ausgangsdatensatz zurück. Für den echten Betrieb daher GitHub Pages oder einen lokalen Webserver nutzen.

## Daten pflegen (zentral über `data.json`)

Da GitHub Pages nur statische Dateien ausliefert, erfolgt das dauerhafte Speichern über einen Commit:

1. In der Seite die Einträge **bearbeiten** (Hinzufügen / Ändern / Löschen). Die Statusleiste wechselt auf *Lokale Änderungen*.
2. Button **„data.json exportieren"** klicken – es wird eine fertige `data.json` heruntergeladen.
3. Diese Datei im Repo ersetzen und committen (am schnellsten direkt auf GitHub: Datei öffnen → **Edit** → **Commit changes**).
4. Beim nächsten Aufruf sehen **alle** den neuen Stand.

Mit **„Lokale Änderungen verwerfen"** lädt man jederzeit wieder den aktuellen Repo-Stand.

### Statusanzeige

| Zustand | Bedeutung |
|---------|-----------|
| 🟢 **Zentraler Stand** | Anzeige entspricht der `data.json` im Repo. |
| 🟡 **Lokale Änderungen** | Es gibt Änderungen, die nur im Browser liegen und noch nicht ins Repo übernommen wurden. |

## Programm-PDFs

Zu einzelnen Fahrten kann ein Programm-PDF hinterlegt werden.

- **Ablage:** Die PDFs liegen im Ordner **`Programme/`** und heißen nach dem Jahr, z. B. `Programme/2018.pdf`.
- **Verlinkung:** Ob ein Jahr verlinkt wird, steuert das Feld **`"pdf": true`** des jeweiligen Eintrags in der `data.json`.
  Verlinkte Jahre erscheinen unterstrichen mit kleinem PDF-Icon und öffnen die Datei in einem neuen Tab.
- **Pflege ohne Code-Änderung:** Im Bearbeiten-Dialog gibt es die Checkbox **„Programm-PDF vorhanden"**.
  Anhaken → `data.json` exportieren → committen. Die `index.html` muss dafür nicht angefasst werden.

> **Wichtig:** Das Häkchen erzeugt nur den *Link*. Die PDF selbst muss vorher als `Programme/JJJJ.pdf` ins Repo hochgeladen werden,
> sonst führt der Link ins Leere. GitHub Pages ist zudem case-sensitive – Ordner (`Programme`) und Dateinamen (`2018.pdf`) exakt so schreiben.

## Karte der besuchten Orte

Unter der Tabelle zeigt eine vereinfachte Deutschland-Silhouette alle Orte, für die Koordinaten hinterlegt sind.

- **Punkte** sind in der Farbe der jeweiligen Rotation eingefärbt; per Maus (Desktop) oder Tippen (Mobil) erscheint ein Tooltip mit Jahr, Ort, Ausrichter und Spiel.
- **Koordinaten** stehen direkt in der `data.json` als Felder **`"lon"`** (Längengrad) und **`"lat"`** (Breitengrad).
  Die Anwendung ist damit selbsttragend – es gibt keine Ortstabelle mehr im Code.
- **Pflege ohne Code-Änderung:** Im Bearbeiten-Dialog gibt es die optionalen Felder **Längengrad/Breitengrad**.
  Werte eintragen → exportieren → committen, und der Punkt erscheint.
- **Koordinaten finden:** z. B. in Google Maps per Rechtsklick auf den Ort → die beiden Zahlen sind *Breitengrad (lat), Längengrad (lon)* – in der Maske entsprechend eintragen.
- Einträge ohne Koordinaten (oder ohne Ort, z. B. 2020 „Corona") erscheinen nicht als Punkt, sondern werden dezent unter der Karte als **„Ohne Kartenposition"** aufgeführt.

## Datenformat

```json
{
  "app": "stammtischfahrten",
  "version": 4,
  "fahrten": [
    { "year": 2018, "who": "Lars", "where": "Quedlinburg", "game": "Bogenschiessen", "pdf": true, "lon": 11.151, "lat": 51.789 },
    { "year": 2020, "who": "", "where": "", "game": "", "note": "Corona", "pdf": false }
  ]
}
```

| Feld    | Typ     | Beschreibung                                                      |
|---------|---------|------------------------------------------------------------------|
| `year`  | Zahl    | Jahr der Fahrt (bestimmt zugleich die Rotation)                  |
| `who`   | Text    | Ausrichter (kann leer sein)                                      |
| `where` | Text    | Ort (kann leer sein)                                             |
| `game`  | Text    | Ausrichterspiel, das den nächsten Gastgeber bestimmt hat         |
| `pdf`   | Boolean | `true`, wenn unter `Programme/JJJJ.pdf` ein Programm liegt       |
| `lon`   | Zahl    | Längengrad für den Kartenpunkt (optional)                        |
| `lat`   | Zahl    | Breitengrad für den Kartenpunkt (optional)                       |
| `note`  | Text    | Optional, z. B. `"Corona"` für ausgefallene Jahre                |

## Spielregel

Es entscheidet immer ein **Ausrichterspiel**, wer als Nächstes ausrichten muss –
bis alle einmal dran waren, dann geht es von vorne los. Jeder Durchlauf = eine Rotation.

## Anpassen

- **Zentrale Daten**: Die Chronik steht in `data.json` (inkl. `pdf`, `lon`, `lat`). Der Ausgangsdatensatz `FALLBACK_DATA` in `index.html` dient nur als Notfall-Fallback (z. B. bei `file://`).
- **Rotationsgrenzen**: Die Funktion `rotationClass(year)` in `index.html` legt fest, welches Jahr zu welcher Rotation gehört.
- **Karten-Silhouette**: Das Array `OUTLINE` und die Projektionswerte (`P_LON_MIN`, `P_LAT_MAX`, `P_KX`, `P_KY`) in `index.html` bestimmen Umriss und Maßstab der Karte.
- **Farben**: Die CSS-Variablen `--gold`, `--navy-*`, `--rot1/2/3` sowie `--rot1/2/3-bright` (Kartenpunkte) oben im `:root`-Block steuern das Farbschema.
- **Logo**: Wird als externe Datei `logo.png` neben der `index.html` geladen.

## Technik

- Reines HTML, CSS und Vanilla JavaScript – keine Frameworks
- Icons und Deutschlandkarte als Inline-SVG
- Logo als externe `logo.png`
- Chronik zentral in `data.json`, Zwischenspeicherung im Browser via `localStorage`

## Projektstruktur

```
stammtischfahrten/
├── index.html        # Anwendung (Oberfläche + Logik + Karte)
├── data.json         # zentrale Chronik inkl. Koordinaten (wird beim Laden gelesen)
├── logo.png          # Logo (extern eingebunden)
├── Programme/        # Programm-PDFs, benannt nach Jahr
│   ├── 2018.pdf
│   ├── 2019.pdf
│   └── ...
└── README.md
```

## Lizenz

Privates Projekt für die Stammtischrunde. Nutzung und Anpassung im Kreis frei.
