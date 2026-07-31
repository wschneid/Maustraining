# Plan: Tastatur-Führerschein – Teil 1 „Tasten-Schule" & Teil 2 „Zehn-Finger-Training"

Ein interaktiver Lern-Trainer, der Kindern die **Tastatur** beibringt – im selben
Stil und mit derselben Technik wie die vorhandenen Trainer
`maus-fuehrerschein-4.html` und `word-fuehrerschein.html`.

> Dieses Dokument ist **nur der Plan**. Es enthält bewusst keinen Code.

---

## ✅ Festgelegte Entscheidungen (Stand 2026-07-03)

- **Phase 0 ist umgesetzt** (Commit: sichtbare Zweiteilung Teil 1 / Teil 2): `part`-Feld in
  `STATIONS`, Teil-Überschriften in der Fortschrittsleiste, Übergangs-Screen „Teil 1 geschafft".
- **Umlaute in lustigen Texten:** **erlaubt** (ä/ö/ü/ß). Die Bildschirm-/echte Tastatur
  hat diese Tasten bereits – echte Reime/Zungenbrecher sind damit möglich.
- **Teil-1-Zwischenprüfung:** **Pflicht-Station** am Ende von Teil 1, belohnt mit dem
  Abzeichen **„Tasten-Kenner"**. Gibt Teil 1 einen klaren Abschluss.
- **Fortschritt speichern:** **`localStorage` ergänzen – vorerst nur im Tastatur-Trainer**
  (Kind kann Teil 1 beenden, pausieren und später bei Teil 2 weitermachen). Eine spätere
  Vereinheitlichung für alle drei Trainer bleibt möglich, ist aber nicht Teil dieses Vorhabens.
- **Zusätzliche Reihen-Stationen (Teil 2):** **obere Reihe** (q w e r t z u i o p) und
  **untere Reihe** (y x c v b n m) – zusammen mit der Grundreihe ein vollständiger Lehrgang.
  Keine eigene Zahlen-/Satzzeichen-Station in diesem Umfang.

---

## 0. Wichtiger Ausgangspunkt: der Trainer existiert schon

Die Datei **`tastatur-fuehrerschein.html`** ist bereits vorhanden und läuft
vollständig (13 Stationen, Gamification, Prüfung, druckbare Urkunde). Sie deckt
beide gewünschten Teile inhaltlich schon weitgehend ab – aber:

- Es gibt **keine sichtbare Zweiteilung** in „Teil 1 / Teil 2".
- Die Zehn-Finger-Übungen nutzen **nüchterne Übungssätze**, noch keine „lustigen
  Texte".
- Der Zehn-Finger-Lehrgang übt **systematisch nur die Grundreihe**; obere und
  untere Reihe fehlen als eigene Lernschritte.

Dieser Plan beschreibt daher eine **Umstrukturierung in zwei benannte Teile plus
gezielte Erweiterungen** – **keinen Neubau**. Vorhandene Bausteine werden 1:1
weiterverwendet:

| Baustein | Fundstelle in `tastatur-fuehrerschein.html` | Verwendung |
|---|---|---|
| Stationen-Liste `STATIONS[]` | `:331` | Teil-Zuordnung + neue Stationen |
| Funktionstasten-Lexikon `FUNC_KEYS[]` | `:368` | Grundlage Teil 1 |
| Bildschirm-Tastatur `buildKeyboard()` / `FINGERS` / `KEY_ROWS` | `:435` / `:410` / `:421` | Teil 1 & 2 |
| Tipp-Engine `buildTyper()` | `:470` | Teil 2 |
| Stationsabschluss `completeStation()` | `:703` | XP/Abzeichen, „Weiter" |
| Fortschrittsleiste + ☰-Drawer `buildProgress()` | `:675` | Teil-Überschriften |
| Level / Abzeichen `LEVELS` / `BADGES` | `:567` / `:576` | neue Teil-Abzeichen |
| Prüfung + Urkunde `SCREENS.exam` / `SCREENS.exam_done` | `:1148` / `:1196` | beide Teile ausweisen |

**Bleibt unverändert:** eine einzige, eigenständige HTML-Datei, kein Framework,
keine externen Dateien – Feedback über Emoji, Inline-SVG und WebAudio, Sprache
durchgängig kindgerechtes Deutsch (Du-Form). Maskottchen „Codi" bleibt.

---

## 1. Ziel & Zielgruppe

- **Ziel Teil 1:** Kinder **kennen die Funktionen der wichtigsten Tasten** und
  üben sie ein – Leertaste, Enter, Rücktaste, Entf, Umschalt (Shift),
  Feststelltaste, Tab, Esc, Strg, Pfeiltasten.
- **Ziel Teil 2:** Kinder üben das **Zehn-Fingersystem** – von der Grundreihe
  über obere/untere Reihe bis zu ganzen Sätzen – **mit lustigen Texten**, die
  Spaß machen und zum Weiterüben motivieren.
- **Abschluss:** eine **Tastatur-Führerschein-Urkunde** zum Ausdrucken, die
  beide Teile ausweist.
- **Zielgruppe:** Grundschule/Unterstufe, passend zum Niveau der Maus-/Word-
  Trainer. **Einsatz:** im Browser, ohne Installation, offline (USB-Stick,
  Schul-PC).

---

## 2. Teil 1 – „Tasten-Schule": die wichtigsten Tasten kennen & üben

### Bereits vorhandene Stationen (Teil 1 zuordnen)

| Station-ID | Titel | Lernziel |
|---|---|---|
| `klesson` | Tasten-Schule | Funktionstasten aus `FUNC_KEYS` kennenlernen (Was macht die Taste?) |
| `kquiz` | Tasten-Quiz | Tasten benennen & Funktion erklären |
| `kfind` | Tasten finden | Die leuchtende Taste auf der Bildschirm-Tastatur drücken |
| `kshift` | Großbuchstaben | Mit Umschalt groß tippen |

### Neue Stationen (Vorschlag)

1. **Pfeiltasten-Reise** (`karrows`): Cursor mit ▲▼◀▶ durch einen Text bewegen,
   an markierte Stellen navigieren. Nutzt die vorhandene Pfeiltasten-Definition
   in `FUNC_KEYS` (`:402`) und `KEY_ROWS` (`:427`).
2. **Korrigier-Werkstatt** (`kfix`): **Rücktaste vs. Entf** gezielt einsetzen –
   vorgegebene Wörter mit Tippfehlern reparieren („links löschen" vs. „rechts
   löschen"). Baut auf den `FUNC_KEYS`-Erklärungen zu Backspace/Delete auf
   (`:375`/`:378`).
3. **Kombi-Tasten-Schnupperkurs** (`kcombo`): **Strg + C / V / Z** spielerisch
   ausprobieren (kopieren, einfügen, rückgängig) – knüpft an die Strg-Erklärung
   an (`:393`) und bereitet den Word-Trainer vor.
4. **Teil-1-Zwischenprüfung** (`exam1`): kleine gemischte Aufgabe aus Teil 1,
   Belohnung mit einem eigenen Abzeichen („Tasten-Kenner"). *Offene Frage:*
   Pflicht oder optional (siehe Abschnitt 6).

**Muster jeder Station** (wie im Trainer schon üblich): kurze Erklärseite →
Übung mit sofortigem Feedback über `completeStation()` → „Weiter zu …".

---

## 3. Teil 2 – „Zehn-Finger-Training" mit lustigen Texten

### Bereits vorhandene Stationen (Teil 2 zuordnen)

| Station-ID | Titel | Lernziel |
|---|---|---|
| `khome` | Grundreihe | a s d f · j k l ö – Finger auf die Home-Tasten |
| `kfingers` | Finger-Training | Den richtigen Finger nehmen (Fingerfarben) |
| `kwords` | Wörter tippen | Ganze Wörter abtippen |
| `ksentence` | Sätze tippen | Tempo & Genauigkeit |
| `kraindrop` | Wortregen | Fallende Wörter abtippen |
| `kreflex` | Blitz-Taste | So schnell wie möglich |
| `krace` | Tipp-Rennen | Wörter im Wettlauf |

### Neue Stationen (Vorschlag)

1. **Obere Reihe** (`ktop`): q w e r t z u i o p – systematisch, mit
   Fingerzuordnung aus `FINGERS`/`KEY_ROWS`.
2. **Untere Reihe** (`kbottom`): y x c v b n m – systematisch.

Damit entsteht ein vollständiger Zehn-Finger-Lehrgang **Grundreihe → obere Reihe
→ untere Reihe → Wörter → Sätze**. Alle Stationen nutzen die vorhandene
Tipp-Engine `buildTyper()` (`:470`) und die Bildschirm-Tastatur.

### Lustige Texte (Kern-Erweiterung)

Neuer, klar getrennter Inhaltsvorrat (z. B. `FUNNY_WORDS` / `FUNNY_SENTENCES`),
gestaffelt nach Schwierigkeit, der die nüchternen `SENTENCES` (`:362`) ergänzt
bzw. in Teil 2 ersetzt:

- **Quatschsätze / Tier-Nonsens:** „Opa tanzt mit der Giraffe Polka.",
  „Der Frosch fährt Fahrrad zum Mond."
- **Kurze Reime:** „Maus im Haus, Laus im Kraus."
- **Leichte Zungenbrecher:** „Fischers Fritz fischt frische Fische."
- Passend zur jeweiligen Reihe: Übungswörter, die nur die bereits gelernten
  Tasten verwenden (z. B. für die Grundreihe „lade", „falls", „lass").

*Zu klären:* Die bestehenden `SENTENCES` **vermeiden Umlaute** (z. B.
„schlaeft" statt „schläft"), vermutlich der Tastatur-Erkennung wegen. Für die
lustigen Texte muss entschieden werden, ob Umlaute erlaubt sind (siehe
Abschnitt 6).

---

## 4. Struktur & Technik der Zweiteilung

- **`part`-Feld in `STATIONS`** (`:331`): jede Station bekommt `part:1` oder
  `part:2`; Start/Prüfung/Urkunde bleiben teilübergreifend.
- **Teil-Überschriften in Fortschrittsleiste & ☰-Drawer** (`buildProgress()`
  `:675`): sichtbare Abschnitte „Teil 1 · Tasten-Schule" und „Teil 2 ·
  Zehn-Finger-Training", damit Kinder wissen, wo sie stehen.
- **Übergangs-Screen** nach Teil 1: „Teil 1 geschafft! 🎉 Jetzt kommt das
  Zehn-Finger-Training." (eigene kleine `SCREENS`-Seite oder Sonderfall in
  `completeStation`).
- **Abschlussprüfung + Urkunde** (`:1148` / `:1196`): Aufgaben aus **beiden**
  Teilen; die Urkunde nennt beide Teile.
- Weiterhin **eine** HTML-Datei, kein Build, keine Bibliotheken.

---

## 5. Gamification & Roadmap

### Gamification (vorhandenes System weiternutzen)
- **XP & Level** bleiben (`LEVELS` `:567`, „Tipp-Lehrling … Tipp-Meister").
- **Neue Abzeichen je Teil** in `BADGES` (`:576`), z. B. „Tasten-Kenner" (Teil 1
  abgeschlossen) und „Finger-Akrobat" (Teil 2 abgeschlossen).
- Maskottchen „Codi", Sound-/Toast-Feedback, Bonus-Elemente wie gehabt.

### Roadmap
- **Phase 0 – Teil-Struktur:** `part`-Feld, Teil-Überschriften in der
  Fortschrittsleiste, Übergangs-Screen. Kein neuer Inhalt, nur Gliederung. **✅ erledigt.**
- **Phase 1 – Teil 1 vervollständigen:** Stationen `karrows`, `kfix`, `kcombo`
  ergänzen; **Pflicht-Zwischenprüfung `exam1` + Abzeichen „Tasten-Kenner"**.
- **Phase 2 – Teil 2 erweitern:** Stationen `ktop` (obere Reihe) und `kbottom`
  (untere Reihe); **lustige Texte** (mit Umlauten) als neuer Inhaltsvorrat, in
  `kwords`/`ksentence`/`kraindrop` verwenden. Zusätzlich `localStorage` für den
  Fortschritt ergänzen.
- **Phase 3 – Abschluss & Feinschliff:** Prüfung/Urkunde auf beide Teile
  ausweiten, Abzeichen „Finger-Akrobat" verknüpfen, Hilfetexte, Schultest.

---

## 6. Geklärte Fragen (Stand 2026-07-03)

1. **Umlaute in lustigen Texten:** **erlaubt** – echte Reime/Zungenbrecher mit
   ä/ö/ü/ß sind möglich.
2. **Teil-1-Zwischenprüfung** (`exam1`): **Pflicht-Station** vor Teil 2, belohnt
   mit Abzeichen „Tasten-Kenner".
3. **Fortschritt speichern** (`localStorage`): **ja, vorerst nur im
   Tastatur-Trainer**; spätere Vereinheitlichung für alle drei Trainer möglich.
4. **Umfang der neuen Reihen-Stationen:** **obere + untere Reihe** – keine eigene
   Zahlen-/Satzzeichen-Station in diesem Umfang.

---

### Nächster Schritt
Phase 0 ist umgesetzt. Als Nächstes folgt **Phase 1** (neue Teil-1-Stationen
`karrows`, `kfix`, `kcombo` + Pflicht-Zwischenprüfung `exam1`) als eigener PR,
danach Phase 2 und 3 – jeweils getrennt, wie beim Word-Trainer.

---

## 7. Nachtrag (Stand 2026-07-23): Erweiterung umgesetzt

Über die ursprünglichen Phasen hinaus wurde der Trainer erweitert:

- **Mehr Übungen (Teil 2):** neue Stationen **Obere Reihe** (`ktop`,
  q w e r t z u i o p), **Untere Reihe** (`kbottom`, y x c v b n m) und
  **Zahlen & Satzzeichen** (`knumbers`, Zahlenreihe 1–0 · Punkt/Komma). Die
  Reihen-Stationen nutzen den gemeinsamen Helfer `rowDrill()`. Damit ist der
  Zehn-Finger-Lehrgang **Grundreihe → obere → untere Reihe** vollständig.
- **Lustige Texte:** neue Inhaltsvorräte `FUNNY_WORDS` und `FUNNY_SENTENCES`
  (mit Umlauten), gemischt in `kwords`, `ksentence` und `kraindrop`.
- **Neue Abzeichen:** `toprow_hero`, `bottomrow_hero`, `number_pro`.
- **Lernzettel:** druckbare Bildschirmseite `SCREENS.lernzettel` (Tasten-Tabelle
  aus `FUNC_KEYS`, Finger-Tastatur, Kombi-Tasten, Merksätze), erreichbar über
  die Urkunden-Seite; nutzt die vorhandenen `@media print`-Regeln.
- **Klausur-Datei:** neue eigenständige Datei **`tastatur-klausur.html`** mit
  unbeschrifteter QWERTZ-Tastatur zum Ausfüllen, Umschalter „Lösung anzeigen“
  und „Finger-Farben“, Name-/Klasse-/Datum-Feldern, druckoptimiert (Querformat).

## 8. Nachtrag (Stand 2026-07-31): Station 3 „Tasten finden“ erweitert

- **Mehr Tasten in Station 3 (`kfind`):** Die Übung „Tasten finden“ prüfte bisher
  nur Buchstaben. Jetzt sind zusätzlich die **Funktions- & Steuertasten aus
  Station 1 (`klesson`) und Station 2 (`kquiz`)** enthalten – direkt aus
  `FUNC_KEYS` gezogen (alle mit `press:true`: Leertaste, Enter, Rücktaste, Entf,
  Umschalt, Feststell, Tab, Esc, Strg, Alt, Pfeiltasten). So wird das in Teil 1
  gelernte Tasten-Wissen auch beim Finden auf der echten Tastatur geübt.
- **Umsetzung:** gemeinsamer Aufgaben-Pool aus Buchstaben + `FUNC_KEYS`, ein
  „Mischbeutel“ (`shuffle`) für Abwechslung und garantiertes Vorkommen der
  Spezialtasten; Treffer-/Fehler-Erkennung über `getChar` bzw. `specialToken`,
  Hinweise nennen Sondertasten mit ihrem Namen.

## 9. Nachtrag (Stand 2026-07-31): Pfeiltasten-Bugfix & neue Sonderzeichen-Station

- **Bugfix Station 5 „Pfeiltasten-Reise“ (`karrows`):** Nach dem Erreichen des
  Käses blieb das Ziel während der 650-ms-Pause bis zur nächsten Runde liegen
  und die Eingabe war nicht gesperrt. Ein kurzes „Wackeln“ (vom Ziel weg und
  wieder darauf) löste den Treffer erneut aus und **zählte doppelt** (bzw. konnte
  die Station vorzeitig abschließen). Behoben mit einer `busy`-Sperre, die beim
  Zielerreichen gesetzt und erst in `newRound()` wieder gelöst wird; nach der
  letzten Runde bleibt die Eingabe gesperrt.
- **Neue Station „Sonderzeichen & Satzzeichen“ (`ksymbols`, Teil 2):** übt die
  mit **Umschalt ⇧** erzeugten Zeichen der Zahlenreihe (`! " § $ % & / ( ) =`)
  und die Satzzeichen `? ; : _`. Die leuchtende Grundtaste + Umschalt wird
  angezeigt; getippt wird das **erzeugte Zeichen** (Erkennung über `getChar`).
  Wird die Grundtaste ohne Umschalt gedrückt, gibt es einen gezielten Hinweis.
  Eingeordnet direkt nach `knumbers`, neues Abzeichen **`symbol_pro`
  („Sonderzeichen-Profi“)**.
- **Sonderzeichen in Prüfung & Lernzettel übernommen:** Die Abschlussprüfung
  (`SCREENS.exam`) hat jetzt **vier** Schritte – neu ist „Ein Sonderzeichen mit
  Umschalt tippen“ (Schritt 2). Der druckbare **Lernzettel** bekam den Abschnitt
  „❗ Sonderzeichen – mit Umschalt ⇧“ mit allen 14 Kombinationen. Die Zeichen­
  tabelle liegt dafür jetzt zentral als `SYMBOL_KEYS` vor und wird von Station,
  Prüfung und Lernzettel gemeinsam genutzt.
- **Bugfix Tastenbeschriftung „ß“:** `"ß".toUpperCase()` liefert in JavaScript
  `"SS"` – im Lernzettel stand deshalb „⇧ + SS = ?“. Neuer Helfer `symLabel()`
  schreibt nur groß, wenn sich die Länge nicht ändert; jetzt steht dort korrekt
  **„⇧ + ß = ?“**.
- **Bugfix abgeschnittene Tastatur in den Prüfungen:** Die Bildschirm-Tastatur ist
  ein fest bemaßtes Element (~790 px). In den Prüfungen sitzt sie in der schmalen
  Spalte `.examdesk` (`flex:2` neben der Aufgabenliste, ~430–590 px), wodurch
  `overflow-x:auto` sie rechts **abschnitt** – Entf, Enter, rechte Umschalt- und
  Strg-Taste waren nicht sichtbar. Neu skaliert `kb.fit()` die Tastatur per
  `transform:scale()` auf die verfügbare Breite; negative Ränder sorgen dafür,
  dass auch der Platzbedarf im Layout stimmt. Die Anpassung läuft automatisch
  beim Aufbau (`requestAnimationFrame`) und bei `resize`. Damit ist die Tastatur
  in **allen** Stationen und Prüfungen und auf jeder Fenstergröße vollständig
  sichtbar (geprüft bei 1280/900/420 px).

---

# Teil 2 → vollwertiger Zehn-Finger-Kurs (Plan)

> Wieder **nur Plan, kein Code.** Aufbauend auf dem, was schon läuft.

## 10. Ausgangslage: was Teil 2 heute ist – und was ihm zum Kurs fehlt

Teil 2 besteht aus 12 Stationen, die inhaltlich das Richtige abdecken
(Grundreihe → obere → untere Reihe → Finger → Zahlen → Sonderzeichen → Wörter →
Sätze → vier Tipp-Spiele). Als **Lehrgang** hat er aber systematische Lücken:

| # | Lücke | Warum das für einen Zehn-Finger-Kurs zählt |
|---|---|---|
| 1 | **Zu große Lernschritte.** `khome`/`ktop`/`kbottom` führen eine *ganze Reihe auf einmal* ein. | Etablierte Lehrgänge führen **2 Tasten pro Lektion** ein (erst `f j`, dann `d k`, …). Nur so entsteht Muskelgedächtnis statt Suchen. |
| 2 | **Übungstexte enthalten ungelernte Tasten.** `kwords`/`ksentence` ziehen aus dem vollen Vorrat. | In einem Kurs darf eine Übung **nur bereits gelernte Tasten** enthalten – sonst muss das Kind zwangsläufig hinsehen. |
| 3 | **Kein Blindschreiben.** Die Zieltaste leuchtet immer auf der Bildschirm-Tastatur. | Blind tippen ist *der* Kern des Systems. Ohne Abgewöhnen des Hinsehens bleibt es Zwei-Finger-Suchen mit Farbcode. |
| 4 | **Keine Kennzahlen.** Nur „geschafft/nicht geschafft“. | Ein Kurs braucht **Anschläge/Minute** und **Trefferquote %**, um Fortschritt zu zeigen und zu motivieren. |
| 5 | **Keine Wiederholung.** Jede Station wird einmal gespielt und ist „done“. | Tippen lernt man durch **verteiltes Wiederholen**, nicht durch einmaliges Durchspielen. |
| 6 | **Kein Lernstand pro Taste.** Fehler verpuffen. | Ohne Fehlerstatistik kann der Kurs nicht gezielt die *schwachen* Tasten nachüben. |
| 7 | **Umschalt ohne Gegenhand-Regel.** `kshift` nimmt jede Umschalt-Taste an. | Korrekt ist: Großbuchstabe links → **rechte** Umschalttaste (und umgekehrt). Das gehört zum System dazu. |
| 8 | **Keine Ergonomie.** Haltung/Handposition kommen nur als Randnotiz vor. | Sitzhaltung, Handhaltung und Pausen gehören in jeden ernsthaften Kurs. |

## 11. Zielbild

Teil 2 wird vom „Stationen-Parcours“ zum **Lehrgang mit Lektionen**:

- **~20 kurze Lektionen** à 3–6 Minuten, jede führt **2 neue Tasten** ein.
- Jede Lektion folgt derselben Dramaturgie:
  **Aufwärmen** (Einzeltasten) → **Silben** (`fjf jfj`) → **Wörter** (nur aus
  gelernten Tasten) → **Satz** → **Tipp-Zeugnis** (Tempo, Treffer, Sterne).
- **Blind-Stufen** steigern sich über den Kurs: Highlight → ohne Highlight →
  Tastatur ausgeblendet.
- **Fehler-Radar** merkt sich pro Taste die Trefferquote und baut daraus
  automatische Wiederholungs-Lektionen.
- Die vier **Tipp-Spiele** bleiben – aber als **Teil 3 „Spielwiese"**, spielbar,
  sobald die nötigen Tasten gelernt sind. Das trennt *Lernen* von *Spielen*,
  ohne Vorhandenes wegzuwerfen.

### Neue Gliederung

| Teil | Inhalt | Stationen |
|---|---|---|
| Teil 1 · Tasten-Schule | wie heute | `klesson` … `exam1` |
| **Teil 2 · Zehn-Finger-Lehrgang** | **Lektions-Landkarte mit ~20 Lektionen** | neu: `lessonmap` + Lektions-Engine |
| **Teil 3 · Spielwiese** | die vier Tipp-Spiele | `kraindrop`, `kreflex`, `krace`, `kspace` |
| Abschluss | Prüfung + Urkunde + Lernzettel | `exam`, `exam_done`, `lernzettel` |

## 12. Der Lektionsplan (Vorschlag)

Reihenfolge nach dem bewährten Prinzip *Zeigefinger → Mittel → Ring → klein*,
danach Reihenwechsel. **Fett** = neue Tasten der Lektion.

| Lektion | Neue Tasten | Schwerpunkt |
|---|---|---|
| 1 | **f j** | Grundstellung finden, Tastenmarkierungen ertasten |
| 2 | **d k** | Mittelfinger |
| 3 | **s l** | Ringfinger |
| 4 | **a ö** | kleiner Finger – Grundreihe komplett |
| 5 | **g h** | Zeigefinger-Streckung nach innen |
| 6 | *Wdh.* | Grundreihe blind, erste echte Wörter (`falls`, `Hals`, `flach`) |
| 7 | **e i** | obere Reihe, Mittelfinger |
| 8 | **r u** | Zeigefinger hoch |
| 9 | **w o** | Ringfinger hoch |
| 10 | **q p** | kleiner Finger hoch |
| 11 | **t z** | Zeigefinger-Streckung oben |
| 12 | **ü ä** | Umlaute |
| 13 | *Wdh.* | Grund- + obere Reihe gemischt |
| 14 | **v m** | untere Reihe, Zeigefinger |
| 15 | **c ,** | Mittelfinger runter |
| 16 | **x .** | Ringfinger runter |
| 17 | **y -** | kleiner Finger runter |
| 18 | **b n** | Zeigefinger-Streckung unten – Alphabet komplett |
| 19 | **⇧ (Gegenhand)** | Großschreibung mit der *anderen* Hand |
| 20 | **Zahlen** | Zahlenreihe 1–0 |
| 21 | **Sonderzeichen** | `! ? : ; " ( )` … (baut auf `ksymbols` auf) |
| 22 | *Abschluss* | freier Text, Tempo-Messung, „Tipp-Führerschein Teil 2“ |

**Wichtig:** Der Wortvorrat jeder Lektion wird **aus den bis dahin gelernten
Tasten gefiltert** – technisch ein Filter über eine Wortliste
(`word => [...word.toLowerCase()].every(c => gelernt.has(c))`). Für die frühen
Lektionen ergänzen wir bewusst **Silben und Kunstwörter** (`fjf`, `jaja`,
`Salat` ab L3), weil echte Wörter aus vier Buchstaben rar sind.

## 13. Technische Bausteine

Nichts davon erfordert ein Framework; alles baut auf Vorhandenem auf.

| Baustein | Neu/vorhanden | Aufgabe |
|---|---|---|
| `LESSONS[]` | **neu** | Lektionsdefinition: `{id, keys:["f","j"], type:"drill"/"repeat", ziel:{apm, quote}}` |
| `lessonEngine()` | **neu** | Generischer Ablauf Aufwärmen→Silben→Wörter→Satz; nutzt intern `buildTyper()` |
| `learnedKeys(lessonId)` | **neu** | Menge aller bis dahin gelernten Tasten → Grundlage der Wortfilter |
| `SYLLABLES` / gefilterte Wortlisten | **neu** | Übungsstoff je Lektion, aus `WORDS`/`FUNNY_WORDS` gefiltert + Silben |
| `buildTyper()` | vorhanden | bleibt die Tipp-Engine, ergänzt um Tempo-/Quotenrückgabe (`secs`, `errors` gibt es schon) |
| `buildKeyboard({fingers})` | vorhanden | ergänzt um **Blind-Stufe** (`hint:"key"/"finger"/"none"`) |
| `state.keyStats` | **neu** | `{ "f": {ok, fail}, … }` → Fehler-Radar & Wiederholungs-Lektionen |
| `state.lessons` | **neu** | pro Lektion `{sterne, bestApm, bestQuote}` |
| `localStorage` | teilweise | Lehrgang **muss** speichern – ein Kurs über 22 Lektionen läuft über mehrere Sitzungen |
| `lessonmap` Screen | **neu** | Landkarte mit Lektionskacheln, Sternen, „weiter wo du warst“ |
| `LEVELS`/`BADGES` | vorhanden | neue Abzeichen je Meilenstein (Grundreihe blind, Alphabet komplett, 100 APM …) |

### Kennzahlen, kindgerecht

- **Anschläge pro Minute (APM)** statt WPM – für Kinder greifbarer und ohne
  Wörter-Definition. Anzeige: „Du schaffst **84 Anschläge pro Minute**.“
- **Trefferquote** in Prozent, plus die drei häufigsten Fehlertasten.
- **Sterne pro Lektion:** ⭐ geschafft · ⭐⭐ ≥ 90 % Treffer · ⭐⭐⭐ ≥ 95 % Treffer
  **und** Zieltempo der Lektion erreicht. Sterne sind der Anreiz zum
  Wiederholen – genau das, was heute fehlt.

### Blind-Stufen

| Stufe | Anzeige | Ab Lektion |
|---|---|---|
| 1 | Zieltaste leuchtet auf der Bildschirm-Tastatur | 1–5 |
| 2 | Nur der **Finger** wird angezeigt (Farbpunkt), keine Taste | 6–13 |
| 3 | Tastatur ausgeblendet, nur der Text | ab 14, und in allen Wdh.-Lektionen |

Die Stufe ist pro Lektion vorgegeben, in den Einstellungen aber
**übersteuerbar** (Barrierefreiheit, schwächere Kinder).

## 14. Roadmap

| Phase | Inhalt | Ergebnis |
|---|---|---|
| **2a** | `LESSONS[]`, `learnedKeys()`, Wortfilter, `lessonEngine()` mit Lektionen 1–6 | Grundreihe als echter Lehrgang spielbar |
| **2b** | `lessonmap` (Landkarte, Sterne), `localStorage`-Persistenz, APM/Quote-Zeugnis | Kurs über mehrere Sitzungen nutzbar |
| **2c** | Lektionen 7–18 (obere/untere Reihe), Blind-Stufen 2 und 3 | vollständiges Alphabet blind |
| **2d** | `state.keyStats` + automatische Wiederholungs-Lektionen („Wackelkandidaten“) | gezieltes Nachüben schwacher Tasten |
| **2e** | Lektionen 19–22 (Umschalt-Gegenhand, Zahlen, Sonderzeichen, Abschluss) | Kurs komplett |
| **2f** | Umbau der Spiele zu **Teil 3**, Ergonomie-Seite, Abzeichen, Urkunde erweitert | Feinschliff |

Jede Phase ist ein **eigener PR** – wie bisher.

## 15. Migration & Rückwärtskompatibilität

- Die heutigen Stationen `khome`/`ktop`/`kbottom`/`kfingers` gehen **in den
  Lektionen auf**. Vorschlag: sie bleiben zunächst als „Schnelldurchlauf“
  erhalten und werden erst in Phase 2f entfernt, damit nie ein halbfertiger
  Stand ausgeliefert wird.
- `kwords`/`ksentence` werden zu **Wiederholungs-Lektionen** mit gefiltertem
  Vorrat statt freiem Vorrat.
- `knumbers`/`ksymbols` werden zu den Lektionen 20/21 – Inhalt bleibt, nur die
  Einbettung ändert sich.
- Gespeicherter Fortschritt alter Stände: unbekannte Lektions-IDs einfach als
  „noch nicht gemacht“ behandeln.

## 16. Offene Fragen

1. **Umfang der Lektionen:** 22 Lektionen sind ein echter Kurs (≈ 2 Schul­stunden
   plus Übung). Soll es eine **Kurzvariante** (z. B. 12 Lektionen ohne
   Zwischenwiederholungen) für den Einsatz in einer Doppelstunde geben?
2. **Teil 3 „Spielwiese“:** Spiele **freischalten** (erst ab bestimmter Lektion)
   oder von Anfang an frei zugänglich lassen?
3. **Sterne-Schwellen:** Sind 90 % / 95 % Trefferquote für die Zielgruppe
   (Grundschule/Unterstufe) realistisch, oder lieber 85 % / 92 %?
4. **Zieltempo:** Welcher APM-Wert gilt am Kursende als „bestanden“? Vorschlag:
   **60 APM bei ≥ 90 % Treffer** – bewusst niedrig gehalten.
5. **Klausur-Datei:** Soll `tastatur-klausur.html` um einen Lektions-Test
   (Papierform) ergänzt werden?
