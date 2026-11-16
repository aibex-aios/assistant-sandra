# Praxis — Pakete

Stufe 3 *Build*, zweiter Teil: das Paket-Repo. Quellen: Spec von AIBOS 06 (*Pakete*, *Aufbau eines Paket-Repos*, *`manifest.yaml`*, *Versionen und Bereiche*), 03 (*Format eines Skills*, *Versionen*, *Assistenten*), 02 (*Klassen*); Regel *AIBOS-Pakete — Vorgaben* des Users, falls geladen.

Maßstab ist immer die Spec der AIBOS-Version aus `requires.aibos` des Pakets (`~/.aibos/versions/v<x.y.z>/doc/spec/`). Was hier steht, ist eine Kurzfassung für v0.7; im Zweifel liest du dort nach.

## Vor dem Anlegen

- **T3:** `plan.md` hat `status: Angenommen`. Das gilt für das Anlegen und für jede spätere Änderung im Paket-Repo.
- Ort, Hub, Sichtbarkeit, Lizenz und Sprache kommen aus den Vorgaben des Users. Fehlen sie, gilt als Ort `<konzeptprojekt>/@Package/<paket>/` mit `/@Package/` in der `.gitignore` des Konzeptprojekts; Lizenz und Hub fragst du, spätestens wenn sie gebraucht werden.

## Namen

| Was | Regel |
|---|---|
| Paket | `<typ>-<name>`, etwa `skill-wortzahl`, `assistant-sandra`; Kleinbuchstaben, Ziffern, Bindestriche, höchstens 64 Zeichen; nie `aibos`, `aibos-…`, `gui`, `none`, `inherit`. AIBOS wertet nur `type` aus; das Schema ist Konvention der AIbex-Pakete (Spec 06 *Pakete*) |
| Element | `<name>` gleich dem Verzeichnisnamen und dem Feld `name` im Frontmatter |
| Assistent | zusätzlich `title` als Anzeigename |

## Typen

`kernel`, `app`, `assistant`, `skill`, `knowledge`, `tutorial`, `distribution`, `worker`. Ein unbekannter Typ wird mit Warnung als `knowledge` behandelt. Kernel, Distributionen und Arbeitspferde sind Sonderfälle mit eigenen Abschnitten der Spec (04, 06 *Distributionen*, 07); prüf sie dort, bevor du eines anlegst.

## Aufbau

Nur `manifest.yaml` ist Pflicht (Spec 06 *Aufbau eines Paket-Repos*):

```text
<paket>/
├── manifest.yaml
├── README.md
├── skills/<name>/SKILL.md
├── assistants/<name>/ASSISTANT.md
│   (darin @!Skill/ für private Skills, @Wissen/ mit Klassen)
├── knowledge/
└── commands/<name>
```

Wissen folgt den Klassen des Masterspace (Spec 02 *Klassen*): `@…` wird geladen, `@@…` ist verfügbar und steht nur im Inhaltsverzeichnis, `@!…` ist verborgen. Ins Paket gehört nur, was beim Ausrollen gebraucht wird; Memos, Notizen, Testdaten und die Artefakte des Durchlaufs bleiben im Konzeptprojekt.

## Manifest

```yaml
name: skill-wortzahl
version: 0.1.0
type: skill
description: Zählt die Wörter einer Datei im Projekt
requires:
  aibos: ">=0.7,<0.8"
contents:
  skills: [wortzahl]
license: Apache-2.0
```

Pflicht: `name` (gleich dem Repo im Hub), `version`, `type`, `description` (ein Satz), `requires.aibos`. Optional: `depends` (nie `aibos`), `contents`, `license`. Jeder Name in `contents` muss als Ordner vorhanden sein; sonst wird das Paket beim Holen abgelehnt (Spec 06 *Prüfung beim Holen*).

## Versionen

- SemVer, neue Pakete beginnen mit `0.1.0`. Eine Version ist ein Tag `v<version>`; `version` im Manifest des getaggten Stands muss gleich sein.
- Die Spec erlaubt Skills und Assistenten eine eigene Version (Spec 03 *Versionen*). Konvention der AIbex-Pakete: Elemente in `skills/` und `assistants/` tragen die Paketversion, private Skills ihre eigene.
- Jeder Skill braucht ein gültiges `version` im Frontmatter, sonst wird er nicht registriert.
- Vor 1.0 darf jede Minor-Version brechen; Holen ohne Bereich bleibt in der Minor-Version (`~=0.Y.Z`).
- Ein Tag wird nie verschoben; jede Änderung, auch am README, ist eine neue Version.

## Lizenz

Nach den Vorgaben des Users. Bei Apache-2.0: `license: Apache-2.0`, `LICENSE` mit unverändertem Text, `NOTICE` mit Paketname und Copyright-Zeile (und, falls vorgegeben, dem Satz zu den Marken).

## Vorlage

Nimm ein veröffentlichtes Paket desselben Typs im getaggten Stand als Vorlage, lokal oder als flachen Klon in ein temporäres Verzeichnis, nur lesend. Übernimm nur den **Aufbau** (Ordner, Frontmatter-Felder, Abschnitte), nie Inhalte, Wissen, Daten oder `contrib/`.

## Anlegen

1. Plan zeigen: Zielordner, Dateien mit Kurzinhalt, Manifest vollständig, Lizenzdateien, `.gitignore`-Eintrag. Erst nach Ja schreiben.
2. `git init -b main` im Zielordner, Dateien schreiben.
3. Lokaler Commit „<paket> 0.1.0: angelegt aus <vorlage>". **Kein Tag**; getaggt wird erst der Release-Kandidat (T4).
4. Stand nennen und `plan.md` abhaken bzw. nachführen.

## Skills, die schreiben oder `aibos` aufrufen

- Jeder schreibende Skill zeigt erst den Plan, fragt und führt dann aus; der bloße Wunsch des Users ist keine Bestätigung des Plans.
- Ein Skill, der `aibos` aufruft, sagt: „Rufe `aibos` über den Weiterleiter auf: mit dem absoluten Pfad aus dem Block `<aibos-kernel-skills>` deines Master-Kontexts, als einzelnen Befehl ohne `cd`, `;`, `&&`, `|` oder Umleitung." Nur so lässt der Hook den Aufruf zu (Spec 02 *Freigabe*).
- Kein Skill legt Dateien eines Hosts an. Was AIBOS beim Registrieren für den Host erzeugt, ist Build-Output.
