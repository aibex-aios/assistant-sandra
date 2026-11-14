# Praxis — Implementierungsplan (`plan.md`)

Stufe 3 *Build*, erster Teil. Quellen: Playbook S. 19–21; dein Leitfaden (*Torstellen*, T2, T3).

## Wozu

Der Plan sagt, **wie** die Spec umgesetzt wird, bevor eine Datei im Paket-Repo entsteht. Im Playbook beginnt jede Umsetzung mit einem geschriebenen Plan, den der Mensch korrigiert, solange Umsteuern nur das Ändern eines Dokuments kostet (S. 19–20). Der angenommene Plan ist später der Maßstab, an dem das Ergebnis gemessen wird.

## Vor dem Schreiben

- **T2:** `spec.md` hat `status: Angenommen`. Sonst schreibst du nicht und sagst, was fehlt.
- Lies Absicht und Spec ganz und sieh dir ein Paket desselben Typs als Vorlage an (Praxis *Pakete*).

## Die vier Teile

Nach dem Beispiel des Playbooks (S. 20):

| Teil | Inhalt |
|---|---|
| **Dateien, die sich ändern** | jede Datei im Paket-Repo, neu oder geändert, mit Pfad relativ zum Repo |
| **Reihenfolge der Arbeit** | nummerierte Schritte; jeder Schritt mit Ergebnis und, wo sinnvoll, einem Commit |
| **Risiken** | was schiefgehen kann und wie du es abfängst |
| **Beweis** | woran sich zeigt, dass es fertig ist: Prüfung mit `sandra-pruefen`, `test.md` ohne Befund, Release-Kandidat, Abnahmetest der Spec |

Frontmatter wie bei allen Artefakten des Durchlaufs, `related` mit `intent.md` und `spec.md`.

## Den Plan befragen

Bevor du ihn vorlegst, prüfst du ihn selbst, wie es das Playbook dem Menschen empfiehlt (S. 19, Schritt 3): Was könnte die Änderung kaputt machen? Welcher Schritt ist am riskantesten? Welche Alternative hast du verworfen und warum? Die Antworten gehören zu den Risiken.

Der Plan ist gut genug, wenn jemand, der das Gespräch nicht kennt, das Paket allein danach bauen könnte (S. 20, Schritt 4).

## Annehmen und umsetzen

1. Zeig den Plan; der User korrigiert.
2. Commit, dann auf ausdrücklichen Satz `status: Angenommen`. Erst das öffnet **T3**: Vorher legst du kein Paket-Repo an und änderst keines.
3. Setze die Schritte der Reihe nach um; nach jedem Schritt nennst du den Stand.

## Abweichungen

Weicht die Umsetzung vom Plan ab, führst du `plan.md` **im selben Commit** nach wie die Änderung, die abweicht (S. 20, Schritt 7). So passen Plan und Paket an jedem Commit zusammen. Liegen Plan und Paket in verschiedenen Repos (Konzeptprojekt und `@Package/<paket>/`), committest du beide unmittelbar nacheinander und nennst beide Commits.

Ist die Abweichung inhaltlich (anderes Verhalten, andere Elemente), ist das kein Fall für den Plan, sondern für die Spec: zurück zu Stufe 2, Spec ändern, erneut annehmen lassen.

Das Playbook schlägt vor, den Gleichlauf von Plan und Code mit einem Hook zu erzwingen (S. 20, Schritt 7). AIBOS hat dafür keinen neutralen Baustein; du hältst dich selbst daran (Praxis *Lücken*).

## Fallen

- **Plan als Wiederholung der Spec:** Der Plan sagt *wie* und *in welcher Reihenfolge*, nicht noch einmal *was*.
- **Zu grobe Schritte:** Ein Schritt, dessen Ergebnis sich nicht prüfen lässt, ist zu groß.
- **„Bau es" als Annahme:** Ein Auftrag ist keine Annahme des Plans und kein Übersteuern von T3.
