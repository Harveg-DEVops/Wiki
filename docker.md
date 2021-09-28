## Start mit docker

Unsere SW-Architektur basiert auf Docker und nützt die docker-compose Erweiterung.
Einen umfassenden Einblick in die Welt von Docker gibt folgender Beitrag von Chrigu: https://github.com/christofluethi/docklands/blob/master/docker-techtalk/docker-slides.pdf  

Die Anweisungen für die Konfiguration der SW wird mittels docker-compose.yaml files gemacht und den entsprechenden config files. Auf dem Zielsystem (Controller) läuft eine Anwendung welche die Container lokal verwalten kann und mittels USB-Stick konfiguriert wird, respektive Upgrades aufgespielt werden. Die Container-Mgmt.-Anwendung läuft in node-red und ist selbst ein Container welcher somit durch den Upgrade Prozess aktualisiert werden muss.

## Basics

### Docker-Container basieren auf Images
* Images werden über Dockerfile definiert
* oder das Speichern von Containern

### Docker-Images werden in Registries gespeichert
* DockerHub
* GitHub Packages container registry: ghcr.io/OWNER/IMAGE-NAME  

### Docker-Host ist die Maschine, auf der der Docker-Daemon läuft  
Linux oder Mac oder Windows, wo wir Docker installieren, ist Docker-Host  

### Docker-Client
docker cli, der mit dem docker daemon spricht  

### Docker-Daemon
ist verantwortlich für das Erstellen von Containern, das Abrufen von Images aus Registries, wenn diese nicht lokal vorhanden sind  
Docker bietet keine eigentliche Containerisierung an, sondern nutzt die vom Host bereitgestellten Daten

### Verwendung unter Linux

docker verwendet die folgenden 4 Hauptkonzepte, um die Containerisierung zu erreichen:  

1. **cgroups Kontrollgruppen**
sind eine Möglichkeit, einer bestimmten Gruppe von Prozessen eine Teilmenge von Ressourcen zuzuweisen (CPU, RAM, Festplatten-IO, Netzwerk-IO, etc.).
cgroups sind ein wichtiger Baustein in der Container-Isolierung, da sie die Isolierung von Hardware-Ressourcen ermöglichen
2. **Namespaces**
isolieren und virtualisieren Systemressourcen (PIDs, UTS, userIDs, Netzwerk , ipc (Interprozesskommunikation und mnt)
* pid-Namensraum**
3. **stapelbare Image-Schichten und Copy-on-Write**

4. **virtuelle Netzwerkbrücken**

Die Container können nur begrenzt in einem virtuellen Netztwerk betrieben werden da z.B. Openhab sich stark auf den HOST bezieht und IP Konfiguration sowie Portfreigabe sehr umfassend ist, wird hier kein virtuelles Netztwerk verwendet und nur auf das host Netzwerk referenziert.


### Docker-Config
Die Docker Konfiguration wird im Verzeichnis config erstellt und enthält unterordner pro Container z.B. config/openhab die Verwaltung und docker-compose.yaml files sind im Ordner config/docker abgelegt.
