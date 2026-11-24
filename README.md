# skill-improve

Paket von AIbex AIBOS mit dem Skill `improve`, Version 0.2.0. Er gibt jedem Baustein (Wissen, Skill, Assistent) einen Kanal für Verbesserungswünsche, ohne dass der Baustein dafür eigene Skills braucht.

- **Erfassen:** Ein Anwender hält einen Wunsch fest. Er landet als Ticket mit `type: improvement` im verborgenen Postfach `<vault-root>/@/@!Ticket/` und ändert nichts an der laufenden Arbeit (Modus RELEASED).
- **IMPROVED:** Wer will, schaltet die Wünsche zu einem Baustein für eine Session ein. Sie wirken dann experimentell mit, erkennbar am Zeichen 🏗️ im Prefix des Assistenten. Eine neue Session ist wieder RELEASED.
- **Sammeln:** Der Maintainer bringt die Wünsche zu seinem Paket ins Konzeptprojekt, nach dessen Sammelregel, und löscht sie danach im Eingang.

## Voraussetzung

AIBOS 0.7.1, Vault gebunden (A/W). `improve` findet Tickets mit `aibos --ticket`; nach einer Zusammenfassung erinnert die Zeile `AIBOS-TICKET` daran, Wünsche im Modus IMPROVED neu zu lesen.

**Übergang:** Bis AIBOS 0.8 liest `improve` auch den alten Eingang `<vault-root>/@/@!Improve/` von `skill-improve` 0.1.0, schreibt aber nur nach `@/@!Ticket/`. Die Dateien im alten Eingang verschiebt niemand automatisch; `improve` weist auf den Umzug hin.

**Freigaben des Hosts:** AIBOS gibt `aibos --ticket` und das Lesen einer Datei im Postfach ohne Rückfrage frei. Für das Schreiben ins Postfach und für das Auflisten des Postfachs (die vollen Dateinamen) fragt Claude Code einmal nach, weil das Postfach an der Vault-Root liegt.

```bash
aibos --fetch skill-improve
```

## Aufruf

| Weg | Beispiel |
|---|---|
| Erfassen | „Ich hab einen Verbesserungsvorschlag für Anja: …“ |
| IMPROVED | „IMPROVED-Modus für Anja“ |
| Sammeln | „Sammle die Wünsche“, nur im Konzeptprojekt eines Pakets |

**Sicherer Aufruf:** `/improve <satz>` als ganzer Prompt. Hat ein Assistent eigene Skills für dieselben Sätze (etwa Anja mit `anja-improve` bis zu ihrem Umbau), wählt nur so sicher `improve`.

## Sammeln

Gesammelt wird nur in einem Konzeptprojekt: ein Projekt mit `@/`, einem zugeordneten Assistenten und dem Paket-Repo unter `@Package/<paket>/`. Wie ein Wunsch dort abgelegt wird, sagt die Sammelregel:

1. `<konzeptprojekt>/@/@Rule/rule-collect.md`, falls vorhanden;
2. sonst `rule-collect.md` im Wissen des Assistenten.

Gibt es keine, weist der Skill das Sammeln zurück. Die Vorlage unten passt für Konzeptprojekte, die mit Memos arbeiten.

### Vorlage: `@/@Rule/rule-collect.md`

```markdown
# Sammelregel

Diese Regel legt fest, wie `skill-improve` einen erfassten Wunsch in dieses Konzeptprojekt überträgt.

- **S1 — Ein Memo je Wunsch** im Memo-Ordner des Pakets: `@Memo/<paket>/`, wenn das Projekt mehrere Pakete hat, sonst `@Memo/`. Nummer: höchste im Ordner plus 1. Dateiname: `NN Wunsch — <Kurztitel>.md`.
- **S2 — Form nach dem Skill `memo`:** kein Frontmatter, Titel ohne Nummer, Kopf in `**Feld:**`-Zeilen:
  `**Datum:**` (Tag des Sammelns), `**Status:** Wunsch`, `**Autor:**` (author, leer: „unbekannt“),
  `**Betrifft:**` (`<package>` `<version>`), `**Quelle:**` (Eingang/Dateiname, erfasst am `<date>`, aus Projekt `<project>`),
  `**Verwandt:**` (Memos aus den Hinweisen, sonst —). Abschnitte: Wunsch, Beispiel, Abgrenzung, Einordnung.
- **S3 — Wörtlich übernehmen:** Wunsch, Beispiel und Abgrenzung unverändert.
- **S4 — Einordnung:** die Hinweise des Skills („deckt sich mit Memo NN“, „erfasst zu x.y.z, inzwischen a.b.c“), dazu, falls erkennbar, der betroffene Teil des Pakets. Entschieden wird beim Aufgreifen.
- **S5 — Ein Commit je Sammellauf:** „Sammeln: <n> Wünsche zu <paket> (Memos NN–MM)“, `<n>` gezählt.
- **S6 — Plan:** Gibt es `@Plan/Plan.md`, bekommt es im selben Commit unter *Offen* den Punkt „<n> gesammelte Wünsche aufgreifen (Memos NN–MM)“.
- **S7 — Melden:** Zahl, Memos, Hinweise, Commit.
```

## Lizenz

Apache-2.0, siehe `LICENSE` und `NOTICE`.
