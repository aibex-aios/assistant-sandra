---
name: sandra-probe
version: 0.2.0
description: >
  Bereitet die Probe eines geprüften AIBOS-Pakets vor (Torstelle T4): Release-Kandidat
  v<x.y.z>-rc<n> lokal taggen, Testprojekt mit aios --init anlegen, den Kandidaten mit
  aios --fetch --from --to hineinholen, Anleitung für den neuen Chat; nach der Probe die
  Abnahme annehmen lassen, die Freigabe-Version lokal schreiben (Torstelle T6) und das
  Testprojekt in den Papierkorb anbieten. Nichts geht nach außen. Verwenden bei "Probe",
  "Release-Kandidat", "rc taggen", "ausprobieren", "Abnahme", "Freigabe-Version".
---

# Probe eines Release-Kandidaten

Ein Skill der Assistentin Sandra. Er taggt **lokal**, legt ein Testprojekt an, holt den Kandidaten hinein und schreibt nach der Abnahme die Freigabe-Version; nichts geht nach außen (kein Push, kein Tag am Remote, kein Hub). Lies zuerst `@@Praxis/pruefen-und-probe.md`.

**Arbeitsweise:** Arbeite mit absoluten Pfaden und `git -C <repo>`, wechsle nie mit `cd`. Rufe `aios` über den Weiterleiter auf: mit dem absoluten Pfad aus dem Block `<aios-kernel-skills>` deines Master-Kontexts, als einzelnen Befehl ohne `cd`, `;`, `&&`, `|` oder Umleitung. Jeder schreibende Befehl von `aios` läuft zuerst mit `--plan`; du zeigst den Plan, fragst und rufst erst nach Ja mit `--yes` auf.

## 1. Lage

Bestimme Durchlauf und Paket-Repo wie `sandra-pruefen`. Lies `test.md`, `spec.md` (Abnahmetest) und die Tags des Repos (`git -C <repo> tag`). Die nächste Kandidatennummer `n` ist die höchste vorhandene `v<x.y.z>-rc<n>` plus 1, sonst 1.

## 2. Torstelle T4

**T4 — vor dem Tag eines Release-Kandidaten: `test.md` mit `result: ohne Befund` für den aktuellen Commit des Paket-Repos.**

Vergleiche den Commit in `test.md` mit `git -C <repo> log -1 --format=%h` und prüfe, dass der Arbeitsbaum committet ist.

- Erfüllt: Sag „T4 erfüllt: `test.md` ist ohne Befund für Commit `<sha>`." und fahre fort.
- Nicht erfüllt: Sag „T4 nicht erfüllt: <fehlt `test.md` | `result: Befunde` | `test.md` gilt für `<sha>`, aktuell ist `<sha>`>.", tagge nicht und schlag die Prüfung mit `sandra-pruefen` vor.
- Übersteuern nur auf einen ausdrücklichen Satz („übersteuere T4"): Sag, dass die Torstellen beratend sind; trag `override` (Torstelle, Datum, Satz) in `plan.md` ein und nenne es im Bericht.

## 3. Plan zeigen und fragen

Zeig alle Schritte auf einmal, frag mit der Auswahl „Ausführen (Commit)“ (Empfohlen), „Ausführen (Commit, Sync)“ (nur unter der Bedingung von Punkt 10: Workspace mit `sync` und eigenes Remote des Projekts; Sync schiebt nur Commits, keine Tags), „Später“ (Leitfaden, *Rückfragen*, Punkt 10) und führ sie nach Ja der Reihe nach aus:

| # | Schritt | Befehl |
|---|---|---|
| 1 | Manifest auf den Kandidaten setzen: `version: <x.y.z>-rc<n>` (die Elemente behalten `<x.y.z>`), Commit „<paket> <x.y.z>-rc<n>" | Datei ändern, `git -C <repo> commit` |
| 2 | lokaler Tag | `git -C <repo> tag v<x.y.z>-rc<n>` |
| 3 | Testprojekt anlegen (falls es fehlt) | `aios --init "<testprojekt>" --plan`, nach Ja `--yes` |
| 4 | Kandidaten holen | `aios --fetch "<paket>==<x.y.z>-rc<n>" --from "<repo>" --to "<testprojekt>" --plan`, nach Ja `--yes` |

- **Testprojekt:** ein Unterprojekt des Konzeptprojekts, Name nach der Namensregel (Präfix des Konzeptprojekts plus `P`) und der Konvention `<Präfix>P-Probe-<Name>`, z. B. `MASPP-Probe-Wortzahl` unter `MASP-Probe-Sandra`. Zeigt `aios --init --plan` einen Verstoß gegen die Namensregel, nenne ihn und den Vorschlag und frag.
- Ein Tag wird nie verschoben. Stimmt ein vorhandener Kandidat nicht, ist ein neuer `-rc<n+1>` fällig.
- Schritt 1 ist eine Änderung im Paket-Repo: T3 gilt (`plan.md` angenommen). Sie ändert nur `version` im Manifest; T4 bezieht sich auf den geprüften Commit unmittelbar davor. Jede andere Änderung braucht eine neue Prüfung.

## 4. Ausführen

Führ die Schritte einzeln aus und prüf nach jedem das Ergebnis:

- nach 2: `git -C <repo> show v<x.y.z>-rc<n>:manifest.yaml` nennt `version: <x.y.z>-rc<n>`;
- nach 3: das Testprojekt hat `@/` und ein eigenes Repo;
- nach 4: keine Meldung der Prüfung beim Holen; `lock.yaml` des Testprojekts nennt `<paket>` mit `<x.y.z>-rc<n>`.

Schlägt ein Schritt fehl, halt an, nenne den Stand und frag. Was schon getaggt ist, bleibt; korrigiert wird mit einem neuen Kandidaten.

## 5. Anleitung für den neuen Chat

Gib dem User eine kurze Anleitung:

1. Einen **neuen Chat** im Testprojekt öffnen (Pfad nennen); erst dort sind die geholten Skills registriert.
2. Einen Skill als ganzen Prompt aufrufen: `/<skill> …`; einen Assistenten im Testprojekt selbst zuordnen: `/aios-assistant <name>`.
3. Die Schritte des Abnahmetests aus `spec.md` der Reihe nach durchgehen; Abweichungen zurückmelden.

## 6. Abnahme

Meldet der User das Ergebnis der Probe zurück:

- **Abweichungen** sind Befunde: zurück zur Korrektur (`sandra-pruefen`, Abschnitt *Befunde beheben*), danach ein neuer Kandidat `-rc<n+1>`.
- **Alles wie erwartet:** Leg die Annahme als Auswahl vor („Ja, die Abnahme ist angenommen (Commit)“ (Empfohlen), mit Sync nur unter der Bedingung von Punkt 10, „Noch nicht“). Sie setzt `result: ohne Befund` in `test.md` voraus. Nach der Wahl setzt du in `test.md` `status: Angenommen` (und `date`), committest „SDLC NN: test.md angenommen (Abnahme)" und nennst den Commit.

## 7. Freigabe-Version (T6)

**T6 — vor dem Commit der Freigabe-Version: `test.md` hat Status `Angenommen`, und sein `commit` ist der Commit, auf den der Tag des Kandidaten zeigt.**

Lies `status` und `commit` aus `test.md` und vergleiche mit `git -C <repo> rev-list -n 1 v<x.y.z>-rc<n>` (dem höchsten Kandidaten).

- Erfüllt: Sag „T6 erfüllt: Abnahme angenommen für `<sha>` = `v<x.y.z>-rc<n>`." und fahre fort.
- Nicht erfüllt: Sag „T6 nicht erfüllt: …" mit dem Grund (Abnahme nicht angenommen bzw. `test.md` gilt für `<sha>`, der Kandidat zeigt auf `<sha>`), schreib nichts und leg die Annahme der Abnahme als Auswahl vor, falls sie fehlt.
- Übersteuern nur auf einen ausdrücklichen Satz („übersteuere T6"), wie bei T4.

Dann Plan zeigen und mit derselben Auswahl wie in Abschnitt 3 fragen:

| # | Schritt | Befehl |
|---|---|---|
| 1 | Manifest auf die Freigabe-Version setzen: `version: <x.y.z>`, Commit „<paket> <x.y.z>" | Datei ändern, `git -C <repo> commit` |
| 2 | lokaler Tag | `git -C <repo> tag v<x.y.z>` |

Prüf danach `git -C <repo> show v<x.y.z>:manifest.yaml`. Kein Push: Veröffentlichen ist Sache des Users.

## 8. Testprojekt in den Papierkorb

Nach der Freigabe-Version biete an, das Testprojekt des Pakets wegzuräumen, bevor jemand synchronisiert. Zeig den Plan: Ziel `~/.Trash/<Name> <Datum>`, dazu die Zeile `/<Name>/` aus der `.gitignore` des übergeordneten Projekts entfernen und diese Änderung committen. Frag mit der Auswahl „In den Papierkorb (Empfohlen)“, „Behalten“. Nach Ja verschieben, committen und sagen, wo es liegt.

## 9. Melden

Nenne Kandidat, Tag, Commits, Testprojekt, Ergebnis des Holens und den Stand des Durchlaufs (Stufe 4 Test: Probe vorbereitet, abgenommen oder freigegeben). Sag ausdrücklich, dass nichts gepusht wurde und das Veröffentlichen Sache des Users ist; fehlt dafür ein Tor in AIBOS, nenne die Lücke (`@@Praxis/luecken.md`).
