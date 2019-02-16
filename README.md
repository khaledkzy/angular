# Angular  -- Single Page Web App
> angular 1 = angular js

> angular 2 till angular 7 are similar but a bit of improvement.

> https://cli.angular.io/

```
sudo npm install -g @angular/cli
ng new my-dream-app
cd my-dream-app
ng serve
```

> app.component.html

> app-root is what get injected dynamically

> [(ngModel)] we will add it app.module.ts

```ts
/** app.component.html */
<input type="text" [(ngModel)]="name">
<p>{{name}}</p>

/** app.components.ts */
export class AppComponent {
  name = 'khaled';
}

/** app.module.ts */
import {FormsModule} from '@angular/forms';
  imports: [
    BrowserModule,
    FormsModule
  ],
```

### Install Bootstrap for css;

> npm install bootstrap@3

> however we need to make angular aware of that. angular.json and this line

```ts
  "styles": [
              "node_modules/bootstrap/dist/css/bootstrap.min.css",
              "src/styles.css"
            ],
 ```

### How apps starts

> `main.ts`

> `platformBrowserDynamic().bootstrapModule(AppModule)`

> which is the app.module.ts -> which then pass it   `bootstrap: [AppComponent]`

### components

> component needs an @ sign because that a component dicrator we then need to import it from '@angular/core'

> we can use the @Component() is know to typescript and then we can configure it.



```ts
import { Component } from '@angular/core';
/** selector is the html tag that allows you to use your components later
/* the name of the selector can be anything.
/* templateUrl: this will hold the html contnent of our component, this is in relative to
our ts file
*/
@Component({
  selector: 'app-server',
  templateUrl: './server.component.html'
})
export class ServerComponent {

}
```
> then we need to register it in the @NgModule `app.module.ts`

```ts
import { ServerComponent } from './server/server.component';
@NgModule({
  declarations: [
    AppComponent,
    ServerComponent,
  ]
```
> inside the app.component.html we can just add it to there with the name of the selector name that we declared earlier. <app-server></app-server>

> we can also create our component using

```ts
ng generate component servers
ng g c servers
```

> We still need to add them app.component.html somewhere

> We can also use template instead templateUrl and it is called inline
```ts
template: `<app-server></app-server><app-server></app-server>`
```

### Style

> styleUrls can take an array of different styles.

> you can also use styles: []

```ts
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  // styleUrls: ['./app.component.css']
  styles: [`
    h3 {
      color: dodgerblue;
    }
  `]
})
```
