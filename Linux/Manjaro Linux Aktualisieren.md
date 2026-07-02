1. Aktualisieren Sie zuerst die offiziellen Schlüsselpakete und laden Sie die Vertrauensdaten neu:

```bash
sudo pacman -Sy manjaro-keyring archlinux-keyring
```

_Falls hier bereits eine Fehlermeldung erscheint, fahren Sie direkt mit Schritt 2 fort._
Initialisieren und laden Sie den Schlüsselbund komplett neu:

```bash
sudo pacman-key --init sudo pacman-key --populate manjaro archlinux
```

2. Beschädigte/unvollständige Paket-Downloads löschen
Da die zuvor heruntergeladenen Pakete in Ihrem Cache (`/var/cache/pacman/pkg/`) als "beschädigt" markiert wurden, sollten Sie diese löschen, damit sie sauber neu heruntergeladen werden. Bestätigen Sie die Abfragen mit **Ja**:

```bash
sudo pacman -Sc
```

3. System Updaten:

```bash
sudo pacman -Syu
```

Visual Studio Code Updaten

```bash
sudo pacman -Syu visual-studio-code-bin
```

Nach Aktualisierung des Kernels und der Nvidia Treiber ist es wahrscheinlich, dass KDE nicht mehr hochfährt. Ich habe folgende Grafikkarte:

```
ǸVidia GT 1030
```

Der Sourcecode des passenden Legacy Kerneltreibers liegt unter:

```
/home/frank/nvidia-580xx-utils
```


Der Bootprozess kann durch 
```
Drücken Sie die Tastenkombination **`Strg` + `Alt` + `F3`** (falls das nicht klappt, versuchen Sie `F4`, `F5` oder `F6`).
```

Anschließend kannst Du Dich über die Konsole einloggen.

Ggf. hilft das Treibertool:

```
nvidia-driver-assistant
```

Ansonsten müssen die aktuellen Nvidia Kerneltreiber incl. transitiver Abhängigkeiten zunächst entfernt werden:

```
# 1. Inkompatible Nvidia-Reste löschen Liste der Module kann abweichen
#sudo pacman -R lib32-nvidia-utils linux612-nvidia

# So hat es funktioniert
sudo pacman -Rns linux-nvidia-meta linux70-nvidia nvidia-utils lib32-nvidia-utils linux612-nvidia nvidia-settings
```

==Unter dem **Linux-Kernel 7.0** gibt es für Manjaro und ältere Nvidia-Karten wie die **GeForce GT 1030** eine Besonderheit: Der offizielle, aktuelle Nvidia-Haupttreiber unterstützt Ihre Karte nicht mehr==. Gleichzeitig bieten die älteren Treiberpakete (wie `linux612-nvidia`) oft keine direkten Module für den brandneuen Kernel 7.
##### Schritt 1: Kernel-Header installieren
Damit das System den Treiber für Kernel 7 kompilieren kann, werden die passenden Header-Dateien benötigt:
```
sudo pacman -Syu linux70-headers base-devel
```

##### Schritt 2: Inkompatiblen Treiber entfernen
Löschen Sie die fehlerhaften Grafiktreiber-Konfigurationen restlos vom System:

```
sudo mhwd -r pci video-nvidia
```

##### Schritt 3.1: Den Nvidia-Legacy-DKMS-Treiber installieren

Für die GT 1030 benötigen Sie den Legacy-Treiber der **575xx- oder 550xx-Serie**. Da es für Kernel 7 kein fertiges Paket gibt, installieren wir die **DKMS-Variante**, die sich beim Installieren selbst anpasst. 

Nutzen Sie dafür das Manjaro-Hardware-Tool oder `pacman`: 

```
# Versuchen Sie zuerst die automatische DKMS-Einrichtung:
sudo pacman -S nvidia-575xx-dkms lib32-nvidia-575xx-utils
```

Verwende Code mit Vorsicht.

*(Sollte das Paket `575xx` nicht gefunden werden, ersetzen Sie die Zahlen im Befehl durch `nvidia-550xx-dkms` und `lib32-nvidia-550xx-utils`).* 

##### Schritt 3.2 Den Kerneltreiber selbst komplieren und installieren:

Funktioniert hat die manuelle Installation. Der passende Treiber liegt schon im Home Verzeichnis:

```
cd /home/frank/nvidia-580xx-utils

# 4. Den Kompiliervorgang starten 
sudo makepkg -si
sudo reboot
```

Empfehlung von Gemini:

```
# 1. Wechseln Sie in den temporären Ordner 
cd /tmp 

# 2. Das ECHTE AUR-Repository für den Grafiktreiber klonen 
git clone https://aur.archlinux.org/nvidia-550xx-dkms.git 

# 3. In das frisch heruntergeladene Verzeichnis wechseln 
cd nvidia-550xx-dkms

# 4. Den Kompiliervorgang starten 
makepkg -s
```

##### Schritt 4: Treiber aktivieren und neu starten

Teilen Sie dem System mit, dass das neu gebaute Nvidia-Modul beim Start geladen werden soll: 

```
sudo modprobe nvidia
sudo reboot
```

Verwende Code mit Vorsicht.

Alternativ kann nach dem Entfernen der störenden Kernelmodule der Legacytreiber neu compiliert und installiert werden:

```
sudo pacman -Syu linux70-headers base-devel
cd /home/frank/nvidia-580xx-utils

```

#### Open Code CLI Installieren
Da OpenCode im Kern ein Terminal-Agent ist, muss die CLI-Umgebung auf deinem System vorhanden sein. Öffne dein [VS Code Terminal](https://code.visualstudio.com/docs/terminal/basics) und installiere das Tool per Installations-Skript: [[1](https://opencode.ai/docs/ide/), [3](https://marketplace.visualstudio.com/items?itemName=wenzewoo.opencode-agent)]

```bash
curl -fsSL https://opencode.ai/install | bash
```

### Open Code UI for VS Code installieren
Such im VS Code Extension Marketplace (`Ctrl` + `Shift` + `X`) nach **OpenCode**.

**Empfehlung:** Installiere das offizielle Plugin oder die beliebte Erweiterung **OpenCode UI for VS Code**. Diese bringt das Terminal-Interface in eine schöne Chat-Seitenleiste mit Datei-Anhängen und Planungs-Übersichten.

### Open Code authentifizieren
Um Zugriff auf die optimierten Programmier-Modelle (wie das kostenlose _BigPicle_ oder die _Zen_-Modelle) zu erhalten, musst du dich einmalig authentifizieren:
```
opencode auth login
```
### Eclipse starten
```
/home/frank/Projekte/Java/eclipse/eclipse
```
### Obsidian starten
```
/home/frank/Downloads\Obsidian-1.12.7.AppImage
```
