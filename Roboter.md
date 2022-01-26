## Aufbau
Die Software Platform "Vertikaler Anbau" wird als Open-Source Code basierend auf dem Projekt Farmbot weitergeführt und alle Bestandteile der neuen SW-Architektur werden nachfolgend kurz erläutert.

Farmbot Web-App (wird 1:1 übernommen und von Farmbot Inc gehostet): https://developer.farm.bot/docs/web-app  
Kommunikations Schnitstelle zu unsere SW: Node-Red welches auf Linux Server läuft: https://github.com/Harveg-DEVops/Wiki/blob/master/Werkzeuge.md#node-red  

### Steuerung & GUI über Linux Server mit „openhabian" & Node-Red

Der Raspberry Pi und andere kleine Einplatinencomputer sind ziemlich bekannte Plattformen für openHAB welches wir als Basis verwenden.  
Die Einrichtung eines voll funktionsfähigen Linux-Systems mit allen empfohlenen Paketen und openHAB-Empfehlungen ist jedoch eine langweilige Aufgabe, die einige Zeit in Anspruch nimmt, und Linux-Neulinge sollten sich über diese technischen Details keine Gedanken machen.   
openHABian zielt darauf ab, ein selbstkonfigurierendes Linux-System bereitzustellen, das speziell auf die Bedürfnisse jedes openHAB-Benutzers zugeschnitten ist. Zu diesem Zweck bietet das Projekt zwei Dinge:
* Vollständige SD-Karten-Images, die mit openHAB und vielen anderen openHAB- und Hardware-spezifischen Vorbereitungen für die Raspberry Pi vorkonfiguriert sind.
* Das openHABian-Konfigurationswerkzeug, um openHAB und viele weitere Dienste auf jedem Debian/Ubuntu-basierten System einzurichten und zu konfigurieren
* Restful API Schnittstelle: https://github.com/Harveg/hiag/wiki/REST-API-Schnittstelle

Die Software Kommunikations-Architektur welche auf dem Projekt Farmbot basiert, kann wie folgt aufgebaut werden:
Links zu Entwicklung Umgebung von Farmbot: 
https://github.com/FarmBot/farmbot_os/blob/staging/docs/target_development/building_target_firmware.md

### Logische Architektur (Message Broker)  
![Farmbot hihl Level overview](https://software.farm.bot/v7/FarmBot-Software/_images/flow_chart.png)  

              
HTTP mit WebSocket ( Überbrückung der Http kommunikation zu Mqtt)  
              
AMQP ( Kommunikationskanal für Software update von Farmbot OS)  
          
MQTT (data sync, comands, sequences)  
Oberste Hirarchie bildet der Server mit dem MQTT Broker  

Mittlere Architektur bildet der Roboter (Rasperry Pi) mit einem MQTT Bridged-Broker (Bridge & Publish & Subscribe)

Unterste Ebene bilden die Clients (Endnutzer) 
* MQTT Kommunikation Subscribe & Publish
* Kommunikation mit Lift Steuerung TBD / z.B. MQTT Synchronisiert

Erklärung MQTT:https://blog.doubleslash.de/mqtt-fuer-dummies/

### Physikalische Architektur

In der obersten Oberste Hirarchie steht der Linux-Server mit dem Domain-Agent worauf sich alle Clients (Lampen, Sensoren) anmelden  
* Fixe IP Adresse IPV4  
* vorgegebenen Subnetzmaske 255.255.255.0 

Verfügbare IP-Range pro IPV4 Netzwerk: 254
Broadcast-Adresse
Adresse 0 (1):                    Router         (Client)
Adressen 1-4 (4):               Reserve  
Adressen 5-24 (20):             Sensoren     (Temperaturfühler, Schalter, Feuchtigkeitssensor, Kameras) 4 / Roboter
Adressen 25-249 (225):         Aktoren         (Lift-Steuerung, Beleuchtungs-Relais, Klima-Steuerung) 45 / Roboter
* Aufteilung des IP-Range 25-69 wie folgt : 25--> Lift Steuerung 26 - 69-> Restliche Aktoren
Adressen 250-255 (5):         Roboter          (Raspberry Pi) 

Die Kommunikation auf der untersten ebene (roboter <-> Senoren /aktoren) folgt über wifi ausser für die Liftsteuerung & Mainboard welche direkt mit dem Raspberry verbunden sind (serial bus RS 485) . 
Hierfür wird auf dem Raspberry ein Wifi 2.4 Ghz Netzwerk für die oben beschriebenen Broadcast adressen eingerichtet.  

### Netzwerkeinstellungen Hotspot: 
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

Arduino Firmware (Wird übernommen und wo nötig angepasst)
