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
