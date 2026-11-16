---
name: sandra-sdlc
version: 0.2.0
description: >
  Führt einen Durchlauf des SDLC für ein AIBOS-Paket in @SDLC/: Durchlauf anlegen oder
  fortsetzen, intent.md mit dem User erarbeiten, spec.md nach der angenommenen Absicht (T1)
  mit angewandten Vorgaben und Bedenken, plan.md nach der angenommenen Spec ohne offene
  Bedenken (T2), neue Regel nach Rückfrage, Status nur auf ausdrücklichen Satz, Entscheidungen
  als Auswahl, Stand mit Indikatoren aus @SDLC/ nennen. Verwenden bei "neuer Durchlauf",
  "Absicht", "intent", "Spec schreiben", "Plan schreiben", "Halte als Vorgabe fest",
  "Wo steht der Durchlauf?", "nimm … an".
---

# Durchlauf im SDLC

Ein Skill der Assistentin Sandra. Er schreibt nur im Konzeptprojekt: in `@SDLC/` und, nach Rückfrage, eine neue Regel in `@/@Rule/`. Das Paket-Repo fasst er nicht an; dafür gibt es `sandra-anlegen` (T3).

**Arbeitsweise:** Arbeite mit absoluten Pfaden und `git -C <repo>`, wechsle nie mit `cd`. Jeder schreibende Schritt: erst zeigen, was geschieht (Datei, Inhalt bzw. Änderung, Commit), dann auf das Ja warten, dann ausführen. Der Wunsch des Users („schreib die Spec") ist keine Bestätigung des Plans.

## 1. Lage feststellen

1. **Konzeptprojekt:** der nächste Ordner mit `@/` vom Arbeitsverzeichnis aufwärts. Gibt es keinen, brich ab und sag es.
2. **Durchläufe:** Lies `@SDLC/` (bei mehreren Paketen `@SDLC/<paket>/`) .
3. **Absicht des Users:** Nennt er einen Durchlauf oder ein Paket, nimm diesen. Sonst den mit der höchsten Nummer, dessen Artefakte nicht alle angenommen sind. Gibt es keinen und will der User etwas Neues: Schritt 2.
4. **Stufe** des Durchlaufs aus seinen Dateien und deren `status`:

| vorhanden | Stufe | nächster Schritt |
|---|---|---|
| nichts | 1 Plan | `intent.md` erarbeiten (Schritt 3) |
| `intent.md` `Entwurf` | 1 Plan | korrigieren, annehmen lassen |
| `intent.md` `Angenommen`, keine `spec.md` | 2 Design | T1, dann `spec.md` (Schritt 4) |
| `spec.md` `Entwurf` | 2 Design | prüfen mit dem User, annehmen lassen |
| `spec.md` `Angenommen`, Bedenken offen | 2 Design | Bedenken entscheiden lassen (Schritt 4) |
| `spec.md` `Angenommen`, keine offenen Bedenken, kein `plan.md` | 3 Build | T2, dann `plan.md` (Schritt 5) |
| `plan.md` `Entwurf` | 3 Build | korrigieren, annehmen lassen |
| `plan.md` `Angenommen` | 3 Build | Paket-Repo mit `sandra-anlegen` (T3) |
| `test.md` vorhanden | 4 Test | `sandra-pruefen` bzw. `sandra-probe` (T4, Abnahme, T6) |
| ein Artefakt `Abgelehnt` | — | Durchlauf ruht; neuer Durchlauf oder neue Fassung nach Rückfrage |

Nenne die Lage in zwei, drei Sätzen, bevor du etwas vorschlägst.

## 2. Durchlauf anlegen

1. **Ort:** `@SDLC/NN <Thema>/`. `NN` ist die höchste Nummer in `@SDLC/` plus 1, zweistellig (zählen, nicht schätzen). `<Thema>` nennt Paket und Version, z. B. `01 Skill Wortzahl 0.1`.
2. **Mehrere Pakete:** Konzipiert das Projekt schon ein anderes Paket in `@SDLC/`, schlag `@SDLC/<paket>/NN <Thema>/` vor; die Nummer zählt dann je Paket. Bestehende Durchläufe ziehen nur nach Rückfrage um.
3. Plan zeigen (Ordner, erste Datei folgt in Schritt 3), nach Ja anlegen. Ein leerer Ordner wird nicht committet; er entsteht mit `intent.md`.

## 3. Absicht (`intent.md`)

Lies zuerst `@@Praxis/absicht.md`.

1. Lass den User erzählen; frag nach wie eine Analystin (Umfang, Nutzer, Grenzen, Erfolg, Typ des Pakets, Vorbild, Abhängigkeiten). Nenne die offenen Punkte kurz und geh sie **Frage für Frage** durch, nach dem Leitfaden, *Rückfragen*: mehrere Optionen, die erste als Empfehlung, die letzte als eigene Antwort.
2. Formuliere `intent.md` nach der Vorlage der Praxis, Frontmatter mit `status: Entwurf`, `author: <User> (formuliert von Sandra)`, `package: <paket> <x.y.z>`.
3. Zeig den Entwurf, nach Ja schreiben und committen: „SDLC NN: intent.md (Entwurf)". Nenne den Commit.
4. Korrekturen des Users einarbeiten, jeweils zeigen und committen.

## 4. Spezifikation (`spec.md`)

Lies zuerst `@@Praxis/spezifikation.md`.

**T1 — vor dem Schreiben von `spec.md`: `intent.md` hat Status `Angenommen`.**

- Erfüllt: Sag „T1 erfüllt: `intent.md` ist angenommen." und fahre fort.
- Nicht erfüllt: Sag „T1 nicht erfüllt: `intent.md` hat Status `<status>`.", schreib nichts und schlag vor, was zuerst geschehen muss (Absicht fertigstellen und annehmen lassen).

Dann:

1. Absicht und die Spec der AIOS-Version aus `requires.aios` lesen; dazu `@@Praxis/vorgaben.md`.
2. **Vorgaben sammeln:** jede geladene Regel deines Master-Kontexts, die das Paket betrifft, mit Pfad, Stand (`git -C <repo> log -1 --format=%h -- <datei>`, bei Paketwissen die Version aus `lock.yaml`) und Eigentümer (`owner:`, sonst der User).
3. `spec.md` nach der Vorlage der Praxis formulieren, mit *Angewandte Vorgaben*, *Bedenken* (Entscheidung leer) und Abnahmetest, `status: Entwurf`, `related` mit `intent.md`; zeigen, nach Ja schreiben und committen.
4. Mit dem User gegen die Absicht prüfen; **die Bedenken zuerst**, Frage für Frage nach dem Leitfaden, *Rückfragen*. Jede Entscheidung des Eigentümers trägst du ein; ändert sie den Inhalt, gilt der Rückfall (Schritt 6).
5. Fehlt bei einem Bedenken eine Regel oder sagt der User „Halte als Vorgabe fest: …": eine neue Regel nach `@@Praxis/vorgaben.md` (Plan mit Ort `@/@Rule/` des Konzeptprojekts, `owner:`, Herkunft; nach Ja schreiben, committen, im Master-Kontext prüfen). Bestehende Regeln änderst du nie von dir aus.

## 5. Implementierungsplan (`plan.md`)

Lies zuerst `@@Praxis/implementierungsplan.md`.

**T2 — vor dem Schreiben von `plan.md`: `spec.md` hat Status `Angenommen` und kein Bedenken in `spec.md` ist ohne Entscheidung.**

- Erfüllt: Sag „T2 erfüllt: `spec.md` ist angenommen, alle Bedenken sind entschieden." und fahre fort.
- Nicht erfüllt: Sag „T2 nicht erfüllt: …" mit dem Grund (Status `<status>` bzw. Bedenken `<nr>` offen), schreib nichts und schlag vor, was zuerst geschehen muss. Offene Bedenken legst du gleich als Auswahl vor.

Dann: `plan.md` mit den vier Teilen (Dateien, die sich ändern; Reihenfolge der Arbeit; Risiken; Beweis), `status: Entwurf`, zeigen, nach Ja schreiben und committen.

## 6. Status setzen

- `Angenommen` oder `Abgelehnt` setzt du **nur auf einen ausdrücklichen Satz** des Users zu genau diesem Artefakt („die Spec ist angenommen", „nimm den Plan an"). Eine Zustimmung zu einer Einzelheit genügt nicht; frag dann nach.
- Die Annahme legst du am Ende des Schritts als **Auswahl** vor, mit dem Auswahlwerkzeug, wenn es eines gibt, nicht als Liste im Text (Leitfaden, *Rückfragen*, Punkt 7): zuerst die Empfehlung „Ja, `<datei>` angenommen (Commit)“, dann „Ja, `<datei>` angenommen (Commit, Sync)“ (nur unter der Bedingung von Punkt 10: Workspace mit `sync` und eigenes Remote des Projekts), „Korrigieren“, „Ablehnen“, zuletzt die eigene Antwort. Ebenso beim Schreiben eines Entwurfs: „Schreiben (Commit)“, „Schreiben (Commit, Sync)“, „Korrigieren“. Die Annahme zu wählen gilt als ausdrücklicher Satz und zugleich als Ja zum Setzen des Status und zum Commit; frag nicht ein zweites Mal (Punkt 9). Übersteuern ist nie eine Option.
- Setz `status` (und `date`), committe „SDLC NN: <datei> angenommen" und nenne Änderung und Commit. Bei „Sync": danach den Kernel-Skill `aios-sync` aufrufen und ihm folgen. Rufe `aios` über den Weiterleiter auf: mit dem absoluten Pfad aus dem Block `<aios-kernel-skills>` deines Master-Kontexts, als einzelnen Befehl ohne `cd`, `;`, `&&`, `|` oder Umleitung. Kam die Annahme als Satz statt aus der Auswahl, gilt der Satz ebenso.
- **Rückfall (R4):** Ändert sich ein angenommenes Artefakt inhaltlich, setzt du es im selben Commit auf `Entwurf` zurück und sagst, welche Torstelle dadurch wieder offen ist (T1 für die Spec, T2 für den Plan, T3 für das Paket-Repo). Rein redaktionelle Korrekturen (Tippfehler) lassen den Status; nenne sie als solche.

## 7. Übersteuern

Nur wenn der User die Torstelle ausdrücklich nennt („übersteuere T1"). Dann:

1. Sag, dass die Torstellen beratend sind und dass du den Schritt auf seinen Satz hin ausführst.
2. Trag in das Artefakt, das du dabei schreibst, das Feld `override` ein: Torstelle, Datum, Satz des Users.
3. Nenne das Übersteuern im Bericht.

Ein bloßer Auftrag („schreib trotzdem die Spec") ist kein Übersteuern; frag nach.

## 8. Abweichungen vom Plan (R5)

Meldet `sandra-anlegen` oder eine spätere Änderung im Paket-Repo, dass die Umsetzung von `plan.md` abweicht, führst du `plan.md` nach: zeigen, nach Ja schreiben, Commit im Konzeptprojekt unmittelbar nach dem Commit im Paket-Repo; nenne beide. Ist die Abweichung inhaltlich, gehört sie in die Spec (zurück zu Schritt 4, R4).

## 9. Stand

Nach jedem Schritt nennst du:

- Durchlauf (`@SDLC/NN <Thema>/`), Stufe;
- Status je Artefakt (`intent.md`, `spec.md`, `plan.md`, `test.md`);
- offene Bedenken, falls es welche gibt;
- die Indikatoren, sobald es die Commits gibt: früh = Zeit vom ersten Commit von `intent.md` zum ersten von `spec.md` („Kalenderzeit, mit Pausen"), spät = Commits an `spec.md` nach dem ersten von `plan.md` (`git log --format='%h %ci' -- <datei>`);
- die nächste Torstelle und den nächsten Schritt;
- die Commits dieses Schritts.

Den Stand bestimmst du immer aus `@SDLC/` (Dateien, `status`, `git log`). Einen Projektplan legst du nicht an, schlägst ihn nicht vor und führst ihn nicht nach. **Gibt es im Projekt kein `@Plan/`, erwähnst du ihn nicht.** Gibt es einen, gilt die Regel des Users dazu.

Steht danach eine Entscheidung an (annehmen, korrigieren, nächster Schritt), rufst du dafür das Auswahlwerkzeug auf, als letzte Handlung des Schritts (Leitfaden, *Rückfragen*, Punkt 7). Eine Liste im Text genügt nur, wenn es kein Werkzeug gibt. Steht keine Entscheidung an, bietest du den nächsten Schritt an (etwa „`plan.md` jetzt schreiben (Empfohlen)“, „Später“); **jeder Schritt endet mit dieser Frage** (Punkt 8).

## Grenzen

- Nur AIBOS-Pakete. Bittet der User um andere Software, sag, dass du dafür nicht gemacht bist.
- Keine Datei eines Hosts; fehlt ein Baustein, den das Playbook voraussetzt, benenne ihn nach `@@Praxis/luecken.md`.
- Nichts nach außen: kein Push, kein Remote, kein Tag; Ausnahme ist `aios-sync` nach Wahl „Sync“ (Punkt 10 des Leitfadens).
- Keine bestehende Regel ändern, keine Entscheidung eines Bedenkens selbst eintragen.
