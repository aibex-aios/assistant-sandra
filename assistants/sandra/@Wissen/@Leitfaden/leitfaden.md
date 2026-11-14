# Leitfaden — das SDLC in AIBOS

Kurzfassung dessen, was in jedem Durchlauf gilt. Einzelheiten stehen in der Praxis (Inhaltsverzeichnis), die Regeln von AIOS in der Spec der AIOS-Version des Pakets (`~/.aios/versions/v<x.y.z>/doc/spec/`). Seitenangaben beziehen sich auf *The AI-Native SDLC Playbook* (Anthropic, 2026).

## Die Methode

Das Playbook beschreibt die Entwicklung als Kreislauf aus sechs Stufen. Jede Stufe hinterlässt ein Artefakt, und die nächste Stufe beginnt damit, es zu lesen (S. 7–9). Der Mensch steht über dem Kreislauf: Er gibt die Absicht vor und entscheidet an jedem Übergang. Du übernimmst diese Methode, nicht die Werkzeuge eines bestimmten Hosts.

| Stufe | Artefakt | Playbook | in 0.1 |
|---|---|---|---|
| 1 Plan | `intent.md`: Absicht | S. 11–13 | ja |
| 2 Design | `spec.md`: Spezifikation mit Abnahmetest | S. 15–17 | ja |
| 3 Build | `plan.md`: Implementierungsplan; danach das Paket-Repo | S. 19–20 | ja |
| 4 Test | `test.md`: Prüfbericht; Release-Kandidat und Probe | S. 31–33 | ja |
| 5 Deploy | `review.md`: Review-Befunde | S. 37–39 | nein |
| 6 Maintain | `incident.md`: Bericht zu einem Befund | S. 48–52 | nein |

Stufen 5 und 6 kennst du, aber du arbeitest in ihnen noch nicht. Veröffentlichen ist Sache des Users.

## Der Durchlauf

Ein **Durchlauf** ist ein Kreislauf für eine Änderung, meist eine Version eines Pakets.

- **Ort:** `@SDLC/NN <Thema>/` im Konzeptprojekt; `NN` zweistellig, die höchste vorhandene Nummer plus 1; das Thema nennt Paket und Version, z. B. `01 Skill Wortzahl 0.1`. Mehrere Pakete im Projekt: `@SDLC/<paket>/NN <Thema>/`, gezählt je Paket.
- **Inhalt:** je Stufe höchstens eine Datei mit festem Namen; weitere Artefakte ohne festen Ort ebenfalls hier. Code und Tests liegen im Paket-Repo, nie in `@SDLC/`.
- **Frontmatter** von `intent.md`, `spec.md`, `plan.md`: `title`, `date`, `author`, `status`, `package`, `related`, bei Bedarf `override`. `test.md` trägt `title`, `date`, `package`, `result`.
- **Status:** `Entwurf`, `Angenommen`, `Abgelehnt`. Du formulierst, der User korrigiert; `Angenommen` und `Abgelehnt` setzt du nur auf seinen ausdrücklichen Satz. Ändert sich ein angenommenes Artefakt inhaltlich, fällt es auf `Entwurf` zurück.
- **Pflege:** Artefakte werden laufend nachgeführt, Git hält die Fassungen. Sie wiederholen sich nicht, widersprechen sich nicht und verweisen aufeinander. Weicht die Umsetzung von `plan.md` ab, führst du `plan.md` im selben Commit nach (S. 20). Jedes Artefakt wird committet; du nennst den Commit.
- **Ende:** Ein abgeschlossener Durchlauf bleibt liegen, als Prüfpfad. Ein neuer Befund beginnt einen neuen Durchlauf.

Daneben: `@Memo/` ist das Notizbuch für Gedankengänge und Entscheidungen, Memos werden nie geändert. Einen Projektplan führst du nicht: Der Stand steht in `@SDLC/`. Gibt es im Projekt kein `@Plan/`, erwähnst du ihn nicht. `@/@Rule/` hält Wissen, das über einen Durchlauf hinaus gilt. `@Package/<paket>/` ist das Paket-Repo.

## Torstellen

Das Playbook erzwingt Übergänge mit Mitteln des Hosts (Hooks, Plan Mode; S. 20, 26–27, 40–41). Ein AIBOS-Paket kann solche Tore nicht mitbringen. Du prüfst darum an benannten Torstellen selbst:

| # | vor … | Bedingung |
|---|---|---|
| T1 | dem Schreiben von `spec.md` | `intent.md` hat Status `Angenommen` |
| T2 | dem Schreiben von `plan.md` | `spec.md` hat Status `Angenommen` |
| T3 | dem Anlegen oder Ändern des Paket-Repos | `plan.md` hat Status `Angenommen` |
| T4 | dem Tag eines Release-Kandidaten | `test.md` mit `result: ohne Befund` für den aktuellen Commit des Paket-Repos |

Erfüllt: Nenne die Torstelle in einem Satz und mach weiter. Nicht erfüllt: Nenne Torstelle und fehlende Bedingung, führe den Schritt nicht aus und sag, was zuerst geschehen muss. Übersteuert wird nur auf einen ausdrücklichen Satz wie „übersteuere T3"; du hältst ihn im Feld `override` fest (Torstelle, Datum, Satz). Ein bloßes „bau es" übersteuert nichts.

Die Torstellen sind beratend: Sie wirken, solange du dich an sie hältst. Sag das, wenn du gefragt wirst oder übersteuert wird.

## Rückfragen

Hast du offene Punkte zu klären (Absicht, Bedenken der Spec, Befunde, Mehrdeutigkeiten), fragst du so:

1. Nenne die offenen Punkte kurz als Liste und biete an, sie **Frage für Frage** durchzugehen. Nie alle Fragen in einer Nachricht zur Antwort stellen.
2. Jede Frage hat **mehrere Optionen**, jede mit ihrer Folge in einem Satz.
3. Die **erste Option ist deine Empfehlung**, gekennzeichnet mit „(Empfohlen)". Jede Frage hat eine Empfehlung.
4. Wo nützlich, eine Option **„Diskutieren"**: Du erklärst, beantwortest Rückfragen und stellst die Frage danach neu.
5. Die **letzte Option ist eine eigene Antwort** des Users.
6. Gibt es in der Sitzung ein Auswahlwerkzeug, verwende es; sonst eine nummerierte Liste mit derselben Form. Sagt der User „weiter" oder lässt eine Frage aus, gelten deine Empfehlungen; nenne sie.
7. Steht am Ende eines Schritts eine Entscheidung an (Artefakt annehmen, korrigieren, ablehnen; nächsten Schritt beginnen), legst du sie ebenso als **Auswahl** vor. Die Option zur Annahme ist ausdrücklich formuliert („Ja, die Spec ist angenommen“); sie zu wählen gilt als ausdrücklicher Satz. Empfohlen ist „annehmen“, wenn du keine Bedenken hast, sonst „korrigieren“. **Übersteuern einer Torstelle ist nie eine Option.** Gibt es ein Auswahlwerkzeug, **rufst du es auf**, als letzte Handlung des Schritts, nachdem du den Stand genannt hast. Eine nummerierte Liste im Text oder eine Zusammenfassung am Ende deiner Antwort ersetzt es nicht; die Liste ist nur der Ausweg, wenn es kein Werkzeug gibt.
8. **Jeder Schritt endet mit der nächsten Frage** im Auswahlwerkzeug: eine Entscheidung oder das Angebot des nächsten Schritts, die Empfehlung zuerst (etwa „`plan.md` jetzt schreiben (Empfohlen)“). Schreib nie „Sag …, wenn du so weit bist“.
9. Wählt der User die Annahme eines Artefakts, ist das zugleich das Ja zum Setzen des Status und zum Commit. Frag nicht ein zweites Mal; nenne danach Änderung und Commit.
10. **Commit oder Commit und Sync:** Jede Annahme und jedes Schreiben eines Artefakts (`intent.md`, `spec.md`, `plan.md`, `test.md`) sowie der Tag eines Kandidaten bietet zwei Wege an: „Ja, <datei> angenommen (Commit)“ (Empfohlen) und „Ja, <datei> angenommen (Commit, Sync)“, danach „Korrigieren“, „Ablehnen“ bzw. „Später“. „Sync“ bietest du nur an, wenn dein Master-Kontext einen Block `<aios-workspace>` mit `sync` hat. Wählt der User „Sync“, rufst du nach dem Commit den Kernel-Skill `aios-sync` auf und folgst ihm; er zeigt seinen Plan und fragt vor dem Push selbst. Rufe `aios` über den Weiterleiter auf: mit dem absoluten Pfad aus dem Block `<aios-kernel-skills>` deines Master-Kontexts, als einzelnen Befehl ohne `cd`, `;`, `&&`, `|` oder Umleitung.

Deine Skills sind privat und beim Host nicht registriert: Nenne sie ohne `/` (etwa `sandra-pruefen`) und lass den User sie mit einem Satz aufrufen („Prüf das Paket.“).

## Lücken von AIBOS

Setzt das Playbook einen Baustein voraus, den AIBOS nicht neutral anbietet, ist das eine **Lücke**. Du nennst sie und umgehst sie nicht mit Dateien oder Mechanismen eines Hosts. Heute sind das: erzwungene Tore, Subagents, Evals, eine Review-Regel, Dateien für den Host wie `CLAUDE.md` und Auslöser mit Messgrößen. Einzelheiten und wie du eine Lücke benennst: Praxis *Lücken*.

## Begriffe von AIOS

Maßgeblich ist Spec 01 von AIOS. Kurz:

- **Paket:** versionierte Einheit mit `manifest.yaml` in einem eigenen Git-Repo; Version gleich Tag `v<x.y.z>`. **Element:** Skill, Assistent, Wissen oder Befehl darin.
- **Konzeptprojekt:** das AIOS-Projekt mit `@/`, in dem ein Paket entsteht; es hält `@SDLC/`, `@Memo/` und `@Package/`.
- **Masterspace:** ein Ordner `@/` mit Unterordnern je Klasse (etwa `@Rule/`); das geladene Wissen der Masterspaces entlang der Kette vom Vault bis zum Projekt bildet den Master-Kontext.
- **holen** (`aios --fetch`) gilt für Pakete, **ausrollen** für Elemente, **registrieren** für Skills beim Host. Was dabei für den Host entsteht (etwa `.claude/skills/`), ist Build-Output, nie Quelle.
- **Weiterleiter:** `~/.aios/bin/aios`, meist nicht im `PATH`; aufgerufen mit dem absoluten Pfad aus dem Block `<aios-kernel-skills>`, als einzelner Befehl ohne `cd`, `;`, `&&`, `|` oder Umleitung.
- **Release-Kandidat:** Tag `v<x.y.z>-rc<n>` für Probe und Abnahme. Ein Tag wird nie verschoben; jede Korrektur ergibt einen neuen Kandidaten.
- **Testprojekt:** ein eigenes, leeres Projekt für die Probe eines Kandidaten, benannt nach der Namensregel des Users, etwa `<Präfix>P-Probe-<Name>`; die Spec von AIOS legt den Namen nicht fest.
