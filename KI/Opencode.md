##### Konfiguration Model Provider

Der Befehl **`opencode auth login`** wird in der [OpenCode CLI Dokumentation](https://opencode.ai/docs/cli/) genutzt, um API-Schlüssel und Anmeldedaten für verschiedene KI-Anbieter (wie OpenRouter, OpenAI oder Google) direkt über das Terminal zu hinterlegen. Die Zugangsdaten werden anschließend sicher in der Datei `~/.local/share/opencode/auth.json` gespeichert.

```
# Mit Auswahlliste
opencode auth login

# Direkt Openrouter nutzen und nur Apikey abfragen
opencode auth login -p openrouter
```
