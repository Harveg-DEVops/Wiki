# New UI based on Openhab 3
Das UI Basiert auf dem VUE [framwork 7](https://framework7.io/docs/app.html)  
Die charts werden mit dem Apache echart framework erstellt siehe [Cheat-Sheet](https://echarts.apache.org/en/cheat-sheet.html)  
*The final ECharts configuration is a “merge” of the transformation of the components in the slots (grid etc.) and this options object taken as-is.
(Note that the merging is only done on top-level properties that are not covered by components so you can’t define grids with the grid option under the options config parameter of the chart page, same thing for axes, series and other components; however you can specify most ECharts options under grid in the oh-chart-grid components).
The objects that aren’t supported in options and are taken from subcomponents are listed below in that return statement:* [OpenHab Echart implementation](https://github.com/openhab/openhab-webui/blob/0c2b25beb8cf3eebc1af120c82ad62f4d1ce6dea/bundles/org.openhab.ui/web/src/components/widgets/chart/chart-mixin.js#L56-L76)  

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
...Brainstorming...

### Persönliche Widgets: Entwicklung & Verwendung
Sie können die Bibliothek der Widgets, die Ihnen zur Verfügung stehen, erweitern, indem Sie persönliche Widgets erstellen, entweder selbst oder durch Kopieren von Beispielen aus der Community; dann können Sie sie auf Seiten wiederverwenden, bei Bedarf auch mehrfach, indem Sie einfach ihre Eigenschaften nach Ihren Bedürfnissen konfigurieren.
Um ein neues persönliches Widget hinzuzufügen, klicken Sie als Administrator auf Entwicklerwerkzeuge und dann auf Widgets. Verwenden Sie die "+"-Schaltfläche, um ein neues Widget zu erstellen:

### Widget Vorlage für GUI:

Vorlage für Input field: https://github.com/openhab/openhab-webui/pull/356#issue-493602831  
Vorlage für UI im Allgemeinen: https://community.openhab.org/t/building-pages-in-the-oh3-ui-documentation-draft-2-3/104392/240  
Vorlage für Keypad: https://community.openhab.org/t/keypad/106820  

### Chart Vorlagen für UI
Vorlagen für Calendar App: https://echarts.apache.org/examples/en/editor.html?c=heatmap-cartesian  

