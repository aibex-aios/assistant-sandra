# Praxis — Absicht (`intent.md`)

Stufe 1 *Plan*. Quellen: Playbook S. 11–13; dein Leitfaden (*Durchlauf*).

## Wozu

Die Absicht ist der Anfang jedes Durchlaufs. Sie hält fest, **was** gewünscht ist, **warum** und **unter welchen Bedingungen**, in den Worten dessen, der die Idee hat. Im Playbook ersetzt sie die lange Kette aus Tickets und Abstimmungen, in der die Idee bei jeder Übergabe etwas verliert (S. 11). Was in der Absicht nicht steht, kann die Spec später nicht einfordern.

## Anlässe

Eine Absicht entsteht, wenn der User eine Idee hat, wenn ein Wunsch an ein Paket herangetragen wird oder wenn ein früherer Durchlauf einen Befund hinterlässt, der eine eigene Änderung braucht (S. 11). In allen Fällen gilt derselbe Weg.

## Ablauf

1. **Erzählen lassen.** Der User beschreibt das Problem frei: was heute nicht geht, wen es betrifft, wie es besser wäre, was nicht dazugehört (S. 12, Schritt 1).
2. **Nachfragen wie eine Analystin.** Du fragst, bis die Idee greifbar ist: Umfang, Nutzer, Grenzen, woran man Erfolg erkennt (S. 12, Schritt 2). Bei AIBOS-Paketen dazu: Welcher Typ (`skill`, `assistant`, `app`, `knowledge` …)? Gibt es ein ähnliches Paket im Hub als Vorbild? Braucht es andere Pakete?
   - Nenne die offenen Punkte kurz und geh sie **Frage für Frage** durch (Leitfaden, *Rückfragen*): mehrere Optionen, die erste als Empfehlung, die letzte als eigene Antwort. Frag nichts, was schon gesagt ist.
3. **Formulieren.** Du schreibst `intent.md` nach der Vorlage unten, Status `Entwurf` (S. 12, Schritt 3).
4. **Korrigieren lassen.** Zeig den Entwurf; der User korrigiert, was du falsch verstanden hast (S. 12, Schritt 4).
5. **Committen.** Jede Fassung wird committet; Autor und Zeit stehen damit fest (S. 12, Schritt 5; S. 13, *Governance*).
6. **Annehmen.** Erst auf einen ausdrücklichen Satz des Users („die Absicht ist angenommen") setzt du `status: Angenommen`. Lehnt er ab, `Abgelehnt`; der Durchlauf endet dann. Im Playbook entspricht die Annahme dem Merge, die Ablehnung dem Schließen (S. 13).

## Vorlage

Die Abschnitte folgen dem Beispiel des Playbooks (S. 13), ergänzt um *Nicht Ziel*:

```markdown
---
title: "Absicht — <Paket> <Version>"
date: JJJJ-MM-TT
author: <User> (formuliert von Sandra)
status: Entwurf
package: <paket> <x.y.z>
related: []
---

# Absicht — <Paket> <Version>

## Problem
## Angestrebtes Ergebnis
## Betroffene Nutzer und Systeme
## Constraints
## Nicht Ziel
## Offene Fragen
```

- **Constraints** nummerierst du C1 … und nennst die Herkunft. Die Vorgaben des Users (etwa die Regel *AIBOS-Pakete — Vorgaben*, bis 0.8 auch *AIOS-Pakete — Vorgaben*) und die Spec von AIBOS gelten immer; du nennst sie, statt sie abzuschreiben.
- **Offene Fragen** nummerierst du ebenfalls. Bis zur Annahme ist jede beantwortet oder ausdrücklich als offen markiert; die Spec greift sie auf (Playbook S. 16, Schritt 3).

## Fallen

- **Lösung statt Problem:** Beschreibt der User gleich eine Umsetzung, frag nach dem Problem dahinter; die Umsetzung gehört in Spec und Plan.
- **Zu groß:** Passen mehrere Pakete oder Versionen hinein, schlag mehrere Durchläufe vor.
- **Stilles Annehmen:** „Passt so" auf eine Rückfrage zu einer Einzelheit ist keine Annahme der ganzen Absicht. Frag ausdrücklich.
- **Nachträgliche Änderung:** Ändert sich die angenommene Absicht inhaltlich, fällt sie auf `Entwurf` zurück, und T1 gilt erneut.

## Messen (Ausblick)

Das Playbook misst die Zeit vom ersten Gespräch bis zur committeten Absicht und den Anteil angenommener Absichten (S. 13). Beides lässt sich aus `git log` des Durchlaufs ablesen; Sandra 0.1 misst es nicht von sich aus (Praxis *Lücken*).
