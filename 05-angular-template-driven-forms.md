# Template driven forms

1. importiere FormsModule im entsprechenden Module (z.B. im app.module.ts) 

## Template
```html
<form #myForm="ngForm" (ngSubmit)="formSubmit(myForm)">
  <div ngModelGroup="gruppe">
    <input type="text" 
      placeholder="firstname" 
      ngModel 
      #firstNameControl="ngModel" 
      name="fname" 
      required 
      minlength="2"/>
    <div *ngIf="firstNameControl.invalid && (firstNameControl.dirty || firstNameControl.touched)">
      <span *ngIf="firstNameControl.hasError('required')">Pflichtfeld</span>
      <span *ngIf="firstNameControl.hasError('minlength')">Minimum 2 Zeichen....</span>
    </div>

    <input type="text" 
      placeholder="lastname" 
      [ngModel]="userData.lastname" 
      #lastNameControl="ngModel" 
      name="lname" 
      required 
      minlength="2"/>
    <div *ngIf="lastNameControl.invalid && (lastNameControl.dirty || lastNameControl.touched)">
      <span *ngIf="lastNameControl.hasError('required')">Pflichtfeld</span>
      <span *ngIf="lastNameControl.hasError('minlength')">Minimum 2 Zeichen....</span>
    </div><br/>
  </div>
  
  <input type="text" 
    placeholder="email" 
    name="email" 
    required 
    pattern="^[a-zA-Z0-9._-]+@[a-zA-Z0-9-.öäüÖÄÜéàèÉÀÈâêîôûÂÊÎÔÛçÇ]+\.[a-zA-Z]{2,13}$"
    ngModel 
    #emailControl="ngModel"/>
  <div *ngIf="emailControl.invalid && (emailControl.dirty || emailControl.touched)">
    <span *ngIf="emailControl.hasError('required')">Pflichtfeld</span>
    <span *ngIf="emailControl.hasError('pattern')">ungültige EMail Adresse</span>
  </div><br/>

  <button type="submit" [disabled]="myForm.invalid">send data</button>
</form>
```

## mögliche CSS-Classen
```CSS
  .ng-touched{}   /* im Feld gewesen und den focus verloren */
  .ng-untouched{} /* gegenteil von ng-touched */
  .ng-dirty{}     /* etwas wurde ins Feld getippt */
  .ng-pristine{}  /* gegenteil von ng-dirty */
  .ng-invalid{}   /* selbstsprechend */
  .ng-valid{}     /* selbstsprechend */
```

## Typescript Klasse
```ts
export class TemplateDrivenFormsComponent implements OnInit {
    // Objekt für die Standartwerte des Formulars
  userData = {
    firstname: '',
    lastname: 'lname1',
    email: ''
  }

  constructor() { }

  ngOnInit(): void {}

  formSubmit(form: NgForm) {
    console.log(form.value);
    alert('daten durch service gesendet...');
    form.reset();
  }
}
```

# Reactive forms
