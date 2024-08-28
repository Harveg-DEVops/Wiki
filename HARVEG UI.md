# Grafische Benutzeroberfläche GUI von openHAB

**Die Startseite**
Die Startseite der Haupt-UI besteht aus 4 Registerkarten und bietet einer Übersicht über die Gesamtanlage;
3 Registerkarten, die eine generierte Ansicht Ihres Modells (siehe Modell) darstellen und nach Standort, Ausrüstung und Eigenschaften geordnet sind. (Tabs unten am Bildrand)

**HabAPP**
Die HABPanel-Benutzeroberfläche wird standardmäßig installiert und ermöglicht die Erstellung von benutzerfreundlichen Dashboards neben der HAuptseite, die sich besonders für (z. B. an der Wand montierte) Tablets eignen. Diese Dashboards werden für den Schüler-Steuerung aus dem Klassenzimmer verwendet. 
* Link1
* Link2
* Link3
* Link4
* Link5

**In der folgenden Tabelle sind die verschiedenen Seitentypen und ihre Kompatibilität zusammengefasst:**

|Typ|Gestaltet in|Angesehen in|Gespeichert in|
|----|-----------|---------|---------|
|Home|Haupt-UI|Haupt-UI|JSON DB|
|[Sitemap](./sitemaps.html)|Haupt-UI oder `.sitemap`-Datei|Basic UI, iOS/Android Apps und andere|Konfigurationsordner oder JSON DB|
|[Layout](./layout-pages.html)|Haupt-UI|Haupt-UI|JSON DB|
|[Map](./Karte-Seiten.html)|Haupt-UI|Haupt-UI|JSON DB|
|[Floorplan](./Grundriss-Seiten.html)|Haupt UI|Haupt UI|JSON DB|
|[Chart](./Diagramm-Seiten.html)|Haupt-UI|Haupt-UI|JSON DB|
|[Tabbed](./Registerkarten-Seiten.html)|Haupt-UI|Haupt-UI|JSON DB|


>https://www.openhab.org/docs/ui/


### Modell
Mit dem semantischen Modell können Sie Ihre openHAB-Objekte gruppieren und kategorisieren, um zusätzliche reale Beziehungen und Informationen über sie bereitzustellen. openHAB kann diese Informationen verwenden, um automatisch die Registerkarten Standort, Ausrüstung und Eigenschaften auf der Startseite zu generieren.
Jedes semantische Modellobjekt (Standort, Ausrüstung oder Punkt) ist ein normales openHAB-Element mit semantischen Tags und in semantischen Gruppen angeordnet. Es ist wichtig zu beachten, dass nicht alle Ihre Objekte in das semantische Modell aufgenommen werden müssen. Für die meisten Systeme ist es sinnvoll, nur die Elemente aufzunehmen, mit denen die Benutzer interagieren werden.
Auf dieser Seite können Sie das semantische Modell Ihres Systems verwalten:
**Modell**


### Rules
Rules sind das Herzstück der Heimautomatisierung - automatisieren Sie mit Auslösern, Aktionen und Bedingungen. Regeln können so einfach sein wie eine Anweisung, ein einzelnes Licht zu einer bestimmten Zeit einzuschalten, aber die Verwendung von Skriptsprachen und Blockly ermöglicht auch viel komplexere Automatisierungen.
Auf dieser Seite können Sie alle grundlegenden Regeln verwalten, die Sie zu Ihrem System hinzugefügt haben:  

### Schedule
Nächste zeitbasierte Regeln anzeigen.

Wenn Sie eine Regel erstellen, fügen Sie das Tag `Schedule` hinzu und definieren einen Zeitauslöser,

![timer-trigger](../images/timer-trigger.png)

dann wird diese Regel in der Kalenderansicht auf dieser Seite angezeigt.
Erstellen Sie zum Beispiel eine Regel, die Folgendes auslöst

jeden Samstag

![cron-saturday](../images/cron-saturday.png)

um 7:00 Uhr morgens

![cron-sieben](../images/cron-seven.png)

und es wird in der Zeitplanansicht angezeigt:

![saturday-morning-rule](../images/saturday-rule-schedule.png)

Eine Regel, die wiederholt geplant wird, z. B. jeden Tag um 8:00 Uhr morgens, wird daher an jedem Tag in der Kalenderansicht angezeigt.

### Persönliche Widgets:
Sie können die Bibliothek der Widgets, die Ihnen zur Verfügung stehen, erweitern, indem Sie persönliche Widgets erstellen, entweder selbst oder durch Kopieren von Beispielen aus der Community; dann können Sie sie auf Seiten wiederverwenden, bei Bedarf auch mehrfach, indem Sie einfach ihre Eigenschaften nach Ihren Bedürfnissen konfigurieren.
Um ein neues persönliches Widget hinzuzufügen, klicken Sie als Administrator auf Entwicklerwerkzeuge und dann auf Widgets. Verwenden Sie die "+"-Schaltfläche, um ein neues Widget zu erstellen:

### Widget Vorlage für GUI:

Vorlage für Input field: https://github.com/openhab/openhab-webui/pull/356#issue-493602831  
Vorlage für UI im Allgemeinen: https://community.openhab.org/t/building-pages-in-the-oh3-ui-documentation-draft-2-3/104392/240  
Vorlage für Keypad: https://community.openhab.org/t/keypad/106820  

### Chart Vorlagen für UI
Vorlagen für Calendar App: https://echarts.apache.org/examples/en/editor.html?c=heatmap-cartesian  

