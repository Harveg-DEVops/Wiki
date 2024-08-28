## Aufbau
Die Software Platform des HARVEG-Gartenrobi basiert auf verschiedenen Open-Source Projekten. Die folgenden drei wesentliche Bestandteile der SW-Architektur werden nachfolgend kurz erläutert.

#### Open-Hab: [Linux-Server und GUI nachfolgend erklärt](https://github.com/openhab/openhabian) --> http://harveg:8080
#### Node-Red: [Kommunikations Schnitstelle zwischen Farmbot & Open-Hab auf unserer Plattform Linux Server](https://github.com/Harveg-DEVops/Wiki/blob/master/Werkzeuge.md#node-red) --> http://harveg:1880
#### Farmbot Web-App: [wird 1:1 übernommen und von Farmbot Inc gehostet](https://developer.farm.bot/docs/web-app) --> https://my.farmbot.io


### openHAB - Steuerung & GUI auf Linux Server mit „openhabian" (openHAB, Node-Red & infoxDB)

Die Software Platform mit den oben geannten Bestandteilen lässt sich auf jedem Linux-System betreiben, ob als Dienst oder in einem Container virtualisiert. 
Die Einrichtung eines voll funktionsfähigen Linux-Systems mit allen empfohlenen Paketen und openHAB-Empfehlungen ist jedoch eine langwierige Aufgabe, die einige Zeit in Anspruch nimmt, und Linux-Neulinge sollten sich über diese technischen Details keine Gedanken machen.   
openHABian zielt darauf ab, ein selbstkonfigurierendes Linux-System bereitzustellen, das speziell auf die Bedürfnisse jeder openHAB-Platform zugeschnitten ist. Zu diesem Zweck bietet das Projekt zwei Dinge:
* Vollständige SD-Karten-Images, die mit openHAB und vielen anderen openHAB- und Hardware-spezifischen Vorbereitungen für die Raspberry Pi vorkonfiguriert sind.
* Das openHABian-Konfigurationswerkzeug, um openHAB und viele weitere Dienste (Node-Red & influxDB) auf jedem Debian/Ubuntu-basierten System einzurichten und zu konfigurieren
* Restful API Schnittstelle: https://github.com/Harveg/hiag/wiki/REST-API-Schnittstelle

### Roboter / Farmbot SW   

Der Roboter selbst ist aufgebaut auf dem Open-Source Projekt Farmbot und die SW läuft auf einem eigenständigen Raspberry PI auf dem Roboterarm.

![Farmbot hihl Level overview](https://software.farm.bot/v7/FarmBot-Software/_images/flow_chart.png)  

Dei Kommunikation zwischen der Steuerung respektive openHAB und dem Roboter erfolgt über die folgenden Kanäle:

* HTTP mit WebSocket ( Überbrückung der Http kommunikation zu Mqtt)  
              
* AMQP ( Kommunikationskanal für Software update von Farmbot OS)  
          
* MQTT (data sync, comands, sequences)  

**Logische Architetur:**

Oberste Hirarchie bildet der Server mit dem MQTT Broker: clever-octopus.rmq.cloudamqp.com Port:1883*

Mittlere Architektur bildet openHAB (Node-RED) mit einem MQTT Bridged-Broker (Bridge & Publish & Subscribe)
Für die sichere Kommunikation zwischen dem Roboter und openHAB muss periodisch ein neues Verbindungs-Token generiert und in die Konfiguration des MQTT-Bridged Broker eingefügt werden. Es kann somit sein, dass der Roboter kurzzeitig nicht mehr erriechbar ist, weil die Gültigkeit des Tokens abgelaufen ist. Dabei kann einfach über die Applikation Node-Red ein neues Token per Knopfdruck generiert werden und in die Konfiguration des MQTT Bridged-Broker eingefügt werden.  

**Abonnierte MQTT Topics**:  
* /emergency_status
* bot/device_7531/status
* bot/device_7531/sync/#
* bot/device_7531/logs
* bot/device_7531/from_clients

Unterste Ebene bilden die Clients (Roboter) 
* device_7531

*MQTT Kommunikation Subscribe & Publish [Erklärung MQTT](https://blog.doubleslash.de/mqtt-fuer-dummies/)

### Physikalische Architektur

Als HW-Plattformen für openHAB, Node-Red und den Roboter wird der Raspberry Pi verwendet. Dieser ist über  Ethernet an das Netzwerk und Internet angeschlossen und lässt sich mit dem Fernzugang aus dem Internet erreichen: [myopenhab.org](https://myopenhab.org/)  

### Netzwerktopologie  

In der obersten Oberste Hirarchie steht der Router mit dem DNS-Server worauf sich alle Clients (Raspberry, Lampen, Sensoren) verbinden.  
* Fixe IP Adresse IPV4  
* vorgegebenen Subnetzmaske 255.255.255.0 

Verfügbare IP-Range pro IPV4 Netzwerk: 254
Broadcast-Adresse
Adresse 0 (1):                    Router         (Client)
Adressen 1-4 (4):               Reserve  
Adressen 5-24 (20):             Sensoren     (Temperaturfühler, Schalter, Feuchtigkeitssensor, Kameras) 4 / Roboter
Adressen 25-249 (225):         Aktoren         (Lift-Steuerung, Beleuchtungs-Relais, Klima-Steuerung) 45 / Roboter
Adressen 250-255 (5):         Roboter          (Raspberry Pi) 

Die Kommunikation auf der untersten ebene (roboter <-> Senoren /aktoren) folgt über wifi ausser für das Wasserventil & Überwachung der Schaltschrank-Speisung (230V), welche direkt mit dem Raspberry verbunden sind (IO's) . 

### Netzwerkeinstellungen Hotspot: (aktuell nicht in Gebrauch)
Die Kommunikation im dezentralen Modus (Platform <-> Senoren /aktoren) folgt über wifi ausser für die Liftsteuerung von Getriebe Nord, welche direkt mit dem Raspberry verbunden sind (Modbus).  
Hierfür wird auf dem Raspberry ein Wifi 2.4 Ghz Netzwerk für die oben beschriebenen Broadcast adressen eingerichtet.

#### 802.11b/g/n with WPA2-PSK and CCMP
A simple but secure AP with maximal compatibility for current hardware: /etc/hostapd/hostapd.conf  
```interface=wlan0  
driver=nl80211  
ssid=<SSIDNAME>  
hw_mode=g  
channel=7  
# limit the frequencies used to those allowed in the country  
ieee80211d=1  
# the country code  
country_code=CH  
# 802.11n support  
ieee80211n=0
# QoS support, also required for full speed on 802.11n/ac/ax  
wmm_enabled=0  
macaddr_acl=0  
auth_algs=1  
ignore_broadcast_ssid=0  
wpa=2  
wpa_passphrase=<PASSWORD>  
wpa_key_mgmt=WPA-PSK  
wpa_pairwise=TKIP  
rsn_pairwise=CCMP
```  

Hilfestellung: https://gist.github.com/christofluethi/1be3b5e6b9010a4d4bdcf2a892fedb47

Erklärung IP-Adresse: https://de.ryte.com/wiki/IP-Adresse
