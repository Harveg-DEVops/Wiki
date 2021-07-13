# Diser Teil vom Wiki wird allen Werkzeugen gewidmet welcher einem die Arbeit mit der Plattform (openHAB) erleichtert

## Backup-Dienst
Für die Absicherung der openHAB-Konfiguration muss nach der Inbetriebnahme oder jeder Änderung zwingend ein Backup über die Konsole (openhabian-config) gemacht werden.
Die gesamte Konfiguration mit allen Einstellungen sowie das HabPanel GUI werden so gesichert und können wieder in einer neuen openHAB Instanz importiert werden.
Aktuell wird die Absicherung auf einem externen Medium manuell mit folgenden Befehlen gemacht: Das "source-directory" ist aus der Konsole zu entnehmen.
> ``lsblk``
> ``sudo  mkdir /media/usb``
> ``sudo mount /dev/sdb1 /media/usb``
> ``sudo cp -r "source directory" /media/usb``
> ``sudo umount /media/usb``

## JSON2CONFIG (Zu installieren)
DIeses kleines Proramm lässt die Items aus der JSON Datenbank bequem in ein Text-file exportieren und somit auch wieder mittels Übertragung in die openhab-conf Umgebung zurück ins System übertragen.
Zum Start einfach auf folgendes GITHUB Projekt gehen und aktuelle Version herunterladen: <https://github.com/voruti/json2config/tree/1.4>

1. Verschiebe das Programm ins Automation Verzeichnis: etc/openhab2/automation
2. verbinde mit SSH zu controller 
> ``cd /var/lib/openhab2/jsondb/``
> ``mv /etc/openhab2/automation/json2config-1.4.jar json2config-1.4.jar``
> ``java -jar json2config-1.4.jar -o /etc/openhab2/automation/json.items``

3.  Öffne das neu erstellte json.items im automation verzeichnis und übertrage die JSON informationen in deinen Setup

## Node-Red
Die Idee ist einfach - Node-RED ist ein großartiges visuelles Tool, das von IBM entwickelt wurde und die Erstellung von Flussdiagramm-Programmen ermöglicht. Es basiert auf MQTT und damit auf ereignisorientiertem Message-Passing-Verhalten über verschiedene Knoten hinweg. Z.B. löst eine Nachricht die Ausführung des Knotens aus, wenn sie entlang der Linien von einem Knoten zum anderen weitergereicht wird. Jeder Knoten kann eine Aktion mit der Nachricht ausführen - sie blockieren, ändern, verzögern, auf einen anderen Pfad routen oder etwas Javascript ausführen.
Node-RED wurde entwickelt, um Konnektivität für die IoT-Welt zu bieten, daher ist es in diesem Sinne wie ein Konkurrent von OpenHAB, aber ich schlage nicht vor, es als Konnektivitätswerkzeug zu verwenden - ich denke, OpenHAB hat hier einen großen Wert, da es all diese Konnektivitätsbindungen hat. Stattdessen schlage ich vor, Node-RED als rein hardware-unabhängige Regel- und Skript-Engine zu verwenden.

### Node-RED Hauptmerkmale
1. Leichtigkeit beim Debuggen. Platzieren Sie einfach Injektions- und Debug-Knoten an beliebiger Stelle im Nachrichtenfluss, verbinden Sie sie, und Sie können ganz einfach alle Funktionen ohne E/As simulieren, indem Sie auf die Quadrate neben den blauen Knoten klicken - dadurch werden Testnachrichten erzeugt, und der gesamte Algorithmus kann leicht verifiziert werden, ohne dass Sie Gefahr laufen, beim testen etwas zu zerstören oder Ihre Mitbewohner zu stören, indem Sie den ganzen Tag die Beleuchtung Ein- & Ausschalten.

2. HW-Unabhängigkeit - da die E/A-Schnittstelle für Node-RED MQTT ist, kann sie nach den Regeln von Node-RED mit jeder Art von Sensoren arbeiten - Z-Welle, KNX, Arduino und sogar mit dem Projekt Farmbot welches MQTT unterstützt.

3. Gemeinsame Nutzung - Node-Red bietet eine einfache JSON-basierte Kopieren-Einfügen-Funktionalität, so dass jeder seine Algorithmen gemeinsam nutzen kann. 

4. Stabilität - für 2 Monate Nutzung und nach 3 eingesetzten Instanzen auch Openhab 3.0 hatte ich keine Probleme mit der Stabilität von Node-RED oder mit Ausnahmeproblemen - es funktioniert einfach.

Die Installation ist dank der Konsole in Openhabian kinderleicht und es werden dierekt die nötigen npm module installiert weiche für die Anbindung an Openhab nötig sind.

### Node-Red X-State Machine (Zusatzmodul)
Dieses Modul für die node.js VM ist ideal zum Abbilden und Entwickeln von Infinite State Machines.
Die Bibliothek wird als npm Packet installiert mit folgendem Befehl: ``~/.node-red`` ``npm install node-red-contrib-xstate-machine``
Grundalge der X-State Machine ist die XState bibliothek von davidkpiano siehe: <https://www.npmjs.com/package/xstate>. Das Konzept der State achine wird hier erklärt <https://www.youtube.com/watch?v=VU1NKX6Qkxc>

### Node-Red ical-Adapter (Zusatzmodul)
Das Modul ical-Adapter fügt die nötige Verbindung zu dem CalDAV Dienst her, wlcher für die Anbauplanung eingesetzt wird. Die eingehenden Befehle werden entsprechend dem Topic verarbeitet sodass verschiedene Verhalten je nach Steuerungsgruppe abgebildet werden können. Primär werden die Abläufe wie Lichtsteuerung (light) & Sequenzensteuerung (job) verarbeitet welche zuvor als Kalender-Event auf dem CalDAV Dienst eingegeben wurden. Mehr Infos siehe: <https://flows.nodered.org/node/node-red-contrib-ical-events>
Die Bibliothek wird als npm Packet installiert mit folgendem Befehl: ``~/.node-red`` ``npm install node-red-contrib-ical-events``

### Node-Red mit Tensor-flow integration
https://developer.ibm.com/technologies/artificial-intelligence/tutorials/building-a-machine-learning-node-for-node-red-using-tensorflowjs/
Basierend auf der Tensorflow.js Bibliothek lassen sich direkt die Module von TF in einen flow integrieren und über das json Format die Ergebnisse in Node-Red auswerten. 
