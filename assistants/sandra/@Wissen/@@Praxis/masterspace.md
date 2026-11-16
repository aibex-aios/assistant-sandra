# Praxis — Masterspace statt Dateien eines Hosts

Quellen: Playbook S. 22–23; Spec von AIBOS 01 (*Begriffe*: Masterspace, Build-Output), 02 (*Klassen*, *Inhaltsverzeichnis des verfügbaren Wissens*).

## Wozu

Das Playbook gibt der KI ihr Projektwissen über eine Datei des Hosts: `CLAUDE.md` an der Wurzel des Repos, mit Konventionen, Befehlen, Aufbau und den Fehlern, die sich wiederholen; sie wird zu Beginn jeder Sitzung gelesen (S. 23). In AIBOS übernimmt diese Aufgabe der **Masterspace** `@/` eines Projekts. Er ist neutral gegenüber dem Host: AIBOS lädt ihn als Master-Kontext, gleich welcher Host darunter läuft.

## Was wohin gehört

| Wissen | Ort | Klasse |
|---|---|---|
| Konventionen, die in jedem Chat gelten (z. B. Ablage, Namen, Sprache) | `@/@Rule/<Thema>.md` | geladen |
| Nachschlagewissen, das nur bei Bedarf gebraucht wird | `@/@@<Thema>/…` | verfügbar, nur im Inhaltsverzeichnis |
| Stand eines Durchlaufs | `@SDLC/NN …/` (Dateien und `status`) | kein Masterspace; einen eigenen Projektplan führst du nicht |
| Entscheidungen eines Durchlaufs | `@SDLC/NN …/`, `@Memo/` | kein Masterspace |

Geladenes Wissen der Masterspaces gilt entlang der Kette: Eine Regel im `@/` eines übergeordneten Ordners gilt für alle Projekte darunter. Leg eine Regel darum so tief wie möglich und so hoch wie nötig ab.

## Wie eine Regel aussieht

Das Playbook rät, das Projektwissen kurz zu halten, weil es in jeder Sitzung ganz gelesen wird und Veraltetes nur Platz nimmt (S. 23, Schritt 5). Das gilt für `@Rule/` genauso:

- eine Datei je Thema, mit einer Überschrift und wenigen Punkten;
- woher die Regel stammt (Durchlauf, Memo), damit sie später geprüft werden kann;
- keine Geschichte, keine Begründung über einen Satz hinaus; Einzelheiten gehören ins verfügbare Wissen.

Das Playbook empfiehlt außerdem, einen Fehler, den die KI zweimal macht, als Korrektur ins Projektwissen aufzunehmen (S. 23, Schritt 4). Bei dir heißt das: Schlage dem User eine Regel in `@/@Rule/` vor; geschrieben wird sie nach seinem Ja.

## Was du nicht tust

- Du legst kein `CLAUDE.md`, kein `AGENTS.md` und keinen Ordner wie `.claude/` an, auch nicht „zusätzlich".
- Was AIBOS beim Registrieren für den Host erzeugt (etwa `.claude/skills/`), ist Build-Output: nie Quelle, jederzeit neu erzeugbar (Spec 01). Du änderst es nicht von Hand.
- Fragt der User nach `CLAUDE.md`, erklärst du, dass der Masterspace diese Rolle übernimmt, und zeigst, wo die gewünschte Regel hingehört. Dateien für bestimmte Hosts aus dem Masterspace zu erzeugen, wäre ein Baustein des Kernels, den es heute nicht gibt (Praxis *Lücken*).

## Fallen

- **Alles laden:** Langes Nachschlagewissen in einer geladenen Klasse macht jeden Chat schwerer; nimm `@@`.
- **Doppelte Regeln:** Steht dieselbe Regel in zwei Masterspaces der Kette, können sie auseinanderlaufen. Eine Stelle, die andere verweist.
- **Paket statt Regel:** Wissen, das in vielen Projekten gebraucht wird, gehört eher in ein Paket (`knowledge-…`) als in viele Masterspaces.
