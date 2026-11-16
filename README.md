# assistant-sandra

Assistentin *Sandra* von AIbex AIBOS, Version 0.3.0: Sie entwickelt AIBOS-Pakete nach dem SDLC aus *The AI-Native SDLC Playbook* (Anthropic, 2026). Sie übernimmt die Methode des Playbooks, jede Stufe endet mit einem Artefakt, das die nächste liest, und bleibt dabei neutral gegenüber dem Host: Sie verwendet nur, was AIBOS selbst bereitstellt, und benennt fehlende Bausteine als Lücke. Jede Antwort beginnt mit „👩‍⚖️ ".

## Holen

**Voraussetzung:** AIbex AIBOS v0.7 ist installiert, und der Vault ist an der Vault-Root mit `aibos --activate v0.7.<n>` gebunden (ein AIBOS-Workspace); sonst tut `aibos --fetch` nichts. Für AIbex AIOS v0.6 gibt es Sandra 0.2.0 (`aios --fetch "assistant-sandra~=0.2.0"`).

```bash
aibos --fetch assistant-sandra
```

Zuordnen mit `/aibos-assistant Sandra` in dem Projekt, in dem das Paket entstehen soll. Sandra und ihre Skills wirken **ab dem nächsten Chat** in diesem Projekt.

## Wie Sandra arbeitet

Jede Änderung an einem Paket ist ein **Durchlauf** in `@SDLC/NN <Thema>/` des Projekts, mit je einem Artefakt pro Stufe:

| Stufe | Artefakt | Skill | Torstelle davor |
|---|---|---|---|
| 1 Plan | `intent.md`: Absicht | `sandra-sdlc` | — |
| 2 Design | `spec.md`: Spezifikation mit angewandten Vorgaben, Bedenken und Abnahmetest | `sandra-sdlc` | T1: Absicht angenommen |
| 3 Build | `plan.md`: Implementierungsplan, dann das Paket-Repo | `sandra-sdlc`, `sandra-anlegen` | T2: Spec angenommen, alle Bedenken entschieden; T3: Plan angenommen |
| 4 Test | `test.md`: Prüfbericht und Abnahme; Release-Kandidat, Probe, Freigabe-Version | `sandra-pruefen`, `sandra-probe` | T4: Prüfung ohne Befund; T6: Abnahme angenommen |

Die Skills sind private Skills von Sandra: Sie wirken, solange Sandra dem Projekt zugeordnet ist, und du rufst sie mit einem Satz im Chat auf, etwa „neuer Durchlauf für …“, „schreib die Spec“, „leg das Paket an“, „prüf das Paket“, „mach die Probe“.

Sandra formuliert, du korrigierst und nimmst jedes Artefakt ausdrücklich an. An den Torstellen prüft sie die Bedingung und hält an, wenn sie nicht erfüllt ist; übersteuern kannst nur du, mit einem ausdrücklichen Satz wie „übersteuere T3". Die Torstellen sind beratend, weil AIBOS heute kein neutrales Tor kennt.

## Neu in 0.2

- **Vorgaben:** Beim Schreiben der Spec wendet Sandra die Regeln deines Masterspace (`@/@Rule/`) an und nennt sie mit Stand und Eigentümer (Feld `owner:` der Regel, sonst du). Wo Absicht und Regel sich reiben, markiert sie ein **Bedenken**; erst wenn alle entschieden sind, geht es zum Plan. Auf Wunsch hält sie eine neue Vorgabe als Regel fest.
- **Indikatoren:** Im Stand nennt Sandra die Zeit von der Absicht zur Spec und die Nacharbeit an der Spec nach dem Plan, aus Git.
- **Abnahme und Freigabe-Version:** Nach der Probe nimmst du die Abnahme an; dann schreibt Sandra die Version lokal (Commit und Tag, T6) und bietet an, das Testprojekt in den Papierkorb zu legen.
- „Commit, Sync" bietet Sandra nur in Projekten mit eigenem Remote an.

## Grenzen

- Nur AIBOS-Pakete, keine andere Software.
- Keine Dateien oder Mechanismen eines bestimmten Hosts.
- Keine Schritte nach außen: Push, Repo am Remote, Tag am Remote, Hub und Index bleiben bei dir.
- Die Stufen 5 *Deploy* und 6 *Maintain* (Review, Incident) kommen in späteren Versionen.
- Subagents, Evals und erzwungene Tore fehlen AIBOS heute; Sandra nennt sie als Lücke.

## Quellen

Sandra misst alles, was AIBOS betrifft, an der installierten Spec (`~/.aibos/versions/v<x.y.z>/doc/spec/`). Auf das Playbook verweist sie mit Seitenzahlen; es ist nicht Teil des Pakets.

## Vorgaben

Ort der Paket-Repos, Hub und Lizenz legst du in einer Regel *AIBOS-Pakete — Vorgaben* (`name: aibos-pakete-vorgaben`, bis 0.8 auch `aios-pakete-vorgaben`) im Masterspace fest. Ohne die Regel legt Sandra Paket-Repos in `<projekt>/@Package/<paket>/` an und fragt den Rest, wenn er gebraucht wird.

## Lizenz

Apache License 2.0 (`LICENSE`), Rechteinhaber AIbex KG (`NOTICE`). Die Marken „AIbex" und „AIbex AIBOS" sind nicht mitlizenziert.
