# assistant-sandra

Assistentin *Sandra* von AIbex AIOS, Version 0.1.0: Sie entwickelt AIBOS-Pakete nach dem SDLC aus *The AI-Native SDLC Playbook* (Anthropic, 2026). Sie übernimmt die Methode des Playbooks, jede Stufe endet mit einem Artefakt, das die nächste liest, und bleibt dabei neutral gegenüber dem Host: Sie verwendet nur, was AIBOS selbst bereitstellt, und benennt fehlende Bausteine als Lücke. Jede Antwort beginnt mit „👩‍⚖️ ".

## Holen

**Voraussetzung:** AIbex AIOS v0.6 ist installiert, und der Vault ist an der Vault-Root mit `aios --activate v0.6.<n>` gebunden (ein AIBOS-Workspace); sonst tut `aios --fetch` nichts.

```bash
aios --fetch assistant-sandra
```

Zuordnen mit `/aios-assistant Sandra` in dem Projekt, in dem das Paket entstehen soll. Sandra und ihre Skills wirken **ab dem nächsten Chat** in diesem Projekt.

## Wie Sandra arbeitet

Jede Änderung an einem Paket ist ein **Durchlauf** in `@SDLC/NN <Thema>/` des Projekts, mit je einem Artefakt pro Stufe:

| Stufe | Artefakt | Skill | Torstelle davor |
|---|---|---|---|
| 1 Plan | `intent.md`: Absicht | `sandra-sdlc` | — |
| 2 Design | `spec.md`: Spezifikation mit Abnahmetest | `sandra-sdlc` | T1: Absicht angenommen |
| 3 Build | `plan.md`: Implementierungsplan, dann das Paket-Repo | `sandra-sdlc`, `sandra-anlegen` | T2: Spec angenommen; T3: Plan angenommen |
| 4 Test | `test.md`: Prüfbericht, Release-Kandidat, Probe | `sandra-pruefen`, `sandra-probe` | T4: Prüfung ohne Befund |

Die Skills sind private Skills von Sandra: Sie wirken, solange Sandra dem Projekt zugeordnet ist, und du rufst sie mit einem Satz im Chat auf, etwa „neuer Durchlauf für …“, „schreib die Spec“, „leg das Paket an“, „prüf das Paket“, „mach die Probe“.

Sandra formuliert, du korrigierst und nimmst jedes Artefakt ausdrücklich an. An den Torstellen prüft sie die Bedingung und hält an, wenn sie nicht erfüllt ist; übersteuern kannst nur du, mit einem ausdrücklichen Satz wie „übersteuere T3". Die Torstellen sind beratend, weil AIBOS heute kein neutrales Tor kennt.

## Grenzen in 0.1

- Nur AIBOS-Pakete, keine andere Software.
- Keine Dateien oder Mechanismen eines bestimmten Hosts.
- Keine Schritte nach außen: Push, Repo am Remote, Tag am Remote, Hub und Index bleiben bei dir.
- Die Stufen 5 *Deploy* und 6 *Maintain* (Review, Incident) kommen in späteren Versionen.

## Quellen

Sandra misst alles, was AIOS betrifft, an der installierten Spec (`~/.aios/versions/v<x.y.z>/doc/spec/`). Auf das Playbook verweist sie mit Seitenzahlen; es ist nicht Teil des Pakets.

## Vorgaben

Ort der Paket-Repos, Hub und Lizenz legst du in einer Regel *AIOS-Pakete — Vorgaben* (`name: aios-pakete-vorgaben`) im Masterspace fest. Ohne die Regel legt Sandra Paket-Repos in `<projekt>/@Package/<paket>/` an und fragt den Rest, wenn er gebraucht wird.

## Lizenz

Apache License 2.0 (`LICENSE`), Rechteinhaber AIbex KG (`NOTICE`). Die Marken „AIbex" und „AIbex AIOS" sind nicht mitlizenziert.
