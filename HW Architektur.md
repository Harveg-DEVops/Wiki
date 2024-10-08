### Hardware  

Produktename für Kunde    | 	Lieferant       |  Eigenschaften  
--------------------------|-------------------|-----------------  
WLAN Uni - Hub            |  Shelly / HARVEG  |  Sensor Aktor Box, Vielzahl Anbindungsmöglichkeiten, 4 Ports, ohne Sensorik  
WLAN Uni - Hub HT	        |  Shelly / HARVEG  |  Sensor Box, Feucht. / Temp., 4 Ports,  ohne Sensorik, mit Umschalter  
WLAN Controller           |	 Andino           |  Schaltzentrale, für Openhab, Grafana,  ohne Sensorik  
WLAN Controller LTE	      |  Andino           |  Schaltzentrale mit LTE Modem für Openhab, Grafana,  ohne Sensorik  
Basic Controller          |  Rasperry PI 4    |  Schaltzentrale mit SSD 128GB (inkl. Back Up) für Openhab, Grafana, Datenbank, 32GB  



### Openhabian on Raspberry 4 Mod. B
Der Raspberry Pi und andere kleine Einplatinencomputer sind ziemlich bekannte Plattformen für openHAB welches wir als Basis verwenden. Die Einrichtung eines voll funktionsfähigen Linux-Systems mit allen empfohlenen Paketen und openHAB-Empfehlungen ist jedoch eine langweilige Aufgabe, die einige Zeit in Anspruch nimmt, und Linux-Neulinge sollten sich über diese technischen Details keine Gedanken machen. openHABian zielt darauf ab, ein selbstkonfigurierendes Linux-System bereitzustellen, das speziell auf die Bedürfnisse jedes openHAB-Benutzers zugeschnitten ist. Zu diesem Zweck bietet das Projekt zwei Dinge:
* Vollständige SD-Karten-Images, die mit openHAB und vielen anderen openHAB- und Hardware-spezifischen Vorbereitungen für die Raspberry Pi und die Pine A64 vorkonfiguriert sind.
* Das openHABian-Konfigurationswerkzeug, um openHAB und viele weitere Dienste auf jedem Debian/Ubuntu-basierten System einzurichten und zu konfigurieren 

### Controller Hardware
Prozessor / SoC	Broadcom BCM2711, Quad core Cortex-A72 64-bit SoC @ 1.5 GHz.
Arbeitsspeicher	1 GB, 2 GB oder 4 GB LPDDR4 SDRAM
Konnektivität	
* dual-band IEEE 802.11ac wireless
* Bluetooth 5.0
*	Gigabit Ethernet
* 2x USB 3.0
* 2x USB 2.0
*	1x MicroSD-Slot
*	2x micro-HDMI
* GPIO	Standardisierter Raspberry Pi 40 pin GPIO Heade

### Datenspeicher
Für die speicherhungrige Datenbank und auch um eine genügende hohe Verfügbarkeit zu erhalten, verwenden wir nur hochwertige SD Karten ab 32GB, im Fall das eine USV vorhanden ist. Wenn keine USV angeschlossen wird welche ein sicheren shut-down garantiert müssen wir dem Raspberry pi über USB 3.0 einen externen SSD Datenträger anbinden:
* Gehäuse: https://www.raidsonic.de/index.php?we_objectID=5665
* Speicher: https://www.intenso.de/produkte/solid-state-drives/m.2-SSD-PCIe

## Konfiguration

### Netzwerkeinstellungen

Für die Netzwerkeinstellungen auf dem Controller wird je nach Verwendungszweck wie folgt vorgegangen:
Für den dezentralen Einsatz empfiehlt sich folgende Konfiguration der Netzwerkeinstellungen: Die Kommunikation im dezentralen Modus (Platform <-> Senoren /aktoren) folgt über wifi ausser für die Liftsteuerung & Mainboard welche direkt mit dem Controller verbunden sind (serial bus) . Hierfür wird ein Acces Point auf dem Controller mit den unten beschriebenen Broadcast Adressen eingerichtet. 
https://gist.github.com/christofluethi/1be3b5e6b9010a4d4bdcf2a892fedb47
Sofern die Plattform Zentral eingesetzt wird muss die Netzwerkinfrastruktur entsprechend den lokalen Gegebenheiten der Netzwerkarchitektur Rechnung getragen werden.

Für Controller AP Wlan0
Parameter	Wert
* IPV4	IP   =   169.168.250.250 /250-254 reserviert für Controller
* Mask       =   255.255.255.0
* Gateway    =   ????
* IPV6	OFF
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

Für Controller mit LTE Modem Wwan0

Anweisung aus Andino System Seite: https://andino.systems/andino-io/products/andino-io_4g-modem  
--> Automatisches starten des Verbindungsvorganges nach Systemstart über ein Script oder Cron job alle 10min welche das Modem Startet und entsprechend die Berechtigung hat die 
4 Schnitstellen: USB0 bis USB4 (2 Datenverbindungen und eine Modem Befehlsverbindung USB2)  
--> Die Verwendung des SMS Node mit dem Andino IO (LTE Modem) über die Serielle Schnitstelle ttyUSB2  
