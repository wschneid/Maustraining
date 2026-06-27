# Plan: Word-Führerschein – Trainer für Word-Grundoperationen

Ein interaktiver Lern-Trainer, der Kindern/Einsteiger:innen die **grundlegenden
Operationen in Microsoft Word** spielerisch beibringt – im selben Stil und mit
derselben Technik wie die bereits vorhandenen Trainer `maus-fuehrerschein-4.html`
und `tastatur-fuehrerschein.html`.

> Dieses Dokument ist **nur der Plan**. Es enthält bewusst keinen Code.

---

## ✅ Festgelegte Entscheidungen (Stand 2026-06-26)

- **Stationsumfang:** wie unten beschrieben (16 Stationen + Abschlussprojekt) – freigegeben.
- **Zielgruppe:** **Word-Anfänger:innen** (echte Einsteiger, noch nie/kaum mit Word gearbeitet).
- **Touch-Bedienung:** **optional** (Maus/Tastatur stehen im Vordergrund; Touch nur „nice to have").
- **Abschlussprojekt** („Steckbrief schreiben"): **Pflicht-Station** vor der Urkunde.
- **Oberfläche:** **vereinfacht**, aber **an aktuelles Word (Microsoft 365 / Word 2021)** angelehnt
  – erkennbares Menüband mit Registerkarten *Start / Einfügen / Layout*.
- **Speicherort:** Entwicklung **vorerst im bestehenden Repo `Maustraining`** als Datei
  `word-fuehrerschein.html` (eigenes Repo war per Integration nicht anlegbar – später umziehbar).

---

## 1. Ziel & Zielgruppe

- **Ziel:** Lernende sollen die wichtigsten Word-Handgriffe sicher beherrschen –
  Text eingeben, markieren, formatieren, kopieren/einfügen, speichern, Listen,
  Tabellen und Bilder einfügen, rückgängig machen usw.
- **Zielgruppe:** Grundschule/Unterstufe bzw. absolute Einsteiger:innen, passend
  zum Niveau der bestehenden Maus-/Tastatur-Trainer.
- **Einsatz:** Im Browser, ohne Installation, **ohne echtes Word**. Eine einzelne
  HTML-Datei, die offline läuft (z. B. von USB-Stick oder Schul-PC).
- **Voraussetzung:** Maus- und Tastatur-Grundlagen (deckt der vorhandene
  „Maus-Führerschein" und „Tastatur-Führerschein" ab) → der Word-Trainer ist die
  logische **dritte Stufe**.

---

## 2. Leitidee: dieselbe bewährte Architektur weiterverwenden

Der Trainer wird **eine einzige, eigenständige HTML-Datei** mit eingebettetem CSS
und JavaScript – exakt wie die bestehenden Trainer. Wiederverwendet werden:

| Baustein | Quelle (bestehend) | Im Word-Trainer |
|---|---|---|
| Stationen-Liste `STATIONS[]` | `maus-fuehrerschein-4.html:610` | neue Word-Stationen |
| Erklärseite vor jeder Station (`stationIntro`/`.explainer`) | `:914` | Funktion zuerst zeigen, dann üben |
| Stationsabschluss (`completeStation`) | `:956` | XP/Abzeichen vergeben, „Weiter" |
| Fortschrittsleiste + ☰-Drawer (`buildProgress`) | `:801` | Navigation über Stationen |
| Gamification: XP, Level, Münzen, Abzeichen, Maskottchen „Codi" | `:692–777` | identisch übernehmen |
| Bonus-Mini-Spiele nach je 3 Stationen (`maybeOfferBonus`) | `:951` | Auflockerung beibehalten |
| Kontextmenü/Untermenü (`buildItems`) | `:993` | Rechtsklick-Menüs in Word |
| Drag-Helfer (`dragMove`, `ghostDrag`) | `:1003` | Bild/Tabelle/Text ziehen |
| Sound-/Toast-/Mascot-Feedback | vorhanden | identisch übernehmen |
| Abschlussprüfung + Urkunde (`exam`/`exam_done`) | vorhanden | „Word-Führerschein"-Urkunde |

**Designsprache:** eigenes Farbschema, das an Word erinnert (Word-Blau statt
Ocean-Türkis), gleiche `--Variablen`-Struktur. Maskottchen „Codi" bleibt, damit
die drei Trainer als Serie erkennbar sind.

---

## 3. Die zentrale Herausforderung: Word im Browser „nachbauen"

Es wird **kein echtes Word** eingebunden und **keine** echte `.docx`-Datei
erzeugt. Stattdessen wird eine **vereinfachte, nachgebaute Word-Oberfläche**
(„Fake-Word") in HTML/CSS dargestellt – genau wie die bestehenden Trainer schon
Fenster, Menüs und Dialoge simulieren.

Wiederverwendbare Word-Oberflächen-Bausteine (einmal bauen, in vielen Stationen nutzen):

1. **Menüband (Ribbon)** mit Registerkarten *Start / Einfügen / Layout* und den
   wichtigsten Schaltflächen (Fett, Kursiv, Unterstrichen, Schriftart, Größe,
   Farbe, Ausrichtung, Listen, Kopieren/Einfügen …). Nur die jeweils geübten
   Buttons sind aktiv/hervorgehoben.
2. **Schreibfläche** = ein `contenteditable`-Bereich, der wie ein Word-Blatt
   („Seite") aussieht. Echtes Tippen, Markieren mit der Maus, Cursor.
3. **Statusleiste / Lineal / Titelleiste** als Deko, teils interaktiv (z. B.
   Zoom-Regler – knüpft an die Maus-Station „Regler" an).
4. **Dialoge** (Speichern unter, Suchen & Ersetzen, Bild/Tabelle einfügen) –
   Wiederverwendung des vorhandenen Dialog-/PopUp-Musters.
5. **Kontextmenü** beim Rechtsklick (Ausschneiden/Kopieren/Einfügen) – nutzt die
   vorhandene `buildItems`-Logik.

**Prüfprinzip pro Station:** Nach jeder Aktion prüft eine kleine `check()`-Funktion
den Zustand der Schreibfläche (z. B. „ist das markierte Wort jetzt fett?",
„steht der eingefügte Text an der richtigen Stelle?") und gibt sofort Feedback
(grüner Hinweis, XP, Maskottchen-Reaktion) – wie in den bestehenden Stationen.

---

## 4. Stationsliste (Vorschlag, didaktisch sortiert)

Vom Einfachen zum Komplexen, jede Station mit kurzer Erklärseite davor. Reihenfolge
und Umfang sind als Startpunkt gedacht (siehe Roadmap Abschnitt 8).

| # | Station | Icon | Lernziel |
|---|---|---|---|
| – | **Start** | 🏁 | Begrüßung, Word-Oberfläche kennenlernen (Ribbon, Blatt, Cursor) |
| 1 | **Tippen & Cursor** | ⌨️ | Text eingeben, Cursor per Klick setzen, Zeilenumbruch (Enter) |
| 2 | **Korrigieren** | ⌫ | Löschen mit Entf/Rücktaste, Tippfehler verbessern |
| 3 | **Rückgängig** | ↶ | Strg+Z / Wiederholen, Symbol im Menüband |
| 4 | **Markieren** | 🖍️ | Wort (Doppelklick), Zeile, Absatz, Alles (Strg+A) markieren |
| 5 | **Fett · Kursiv · Unterstrichen** | **B** | Zeichenformatierung über Menüband & Tastenkürzel |
| 6 | **Schriftart & Größe** | 🔤 | Schriftart wählen, Größe ändern, Schriftfarbe |
| 7 | **Ausrichten** | ⬛ | Links / Zentriert / Rechts / Blocksatz |
| 8 | **Listen** | • | Aufzählung & Nummerierung erstellen |
| 9 | **Kopieren & Einfügen** | 📋 | Strg+C/X/V + Rechtsklick-Menü, Text verschieben |
| 10 | **Suchen & Ersetzen** | 🔍 | Wort finden und ersetzen (Dialog) |
| 11 | **Bild einfügen** | 🖼️ | Registerkarte Einfügen → Bild platzieren, verschieben |
| 12 | **Tabelle einfügen** | ▦ | Tabelle erstellen, Zelle anklicken, Text eintragen |
| 13 | **Überschriften / Formatvorlagen** | 🅷 | Überschrift formatieren, Dokument gliedern |
| 14 | **Speichern** | 💾 | „Speichern unter", Dateiname & Ort wählen (Dialog) |
| 15 | **Drucken / Seitenansicht** | 🖨️ | Vorschau öffnen, Druckdialog verstehen |
| 16 | **Rechtschreibung** | ✅ | Unterringeltes Wort per Rechtsklick korrigieren |
| – | **Prüfung** | 🏆 | Mehrere Schritte am Stück → Word-Führerschein-Urkunde |

> Eine **Mini-Schreibaufgabe** als Abschluss-Projekt ist denkbar: „Erstelle einen
> kleinen Steckbrief" (Überschrift fett, Liste, Bild, speichern) – fasst mehrere
> Stationen zusammen, optional als Bonus.

---

## 5. Aufbau einer einzelnen Station (einheitliches Muster)

Jede Station folgt demselben dreiteiligen Ablauf wie in den bestehenden Trainern:

1. **Erklärseite** (`.explainer`): Was ist das? Wofür braucht man es? Großes Bild
   der betreffenden Word-Schaltfläche, 2–3 Stichpunkte, „So geht's"-Schritte,
   Button **„Los geht's ›"**.
2. **Übung** auf der Fake-Word-Oberfläche: konkrete Aufgabe im Instruktionsbalken
   (z. B. „Mache das Wort *Sommer* fett."). Lernende führen die echte Maus-/
   Tastatur-Aktion aus; nur die relevanten Ribbon-Buttons sind aktiv.
3. **Prüfung & Abschluss** (`completeStation`): sofortiges Feedback, XP/Münzen,
   evtl. Abzeichen, Maskottchen-Jubel, Button **„Weiter zu …"**. Mehrere kleine
   Aufgaben pro Station (Zähler `x/n`), damit es sich gefestigt anfühlt.

**Hilfen:** dezenter Tipp-Text unten; nach Fehlversuch deutlichere Hilfe / Pfeil
auf den richtigen Button (das „Hint"-Muster ist vorhanden). Nichts ist eine
Sackgasse – Lernende können eine Station auch überspringen.

---

## 6. Gamification (1:1 übernehmen, nur umbenennen)

- **XP & Level** mit Word-Titeln, z. B. *Wort-Lehrling → Wort-Schüler →
  Wort-Kenner → Wort-Profi → Wort-Meister*.
- **Münzen** und **Abzeichen** je Können (z. B. „Format-Künstler" fürs erste
  Fett/Kursiv, „Tabellen-Baumeister", „Bilder-Profi", „Schreib-Champion").
- **Maskottchen „Codi"** mit Reaktionen (freut sich, denkt nach).
- **Bonus-Mini-Spiele** nach je 3 Stationen (vorhandenes Set wiederverwenden,
  optional ein neues Word-Thema).
- **Abschluss-Urkunde** „Word-Führerschein" zum Ausdrucken/Speichern.

---

## 7. Technische Hinweise & Entscheidungen

- **Eine HTML-Datei**, eingebettetes CSS/JS, keine externen Bibliotheken, keine
  Build-Tools – konsistent mit den vorhandenen Trainern (Offline-Tauglichkeit).
- **Schreibfläche:** `contenteditable`-Bereich; Formatierung über
  `document.execCommand`-Ersatz bzw. eigene, kontrollierte Toggle-Logik auf der
  aktuellen Auswahl (bewusst eingeschränkt, damit nur das Geübte möglich ist).
- **Fortschritt speichern:** Die vorhandenen Trainer halten den Fortschritt nur im
  Speicher (kein `localStorage`). → **Offene Entscheidung:** optional `localStorage`
  ergänzen, damit Kinder zwischendurch pausieren können (Empfehlung: ja, klein
  gehalten). Gilt dann idealerweise für alle drei Trainer einheitlich.
- **Touch/Tablet:** so weit möglich auch per Touch bedienbar (Schulen mit
  Tablets) – bestehende Trainer sind primär Maus-orientiert; Touch ist ein
  „Nice-to-have", das je Station geprüft wird.
- **Sprache:** durchgängig kindgerechtes Deutsch, Du-Form, kurze Sätze – wie
  bisher.
- **Barrierefreiheit:** ausreichend Kontrast, große Klickflächen, Tastatur-
  Bedienbarkeit, `aria-label` an Buttons (bereits Praxis im Maus-Trainer).

---

## 8. Roadmap / Meilensteine

> **Umsetzungsstand (2026-06-27): vollständig umgesetzt.** Alle Phasen 0–5 sind in
> `word-fuehrerschein.html` realisiert: Grundgerüst + Fake-Word-Oberfläche, alle 16
> Stationen, das Pflicht-Projekt „Steckbrief" und die Prüfung mit druckbarer Urkunde.
> Jede Station wurde headless mit Chromium auf Lösbarkeit und JS-Fehlerfreiheit getestet.

**Phase 0 – Gerüst (Grundlage für alles)**
- Datei `word-fuehrerschein.html` anlegen, Kopf/Footer/Drawer/HUD aus bestehendem
  Trainer übernehmen, Word-Farbschema setzen.
- Wiederverwendbare **Fake-Word-Oberfläche** bauen: Ribbon + Schreibfläche +
  Statusleiste als Komponenten, die Stationen ein-/ausblenden können.
- `STATIONS`-Liste, Erklärseiten-Gerüst, `completeStation`, Gamification anhängen.

**Phase 1 – Kern-Stationen (Text)**
- Stationen 1–5: Tippen/Cursor, Korrigieren, Rückgängig, Markieren, Fett/Kursiv/
  Unterstrichen. Das ist der wichtigste, alltagstauglichste Block.

**Phase 2 – Formatieren & Struktur**
- Stationen 6–9: Schriftart/Größe/Farbe, Ausrichten, Listen, Kopieren/Einfügen.

**Phase 3 – Einfügen & Werkzeuge**
- Stationen 10–13: Suchen/Ersetzen, Bild, Tabelle, Überschriften.

**Phase 4 – Datei & Abschluss**
- Stationen 14–16: Speichern, Drucken, Rechtschreibung.
- **Prüfung** + Urkunde, Bonus-Spiele final verknüpfen.

**Phase 5 – Feinschliff**
- Touch-Test, Barrierefreiheit, Hilfetexte verbessern, optional `localStorage`,
  Schultest mit echten Kindern → Aufgaben nach Beobachtung anpassen.

---

## 9. Dateistruktur & Benennung

- Neue Datei im Repo-Wurzelverzeichnis: **`word-fuehrerschein.html`**
  (Schema wie `maus-fuehrerschein-4.html`, `tastatur-fuehrerschein.html`).
- Optional später eine kleine **`index.html`** als Startseite, die alle drei
  Trainer (Maus / Tastatur / Word) als „Führerschein-Serie" verlinkt.

---

## 10. Offene Fragen (für Abstimmung)

1. **Genauer Zielumfang:** Reichen die 16 vorgeschlagenen Stationen, oder soll
   z. B. „Kopf-/Fußzeile", „Seitenränder" oder „Serienbrief" dazu? (Empfehlung:
   erst mit dem Kernumfang starten.)
2. **Fortschritt speichern** (`localStorage`) – ja/nein, und ggf. für alle drei
   Trainer einheitlich nachrüsten?
3. **Touch-Bedienung** als Pflichtanforderung oder optional?
4. **Abschluss-Projekt** „Steckbrief schreiben" als eigene Pflicht-Station oder
   als optionaler Bonus?
5. **Word-Versionsbezug:** Soll die nachgebaute Oberfläche an eine bestimmte
   Word-/Office-Version angelehnt sein (Ribbon-Anordnung), oder bewusst
   vereinfacht/zeitlos?

---

### Nächster Schritt
Nach Freigabe dieses Plans (insbesondere Stationsumfang und Punkte aus Abschnitt 10)
kann mit **Phase 0** begonnen werden: Gerüst + wiederverwendbare Fake-Word-Oberfläche.
