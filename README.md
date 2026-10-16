# rw-pi-backpack

Basis für ein Pi Package mit Extensions, Skills, Prompt-Vorlagen und Themes.

## Struktur

```text
rw-pi-backpack/
├── package.json
├── agents/
├── extensions/
├── skills/
├── prompts/
└── themes/
```

- `agents/`: Markdown-Dateien für die Extension `pi-subagents`.
- `extensions/`: TypeScript- oder JavaScript-Dateien. Mehrteilige Extensions erhalten ein Unterverzeichnis mit `index.ts` oder `index.js`.
- `skills/`: Ein Unterverzeichnis pro Skill mit einer `SKILL.md`.
- `prompts/`: Markdown-Dateien für wiederverwendbare Prompt-Befehle.
- `themes/`: JSON-Dateien für Pi-Themes.

Das Paket enthält das Theme `cyberpunk`. Die Verzeichnisse `extensions/`, `skills/` und `prompts/` enthalten nur `.gitkeep`-Platzhalter. Die Ressourcenpfade sind in `package.json` unter `pi` festgelegt.

## Lokal verwenden

Vom Paketverzeichnis aus für einen einzelnen Start laden:

```bash
pi -e .
```

Dauerhaft in der persönlichen Pi-Konfiguration registrieren:

```bash
pi install /absoluter/pfad/zu/rw-pi-backpack
```

Nach Änderungen in einer laufenden Pi-Sitzung `/reload` ausführen.

## Subagents

Die Agent-Definitionen in `agents/` werden über `pi.subagents.agents` in `package.json` bereitgestellt und mit dem Paket ausgeliefert. Sie benötigen die separat installierte Extension:

```bash
pi install npm:pi-subagents
```

Enthalten sind `advisor`, `docs`, `grep`, `search` und `linear-issue-reader`. Eigene Benutzer- oder Projektdefinitionen mit demselben Agent-Namen haben Vorrang vor Paketdefinitionen.

Die in den Agent-Dateien festgelegten Modelle müssen verfügbar sein. `linear-issue-reader` benötigt den MCP-Server `linear`.

## Cyberpunk-Theme

`themes/cyberpunk.json` enthält ein dunkles Theme mit Neon-Pink, Cyan und Gelb. Es gestaltet die Oberfläche, Markdown, Syntax und HTML-Exporte. Die Hintergrundfarbe des Terminals bleibt unverändert.

Vom Paketverzeichnis aus mit dem Theme starten:

```bash
pi -e . --use-theme cyberpunk
```

Nach der Installation des Pakets das Theme über `/settings` auswählen oder Pi mit `pi --use-theme cyberpunk` starten. Nach Änderungen am Theme `/reload` ausführen. Ein dunkler Terminalhintergrund wird empfohlen.

## Erweitern

Neue Ressourcen in das passende Verzeichnis legen. Für die Basisstruktur sind keine Abhängigkeiten und kein Build-Schritt erforderlich.

Wenn eine Extension von Pi bereitgestellte Pakete importiert, diese als `peerDependencies` mit dem Versionsbereich `"*"` deklarieren. Nicht in `dependencies` aufnehmen oder bündeln. Andere Laufzeitabhängigkeiten unter `dependencies` eintragen.

## Paketinhalt prüfen

```bash
npm pack --dry-run
```

## Referenz

[Pi-Packages-Dokumentation](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/packages.md)
