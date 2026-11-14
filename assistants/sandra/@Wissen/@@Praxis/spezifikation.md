# Praxis — Spezifikation (`spec.md`)

Stufe 2 *Design*. Quellen: Playbook S. 15–17; Spec von AIOS 03 (Skills und Assistenten), 06 (Pakete); dein Leitfaden (*Torstellen*, T1).

## Wozu

Die Spec übersetzt die angenommene Absicht in etwas, nach dem gebaut werden kann: welche Elemente das Paket hat, wie sie sich verhalten, wo ihre Grenzen liegen und woran man erkennt, dass es fertig ist. Im Playbook entstehen Anforderungen und Entwurf in **einem** Durchgang, und der Product Owner prüft die Spec, statt sie selbst zu schreiben (S. 15). So hältst du es auch: Du schreibst, der User prüft.

## Vor dem Schreiben

- **T1:** `intent.md` hat `status: Angenommen`. Sonst schreibst du nicht und sagst, was fehlt.
- Lies die Absicht ganz, dazu die Vorgaben des Users und die Spec der AIOS-Version aus `requires.aios`. Sie sind die Vorgaben, an die sich die Spec halten muss; im Playbook übernehmen diese Rolle die Skills der Organisation (S. 15, 17).

## Inhalt

| Abschnitt | Inhalt |
|---|---|
| Zweck | ein, zwei Sätze aus der Absicht; Verweis auf `intent.md` |
| Umfang | Tabelle *in dieser Version / später* |
| Elemente | Paketname nach dem Namensschema, Typ, Manifest in Kürze, je Element Name und Aufgabe (Skills, Assistent, Wissen, Befehle) |
| Verhalten | was jedes Element tut, Schritt für Schritt aus Sicht des Users; wo es schreibt; wo es fragt |
| Grenzen | was das Paket ausdrücklich nicht tut |
| Bedenken | Punkte, an denen Absicht und Vorgaben sich reiben oder du unsicher bist, deutlich markiert (S. 15–16) |
| Abnahmetest | nummerierte Schritte mit erwartetem Ergebnis (unten) |

Halte die Spec knapp. Was in der Absicht steht, wiederholst du nicht, sondern verweist darauf.

## Abnahmetest

Der Abnahmetest ist das Herz der Spec: Er sagt vorab, woran „fertig" erkannt wird.

- Jede Zeile: Nummer, wer handelt (User oder Sandra), was getan wird, was erwartet wird.
- Aus Sicht des Users, mit echten Aufrufen: ein Skill als ganzer Prompt `/<skill> …`, ein Assistent im Testprojekt zugeordnet.
- Er läuft auf einem **Release-Kandidaten** `v<x.y.z>-rc<n>` in einem eigenen Testprojekt; den Ablauf kennt die Praxis *Prüfen und Probe*.
- Ordne die Schritte nach ihren Abhängigkeiten; was erst nach dem Holen gilt, kommt danach.
- Jeder Befehl von `aios` darin muss so in der Spec von AIOS stehen; erfinde keine Option.

## Prüfen mit dem User

Gehe mit dem User die Spec gegen die Absicht durch (S. 16, Schritte 3–4):

1. Löst die Spec das Problem aus `intent.md`?
2. Ist jede offene Frage der Absicht beantwortet oder bewusst weitergetragen?
3. Die markierten Bedenken zuerst klären, Frage für Frage (Leitfaden, *Rückfragen*); jedes bekommt eine Entscheidung.

Danach committest du `spec.md` neben `intent.md`; das Paar zeigt, was gewünscht und was entschieden wurde (S. 16, Schritt 5; S. 17). Auf einen ausdrücklichen Satz setzt du `status: Angenommen`; erst das öffnet T2 und damit den Implementierungsplan (S. 16, Schritt 6).

## Fallen

- **Spec ohne Abnahmetest:** Dann gibt es später keinen Maßstab für T4 und die Probe.
- **Erfundene Felder:** Manifest- und Frontmatter-Felder nur nach der Spec von AIOS (06 *`manifest.yaml`*, 03 *Format eines Skills*, *Assistenten*).
- **Änderung nach der Annahme:** Fällt die Spec auf `Entwurf` zurück, gilt T2 erneut; ein schon angenommener Plan muss geprüft werden.
- **Spät gefundene Lücke:** Zeigt sich beim Bauen, dass die Spec etwas offenlässt, kehrst du zur Spec zurück, statt still im Paket zu entscheiden. Das Playbook zählt solche Nacharbeit nach dem ersten `plan.md` als Spätindikator (S. 17).
