# Praxis — Vorgaben

Stufe 2 *Design* und das Wissen, das über einen Durchlauf hinaus gilt. Quellen: Playbook S. 15–17 (*Requirements and design*), S. 25 (*Skills as institutional knowledge*); Spec von AIOS 02 (*Klassen*, K1–K6); dein Leitfaden (*Vorgaben*, T2).

## Wozu

Im Playbook entsteht die Spec unter den Regeln der Organisation, nicht gegen sie: Was sonst erst ein spätes Review fände, wird beim Schreiben angewandt, und Reibungen kommen als markierte Bedenken auf den Tisch, bevor gebaut wird (S. 15–17). Das Wissen dafür ist dort in Skills gefasst, mit benanntem Eigentümer und schriftlicher Quelle (S. 25). In AIBOS ist der neutrale Ort für solches Wissen die **Regel** im Masterspace. Du wendest Regeln an; du erfindest keine eigene Form dafür.

## Welche Regeln gelten

- **Vorgaben** sind die Regeln, die im Master-Kontext des Konzeptprojekts geladen sind: aus `@/@Rule/` jedes Masterspace der Kette vom Vault bis zum Projekt und aus dem Wissen geholter Pakete.
- Du nennst in `spec.md` jede Regel, die das Paket betrifft (Namen, Lizenz, Sprache, Ablage, Verhalten von Skills …). Regeln ohne Bezug zum Paket lässt du weg.
- Was keine geladene Regel ist (ein Memo, ein Wunsch im Gespräch), ist keine Vorgabe. Ist es wichtig, schlag vor, es als Regel festzuhalten (unten).

## Stand und Eigentümer

| Angabe | so ermittelst du sie |
|---|---|
| Pfad | Pfad der Regeldatei, wie er im Master-Kontext steht |
| Stand | `git -C <repo der regel> log -1 --format=%h -- <datei>`; bei einer Regel aus einem geholten Paket dessen Version aus `lock.yaml` |
| Eigentümer | Feld `owner:` im Frontmatter der Regel; fehlt es, ist der User Eigentümer |

`owner:` ist eine Konvention von dir, kein Feld von AIOS; AIOS lädt Regeln vollständig als Text. In bestehenden Regeln legst du das Feld nicht von dir aus an.

## Bedenken

Ein **Bedenken** entsteht, wo die Absicht einer Vorgabe widerspricht, wo zwei Vorgaben sich widersprechen oder wo du unsicher bist, ob eine Vorgabe greift.

1. Trag es in die Tabelle *Bedenken* von `spec.md` ein: Nummer, Bedenken in einem Satz, Vorgabe (Pfad), Eigentümer, Entscheidung (leer).
2. Leg offene Bedenken dem Eigentümer Frage für Frage vor (Leitfaden, *Rückfragen*), mit Optionen wie „Vorgabe einhalten", „Ausnahme für dieses Paket", „Absicht ändern".
3. Trag die Entscheidung ein. Ändert sie den Inhalt der Spec, fällt `spec.md` auf `Entwurf` zurück.
4. Solange ein Bedenken offen ist, ist **T2** nicht erfüllt: kein `plan.md`.

Ist der Eigentümer nicht der User, sagst du das und fragst den User, wie die Entscheidung des Eigentümers eingeholt wird; du entscheidest nicht an seiner Stelle.

## Eine neue Regel schreiben

Manchmal fehlt die Regel, an der ein Bedenken hängt, oder der User will eine Vorgabe festhalten („Halte als Vorgabe fest: …").

1. **Plan zeigen:** Dateiname, Ort, Text, `owner:`, Herkunft. Ort ist `@/@Rule/` des Konzeptprojekts; einen höheren Masterspace nur, wenn der User ihn nennt.
2. **Inhalt:** Frontmatter mit `owner:`; ein Titel; die Vorgabe in wenigen klaren Sätzen; ein Satz zur Herkunft (Dokument oder Gespräch mit Datum).
3. **Nach Ja schreiben und committen** (Leitfaden, *Rückfragen* 10).
4. **Prüfen, dass sie gilt:** Die Regel muss im Master-Kontext stehen; das zeigt `aios --context` im Konzeptprojekt (über den Weiterleiter: mit dem absoluten Pfad aus dem Block `<aios-kernel-skills>` deines Master-Kontexts, als einzelnen Befehl ohne `cd`, `;`, `&&`, `|` oder Umleitung). In einem neuen Chat ist sie geladen.
5. **Lücke nennen:** Ob die Regel im Verhalten wirkt, prüft im Playbook ein Test, ob der Skill auslöst (S. 25); dafür fehlen AIBOS Evals (Praxis *Lücken*).

Eine bestehende Regel änderst du nie von dir aus. Will der User sie ändern, zeig die Änderung und lass den Eigentümer zustimmen (S. 25, Schritt 5).

## Fallen

- **Vorgaben aus dem Gedächtnis:** Nur was im Master-Kontext steht, gilt. Nenne nichts, was du nicht dort gelesen hast.
- **Stand vergessen:** Ohne Stand lässt sich später nicht zeigen, welche Fassung einer Regel galt (S. 17).
- **Bedenken still entschieden:** Du trägst nie selbst eine Entscheidung ein; sie kommt vom Eigentümer.
