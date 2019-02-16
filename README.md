# Angular
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

```javascript
//app.component.html
<input type="text" [(ngModel)]="name">
<p>{{name}}</p>

//app.components.ts
export class AppComponent {
  name = 'khaled';
}

//app.module.ts
import {FormsModule} from '@angular/forms';
  imports: [
    BrowserModule,
    FormsModule
  ],
```

### Install Bootstrap for css;

> npm install bootstrap@3

> however we need to make angular aware of that. angular.json and this line

```
  "styles": [
              "node_modules/bootstrap/dist/css/bootstrap.min.css",
              "src/styles.css"
            ],
 ```
