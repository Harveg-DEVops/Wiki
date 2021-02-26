## Start mit docker

Unsere SW-Architektur basiert auf Docker und nützt die docker-compose Erweiterung.
Einen umfassenden Einblick in die Welt von Docker gibt folgender Beitrag von Chrigu: https://github.com/christofluethi/docklands/blob/master/docker-techtalk/docker-slides.pdf  

## Basics

### Docker-Container basieren auf Images
* Images werden über Dockerfile definiert
* oder das Speichern von Containern
* Docker-Images werden in Registries gespeichert

### DockerHub

Docker-Host ist die Maschine, auf der der Docker-Daemon läuft  
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





