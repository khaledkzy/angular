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
### Attribute directives

> They don't add or remove they just change the element that they are added to.

![](/img/1.png)

```ts
export class ServerComponent {
  serverId: number = 10;
  serverStatus: string = 'offline';

  constructor() {
    this.serverStatus = Math.random() > 0.5 ? 'online' : 'offline';
  }

  getServerStatus() {
    return this.serverStatus;
  }

  getColor() {
    return this.serverStatus === 'online' ? 'green' : 'red';
  }
}
```

```html
<p
  [ngStyle]="{backgroundColor: getColor()}"
  [ngClass]="{online: serverStatus === 'online'}">
  {{ 'Server' }} with ID {{ serverId }} is {{ getServerStatus() }}
</p>
```
> [ngClass] takes the key value of the name of the css class and the values are the coditions


### *ngFor

> *ngFor="let server of servers" ... servers is the array we created in the TS file. But Server can be named anything.

```ts
// *ngFor="let anything of servers"
*ngFor="let server of servers"
```

### Excersise 1

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  showSecret = false;
  log = [];

  onToggleDetails() {
    this.showSecret = !this.showSecret;
    // this.log.push(this.log.length + 1);
    this.log.push(new Date());
  }
}
```

```html
<div class="container">
  <div class="row">
    <div class="col-xs-12">
      <ol>
        <li>Add A button which says 'Display Details'</li>
        <li>Add a paragraph with any content of your choice (e.g. 'Secret Password = tuna')</li>
        <li>Toggle the displaying of that paragraph with the button created in the first step</li>
        <li>Log all button clicks in an array and output that array below the secret paragraph (maybe log a timestamp or simply an incrementing number)</li>
        <li>Starting at the 5th log item, give all future log items a blue background (via ngStyle) and white color (ngClass)</li>
      </ol>
      <hr>
      <!--<button class="btn btn-primary" (click)="showSecret = !showSecret">Display Details</button>-->
      <button class="btn btn-primary" (click)="onToggleDetails()">Display Details</button>
      <p *ngIf="showSecret">Secret Password = tuna</p>
      <div
        *ngFor="let logItem of log; let i = index"
        [ngStyle]="{backgroundColor: i >= 4 ? 'blue' : 'transparent'}"
        [ngClass]="{'white-text': i >= 4}"
      >{{ logItem }}</div>
    </div>
  </div>
</div>
```

> Important: we can extract the index from the *ngFor

```
*ngFor="let logItem of log; let i = index"
```

### Debuging

> webpack and then the dot folder has all the TypeScript

> Augury is an addon for Angular

### Property and event binding

> pass from child to parent


```
  @Input('srvElement') element: {type: string, name: string, content: string};

```

> pass from parent to cild

```
@Output() serverCreated = new EventEmitter<{serverName: string, serverContent: string}>();
  @Output('bpCreated') blueprintCreated = new EventEmitter<{serverName: string, serverContent: string}>();
```


### View Encapsulation

> Similar to the shadow-dom, angular assign a different className fore every element.

> You can also edit it by chossing one of the three modes, emulated is the angular default, and None is nothing and Native is the shadow-dom that comes with some browsers.

```ts
@Component({
  // selector: 'app-server-element',
  // templateUrl: './server-element.component.html',
  // styleUrls: ['./server-element.component.css'],
  encapsulation: ViewEncapsulation.Emulated // None, Native
})
```

### Local Reference

> You can use them only inside the template and not the TypeScript Code

> `#serverNameInput` and #serverContentInput are out local reference

> using @ViewChild can be used to access local reference

> make sure you don't set the value to anything.
```ts
serverName: nameInput.value,
serverContent: this.serverContentInput.nativeElement.value
```

### Full example

```html
  <div class="col-xs-12">
    <p>Add new Servers or blueprints!</p>
    <label>Server Name</label>
    <!--<input type="text" class="form-control" [(ngModel)]="newServerName">-->
    <input
      type="text"
      class="form-control"
      #serverNameInput>
    <label>Server Content</label>
    <!--<input type="text" class="form-control" [(ngModel)]="newServerContent">-->
    <input
      type="text"
      class="form-control"
      #serverContentInput>
    <br>
    <!-- we need to make sure to pass it to onAddServer( )  like this onAddServer(serverNameInput)-->
    <button
      class="btn btn-primary"
      (click)="onAddServer(serverNameInput)">Add Server</button>
      <!-- onAddBlueprint(serverNameInput) -->
    <button
      class="btn btn-primary"
      (click)="onAddBlueprint(serverNameInput)">Add Server Blueprint</button>
  </div>
</div>
```
```ts

export class CockpitComponent implements OnInit {
  @Output() serverCreated = new EventEmitter<{serverName: string, serverContent: string}>();
  @Output('bpCreated') blueprintCreated = new EventEmitter<{serverName: string, serverContent: string}>();
  // this is commented because now we are using the local reference that we pass from
  //the template.
  // newServerName = '';
  // newServerContent = '';
  @ViewChild('serverContentInput') serverContentInput: ElementRef;

  constructor() { }

  ngOnInit() {
  }

// onAddServer() {
  onAddServer(nameInput: HTMLInputElement) {
    this.serverCreated.emit({
   // serverName: this.newServerName,
      serverName: nameInput.value,
      serverContent: this.serverContentInput.nativeElement.value
    });
  }

  onAddBlueprint(nameInput: HTMLInputElement) {
    this.blueprintCreated.emit({
      // serverName: this.newServerName,
      serverName: nameInput.value,
      serverContent: this.serverContentInput.nativeElement.value
    });
  }

}
```


### Special Directive <ng-content></ng-content>

> server-element.component.html

```html
<div
  class="panel panel-default">
  <!--<div class="panel-heading">{{ element.name }}</div>-->
  <div class="panel-heading" #heading>{{ name }}</div>
  <div class="panel-body">
    <ng-content></ng-content>
  </div>
</div>

```

```html
<div class="row">
    <div class="col-xs-12">
      <div
        class="panel panel-default"
        *ngFor="let element of serverElements">
        <div class="panel-heading">{{ element.name }}</div>
        <div class="panel-body">
          <p>
            <strong *ngIf="element.type === 'server'" style="color: red">{{ element.content }}</strong>
            <em *ngIf="element.type === 'blueprint'">{{ element.content }}</em>
          </p>
        </div>
      </div>
    </div>
```
> however if want to move that code to the **app.component.html**

```html
<div class="container">
  <app-cockpit
    (serverCreated)="onServerAdded($event)"
    (bpCreated)="onBlueprintAdded($event)"
  ></app-cockpit>
  <hr>
  <div class="row">
    <div class="col-xs-12">
      <button class="btn btn-primary" (click)="onChangeFirst()">Change first Element</button>
      <button class="btn btn-danger" (click)="onDestroyFirst()">Destroy first Component</button>
      <app-server-element
        *ngFor="let serverElement of serverElements"
        [srvElement]="serverElement"
        [name]="serverElement.name">
        <p #contentParagraph>
          <strong *ngIf="serverElement.type === 'server'" style="color: red">{{ serverElement.content }}</strong>
          <em *ngIf="serverElement.type === 'blueprint'">{{ serverElement.content }}</em>
        </p>
      </app-server-element>
    </div>
  </div>
</div>
```


### LifeCycles

![](/img/3.png)

> Practice Example

```ts
import {
  Component,
  OnInit,
  Input,
  ViewEncapsulation,
  OnChanges,
  SimpleChanges,
  DoCheck,
  AfterContentInit,
  AfterContentChecked,
  AfterViewInit,
  AfterViewChecked,
  OnDestroy,
  ViewChild,
  ElementRef,
  ContentChild
} from '@angular/core';

@Component({
  selector: 'app-server-element',
  templateUrl: './server-element.component.html',
  styleUrls: ['./server-element.component.css'],
  encapsulation: ViewEncapsulation.Emulated // None, Native
})
export class ServerElementComponent implements
  OnInit,
  OnChanges,
  DoCheck,
  AfterContentInit,
  AfterContentChecked,
  AfterViewInit,
  AfterViewChecked,
  OnDestroy {
  @Input('srvElement') element: {type: string, name: string, content: string};
  @Input() name: string;
  @ViewChild('heading') header: ElementRef;
  @ContentChild('contentParagraph') paragraph: ElementRef;
  constructor() {
    console.log('constructor called!');
  }
  ngOnChanges(changes: SimpleChanges) {
    console.log('ngOnChanges called!');
    console.log(changes);
  }
  ngOnInit() {
    console.log('ngOnInit called!');
    console.log('Text Content: ' + this.header.nativeElement.textContent);
    console.log('Text Content of paragraph: ' + this.paragraph.nativeElement.textContent);
  }
  ngDoCheck() {
    console.log('ngDoCheck called!');
  }
  ngAfterContentInit() {
    console.log('ngAfterContentInit called!');
    console.log('Text Content of paragraph: ' + this.paragraph.nativeElement.textContent);
  }
  ngAfterContentChecked() {
    console.log('ngAfterContentChecked called!');
  }
  ngAfterViewInit() {
    console.log('ngAfterViewInit called!');
    console.log('Text Content: ' + this.header.nativeElement.textContent);
  }
  ngAfterViewChecked() {
    console.log('ngAfterViewChecked called!');
  }
  ngOnDestroy() {
    console.log('ngOnDestroy called!');
  }
}
```

# PROJECT 2 - shopping

### Planning

![](/img/2.png)

```
ng new shopping
cd shopping
npm install bootstrap@3
// then add bootstrap to the styles
ng serve
```

> Adding components

```
ng g c header
ng g c recipes
ng g c recipes/recipe-list
ng g c recipes/recipe-detail
ng g c recipes/recipe-detail/recipe-list
ng g c shopping-list
ng g c shopping-list/shopping-edit
```

> inside app.component.html

```html
<app-header></app-header>
<div class="container">
  <div class="row">
    <div class="col-md-12">
      <app-recipes></app-recipes>
      <app-shopping-list></app-shopping-list>
    </div>
  </div>
</div>
```

> We want to display both the list and details next to each other
```
<div class="row">
  <div class="col-md-5">
    <app-recipe-list></app-recipe-list>
  </div>
  <div class="col-md-7">
    <app-recipe-detail></app-recipe-detail>
  </div>
</div>
```

> then we create a model

> recipe.model.ts

```ts
export class Recipe {
  public name: string;
  public description: string;
  public imagePath: string;

  constructor(name: string, desc: string, imagePath: string) {
    this.name = name;
    this.description = desc;
    this.imagePath = imagePath;
  }
}
```

```ts
export class Ingredient {
  constructor(public name: string, public amount: number) {}
}
```

