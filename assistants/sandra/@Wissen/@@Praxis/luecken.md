# Praxis — Lücken von AIBOS

Quellen: Playbook S. 20, 26–29, 32–35, 38–41, 48–51; Spec von AIOS 01 (*Grundregeln*, *Begriffe*), 02 (*Hooks*, *Freigabe*), 03 (*Assistenten*), 06 (*`manifest.yaml`*).

## Grundsatz

Das Playbook beschreibt seine Methode mit den Werkzeugen eines bestimmten Hosts. Du übernimmst die Methode und verwendest nur, was AIBOS selbst neutral anbietet. Fehlt ein Baustein, ist das eine **Lücke von AIBOS**. Du benennst sie und umgehst sie nicht mit einer Datei oder einem Mechanismus des Hosts, auch nicht vorübergehend.

## Die Lücken heute

| Baustein | im Playbook | in AIBOS v0.6 | was du stattdessen tust |
|---|---|---|---|
| erzwungene Tore | Hooks, die Aktionen blockieren oder eine Freigabe verlangen (S. 26–27, 40–41); Plan Mode, der vor dem Plan keine Änderung zulässt (S. 20) | Hooks gibt es nur von AIOS selbst (Spec 02 *Hooks*); `contents.hooks` eines Pakets ist im Format vorgesehen, aber ohne Wirkung (Spec 06); Wunsch W74 | beratende Torstellen T1–T4, T6 |
| Gleichlauf Plan und Code | Hook, der ihn erzwingt (S. 20) | keiner | `plan.md` im selben Commit nachführen |
| Subagents | Helfer mit eigenem Kontext, z. B. ein Prüfer (S. 27–29) | kein Element; die Agentenfunktion des Hosts wird nicht verwendet (Spec 03 *Assistenten*); Wunsch W75 | selbst prüfen, in einem eigenen Schritt mit `sandra-pruefen` |
| Schutz der Tests | Hook gegen Änderungen an Testdateien (S. 32–33) | keiner | Befunde nicht durch Ändern des Abnahmetests „beheben"; Änderungen an `spec.md` gehen über die Spec |
| Evals | Suite, die bei jeder Änderung an Skills läuft (S. 34–35); Test, ob ein Skill auslöst (S. 25) | kein Element; Wunsch W76 | Probe und Abnahmetest auf jedem Kandidaten; bei einer neuen Regel nur die Prüfung, dass sie im Master-Kontext steht |
| Review-Regel | Regeln für das Review jedes Pull Requests (S. 38–39) | kein Element; Wunsch W77 | — (Stufe 5, kommt später) |
| Dateien für den Host | `CLAUDE.md` (S. 23) | nur der Masterspace; für den Host nur `.claude/skills/` als Build-Output (Spec 01) | Praxis *Masterspace* |
| Auslöser | die Annahme der Absicht startet die Spec ohne Zutun (S. 16); Überwachung startet einen neuen Kreislauf (S. 48–51) | kein Element | der User stößt jeden Schritt an |
| Messen | Früh- und Spätindikatoren je Stufe (S. 48–51) | kein Element | die Indikatoren des Plays *Requirements & design* aus `git log` (Praxis *Spezifikation*); die übrigen fehlen |
| fremdes Repo begleiten | Arbeit im Repo des Produkts | ein Assistent wirkt nur in einem Projekt mit `@/`; Wunsch W78 | Pakete entstehen im Konzeptprojekt |

Die Wünsche W74–W79 sind dem Kernel von AIBOS als ein Bündel übergeben. Ob und wann sie kommen, entscheidet AIBOS, nicht du.

## Wie du eine Lücke benennst

Sobald das Playbook an einer Stelle einen fehlenden Baustein voraussetzt:

1. **Sag es kurz:** welcher Baustein fehlt, wo das Playbook ihn verlangt (Seite), was AIBOS stattdessen hat.
2. **Sag, was du tust:** den Ersatz aus der Tabelle, oder dass die Stufe in dieser Version nicht abgedeckt ist.
3. **Nicht umgehen:** kein Hook, kein Agent, keine Datei eines Hosts, auch wenn der User es vorschlägt. Erklär, warum: Das Paket soll in jedem Vault und mit jedem Host gleich wirken.
4. **Festhalten, wenn es neu ist:** Ist die Lücke nicht in der Tabelle, schlag dem User ein Memo im Konzeptprojekt vor, das sie mit Fundstelle beschreibt. Ob daraus ein Wunsch an den Kernel wird, entscheidet der User.

## Beratend heißt beratend

Deine Torstellen wirken nur, solange du dich an sie hältst. Wird eine übersteuert oder fragt der User danach, sagst du das offen: Erzwingen könnte sie erst ein neutrales Tor im Kernel. Du tust nicht so, als sei ein Schritt technisch gesperrt.

## Was keine Lücke ist

- Eine Frage, die die Spec von AIOS offenlässt: Das ist ein offener Punkt der Spec; nenne ihn als offen und rate nicht.
- Ein Werkzeug, das AIBOS anders löst, aber löst (z. B. Release-Kandidaten statt Pull Requests): Das ist eine andere Form, kein fehlender Baustein.
