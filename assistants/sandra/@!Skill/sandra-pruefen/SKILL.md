---
name: sandra-pruefen
version: 0.1.0
description: >
  Prüft das Paket-Repo eines AIBOS-Pakets gegen die Spec von AIOS und gegen spec.md des
  Durchlaufs: Manifest, Namen, contents, Versionen, Frontmatter, Rückfragen in Skills,
  Neutralität gegenüber dem Host, Lizenz, Inhalt ohne Notizen. Schreibt den Prüfbericht
  test.md in den Durchlauf, nichts ins Paket. Verwenden bei "Paket prüfen", "prüf das Paket",
  "ist das Paket fertig?", "test.md".
---

# Paket prüfen

Ein Skill der Assistentin Sandra. Er **liest** das Paket-Repo und schreibt nur `test.md` im Durchlauf. Korrekturen am Paket schlägt er vor; ausgeführt werden sie erst nach Ja und nach T3. Lies zuerst `@@Praxis/pruefen-und-probe.md`.

**Arbeitsweise:** Arbeite mit absoluten Pfaden und `git -C <repo>`, wechsle nie mit `cd`. Rufe `aios` über den Weiterleiter auf: mit dem absoluten Pfad aus dem Block `<aios-kernel-skills>` deines Master-Kontexts, als einzelnen Befehl ohne `cd`, `;`, `&&`, `|` oder Umleitung.

## 1. Was geprüft wird

- **Durchlauf** und **Paket-Repo** wie bei `sandra-sdlc` bzw. aus `plan.md` (*Dateien, die sich ändern*). Nennt der User ein Repo, nimm dieses.
- **Geprüfter Stand:** der aktuelle Commit, `git -C <repo> log -1 --format=%h`. Ist der Arbeitsbaum nicht committet (`git -C <repo> status --short`), ist das ein Befund; geprüft wird trotzdem, aber `test.md` kann dann nicht `ohne Befund` lauten.
- **Maßstäbe**, in dieser Reihenfolge:
  1. **AIOS:** die Spec der AIOS-Version aus `requires.aios` (`~/.aios/versions/v<x.y.z>/doc/spec/`), besonders 06 *`manifest.yaml`* und *Prüfung beim Holen*, 03 *Format eines Skills* und *Assistenten*. Kannst du sie nicht lesen, sag das und brich ab.
  2. **Paket:** `spec.md` des Durchlaufs.

## 2. Prüfliste

| Block | Prüfe |
|---|---|
| Manifest | Pflichtfelder `name`, `version`, `type`, `description`, `requires.aios`; `type` bekannt; `description` ein Satz; `aios` nicht in `depends` |
| Namen | Paket `<typ>-<name>` gleich dem Repo-Ordner; Elementnamen gleich Verzeichnis und `name` im Frontmatter; nicht `aios`, `aios-…`, `gui`, `none`, `inherit` |
| `contents` | jeder Name als Ordner vorhanden; jeder Ordner in `skills/`, `assistants/`, `commands/` genannt; keine doppelten Elementnamen; `gui` in `skills` |
| Versionen | gültiges SemVer in Manifest und jedem Frontmatter; Elemente in `skills/` und `assistants/` mit der Paketversion (ohne `-rc<n>`); letzter Tag (`git -C <repo> tag`) kleiner als die Version im Manifest |
| Assistent | `name`, `title`, `version`, `description`; jeder Name in `skills` auflösbar; Rollentext mit Personalität, Handwerk, Grenzen; Wissen mit Klassen und Inhaltsverzeichnis |
| Skills | `name`, `version`, `description` (höchstens 1024 Zeichen); jeder schreibende Skill mit Plan → Rückfrage → Ausführen; jeder Skill, der `aios` aufruft, mit dem Satz zum Weiterleiter |
| Abhängigkeiten | nur wenn `depends` gesetzt: jedes Paket im Hub vorhanden (`aios --library <paket>`, nur lesend), Bereich mit `~=` |
| Neutralität | `grep -rn` im Repo nach `.claude` und `CLAUDE.md`: Treffer nur, wo eine Lücke beschrieben wird; keine Datei und kein Ordner eines Hosts |
| Lizenz | nach den Vorgaben des Users; bei Apache-2.0 `LICENSE` und `NOTICE` vorhanden |
| Inhalt | keine Memos, Notizen, Testdaten, `.DS_Store`, Geheimnisse; `README.md` vorhanden |
| Paket-Spec | Elemente, Verhalten und Grenzen wie in `spec.md`; Abnahmetest in `spec.md` vorhanden |

Zähle und suche, statt zu schätzen; nenne zu jedem Befund Datei und Zeile.

## 3. `test.md` schreiben

Befunde nummerierst du B1 …; such vorher im Durchlauf nach der höchsten vergebenen Nummer (`grep -w`) und zähle weiter.

Zeig den Bericht im Chat, dann den Plan: `test.md` im Durchlauf neu bzw. fortgeschrieben, Commit im Konzeptprojekt. Nach Ja schreiben:

```markdown
---
title: "Prüfbericht — <paket> <x.y.z>"
date: JJJJ-MM-TT
package: <paket> <x.y.z>
result: ohne Befund | Befunde
---

Geprüfter Stand: Commit <sha> im Paket-Repo

| Block | Ergebnis | Fundstelle | Maßstab |
|---|---|---|---|

## Befunde

## Ergebnis
```

- `result: ohne Befund` nur, wenn kein Block einen Befund hat und der Arbeitsbaum committet ist.
- Gibt es schon ein `test.md`, schreibst du es für den neuen Commit fort; frühere Fassungen stehen in Git.
- Commit: „SDLC NN: test.md (<ergebnis>)". Nenne den Commit.

## 4. Befunde beheben

Schlag je Befund eine Korrektur vor; gibt es mehrere Wege, frag Frage für Frage (Leitfaden, *Rückfragen*). Korrekturen am Paket-Repo brauchen T3 (`plan.md` angenommen) und das Ja des Users; danach prüfst du neu und schreibst `test.md` für den neuen Commit fort. Weicht eine Korrektur vom Plan ab, gilt R5.

## 5. Melden

Nenne Ergebnis, geprüften Commit, Commit von `test.md` und den nächsten Schritt: bei `ohne Befund` die Probe mit `sandra-probe` (T4), sonst die Korrekturen; als Frage im Auswahlwerkzeug (Leitfaden, *Rückfragen*, Punkt 8).

Sag auch, was diese Prüfung **nicht** abdeckt: die Wirkung im Projekt. Die zeigt erst die Probe des Release-Kandidaten in einem Testprojekt.
