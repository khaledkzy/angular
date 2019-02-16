# Angular

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

> you can change the selectors you can use by attribute and you can also use by class

```ts
// servers.component.ts
@Component({
  // selector: '[app-servers]',
  // selector: '.app-servers',
  selector: 'app-servers',
  // template: `
  //   <app-server></app-server>
  //   <app-server></app-server>`,
  templateUrl: './servers.component.html',
  styleUrls: ['./servers.component.css']
})

// app.component.html
      <app-servers></app-servers>
      <!--<div app-servers></div>-->
      <!--<div class="app-servers"></div>-->
```

### string interpolatin
> you can also use string interpolatin but at the end you must get something that convert to string.

```
<p>{{ 'Server' }} with ID {{ serverId }}  is {{ getServerStatus()}} and {{ serverStatus }}</p>
```
```ts
export class ServerComponent {
  serverId: number = 10;
  serverStatus: string = 'offline';
  getServerStatus() {
    return this.serverStatus;
  }
}
```

### property binding and event binding and two way binding

 > add a new property to that holds a boolean with false
 > property binding []
> event binding ()

> (click) event give us information about the event. the **$** and pass it the event

> $event

>

>
 ```ts
// server.components
export class ServersComponent implements OnInit {
// this is just a property that hold a boolean
  allowNewServer = false;
  //
  serverCreationStatus = 'No server was created!';
  serverName = 'Testserver';

// after 2 seconds then change that property to true
  constructor() {
    setTimeout(() => {
      this.allowNewServer = true;
    }, 2000);
  }

  ngOnInit() {
  }

    // the on at the begginig means the user will click
  onCreateServer() {
    this.serverCreationStatus = 'Server was created! Name is ' + this.serverName;
  }

  onUpdateServerName(event: Event) {
    this.serverName = (<HTMLInputElement>event.target).value;
  }
}
 ```

 ```html
 <!-- servers.component.html -->
 <!-- [disabled] is the property binding by inclosing square brackets  -->
 <!-- this indicates to angular the we need to use binding -->
<!-- (click) is the event binding and then pass it the method  -->
<label>Server Name</label>
<!-- <input
  type="text"
  class="form-control"
  (input)="onUpdateServerName($event)"> -->
  <!-- <p>{{ serverName }}</p> -->
<input
  type="text"
  class="form-control"
  [(ngModel)]="serverName">
<p>{{ serverName }}</p>
<button
  class="btn btn-primary"
  [disabled]="!allowNewServer"
  (click)="onCreateServer()">Add Server</button>
<!--<p [innerText]="allowNewServer"></p>-->
<p>{{ serverCreationStatus }}</p>
<app-server></app-server>
<app-server></app-server>

 ```

> this is a recognized expression that angular understands `[innerText]="allowNewServer"`

> to change the property of an html element use preperty binding

> to output text then use string interpolation

```
<p [innerText]="allowNewServer"></p>
<p>{{allowNewServer}}</p>
```

### Two way data binding

> you combine [ ] and () using the ngModel

```html
<input
  type="text"
  class="form-control"
  [(ngModel)]="serverName">
<p>{{ serverName }}</p>
```

### Directives

> ***ngIf**

> *ngIf="serverCreated" any expression that returns true or false

```html
<label>Server Name</label>
<!-- <input
  type="text"
  class="form-control"
  (input)="onUpdateServerName($event)"> -->
<input
  type="text"
  class="form-control"
  [(ngModel)]="serverName">
<p>{{ serverName }}</p>
<button
  class="btn btn-primary"
  [disabled]="!allowNewServer"
  (click)="onCreateServer()">Add Server</button>
<!--<p [innerText]="allowNewServer"></p>-->
<p *ngIf="serverCreated">Server was created, server name is {{ serverName }} ---> ngIf with serverCreated and serverName </p>
<p>{{ serverCreationStatus }} ---> serverCreationStatus </p>
<app-server></app-server>
```

```ts
// this will be added to the ts
serverCreated = false;
```

> we can use ngIf if else however we don't need to that all time

```html
<p *ngIf="serverCreated; else noServer">Server was created , server name is {{ serverName }} ---> ngIf  and else statement </p>
<ng-template #noServer>
  <p>
    No server was added
  </p>
</ng-template>
```
![](/img/1.png)
### Attribute directives

> They don't add or remove they just change the element that they are added to.


