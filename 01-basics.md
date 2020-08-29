# First steps

## installation
``` 
npm install -g @angular/cli
ng --version
```

## first project
```
ng new projectname
```

dito mit inline style, inline template, ohne tests, mit einem Routing Module und SCSS als Standart
```
ng new projectname --inlineStyle=true --inlineTemplate=true --skipTests=true --routing=true --style=scss
```

## run project
```
cd appname
ng serve --open (http://localhost:4200)
```

# Debugging helper for chrome 
___"Augury" siehe augury.angular.io___



# VS Code extensions

> Quick open (strg+p)


__Linter für ts:__
```
ext install ms-vscode.vscode-typescript-tslint-plugin 
```

__Autoimporter__
```
ext install steoates.autoimport 
```

__Angular snippets__
```
ext install johnpapa.angular2 
```

__Angular Dateien generieren lassen__
```
ext install alexiv.vscode-angular2-files 
```

__code highlighting für den inhalt von js template strings (sonst würde alles in einer Farbe dargestellt werden).__
```
ext install natewallace.angular2-inline
```

__chrome debugger extension__
```
ext install msjsdiag.debugger-for-chrome
```


# cli Befehle
erstelle Componente (ng generate component ....)
```
ng g c componentName
```

Parameter für inline template, inline css und ohne tests
```
ng g c componentName --inlineTemplate=true --inlineStyle=true --flat --skipTests=true
```
> --flat = kein Ordner erstellen


service
```
ng g s serviceName
```

pipe
```
ng g p pipeName
```

directive
```
ng g d directiveName
```


# Events
```html
<button (click)="functionName()">btn</button>
```
falls man das event in der funktion übergeben möchte
```html 
<button (click)="functionName($event)">btn</button>
```

# Directives

## was sind directives? 
__Directives sind Befehle die wir im DOM Absetzen. Es gibt zwei Arten von Directives. Es existieren Attribute und Structural Directives. "Attribute Directives" interagieren immer mit dem Element auf dem Sie angewendet wurden. Z.B. "ngClass" oder "ngStyle". Dabei ist kein Property oder Event Binding notwendig. "Structural Directives" verändern die Struktur des ViewContainers des Elements in welchem diese angewendet wird. Struktur directives fangen immer mit einem * an. Z.B. *ngIf oder *ngFor__

## __ngFor example__
```html
<li *ngFor="let el of tasks">{{el}}</li> 
```
## __ngFor extended example__
```html
<li *ngFor="let el of tasks; index as i; first as f; last as l;">
  {{el}} index = {{i}}, erstesEl = {{f}}, letztesEl = {{l}}
</li>
```

Man bekommt Infos über den __index__, das erste und letzte element.

__First__ und __last__ geben einen boolschen Wert zurück.

Beim ersten Druchgang ist __first__ true und beim letzten ist __last__  true.

Es gibt noch __even__ und __odd__. Diese arbeiten auch mit einem boolschen Wert und funktionieren vom Prinzip her gleich wie __first__ und __last__.


## __ngIf und else__
```html
<p *ngIf="activeated; else other">{{info}}</p>
<p #other>el noch nicht aktiviert</p>
```

## __was macht Angular mit dem *__
Angular wandelt den Tag mit dem Stern eigentlich immer einen html5 template tag mit Property-Binding um. Anbei ein Beispiel:

Aus:
```html
<p *ngIf="userActive">{{info}}</p>
```
Macht Angular das:
```html
<template [ngIf]="userActive">
  <p>{{info}}</p>
</template>
```

## __ngSwitch & ngSwitchCase__
```html
<div [ngSwitch]="myValue">
  <p *ngSwitchCase="1">die variable myValue hat momentan den Wert 1</p>
  <p *ngSwitchCase="2">die variable myValue hat momentan den Wert 2</p>
  <p *ngSwitchCase="3">die variable myValue hat momentan den Wert 3</p>
  <p *ngSwitchDefault>die variable myValue hat momentan einen unbekannten Wert</p>
</div>
```

# pipes
<p>{{name | uppercase}}</p>
... weitere bsp. siehe offizielle Doku.

# Property bindings
__Beispiel:__
```html
<button [disabled]="varTrueOrFalse">btn property binding</button>
```

# 2-Way bindings (antipattern!)
__Beispiel:__
```html
<input type="text" [(ngModel)]="varName" />
```

# Class bindings
__Beispiel:__
```html
<p [ngClass]="{
  classe1: var1AusKlasseIsTrue_DannWirdClasse1Gesetzt, 
  classe2: var2AusKlasseIsTrue_DannWirdClasse2Gesetzt
}">
  text...
</p>
```
# Style bindings
__sollte eigentlich nicht verwendet werden! Für was gibt es CSS und CSS Klassen!? Anbei ein Beispiel:__
```html
<p [ngStyle]="{
  'color': meineFarbe, 
  'margin': meinMargin, 
  'padding': '15px'
}">
  text...
</p>
```

# Communication between components
## Property passing (ng-content)
__ng-content__ ist eine Platzhalter-Direktive die den übergebenden Inhalt(HTML) aus dem parent component anzeigt. CSS angagen werden dabei aber aus der parent component übernommen. Anbei ein Beipsiel.

__template parent component__
```html
<app-child>
  <h1>text...</h1> 
  <!-- der Inhalt der in ng-content dargestellt wird -->
</app-child>
```
__template component app-child__
```html
<h1>child component</h1>
<ng-content></ng-content> 
<!-- Vorsicht: der ng-content übernimmt die CSS angaben der parent componente -->
```

## Property passing (@input)
__parent template:__
```html
<div>
	<sub-componente [propX]="varNameFromParent"></sub-componente>
	<sub-componente [propX]="'werte benötigen zusätzliche Klammern'"></sub-componente>
</div>
```
__child component:__
```ts
// 1. import Input von angular core

// in der child component kann nun auf den übergebenen Wert über die var propX darauf zugegriffen werden
@Input() propX;

// falls man die variable im Child anders benennen möchte. Ist aber zu vermeide!
@Input('propX') otherVarName; 
```


## Child call parent methode (@Output & EventEmitter)

1. import EventEmitter, Output von angular core

__Parent__
```html
<!-- template -->

<child-component (childOutputMethodeNameEmitter)="myMehtodeHere($event)"></child-component>
```
```ts
// Typescript class

myMehtodeHere(nr: number) {
	alert(nr);
}
```

__Child__
```html
<!-- template -->

<button (click)="notifyParent()">notify parent</button>
```
```ts
// Typescript class

@Output() childOutputMethodeNameEmitter = new EventEmitter<number>();

notifyParent() {
  // call parent "myMehtodeHere(param)" methode.
  this.childOutputMethodeNameEmitter.emit(177); 
  alert('parent wurde benachrichtigt!');
}
```



# services 
Services sind globale Singletons, ausser man definiert sie z.B. auf ebene component, dann werden diese initialisiert immer wenn die Komponente neu initialisiert wird. 

## 1. erstellen
> ng g s services/MyXyService

Ein über das CLI erzeugter Service ist Standartmässig über "providedIn: 'root'" von überall und von jedem definierten Modul aus zugreifbar. Möchte man den Service aber nur in einem  Modul verwenden und den Zugriff beschränken, so müsste man in der service.ts Datei "providedIn: 'root'" rausnehmen und den Service im zu verwendeten Modul (my-module.ts) unter Providers definieren. __Merke dir: Sobald ein Modul neu instanziiert wurde, wird auch der Service neu instanziiert. Der Service ist in dieser Konstellation nur auf Ebene des Moduls ein Singleton!__

## 2. services/MyXyService.service.ts
__anbei eine einfache Service Logik, die einfach dummy Daten zurückgibt__
```ts
@Injectable({
  providedIn: 'root'
})
export classe MyXyService{
	data[number];
	constructor() {
		this.data = [5,4,7,2];
	}
	getData() {
		return this.data;
  }
  add(entry: number) {
    this.data.push(entry);
  }
}
```

## 2. services in einer component verwenden
```ts
@import {MyXyService} from ....
clas ComponenteXY {
	data:[number] = [];

	constructor(private myService: MyXyService) {
    this.data = this.myService.getData();
		// this.myService.getData().subscribe(val => this.data = val;); 
    // subscribe wenn asynchron resp. wenn ein Promise zurückgegeben wird
	}
	
	
	clickEvent() {
		console.log(this.data);
    let randNumber = Math.floor(Math.random() * 100) + 1;
		this.myService.add(randNumber);
    console.log(this.myService.getData());
	}
}

```

# Referenzen mit # setzen und zugreifen mit ViewChild

mit # erhält man die Referenz auf ein Element. Über die Referenz kann man auf z.B. den Value oder den Style zugreifen.
```html
<input type='text' #ref1 (keydown)="0"/>
<p>{{ref1.value}}</p>
```
Das Event "keydown" brauchts in diesem Fall nur um die Angular change detection zu starten.

in der ts Klasse kann die Referenz als Property mit Hilfe vom decorator  @ViewChild verwendet werden.
```ts
@ViewChild('ref1') myRef: ElementRef;
methode() {
  // Zugriff über nativeElement
  this.myRef.nativeElement.value = 'new value';
}
```

Elemente die vom parent component übergeben werden und eine Referenz haben und hier im child component mit ng-content angezeigt werden. Auf diese kann nicht mit ViewChild zugegriffen werden. Für den Zugriff benötigt man hier __ContentChild__.

parent template
```html
<app-child>
  <p #refParent>content....</p>
</app-child>
```

child template
```html
<p>child template...</p>
<ng-content></ng-content>
```
child ts-class
```ts
@ContentChild('refParent') retParent: ElementRef;
methode() {
  // Zugriff über nativeElement
  this.refParent.nativeElement.innerText = 'child has override the content...';
}
```


# Component lifecycle hooks

> __ngOnInit__ bei der initialisierung nach dem ersten __ngOnChanges__

> __ngOnChanges__  vor __ngOnInit__ und wenn eine gebunden @Input property den Inhalt geändert hat.

> __ngDoCheck__ wird immer aufgerufen wenn Angular eine change detection durchführt.

> __ngAfterContentInit__ nachdem der content via __ng-content__ eingefügt wurde.

> __ngAfterContentChecked__ nachdem der content von __ng-content__ durch die change detection überprüft wurde.

> __ngAfterViewInit__ nachdem die view der componente initialisiert wurde und der Child views.

> __ngAfterViewChecked__ nachdem die view der componente und der child views überprüft wurden (change detection).

> __ngOnDestroy__ wenn eine componente zerstört wird.
