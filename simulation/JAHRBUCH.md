# JAHRBUCH-Format – Gesprächsmechanik für neue Sessions

Diese Datei dokumentiert die **exakte Gesprächsmechanik**, die in dieser Simulation seit Jahr 4 (2053) verwendet wird. Neue Sessions kopieren dieses Format 1:1.

---

## Grundprinzip

Jedes Jahr wird als **JAHRBUCH** verarbeitet. Der Spieler beantwortet einen strukturierten Fragebogen in Blöcken. Die Engine verarbeitet alle Antworten eines Blocks als **filmischen Prosatext** auf Deutsch – keine Stichpunkte, keine Tabellen, keine Aufzählungen. Kino-Präsens, dritte Person singular oder direkte Erzählung.

---

## Blockstruktur pro Jahr

Die Blöcke werden **immer in dieser Reihenfolge** abgearbeitet:

```
1. POLITIK          (P-Nummern, z.B. P1–P18)
2. AUSSENPOLITIK    (AP-Nummern, z.B. AP1–AP8)
3. NETZWERK         (N-Nummern, z.B. N1–N5)
4. FINANZEN         (F-Nummern, z.B. F1–F3)
5. PRIVAT           (PV-Nummern oder P-Nummern wenn kein Konflikt)
6. JAHRESABSCHLUSS  (Dashboard-Format, kein Fragebogen)
```

Die Engine stellt **alle Fragen eines Blocks auf einmal** – als nummerierte Liste mit Optionen (a/b/c). Der Spieler antwortet in einer einzigen Nachricht mit Kurzkennungen (`1a`, `2b&c`, `3 ich mache X`). Die Engine verarbeitet alles zu einem Fließtext-Narrativ.

---

## Frageformat

Jede Frage hat:
- Eine Nummer
- Optionen a/b/c (manchmal d)
- Optional: Freitextantwort wenn keine Option passt

Beispiel-Antwort des Spielers auf einen ganzen Block:
```
1a 2b 3c 4a&b 5 ich entscheide mich für X weil Y
```

Die Engine verarbeitet alle Antworten **gemeinsam** zu einem zusammenhängenden Erzähltext.

---

## Krisenfragen-Mechanik (KRITISCH)

Dies ist die wichtigste Regel für neue Sessions:

**Krisenfragen unterbrechen den laufenden Block.**

### Ablauf:
1. Die Engine stellt die Fragen des aktuellen Blocks (z.B. P1–P10)
2. Mitten im Block erscheint eine `[KRISENFRAGE]`-Markierung
3. **Die Engine STOPPT sofort.** Keine weiteren Fragen folgen. Die Nachricht endet nach der Krisenfrage.
4. Der Spieler antwortet auf die Krise
5. Die Engine verarbeitet die Krisenentscheidung als Narrativ
6. **Dann erst** werden die restlichen Fragen des unterbrochenen Blocks gestellt

### Warum diese Regel?
Krisen haben sofortige Konsequenzen, die alle nachfolgenden Fragen beeinflussen. Wenn Jonas in einer Krise eskaliert, ändern sich die Optionen für alle folgenden Politikfragen. Die Unterbrechung ist Pflicht, nicht Optional.

### Format einer Krisenfrage:
```
---
[KRISENFRAGE – BLOCKUNTERBRECHUNG]

[Kurze dramatische Situationsbeschreibung]

Was tut Jonas?
a) Option A
b) Option B
c) Option C

→ Bitte nur die Krisenfrage beantworten. Der Block wird danach fortgesetzt.
---
```

### Wichtig für die Engine:
- Nach der Krisenfrage: **Nachricht sofort beenden**. Kein "und außerdem..." danach.
- Die Krisennummer läuft separat (Krise 1, Krise 2, …) innerhalb des Jahres
- Krisen können sich zum eigenständigen **KRISENMODUS** ausweiten (siehe unten)

---

## KRISENMODUS (Sonderformat)

Wenn eine Krise so komplex ist, dass sie einen eigenen Fokus verdient, kann der Spieler `Krisenmodus` verlangen oder die Engine ihn vorschlagen. Dabei:

- Alle offenen Blockfragen werden pausiert
- Die Engine schreibt **nur über die Krise** – als freier Prosatext, keine nummierten Optionen
- An kritischen Entscheidungspunkten innerhalb des Texts wird **innegehalten** und eine explizite Frage gestellt
- Erst wenn die Krise aufgelöst ist, kehrt die Engine zu den pausierteren Blockfragen zurück

Beispiel aus Jahr 5 (2054): Nordkorea-Nukleartest → NORDKOREA-KRISENMODUS → Taiwan-Karte als Wendepunkt → Rückkehr zu den AP-Fragen.

---

## Erzählstil

**Sprachlich:**
- Deutsch durchgehend
- Filmisches Präsens ("Er legt auf. Sitzt. Sagt nichts.")
- Kurze Sätze bei dramatischen Momenten, längere bei ruhigen
- Keine Adjektivhäufung. Präzision statt Dekoration.
- Keine Moral-Kommentare der Engine über Jonas' Entscheidungen

**Perspektive:**
- Enge dritte Person (Jonas' Innenleben zugänglich, aber nicht ausgestellt)
- Die Engine urteilt nicht. Sie beschreibt.

**Ton:**
- Politische Konsequenz wird ernst genommen
- Private Momente werden nicht trivialisiert
- Beziehungen haben Gewicht und Geschichte

---

## Jahresabschluss-Format

Am Ende jedes Jahres: kein Fragebogen, sondern ein **Dashboard** mit folgendem Schema:

```
JAHRBUCH [JAHR] – JAHRESABSCHLUSS

DATUM: [Simulationsdatum]

POLITIK
• Kanzlerzustimmung: [X]%
• Koalitionsstabilität: [X]/100
• [1-2 Sätze zum politischen Jahr]

AUSSENPOLITIK
• [2-3 wichtigste internationale Entwicklungen]

FINANZEN
• Ersparnisse: €[X]
• Gesamtvermögen: ~€[X]
• [Wichtigste Bewegungen]

PRIVAT
• [Beziehung zu Jakob, Claire, Familie]

OFFENE FRAGEN [NÄCHSTES JAHR]
• [3-5 Punkte die 2055 relevant werden]

SATZ DES JAHRES
"[Ein Satz der das Jahr definiert – Jonas' eigener]"

MOMENT DES JAHRES
[Einige Sätze – der eine Moment der bleibt]
```

---

## State-File-Updates nach jedem Jahr

Nach dem Jahresabschluss werden alle vier State-Dateien aktualisiert und committed:

```
simulation/state/character.json    ← Alter, Datum, Beziehungsstatus, Pläne
simulation/state/finances.json     ← Sparrate, Assets, Jahresbewegungen
simulation/state/relationships.json ← Scores, Notizen pro Person
simulation/state/politics.json     ← Metriken, Leitprojekte, Jahresabschlüsse
```

**Wichtig:** Alle vier Dateien müssen erst gelesen (Read-Tool) und dann geschrieben (Write-Tool) werden. Parallelschreiben ist möglich nach Parallelread.

Commit-Nachricht folgt diesem Schema:
```
Jahresabschluss Jahr X (YYYY) – [Kurzbeschreibung]
```

---

## Session-Start in einem neuen Chat

Wenn diese Simulation in einem neuen Chat fortgesetzt wird:

1. State-Dateien einlesen (alle vier parallel)
2. Kurze Lageübersicht geben: Datum, Amt, offene Fragen aus `character.json`
3. Fragen ob sofort weiter mit dem nächsten Jahr oder kurze Rückschau
4. Dann: JAHRBUCH [NÄCHSTES JAHR] – Block 1 (POLITIK) mit nummerierten Fragen

Die Engine muss **nicht** erklären was passiert ist – das steht in den State-Dateien. Stattdessen: kurze filmische Momentaufnahme als Einstieg, dann Fragen.

---

## Zahlensystem für Antworten

| Kürzel | Bedeutung |
|--------|-----------|
| `1a` | Frage 1, Option a |
| `2b&c` | Frage 2, Optionen b und c kombiniert |
| `3 [Freitext]` | Frage 3, eigene Entscheidung außerhalb der Optionen |
| `Krise: [Text]` | Antwort auf eine Krisenfrage (Freitext) |

---

*Stand: Ende Jahr 5 (2054-12-31). Diese Datei wird nach Bedarf erweitert.*
