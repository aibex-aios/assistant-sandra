---
name: sandra-anlegen
version: 0.3.0
description: >
  Legt das Paket-Repo eines AIBOS-Pakets nach dem angenommenen plan.md eines Durchlaufs an
  (Torstelle T3): Typ und Name nach dem Namensschema, Ort nach den Vorgaben des Users, Aufbau
  nach einem Paket desselben Typs, manifest.yaml 0.1.0, Lizenzdateien, README.md, lokaler
  Commit, kein Tag. Verwenden bei "Paket anlegen", "Repo anlegen", "bau es", "leg das Paket an".
---

# Paket-Repo anlegen

Ein Skill der Assistentin Sandra. Er legt ein Paket-Repo **lokal** an; nichts geht nach außen. Lies zuerst `@@Praxis/pakete.md`.

**Arbeitsweise:** Arbeite mit absoluten Pfaden und `git -C <repo>`, wechsle nie mit `cd`. Erst den Plan zeigen, dann auf das Ja warten, dann ausführen. „Bau es" ist ein Auftrag, keine Bestätigung des Plans und kein Übersteuern.

## 1. Durchlauf finden

Bestimme Konzeptprojekt und laufenden Durchlauf wie `sandra-sdlc` (Schritt 1): `@SDLC/NN <Thema>/` bzw. `@SDLC/<paket>/NN <Thema>/`. Gibt es keinen Durchlauf mit `plan.md`, sag das und schlag `sandra-sdlc` vor.

## 2. Torstelle T3

**T3 — vor dem Anlegen oder Ändern des Paket-Repos: `plan.md` hat Status `Angenommen`.**

- Erfüllt: Sag „T3 erfüllt: `plan.md` ist angenommen." und fahre fort.
- Nicht erfüllt: Sag „T3 nicht erfüllt: `plan.md` hat Status `<status>`.", lege nichts an und schlag vor, was zuerst geschehen muss (Plan fertigstellen und annehmen lassen).
- Übersteuern nur auf einen ausdrücklichen Satz („übersteuere T3"): Sag, dass die Torstellen beratend sind; trag `override` (Torstelle, Datum, Satz) in `plan.md` ein und nenne es im Bericht.

T3 gilt für jede spätere Änderung im Paket-Repo ebenso, nicht nur für das Anlegen.

## 3. Was angelegt wird

Lies `spec.md` und `plan.md` des Durchlaufs. Daraus:

| Punkt | Quelle |
|---|---|
| Typ und Name | `spec.md`; Name `<typ>-<name>`, Element `<name>` (Praxis *Pakete*, *Namen*) |
| `description` | ein Satz aus `spec.md` |
| `requires.aibos` | `">=0.7,<0.8"`, außer `spec.md` nennt anderes; `requires.aios` mit `<0.7` holt AIBOS 0.7 nicht (Spec 06, *Alter Schlüssel bis 0.8*) |
| Dateien | `plan.md`, *Dateien, die sich ändern* |
| Ort | Vorgaben des Users (Regel *AIBOS-Pakete — Vorgaben*, bis 0.8 auch *AIOS-Pakete — Vorgaben*, falls geladen); sonst `<konzeptprojekt>/@Package/<paket>/` |
| Lizenz | Vorgaben des Users; ohne Vorgabe offen lassen und nennen |

Gibt es den Zielordner schon, brich ab und sag es.

## 4. Vorlage

Nimm ein Paket **desselben Typs** als Vorlage für den Aufbau:

1. zuerst ein geholtes Paket im Masterspace der Kette (`@/@!Package/<paket>/`, vom Projekt aufwärts);
2. sonst den getaggten Stand eines veröffentlichten Pakets im Hub, als flachen Klon in ein temporäres Verzeichnis außerhalb des Vaults, nur lesend: `git clone --depth 1 --branch v<version> <repo-url> <tmp>`.

Übernimm nur den **Aufbau** (Ordner, Frontmatter-Felder, Abschnitte), nie Inhalte, Wissen, Daten oder `contrib/`. Findest du keine Vorlage, baue nach der Praxis *Pakete* ohne Vorlage und sag das.

## 5. Plan zeigen und fragen

Zeig vor dem Schreiben:

- Zielordner;
- jede Datei mit Kurzinhalt, `manifest.yaml` vollständig;
- Lizenzdateien;
- Eintrag `/@Package/` in der `.gitignore` des Konzeptprojekts, falls der Ort unter `@Package/` liegt und die Zeile fehlt;
- die Commits (Paket-Repo, Konzeptprojekt).

Schreibe erst nach Ja.

## 6. Anlegen

1. Zielordner anlegen, `git -C <repo> init -b main`.
2. `manifest.yaml` mit `version: 0.1.0` und `contents` passend zu den Ordnern; jeder Skill und Assistent mit `version: 0.1.0` im Frontmatter.
3. Elemente nach `spec.md`: bei Skills `SKILL.md` mit `name`, `version`, `description` und einem Ablauf, der bei jedem schreibenden Schritt erst den Plan zeigt und fragt. Ruft ein Skill `aibos` auf, bekommt er den Satz: „Rufe `aibos` über den Weiterleiter auf: mit dem absoluten Pfad aus dem Block `<aibos-kernel-skills>` deines Master-Kontexts, als einzelnen Befehl ohne `cd`, `;`, `&&`, `|` oder Umleitung."
4. Lizenz: bei Apache-2.0 `LICENSE` als unveränderte Kopie des Lizenztexts (etwa aus dem Paket dieser Assistentin oder der Vorlage), `NOTICE` mit Paketname, Copyright-Zeile und, falls vorgegeben, dem Satz zu den Marken.
5. `README.md`: Zweck, Voraussetzung (AIBOS v0.7), Holen mit `aibos --fetch <paket>`, Aufruf, Lizenz.
6. Commit im Paket-Repo: „<paket> 0.1.0: angelegt aus <vorlage>" (bzw. „ohne Vorlage"). **Kein Tag**; getaggt wird erst der Release-Kandidat mit `sandra-probe` (T4).
7. Falls nötig: `/@Package/` in die `.gitignore` des Konzeptprojekts, dort nur diese Zeile committen.

Keine Datei eines Hosts: kein `CLAUDE.md`, kein `.claude/`. Fehlt etwas, das die Spec verlangt, nenne es als Lücke (`@@Praxis/luecken.md`).

## 7. Plan nachführen und melden

- Weicht das Angelegte von `plan.md` ab (andere Datei, andere Reihenfolge), führe `plan.md` nach R5 nach: zeigen, nach Ja, Commit im Konzeptprojekt unmittelbar nach dem Commit im Paket-Repo. Inhaltliche Abweichungen gehören in die Spec; dann halte an und frag.
- Melde: Ort, Dateien, Commits, Stand des Durchlaufs (Stufe 3 Build, Paket-Repo angelegt) und den nächsten Schritt: Inhalte nach `plan.md`, dann die Prüfung mit `sandra-pruefen`. Biete den nächsten Schritt als Frage im Auswahlwerkzeug an (Leitfaden, *Rückfragen*, Punkt 8).
