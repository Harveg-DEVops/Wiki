# Diser Teil vom Wiki wird allen Werkzeugen gewidmet welcher einem die Arbeit mit der SW-Platform vom Roboter (openHAB) erleichtert

## Node-Red
Die Idee ist einfach - Node-RED ist ein großartiges visuelles Tool, das von IBM entwickelt wurde und die Erstellung von Flussdiagramm-Programmen ermöglicht. Es basiert auf MQTT und damit auf ereignisorientiertem Message-Passing-Verhalten über verschiedene Knoten hinweg. Z.B. löst eine Nachricht die Ausführung des Knotens aus, wenn sie entlang der Linien von einem Knoten zum anderen weitergereicht wird. Jeder Knoten kann eine Aktion mit der Nachricht ausführen - sie blockieren, ändern, verzögern, auf einen anderen Pfad routen oder etwas Javascript ausführen.
Node-RED wurde entwickelt, um Konnektivität für die IoT-Welt zu bieten, daher ist es in diesem Sinne wie ein Konkurrent von OpenHAB, aber ich schlage nicht vor, es als Konnektivitätswerkzeug zu verwenden - ich denke, OpenHAB hat hier einen großen Wert, da es all diese Konnektivitätsbindungen hat. Stattdessen schlage ich vor, Node-RED als rein hardware-unabhängige Regel- und Skript-Engine zu verwenden.

### Node-RED Hauptmerkmale
1. Leichtigkeit beim Debuggen. Platzieren Sie einfach Injektions- und Debug-Knoten an beliebiger Stelle im Nachrichtenfluss, verbinden Sie sie, und Sie können ganz einfach alle Funktionen ohne E/As simulieren, indem Sie auf die Quadrate neben den blauen Knoten klicken - dadurch werden Testnachrichten erzeugt, und der gesamte Algorithmus kann leicht verifiziert werden, ohne dass Sie Gefahr laufen, beim testen etwas zu zerstören oder Ihre Mitbewohner zu stören, indem Sie den ganzen Tag die Beleuchtung Ein- & Ausschalten.

2. HW-Unabhängigkeit - da die E/A-Schnittstelle für Node-RED MQTT ist, kann sie nach den Regeln von Node-RED mit jeder Art von Sensoren arbeiten - Z-Welle, KNX, Arduino und sogar mit dem Projekt Farmbot welches MQTT unterstützt.

3. Gemeinsame Nutzung - Node-Red bietet eine einfache JSON-basierte Kopieren-Einfügen-Funktionalität, so dass jeder seine Algorithmen gemeinsam nutzen kann. 

4. Stabilität - für 2 Monate Nutzung und nach 3 eingesetzten Instanzen auch Openhab 3.0 hatte ich keine Probleme mit der Stabilität von Node-RED oder mit Ausnahmeproblemen - es funktioniert einfach. Für die sichere Verwendung von Node-red mit Internetanbindung empfielt sich folgendes Tutorial: http://stevesnoderedguide.com/securing-node-red-ssl

Die Installation ist dank der Konsole in Openhabian kinderleicht und es werden dierekt die nötigen npm module installiert weiche für die Anbindung an Openhab nötig sind.

### Node-Red Konfiguration
Die Node-Red Konfiguration besteht aus einem Textbasierten JSON-File, welches alle Flows beinhaltet und auch die entsprechende Konfiguration der installierten Module beinhaltet (MQTT,Openhab-Server etc.). Davon ist jeweils eine Backup-Variante bei jeder Änderung in Node-Red zu machen und bei den Konfigurations-Files der entsprechenden Instanz abzulegen.

### Node-Red Docker management (Optional)
Node-Red kann auch als Docker-Container betrieben werden was jedoch nicht empfohlen wird, da sämtliche Vorteile der Anbindung zur HW entfallen und entsprechend für alle Anbindungen entsprechende EInstellungen im Container gemacht werden müssen.
Beispiel wie das Container managment mittels node-red realisiert werden kann:https://skylar.tech/restarting-stuck-game-servers-with-node-red/

Hinzufügen des .socket volumen für node-red mit folgendem compose-beispiel:
```json
  nodered:
    container_name: nodered
    build: ./services/nodered/.
    restart: unless-stopped
    privileged: true
    environment:
      - TZ=Etc/UTC
    ports:
      - "1880:1880"
    volumes:
      - "./volumes/nodered/data:/data"
      - "/var/run/docker.sock:/var/run/docker.sock"
    devices:
      - "/dev/USB0:/dev/USB0"
      - "/dev/ttyAMA0:/dev/ttyAMA0"
```

# Editoren - Verschiedene Wege zur Vereinfachung Ihrer textuellen Konfiguration

Derzeit gibt es mehrere Lösungen, die Ihnen helfen können, Ihre openHAB-Instanz textuell zu konfigurieren.
Diese Dokumentationsseite kann Ihnen helfen, die richtige Lösung für Sie auszuwählen und einzurichten.

## Backup-Dienst
Für die Absicherung der openHAB-Konfiguration muss nach der Inbetriebnahme oder jeder Änderung zwingend ein Backup über die Konsole (openhabian-config) gemacht werden.
Die gesamte Konfiguration mit allen Einstellungen sowie das HabPanel GUI werden so gesichert und können wieder in einer neuen openHAB Instanz importiert werden.
Aktuell wird die Absicherung auf einem externen Medium manuell mit folgenden Befehlen gemacht: Das "source-directory" ist aus der Konsole zu entnehmen.
> ``lsblk``
> ``sudo  mkdir /media/usb``
> ``sudo mount /dev/sdb1 /media/usb``
> ``sudo cp -r "source directory" /media/usb``
> ``sudo umount /media/usb``
Zudem ist vom Zeitpunkt der Inbetriebnahme ein Backup auf einer SSD erhalten welches als Ersatz bei einem Ausfall des Sperichermediums zum Einsatz kommt.

## JSON2CONFIG (Zu installieren)
DIeses kleines Proramm lässt die Items aus der JSON Datenbank bequem in ein Text-file exportieren und somit auch wieder mittels Übertragung in die openhab-conf Umgebung zurück ins System übertragen.
Zum Start einfach auf folgendes GITHUB Projekt gehen und aktuelle Version herunterladen: <https://github.com/voruti/json2config/tree/1.4>

1. Verschiebe das Programm ins Automation Verzeichnis: etc/openhab2/automation
2. verbinde mit SSH zu controller 
> ``cd /var/lib/openhab2/jsondb/``
> ``mv /etc/openhab2/automation/json2config-1.4.jar json2config-1.4.jar``
> ``java -jar json2config-1.4.jar -o /etc/openhab2/automation/json.items``

3.  Öffne das neu erstellte json.items im automation verzeichnis und übertrage die JSON informationen in deinen Setup

## Netzwerk-Vorbereitungen

Alle Editoren, die zur Konfiguration von openHAB verwendet werden, müssen in der Lage sein, auf die Konfigurationsdateien auf dem entfernten openHAB-Host zuzugreifen.

Dies kann durch die Verwendung einer [Netzwerkfreigabe](https://en.wikipedia.org/wiki/Shared_resource) erreicht werden, die auf dem entfernten Host eingerichtet und auf Ihrem lokalen Computer gemountet wird.
Die Schritte, die erforderlich sind, um eine [Netzwerkfreigabe] (https://en.wikipedia.org/wiki/Shared_resource) auf Ihrem lokalen Host-Computer einzurichten, sind spezifisch für das Host-Betriebssystem.
Wie Sie Samba auf einem Linux-System einrichten und verwenden, wird im [Linux-Artikel]({{base}}/installation/linux.html#network-sharing) beschrieben.
Wenn Sie [openHABian]({{base}}/installation/openhabian.html) verwenden, sind die Netzwerkfreigaben bereits für Sie konfiguriert, Sie müssen sie nur noch lokal einhängen.

Achtung Windows-Benutzer:_ Der direkte Zugriff auf Netzwerkfreigaben (UNC-Pfade) wird oft nicht unterstützt. Bitte stellen Sie sicher, dass Sie die Netzwerkfreigabe auf einen Laufwerksbuchstaben mounten.

## openHAB VS Code Erweiterung

openHAB VS Code ist eine Erweiterung für den [Visual Studio Code](https://code.visualstudio.com) Editor.
Sie finden sie auf dem [Microsoft Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=openhab.openhab).

  ![openHAB VS Code Extension demo](images/vscode_demo.gif)

### Installation

1. Installieren Sie [Visual Studio Code](https://code.visualstudio.com/Download) auf Ihrem Desktop-Computer (nicht auf dem openHAB-Host)
1. Öffnen Sie die Seitenleiste der Erweiterung.
  ![openHAB VS Code Extension alternative Installation](images/vscode_extensiontab_icon.png)
1. Suchen Sie nach openHAB und installieren Sie die Erweiterung.

[Besuchen Sie die Erweiterungen GitHub Seite für weitere Informationen](https://github.com/openhab/openhab-vscode/blob/main/README.md „GitHub Repo for the VS Code Extension“)

### Validierung von Rules

Diese Erweiterung hat die Möglichkeit, Rules zu überprüfen und durch einen sogenannten `Language Server` zu validieren.
(Wenn Sie mehr darüber wissen wollen, schauen Sie [hier](https://langserver.org/).)
Die Validierung setzt eine laufende openHAB-Installation in Ihrer Umgebung voraus und kann mit einigen einfachen Schritten aktiviert werden.
Alle wichtigen Informationen finden Sie in den Erweiterungen [readme file](https://github.com/openhab/openhab-vscode#validating-the-rules).

## Andere Editor-Integrationen

Die hier zusammengefassten Projekte bieten Syntaxhervorhebung für verschiedene Texteditoren, haben aber keine _on top_ Funktionalität.

### Nano

Nano ist ein gängiger Editor in Linux-Systemen.
Sie finden die Syntax-Datei und Installationsanweisungen auf [openhabnano](https://github.com/airix1/openhabnano).

### Notepad++

Notepad++ ist ein kostenloser Quellcode-Editor für Windows.
Version 6.2 oder höher ist erforderlich.
Die Syntaxdateien finden Sie unter [openhab-samples](https://github.com/

Beispiel von :https://discourse.nodered.org/t/node-red-and-docker-and-mount/44227
