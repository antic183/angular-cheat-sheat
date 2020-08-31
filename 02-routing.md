# Routing

das Template

Das router-outlet fügt die korrekte component ein. Wie ein Platzhalter. 
Die entsprechenden components werden in den Routes definiert.
```html
...navigation...
<router-outlet></router-outlet>
...footer...
```

die Navigation:
__app.module.ts__
```ts
// 1.
import {RouterModule, Routes} from "@angular/router";

// 2.
const myRoutes: Routes = [
  {path: '', redirectTo = 'home', pathMatch: 'full' },
  // pathMatch: 'full' --> exakte url notwendig wegen weiterleitung
  {path: 'home', component: HomeComponent},
  {path: 'about', component: AboutComponent},
  {path: 'contact', component: ContactComponent},
  {path: 'article/:articleId', component: ArticleDetailComponent}, 
  {path: '**', component: My404Component}, // 404

  /* 
    damit auf den Parameter articleId zugegriffen werden kann, 
    muss in der component "ArticleDetailComponent" folgendes gemacht werden:
      => import {ActivatedRoute} from @angular/route 
      => inject im constructor(private activatedRoute: ActivatedRoute) 
      => Verwendung, Zugriff auf den Prameter: 
         methode() {console.log(this.activatedRoute.snapshot.params['articleId']);}
  */
];

// 3.
imports: [
  RouterModule.forRoot(myRoutes), 
  // forChild() anstatt forRoot() verwenden
]
```

__Verdendung im template__
```html
<a routerLink="/home" routerLinkActive="active">Home</a>
<a routerLink="/about" routerLinkActive="active">Über uns</a>
<a routerLink="/contact" routerLinkActive="active">Kontakt</a>

Artikel:
<a routerLink="/article/115">Artikel A</a>
<a routerLink="/article/180">Artikel A</a>
<!-- alternative syntax: [routerLink]="['/article', articleId]" -->
```
__"routerLinkActive"__ setzt eine CSS-Klasse wenn der entsprechende Link aktiv ist.



# Routing mit Sub-Routing aus Sub-Module

__app.module.ts__
```ts
const myRoutes: Routes = [
	{path: '', component: HomeComponent, pathMatch: 'full' }
	{path: 'submodule-b', component: SubmoduleNavigationComponent},
	{path: '**', component: My404Component}, // 404
];

// import submoduleb
imports: [
  SubmoduleBModule
]
```

__submoduleb.module.ts__ wichtig __forChild()__ verwenden
```
const submoduleRoutes: Routes = [
  {
    path: 'submodule-b', 
    component: SubmoduleNavigationComponent,
    children :[
      {path: 'b-1', component: B1Component}, // url: submodule-b/b-1
      {path: 'b-2', component: B2Component}  // url: submodule-b/b-2
    ]
  },
];
...
imports: [
  CommonModule,
  RouterModule.forChild(submoduleRoutes),
]
...
```

SubmoduleNavigationComponent:
```html
<h1>navigation submodule</h1>
<a routerLink="/submodule-b/b-1" routerLinkActive="active">B1</a>
<a routerLink="/submodule-b/b-1" routerLinkActive="active">B2</a>
<router-outlet></router-outlet>
```


# Routing Guard
...