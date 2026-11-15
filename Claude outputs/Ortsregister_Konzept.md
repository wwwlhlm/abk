# Ortsregister – Konzept

Stand: 15. November 2026 · in `abk/index.html` umgesetzt (Fenster „Orte“)

## Ziel

Alle Orte im Tagebuch sollen Zusatzinfos bekommen (Typ, Genauigkeit, Koordinaten), ohne das Frontmatter der Tagesdateien zu verkomplizieren. Das gilt für die **Kaffeeorte** (`coffee_locations`) genauso wie für die **spezifischen Orte** (`places`). Dafür gibt es ein separates Register, das über den Namen mit den Tagen verknüpft ist.

## Entscheidungen

- **Frontmatter bleibt unverändert.** Der Name, wie er heute in `coffee_locations` und `places` steht (`Miro`, `Bonheur`, `Letzi`, `Pragelpass`), ist der Schlüssel.
- **Ein Register für beide Listen.** Kaffeeorte und spezifische Orte werden gleich behandelt: gleiche Felder, gleiche Funktionen. Ein Ort, der in beiden Listen vorkommt, hat nur einen Eintrag.
- **Gepflegt wird das Register in der App**, in einem eigenen Fenster, das aus den Details geöffnet wird.
- **Koordinaten kommen nach und nach dazu.** Nichts ist Pflicht, ein Name ohne Eintrag funktioniert weiter wie heute.
- **Umbenennen und Zusammenführen** müssen beide möglich sein.
- **Typ und Genauigkeit sind getrennt.** Der Typ sagt, *wo* der Kaffee getrunken wurde (Café, zuhause …). Die Genauigkeit sagt, *wie genau* der Name die Lage angibt (genauer Ort, irgendwo in einer Ortschaft, irgendwo in einem Land).
- **Ortschaften sind ein eigener Teil des Registers.** Ihre Namen kommen aus dem Feld `location` der Tage (Zurich, Berlin …). Land und Position einer Ortschaft werden einmal gesetzt und gelten für alle Orte dort.
- **Späteres Ziel:** eine interaktive Karte auf der eigenen Webseite. Dazu Auswertungen, z. B. Kaffee im Café, zuhause oder bei Freund\*innen.

## Datenlage (Index, 2558 Tage)

- 220 verschiedene Kaffeeorte und 11 spezifische Orte (z. B. Letzi, RoteFabrik, Pragelpass, Schwägalp, Musenalp). Nur `Musenalp` kommt in beiden Listen vor.
- Die Namen sind konsequent geschrieben, ohne Leerzeichen, mit Unterstrich (`Home_Berlin`, `Studio_Way`). Echte Doppelungen sind kaum vorhanden.
- `_div` ist eine eigene Konvention für „irgendwo, nicht genauer“. Bei Kaffeeorten heisst `ZH_div`: in einem nicht bestimmten Café in Zürich. `ZH_div` und `CH_div` sind deshalb keine Doppelten, sondern unterschiedlich genau.
- **Ortschaften als Kaffeeort** (z. B. `Berlin`) heissen ebenfalls: in einem nicht bestimmten Café in dieser Ortschaft. Sie sind damit dasselbe wie `ZH_div`, nur ohne Suffix.
- Die Namen mischen verschiedene Arten von Orten:
  - Cafés: Miro, Bonheur, CSTJ, Lalere
  - Unbestimmte Cafés: ZH_div, CH_div, Berlin
  - Arbeitsorte: Bodara, Lab
  - Zuhause: Home, Home_Berlin, Home_Rügen
  - Kaffee bei Freund\*innen, heute nicht unterschieden
  - Spezifische Orte: Pässe, Berge, Veranstaltungsorte, Quartiere

## Datei

`orte.json` im Diary-Repo, neben `search-index.json`. Gelesen und geschrieben wird über denselben GitHub-Weg wie der Index: vor dem Speichern die aktuelle SHA holen, nur schreiben, wenn sich etwas geändert hat.

```json
{
  "v": 1,
  "orte": {
    "Miro":        { "typ": "café",    "koord": [47.3769, 8.5417], "ortschaft": "Zurich" },
    "ZH_div":      { "typ": "café",    "genau": "ortschaft", "ortschaft": "Zurich" },
    "Berlin":      { "typ": "café",    "genau": "ortschaft", "ortschaft": "Berlin" },
    "CH_div":      { "typ": "café",    "genau": "land", "land": "CH" },
    "Eltern":      { "typ": "privat",  "ortschaft": "Bern" },
    "Bodara":      { "typ": "arbeit",  "koord": [47.38, 8.53], "ortschaft": "Zurich" },
    "Home_Berlin": { "typ": "zuhause", "koord": [52.52, 13.40], "ortschaft": "Berlin" },
    "Pragelpass":  { "typ": "ort",     "art": "Pass", "koord": [47.00, 8.95] },
    "Letzi":       { "typ": "ort",     "art": "Veranstaltung", "koord": [47.38, 8.50], "ortschaft": "Zurich" },
    "Lalere":      { "alias": "Lalère" }
  },
  "ortschaften": {
    "Zurich": { "land": "CH", "koord": [47.3769, 8.5417] },
    "Berlin": { "land": "DE", "koord": [52.52, 13.405] }
  },
  "ignoriert": [["Kafi_Zähringer", "Café_Zähringer"]]
}
```

Alle Felder ausser dem Namen sind optional.

### Felder

- **typ** – wo: `café` · `arbeit` · `zuhause` · `privat` (bei Freund\*innen) · `ort`
  - `ort` ist der Normalfall für spezifische Orte.
  - Die früher geplanten Typen „stadt“ und „diverse“ entfallen, sie werden über `genau` und `ortschaft` abgebildet.
- **art** – nur bei `typ: ort`, freier Text mit Vorschlägen: Berg, Pass, Veranstaltung, Quartier. Bereits benutzte Arten werden wieder vorgeschlagen, eigene können dazukommen.
- **genau** – wie genau der Name die Lage angibt:
  - fehlt = genauer Ort (Normalfall), mit eigenen Koordinaten
  - `ortschaft` = irgendwo in einer Ortschaft. Keine eigenen Koordinaten, es gilt die Position der Ortschaft.
  - `land` = irgendwo in einem Land. Keine Koordinaten, dafür `land`.
- **koord** – `[lat, lon]`, auf 4 Nachkommastellen gerundet (ca. 10 m). Nur bei genauen Orten.
- **ortschaft** – Name wie im Feld `location` der Tage (z. B. `Zurich`). Gewählt mit derselben Chip-Darstellung wie beim Erfassen, inkl. Land-Kürzel. Bei genauen Orten optional, bei `genau: ortschaft` nötig.
- **land** – nur bei `genau: land` (z. B. `CH`). Sonst kommt das Land von der Ortschaft.
- **oeffentlich** – `false` blendet einen Ort auf der Karte aus.
- **alias** – Verweis auf einen anderen Namen (siehe Zusammenführen).
- **ortschaften** (eigener Teil) – pro Ortschaft `land` und `koord`. Das Land wird vorgeschlagen aus den Tagen (welches Land meist zu diesem Ort steht) und kann überschrieben werden.
- **ignoriert** (oben in der Datei) – Paare, die als „Kein Duplikat“ markiert wurden und nicht mehr vorgeschlagen werden.

**Bei Freund\*innen (`privat`)** hat eine Ortschaft, aber nie Koordinaten und erscheint nie auf der Karte.

**Alte Einträge** mit `stadt` bzw. `genau: stadt` werden beim Laden automatisch umgestellt; Schreibweisen wie „Zürich“ werden an die Ortschaft „Zurich“ aus den Tagen angeglichen.

## Umbenennen und Zusammenführen

- **Umbenennen:** Ein Ort bekommt einen neuen Namen, und es gibt noch keinen Ort mit diesem Namen. Der Name wird in allen betroffenen Tagen und im Register ersetzt, in beiden Listen (`coffee_locations` und `places`). Gibt es den Namen schon, bietet die App stattdessen Zusammenführen an.
- **Zusammenführen:** Zwei Namen meinen denselben Ort.
  1. *Als Verweis (Standard).* Im Register steht `"Lalere": { "alias": "Lalère" }`. App, Dashboard, Suche und Vorschläge behandeln beide als einen Ort, vorgeschlagen wird nur noch der Zielname. Es gibt keine Schreibvorgänge, und es lässt sich jederzeit rückgängig machen.
  2. *Überall ersetzen.* Der falsche Name wird in allen Tagen durch den Zielnamen ersetzt, damit es auch in Obsidian sauber ist. Technisch ist das dasselbe wie Umbenennen.
- **Angaben beim Zusammenführen:** Hat der Zielort schon Typ oder Position, gelten diese. Fehlt ihm etwas, übernimmt er es vom anderen Namen.
- **Für beide gilt beim Ersetzen:** ein Schreibvorgang pro betroffenem Tag, deshalb immer mit Vorschau („5 Tage werden geändert“) und auf ausdrücklichen Knopfdruck. Steht ein Name nach dem Ersetzen doppelt an einem Tag, bleibt er nur einmal stehen. Der Suchindex wird danach mitaktualisiert.
- **Ablauf, damit ein Abbruch nichts kaputt macht:**
  1. Im Register wird der alte Name sofort als Verweis auf den neuen eingetragen. Die App behandelt damit beide schon als einen Ort.
  2. Die Tage werden nacheinander geschrieben, mit Fortschritt und Abbrechen-Knopf.
  3. Erst am Schluss wird der Verweis entfernt und der Suchindex aktualisiert.
  4. Bei einem Abbruch oder Fehler bleibt der Verweis stehen. Der Zustand ist stimmig und lässt sich später fortsetzen.

## Ortschaften: eine Verknüpfung für alles

- Eine Ortschaft (z. B. `Zurich`) ist **ein** Eintrag im Register. Die Tage (Feld `location`) und die Orte (Feld `ortschaft`) zeigen beide darauf.
- Land und Position werden einmal gesetzt und gelten überall: beim Erfassen (Land-Vorschlag), in der Ortsübersicht, für Orte „irgendwo in …“ und später auf den Karten.
- **Die Tagesdateien bleiben unverändert.** Die Verknüpfung entsteht nur über den Namen, im Frontmatter kommt nichts dazu.
- **Entschieden:** Die Positionen der Ortschaften speisen später auch eine Karte „wo war ich“ aus dem Feld `location` der Tage. Die Tageskoordinaten (`coordinates:`) bleiben davon getrennt.

## In der App

Eigenes Fenster **„Orte“**, aus den Details geöffnet („Orte verwalten“). Wegen der Suche ist es eine eigene Seite wie die Suche, kein Sheet. Oben umschaltbar zwischen **Orte** und **Ortschaften**.

- **Ortschaften:** alle Namen aus dem Feld Ort der Tage, mit Anzahl Tage, Land, Position und Anzahl verknüpfter Orte. Filter: ohne Position, ohne Land, mit Orten. Antippen öffnet die Ortschaft: Land, Position und die Liste der Orte dort.
- **Beim Erfassen** steht unter „Ort“ ein Hinweis „Naples · Position setzen“, solange eine Ortschaft keine Position hat. Im Ort-Editor öffnet das Antippen der Ortschaft dieselbe Ortschaft.

- **Ortsliste:** alle Namen aus beiden Listen nach Häufigkeit, mit Anzahl Tage, Typ (und Art), Genauigkeit, Herkunft (Kaffeeort, spezifischer Ort oder beides) und einem Zeichen, ob Koordinaten vorhanden sind. Verweise erscheinen beim Zielort („+ Lalere“). Mit Suche und Filtern: ohne Koordinaten, ohne Typ, Ortschaft/Land, Kaffeeorte, spezifische Orte, nach Typ. Die Zahlen kommen direkt aus dem Suchindex.
- **Ort bearbeiten:** Typ wählen (bei „Ort“ zusätzlich die Art), Genauigkeit wählen, Position setzen. Weil eine Karte das spätere Ziel ist, kann eine Karte zum Antippen später dazukommen.
- **Position:** ein einziges Feld, das den aktuellen Wert zeigt. Überschreiben, Koordinaten oder einen Karten-Link einfügen (Google Maps, OpenStreetMap) oder leeren, um die Position zu entfernen. Gespeichert wird beim Verlassen des Felds bzw. mit Enter, Escape stellt den alten Wert wieder her. Daneben ein Knopf für den aktuellen Standort, darunter „Auf der Karte ansehen“. Gleich für Orte und Ortschaften.
- **Umbenennen …** und **Zusammenführen mit …**, jeweils mit Vorschau (z. B. „Lalere (5 Tage) → Lalère (12 Tage)“).
- **Doppelte erkennen:** ähnliche Schreibweisen vorschlagen (Gross-/Kleinschreibung, Akzente, Unterstrich, ein vertippter Buchstabe). Paare aus zwei `_div`-Namen werden nie vorgeschlagen. „Kein Duplikat“ merkt sich das Paar in `ignoriert`.
- **Neue Namen:** dezenter Hinweis in den Details, „Miro · Ort zuordnen“, für Kaffeeorte und spezifische Orte gleich. Kein Pflichtschritt.

## Entfernen, was nicht mehr vorkommt

- Ein **Ort**, der in keinem Tag mehr vorkommt, kann aus dem Register entfernt werden, samt Verweisen auf ihn und seinen „Kein Duplikat“-Paaren.
- Eine **Ortschaft** kann entfernt werden, wenn sie in keinem Tag und bei keinem Ort mehr vorkommt. Hängt noch ein Ort daran, muss dort zuerst eine andere Ortschaft gewählt werden.
- **Einzeln** im Editor (Karte „Nicht mehr verwendet“) oder **gesammelt** über den Filter „Nicht mehr verwendet“ mit „Alle N entfernen“.
- Immer zweistufig: erster Tipp fragt nach („Wirklich entfernen?“), erst der zweite löscht.
- Gelöscht wird nur der Eintrag in `orte.json`. Die Tagesdateien bleiben unberührt, der alte Stand bleibt im Git-Verlauf.

## Karte auf der Webseite (später)

- Die Karte bekommt einen eigenen, gefilterten Export, z. B. `data/orte.json` im öffentlichen Repo. Sie liest nie das Register direkt.
- **Öffentlich** sind nur Orte mit Koordinaten und den Typen café, arbeit und ort. Zuhause und privat werden nie exportiert, ebenso Orte mit `genau: land`. Pro Ort kann es zusätzlich `"oeffentlich": false` geben, um einzelne Orte auszublenden.
- **Darstellung:** Genaue Orte als Punkt. Orte mit `genau: ortschaft` als Markierung der Ortschaft (z. B. Kreis mit „18 Tage in Cafés“), damit sie nicht wie ein bestimmtes Café im Zentrum aussehen.
- **Mögliche Inhalte pro Ort:** Name, Typ, Art, Genauigkeit, Koordinaten, Anzahl Besuche, erster und letzter Besuch, vielleicht nach Jahr filterbar. Keine Tagestexte und keine Personen.
- Veröffentlicht wird über den bestehenden Ablauf in Synchronisieren: Prüfen, dann Veröffentlichen.

## Auswertungen

- Gezählt wird nach **Typ**, unabhängig von der Genauigkeit: `ZH_div`, `Berlin` und `Miro` sind alle Kaffee im Café.
- Die Genauigkeit spielt nur für die Karte eine Rolle.
- Über `ortschaft` lässt sich zusätzlich nach Ortschaft und Land auswerten, auch für Kaffee bei Freund\*innen.

## Abgrenzung

- **Tageskoordinaten** (`coordinates:` im Frontmatter) haben kein Eingabefeld mehr in der App; Orte und Ortschaften decken das ab. Vorhandene Werte bleiben in den Tagesdateien und werden beim Speichern unverändert zurückgeschrieben, das Feld `coordinates:` bleibt in der Struktur. Das Register sagt „wo liegt dieser Ort“, die Karte „wo war ich“ entsteht aus den Ortschaften im Feld `location`.
- **Datenschutz:** Orte vom Typ „privat“ bekommen keine Koordinaten. Zuhause-Orte können Koordinaten haben, landen aber nie auf der Webseite. Das Register selbst bleibt im privaten Diary-Repo. Der bestehende Jahres-Export enthält keine Orte und ist nicht betroffen.

## Technische Notizen

- Im Suchindex sind `coffeeLocations` und `places` mit Leerzeichen getrennt. Das funktioniert, weil die Namen keine Leerzeichen enthalten. Beim Umbenennen deshalb keine Leerzeichen zulassen (oder den Index auf Listen umstellen).
- Verweise (Alias) überall auflösen: Dashboard-Verteilung, Suche, Vorschläge beim Erfassen.
- Beim Ersetzen über viele Tage die Schreibvorgänge nacheinander ausführen, mit Fortschritt und der Möglichkeit, bei einem Fehler sauber abzubrechen (Ablauf siehe oben).
- Wechselt ein Ort auf „privat“, auf `genau: ortschaft` oder auf `genau: land`, werden seine eigenen Koordinaten gelöscht.
- Beim Ersetzen in den Tagen bleiben bei Kaffee alle Einträge stehen (jede Zeile zählt einen Kaffee); nur bei den spezifischen Orten wird ein doppelter Eintrag auf einen reduziert.

## To-do

- **`coordinates:` aus den Tagesdateien entfernen.** Das Feld wurde nie genutzt, Orte und Ortschaften liegen jetzt in `orte.json`. Dafür müsste man:
  - prüfen, ob irgendeine Tagesdatei doch einen Wert hat, und diese Werte vorher ins Register übernehmen,
  - die Zeile `coordinates:` aus allen Tagesdateien löschen (einmaliger Durchlauf, ein Schreibvorgang pro Tag, mit Vorschau wie beim Umbenennen),
  - die Vorlage für neue Tage (App und Obsidian-Template) und `buildDay` in der App anpassen, damit das Feld nicht wieder entsteht,
  - prüfen, ob der Jahres-Export, `parsen` oder die alte Erfassen-Seite das Feld noch lesen.

## Offen

- Welche Auswertungen neben der Karte konkret gewünscht sind.
- Ob die Vorschläge für „Art“ so passen oder anders heissen sollen.

