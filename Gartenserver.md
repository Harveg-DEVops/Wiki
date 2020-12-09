# Für die richtige Arbeitsumgebung im Testcenter wird ein Server für die zentrale Datenablage und verwaltung der Entwicklungsumgebung installiert.
Der Server ist als Notlösung erstmas ein Desktop PC mit 1TB SSD und Ubuntu 18.04 welches als Betriebssystem für folgende Applikationen gewählt wurde:

* Docker (Keine Ahnung was das ist aber alle sprechen davon;)--> SrumMaster
* Nextcloud nextcloud.harveg.ch / <Radical>[https://radicale.org/3.0.html] CalDAV Server, damit werden wir den Anbau planen können (Wird aktuell mittels Nutzung von nextcloud auf nextcloud.harveg.ch obsolet)
* <Resilio Sync>[https://help.resilio.com/hc/en-us/categories/200140177-Get-started-with-Sync] (Peer to Peer Datensynchronisation)
* <Farmbot-Web-App>[https://github.com/FarmBot/Farmbot-Web-App] (Testserver für Farmbot wird erst selber gehostet wenn die entsprechende Manpower vorhanden ist)

# Setup (under construction)
Ubuntu 18.04 TBD
Python 3.7 TBD
Radical 3.0 https://radicale.org/3.0.html#tutorials/simple-5-minute-setup


# Install docker
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common --yes
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial stable" --yes
sudo apt-get update --yes
sudo apt-get install docker-ce --yes
sudo docker run hello-world # Should run!
# Install docker-compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
