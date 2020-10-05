## Infos zu Angular / Typescript (universal)

help for questions gitter modul angular

Syntax basiert auf JavaScript jedoch ES2015 wird genutzt für zusätzliche Befehle eingeführt
ES2015 != NgModule

https://www.typescriptlang.org/play
Javascript != typescript
für die deklaration einer "klasse" wird ein Interface definiert (Art Vertrag wo der compiler überprüft ob alle objekte defiert wurden)
Für Objekte welche keine Methoden haben (Nur Form definiert werden) einfach interface verwenden. (Interfaces generieren keinen code)
const enum generieren auch keinen code

Angular ist basierend auf Bausteine welche immer aus Klassen besteht auch Module sind klassen!

Decorator: Wird benötigt um aus einer Klasse eine Komponente zu bauen (Logik, Template, Style) -- Logik (klasse) template & Styles (decorator)
selector gibt an wo das template gerendert wird (definiert den Rahmen)
NgModul: sind Klassen die nur in Angular verwendet werden
Das module word als gesonderte Datei gespeichert z.B. root.module und wird vom Angular compiler intepretiert =! ES2015
Jede KOmponente muss an die NgModule gehängt wrden (registirert werden)
Das Wurzelmodul kann weitere Module imprtieren (daraus generieren wir einen Baum) Zuoberst ist das Root.module

ROOT MODULE:
```import ...

@NgModule ({
  imports : [BrowserModule],
  declerations: [ContactsAppComponents],
  bootsrtap: [ContactsAppComponents] **wird benötigt um die App zu verlinken mit dem bootsrtap module 
  
  })
  export class ContactsModule
  
  BOOTSTRAPPING (Motor Starten)
  import {
  ```
Debuggen immer für die Laufzeit innerhalb der developer tools im browser

Directive hat keine eingens Template im Gengensatz zu Component
directive erweitert eie component z.B. mat-list-item *ngFor="let item of items" erweiter jedes Element in mat-list-item mit einer lokalen variabel

HTTP in Angular
Es gibt zum fetchen von Daten di eXMLHttpRequest API wobei diese funktion auch für JSON Abfragen funktioniert

HttpClientModul: Wird als Modul importiert Konvertiert response string automatisch z.B. import{ HttpClientModul } from 
Error handling with retry()


### Projekt Vorlage für GUI:

Vorlage für Openhab UI https://github.com/jh1777/OpenHabUI
https://community.openhab.org/t/customizable-dashboard-ui/97894
