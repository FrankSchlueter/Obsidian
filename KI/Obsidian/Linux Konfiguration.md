
Wenn Sie das AppImage nutzen, fehlt oft die passende `.desktop`-Datei im System.
1. Nutzen Sie ein Tool wie **AppImageLauncher**, das die Integration automatisch übernimmt.
2. Alternativ erstellen Sie manuell eine Datei unter `~/.local/share/applications/obsidian.desktop` und fügen Sie folgende Zeile hinzu: `Exec=/pfad/zu/Obsidian.AppImage %u` (wichtig ist das `%u` am Ende).
3. Registrieren Sie das Protokoll im Terminal:
```
xdg-mime default obsidian.desktop x-scheme-handler/obsidian
```
Manjaro hat den Befehl qtpaths nicht gefunden.

Lösung:
```
sudo pacman -S qt6-base
sudo ln -s /usr/lib/qt6/bin/qtpaths /usr/local/bin/qtpaths
```

Da Sie Obsidian unter **Manjaro Linux** als **AppImage** nutzen, liegt der Fehler daran, dass das System die von Ihnen registrierte `obsidian.desktop`-Datei nicht finden kann oder das AppImage nicht korrekt integriert ist.

Öffnen Sie ein Terminal und erstellen Sie die Datei im Anwendungsordner Ihres Benutzers mit diesem Befehl:

```
nano ~/.local/share/applications/obsidian.desktop
```
Kopieren Sie den folgenden Block und fügen Sie ihn in die Datei ein. **Wichtig:** Ersetzen Sie `/pfad/zu/ihrem/Obsidian.AppImage` durch den echten, absoluten Pfad, wo Ihre AppImage-Datei auf der Festplatte liegt (z. B. `/home/ihrname/Downloads/Obsidian-1.5.3.AppImage`).

```
[Desktop Entry]
Name=Obsidian
Exec=/pfad/zu/ihrem/Obsidian.AppImage %u
Terminal=false
Type=Application
Icon=obsidian
Categories=Office;TextEditor;
MimeType=x-scheme-handler/obsidian;
```