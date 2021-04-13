# New UI based on Openhab 3

In der folgenden Tabelle sind die verschiedenen Seitentypen und ihre Kompatibilität zusammengefasst:

|Typ|Gestaltet in|Angesehen in|Gespeichert in|
|----|-----------|---------|---------|
|Home|Haupt-UI|Haupt-UI|JSON DB|
|[Sitemap](./sitemaps.html)|Haupt-UI oder `.sitemap`-Datei|Basic UI, iOS/Android Apps und andere|Konfigurationsordner oder JSON DB|
|[Layout](./layout-pages.html)|Haupt-UI|Haupt-UI|JSON DB|
|[Karte](./Karte-Seiten.html)|Haupt-UI|Haupt-UI|JSON DB|
|[Grundriss](./Grundriss-Seiten.html)|Haupt UI|Haupt UI|JSON DB|
|[Diagramm](./Diagramm-Seiten.html)|Haupt-UI|Haupt-UI|JSON DB|
|[Registerkarte](./Registerkarten-Seiten.html)|Haupt-UI|Haupt-UI|JSON DB|

Übersetzt mit www.DeepL.com/Translator (kostenlose Version)
>https://www.openhab.org/docs/ui/
...Brainstorming...

### Persönliche Widgets: Entwicklung & Verwendung
Sie können die Bibliothek der Widgets, die Ihnen zur Verfügung stehen, erweitern, indem Sie persönliche Widgets erstellen, entweder selbst oder durch Kopieren von Beispielen aus der Community; dann können Sie sie auf Seiten wiederverwenden, bei Bedarf auch mehrfach, indem Sie einfach ihre Eigenschaften nach Ihren Bedürfnissen konfigurieren.
Um ein neues persönliches Widget hinzuzufügen, klicken Sie als Administrator auf Entwicklerwerkzeuge und dann auf Widgets. Verwenden Sie die "+"-Schaltfläche, um ein neues Widget zu erstellen:

### Widget Vorlage für GUI:

Vorlage für Input field: https://github.com/openhab/openhab-webui/pull/356#issue-493602831
Vorlage für UI im Allgemeinen: https://community.openhab.org/t/building-pages-in-the-oh3-ui-documentation-draft-2-3/104392/240
