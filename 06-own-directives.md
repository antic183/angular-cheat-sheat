
# eigene Directives

## Einfachstes Beispiel:

> ng g d red

erzeugt red.directive.ts & red.directive.spec.ts

__red.direvtive.ts__
```JS
import { Directive, ElementRef, HostListener } from '@angular/core';
@Directive({
  selector: '[appRed]'
})
export class RedDirective {
  constructor(private el: ElementRef) {
    el.nativeElement.style.backgroundColor = 'red';
  }

  // events: mouseenter, mouseleave, click, etc...
  @HostListener('click') onMouseEnter() {
    let color = (prompt("Welche Hintergrundfarbe soll das element erhalten?", "grey")).toLowerCase();
    let availableColors = ['red','green','orange','blue','purple','black','white','grey'];
    if (availableColors.includes(color)) {
      this.el.nativeElement.style.backgroundColor = color;
    } else {
      alert('Farbe unbekannt');
    }
  }
}
```

__Directive anwenden__

Die appRed directive setzt den Hintergrund auf Rot. Durch einen click kann dieser verändert werden.
```html
<p appRed>dieser Text wird Rot</p>
```

## Parameter übergeben
```html
<p appRed='orange'>dieser Text wird Rot</p>
```

```JS
import { Directive, ElementRef, HostListener, Input, OnInit } from '@angular/core';

@Directive({
  selector: '[appRed]'
})
export class RedDirective implements OnInit{
  @Input('appRed') defaultColor: string;

  constructor(private el: ElementRef) {
  }

  ngOnInit() {
    this.el.nativeElement.style.backgroundColor = this.defaultColor || 'red';
  }

  ...
}
```


# eigene strucutural Directive