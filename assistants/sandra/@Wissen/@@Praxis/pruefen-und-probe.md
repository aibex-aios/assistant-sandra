# Praxis — Prüfen und Probe

Stufe 4 *Test*. Quellen: Playbook S. 31–35; Spec von AIBOS 06 (*`manifest.yaml`*, *Prüfung beim Holen*, *Versionen und Bereiche*, *Holen — `aibos --fetch`*), 05 (*Namensregel*, `aibos --init`), 03 (*Format eines Skills*, *Assistenten*); dein Leitfaden (T4).

## Wozu

Das Playbook verlangt, dass die KI ihre Arbeit selbst prüft und Fehler behebt, bevor ein Mensch sie sieht (S. 31). Ein AIBOS-Paket hat keinen Build und meist keine Testsuite; seine Prüfung besteht aus zwei Teilen: dem **statischen Prüfen** gegen die Spec von AIBOS und die Spec des Pakets, festgehalten in `test.md`, und der **Probe** des Release-Kandidaten in einem Testprojekt.

## Prüfen (`sandra-pruefen`)

Du liest nur und schreibst nichts ins Paket. Das Ergebnis kommt nach `test.md` im Durchlauf.

| Block | Prüfe |
|---|---|
| Manifest | Pflichtfelder; `type` bekannt; `description` ein Satz; `requires.aibos` gesetzt; `aibos` nicht in `depends` |
| Namen | Paket `<typ>-<name>` gleich dem Repo-Ordner; Elementnamen gleich Verzeichnis und `name` im Frontmatter; keine verbotenen Namen |
| `contents` | jeder Name als Ordner vorhanden; jeder Ordner genannt; keine doppelten Elementnamen |
| Versionen | gültiges SemVer in Manifest und jedem Frontmatter; Elemente nach der Konvention des Pakets; letzter Tag kleiner als die Version im Manifest |
| Assistent | Frontmatter vollständig (`name`, `title`, `version`, `description`); jeder Skill in `skills` auflösbar; Rollentext mit Personalität, Handwerk, Grenzen; Wissen mit Klassen und Inhaltsverzeichnis |
| Skills | `name`, `version`, `description` (höchstens 1024 Zeichen); jeder schreibende Skill mit Plan → Rückfrage → Ausführen; jeder Skill, der `aibos` aufruft, mit dem Satz zum Weiterleiter |
| Neutralität | keine Datei und kein Mechanismus eines Hosts im Paket (Suche nach `.claude`, `CLAUDE.md`); Fundstellen nur dort, wo eine Lücke beschrieben wird |
| Lizenz | nach den Vorgaben; bei Apache-2.0 `LICENSE` und `NOTICE` |
| Inhalt | keine Memos, Notizen, Testdaten, `.DS_Store`, Geheimnisse; `README.md`; Arbeitsbaum committet |
| Paket-Spec | Elemente, Verhalten und Grenzen wie in `spec.md`, auch die Entscheidungen zu den Bedenken; Abnahmetest vorhanden |

**`test.md`:**

```markdown
---
title: "Prüfbericht — <paket> <x.y.z>"
date: JJJJ-MM-TT
package: <paket> <x.y.z>
commit: <sha>
result: ohne Befund
status: Entwurf
---

Geprüfter Stand: Commit <sha> im Paket-Repo

| Block | Ergebnis | Fundstelle | Maßstab |
|---|---|---|---|

## Befunde
## Ergebnis
```

Befunde nummerierst du B1 … (vorher im Durchlauf nach der höchsten Nummer suchen), jeden mit Vorschlag. Behoben wird erst nach Ja und nach T3; danach prüfst du neu und schreibst `test.md` für den neuen Commit fort. `result: ohne Befund` gilt nur für den Commit, der in `commit` steht. Jede neue Prüfung schreibt `status: Entwurf`.

## Release-Kandidat (`sandra-probe`, T4)

- **T4:** `test.md` hat `result: ohne Befund` für den **aktuellen** Commit des Paket-Repos (`git -C <repo> log -1`). Sonst kein Tag.
- Ein eigener Commit setzt `version: <x.y.z>-rc<n>` im Manifest; die Elemente behalten `<x.y.z>`. Dann der lokale Tag `v<x.y.z>-rc<n>`.
- Der Tag wird nie verschoben. Jede Korrektur ergibt einen neuen Kandidaten `-rc<n+1>`.
- Gepusht wird nichts; das ist Sache des Users.

## Probe im Testprojekt

1. **Testprojekt:** ein eigenes, leeres AIBOS-Projekt, Name nach der Namensregel (Spec 05) und der Konvention des Users, etwa `<Präfix>P-Probe-<Name>` als Unterprojekt des Konzeptprojekts. Angelegt mit `aibos --init "<pfad>" --plan`, nach Ja mit `--yes`.
2. **Holen:** `aibos --fetch "<paket>==<x.y.z>-rc<n>" --from <paket-repo> --to <testprojekt>`. Mit `--from` kommt das Paket aus dem lokalen Repo statt aus einem Hub; geholt wird nur ein getaggter Stand. Die Vorabkennung muss im Bereich stehen, sonst passt der Kandidat nicht.
3. **Ausprobieren im neuen Chat:** Der User öffnet einen Chat im Testprojekt. Ein Skill wird als ganzer Prompt aufgerufen (`/<skill> …`); ein Assistent wird im Testprojekt selbst mit `/aibos-assistant <name>` zugeordnet.
4. **Abnahmetest:** Die Schritte aus `spec.md` der Reihe nach; du hältst den Stand je Schritt fest. Abweichungen sind Befunde und führen zu einem neuen Kandidaten.

Jeder Aufruf von `aibos` geht über den Weiterleiter (Praxis *Pakete*).

## Abnahme und Freigabe-Version (`sandra-probe`, T6)

- **Abnahme:** Nach der Probe legst du die Annahme von `test.md` als Auswahl vor („Ja, die Abnahme ist angenommen"). Sie setzt `result: ohne Befund` voraus. Wählt der User sie, setzt du `status: Angenommen` und committest.
- **T6:** `test.md` hat `status: Angenommen`, und `commit` ist der Commit, auf den der Tag des Kandidaten zeigt (`git -C <repo> rev-list -n 1 v<x.y.z>-rc<n>`). Sonst keine Freigabe-Version.
- **Freigabe-Version:** ein eigener Commit setzt `version: <x.y.z>` im Manifest, dann der lokale Tag `v<x.y.z>`. Kein Push; Veröffentlichen ist Sache des Users.

## Testprojekt in den Papierkorb

Nach der Freigabe-Version brauchst du das Testprojekt des Pakets nicht mehr. Biete an, es wegzuräumen, bevor jemand synchronisiert:

1. Plan zeigen: Ziel `~/.Trash/<Name> <Datum>`, dazu die Zeile `/<Name>/` aus der `.gitignore` des übergeordneten Projekts entfernen.
2. Nach Ja verschieben und die `.gitignore` committen. Nennen, wo es liegt.

## Was das Playbook mehr verlangt

Das Playbook erzwingt die Prüfung vor „fertig" und schützt Testdateien mit Hooks (S. 32–33) und lässt Evals bei jeder Änderung an Skills laufen (S. 34–35). Beides fehlt AIBOS als neutraler Baustein; du prüfst von Hand und nennst die Lücke (Praxis *Lücken*).
