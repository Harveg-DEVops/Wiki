### Openhabian on Raspberry 4 Mod. B
Der Raspberry Pi und andere kleine Einplatinencomputer sind ziemlich berühmte Plattformen für openHAB. 
Die Einrichtung eines voll funktionsfähigen Linux-Systems mit allen empfohlenen Paketen und openHAB-Empfehlungen ist jedoch eine langweilige Aufgabe, die einige Zeit in Anspruch nimmt, und Linux-Neulinge sollten sich über diese technischen Details keine Gedanken machen. 
openHABian zielt darauf ab, ein selbstkonfigurierendes Linux-System bereitzustellen, das speziell auf die Bedürfnisse jedes openHAB-Benutzers zugeschnitten ist. 
Zu diesem Zweck bietet das Projekt zwei Dinge:
* Vollständige SD-Karten-Images, die mit openHAB und vielen anderen openHAB- und Hardware-spezifischen Vorbereitungen für die Raspberry Pi und die Pine A64 vorkonfiguriert sind.
* Das openHABian-Konfigurationswerkzeug, um openHAB und viele verwandte Dinge auf jedem Debian/Ubuntu-basierten System einzurichten und zu konfigurieren

### Controller Hardware
Prozessor / SoC	Broadcom BCM2711, Quad core Cortex-A72 64-bit SoC @ 1.5 GHz.
Arbeitsspeicher	1 GB, 2 GB oder 4 GB LPDDR4 SDRAM
Konnektivität	•	dual-band IEEE 802.11ac wireless
* Bluetooth 5.0
*	Gigabit Ethernet
* 2x USB 3.0
* 2x USB 2.0
*	1x MicroSD-Slot
*	2x micro-HDMI
* GPIO	Standardisierter Raspberry Pi 40 pin GPIO Heade

## Konfiguration

### Netzwerkeinstellungen

Für die Netzwerkeinstellungen auf dem Controller wird je nach Verwendungszweck wie folgt vorgegangen:
Für den dezentralen Einsatz empfiehlt sich folgende Konfiguration der Netzwerkeinstellungen: Die Kommunikation im dezentralen Modus (Platform <-> Senoren /aktoren) folgt über wifi ausser für die Liftsteuerung & Mainboard welche direkt mit dem Controller verbunden sind (serial bus) . Hierfür wird ein Acces Point auf dem Controller mit den unten beschriebenen Broadcast Adressen eingerichtet. 
https://gist.github.com/christofluethi/1be3b5e6b9010a4d4bdcf2a892fedb47
Sofern die Plattform Zentral eingesetzt wird muss die Netzwerkinfrastruktur entsprechend den lokalen Gegebenheiten der Netzwerkarchitektur Rechnung getragen werden.

Für Controller AP Wlan0
Parameter	Wert
* IPV4	IP   =   169.254.250.250 /250-254 reserviert für Controller
* Mask       =   255.255.255.0
* Gateway    =   ????
* IPV 6	OFF