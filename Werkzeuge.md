# Diser Teil vom Wiki wird allen Werkzeugen gewidmet welcher einem die Arbeit mit der Plattform (openhabian) erleichtert

## Home Bulider (Vorinstalliert)


## JSON2CONFIG (Zu installieren)
DIeses kleines Proramm lässt die Items aus der JSON Datenbank bequem in ein Text-file exportieren und somit auch wieder mittels Übertragung in die openhab-conf Umgebung zurück ins System übertragen.
Zum Start einfach auf folgendes GITHUB Projekt gehen und aktuelle Version herunterladen: <https://github.com/voruti/json2config/tree/1.4>

1. Verschiebe das Programm ins Automation Verzeichnis: etc/openhab2/automation
2. verbinde mit SSH zu controller 
' cd /var/lib/openhab2/jsondb/
mv /etc/openhab2/automation/json2config-1.4.jar json2config-1.4.jar
java -jar json2config-1.4.jar -o /etc/openhab2/automation/json.items
'
3.  Öffne das neu erstellte json.items im automation verzeichnis und übertrage die JSON informationen in deinen Setup
