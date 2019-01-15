## Angular Installation and App Creation
In terminal run: 

```terminal
npm install -g @angular/cli
ng new my-app
cd my-app
code .
ng serve --open
```

## Angular Component/Service/Class creation

To create components/services/classes:

```terminal
ng g c <component-name>
ng g s <service-name>
ng g cl <class-name>
```

In Angular, services are methods that are available to all components. These could be simple functions to api requests. On the other hand, classes are normally used as models ( like Schemmas in NodeJS)

You could also create a component/service/class inside a new folder like this:

```terminal
ng g c <folder-name>/<component-name>
```

For example, the following line will create a component products inside the folder components:

```terminal
ng g c components/products
```

To let Angular services to be used by other components, assuming that you've created a service inside a services folder, go to app.module.ts and import the following:

```javascript
import { NameService } from './services/name.service'
```

In the same file, inside providers array, add the name of the imported service:

```javascript
providers: [
    NameService,
]
```

## Implementing Bootstrap
First npm install bootstrap with the line below, this will install bootstrap in our node_modules:

```terminal
npm install --save bootstrap
```

then go to angular.json file and add "../node_modules/bootstrap/dist/css/bootstrap/dist/css/bootstrap.min.css" inside styles. It should look like this:

```json
"styles":[
    "../node_modules/bootstrap/dist/css/bootstrap/dist/css/bootstrap.min.css",
    "styles.css"
]
```

## Data binding 

1. String Interpolation (Curly Braces)
Similar to Python templating, Angular uses {{ some.variable.json.returned.from.backend}}. For example:

```html
<h1>Hello my name is {{ json.data.name }}</h1>
```

You can also call a method:

```html
<h1>Hello my name is {{ getServerStatus() }}</h1>
```

2. Property Binding (Square brakets)
Say we got a disable button, and we want to enable it after 2 seconds. Say we have a server.component.ts and inside it we have:

```typescript
import { Component } from '@angular/core';

@Component({
    selector: 'app-server',
    templateUrl: './server.component.html',
    styleUrls: ['./server.component.scss']
})
export class ServersComponent implements OnInit{
    allowNewServer = false

    constructor(){
        setTimeOut(()=>{
            this.allowNewServer = true;
        }, 2000);
    }

    ngOnInit(){

    }
}
```
To allow it to be clickable after 2 secs, in our server.component.html:

```html
<button class="btn btn-primary" [disable]="!allowNewServer">Allow to be clickable</button>
<!-- You can also change text of tag-->
<p [innertext]="allowNewServer"></p>
<!-- Syntax is [attribute_of_tag]=>"method()/variable" -->
```

3. Event Binding (Parenthesis)
Works as an event listener. say we add a onCreate() method to out server component:

```typescript
import { Component } from '@angular/core';

@Component({
    selector: 'app-server',
    templateUrl: './server.component.html',
    styleUrls: ['./server.component.scss']
})
export class ServersComponent implements OnInit{
    allowNewServer = false;
    serverCreationStatus = "No server was created";

    constructor(){
        setTimeOut(()=>{
            this.allowNewServer = true;
        }, 2000);
    }

    ngOnInit(){

    }

    onCreate(){
        return this.serverCreationStatus = "server was created";
    }
}
```

In out html component then:

```html
<button class="btn btn-primary" [disable]="!allowNewServer" (click)="onCreate()">Allow to be clickable</button>
<!-- Syntax is: (event)=>"methodToRun()" -->
```
Other events can be (click), (keyup), (keyup.enter), (blur), etc

Important: 
```html
<input type="text" class="form-control" (input)="onUpdate($event)">
```
Like React, $event represents the letters we press in the keyboard. This is a reserved word that gives us access to reserved data. In this example, event is type Event and to access it in out component.ts, we can use dot notation: (<HTMLInputElement>event.target).value

4. Two way Binding (Square Braket and Parenthesis)
To use this, first we need to import the module in our compoment.ts:

```typescript
import { FormsModule } from '@angular/forms'
```
Binds in both directions from component to html and html to component. The syntax is:

```html
<button class="btn btn-primary" [(ngModel)]="serverName">Allow to be clickable</button>
```

## Directives
They are instructions for the DOM. For example:

```html 
<p appTurnGreen>Recieves a Green Background!</p>
```

```typescript
@Directive({
    selector:'[appTurnGreen]'
})

export class TurnGreenDirective{
    ...
}
```

### ngIf Directive
Like in Python/Django we can do some template logic, except that the if statement goes inside a html tag:

```html
<!-- If statement equivalent -->
<p *nfIf="contidional">

</p>
```
If/else equivalent:

```html
<!-- If/else statement -->
<p *nfIf="contidional; else elseBlock">
    <!-- code goes here -->
</p>
<ng-template #elseBlock>
    <!-- code goes here -->
</ng-template>
```

### ngStyle Directive
Say we want to change the color of a button depeding on server status, to do that we use ngStyle:

```typescript
import { Component } from '@angular/core';

@Component({
    selector: 'app-server',
    templateUrl: './server.component.html',
    styleUrls: ['./server.component.scss']
})
export class ServersComponent implements OnInit{
    serverId: number = 10
    serverStatus: string = 'offline'

    constructor(){
        this.serverStatus = Math.random() > 0.5 ? 'online' : 'offline';
    }

    getServerStatus(){
        return this.serverStatus;
    }

    getColor(){
        return this.serverStatus === "online" ? "green" : "red"
    }
}
```

```html
<p [ngStyle]="{backgroundColor: getColor()}">This is button</p>
```

ngStyle allows us to update the style of a html tag dynamically.

### ngClass
The same way, we can add or remove classes dynamically. Using the same script previously, lets add a class .online in our server.component.css

```CSS
.active{
    color: white;
}
```

```html
<p [ngClass]="{'active': serverStatus === 'online'}" ][ngStyle]="{backgroundColor: getColor()}">This is button</p>
```

```typescript
import { Component } from '@angular/core';

@Component({
    selector: 'app-server',
    templateUrl: './server.component.html',
    styleUrls: ['./server.component.scss']
})
export class ServersComponent implements OnInit{
    serverId: number = 10
    serverStatus: string = 'offline'

    constructor(){
        this.serverStatus = Math.random() > 0.5 ? 'online' : 'offline';
    }

    getServerStatus(){
        return this.serverStatus;
    }

    getColor(){
        return this.serverStatus === "online" ? "green" : "red"
    }
}
```

ngClass updates a class only if a condition is met.

### ngFor
For loop in Angular templating. Syntax:

```html
<div class="parent" *ngFor="let server of servers">
```

You can also add the component itself. Say we are working on app-server component, you can do:

```html
<app-server *ngFor="let server of servers"></app-server>
```

### Getting index when using ngFor
Using the previous loop, we can get the index of the current server doing:

<div class="parent" *ngFor="let server of servers; let i = index">
```

Here, index is a reserved word, not a taco. 

## Basics of typescript


## Angular-Firebase Integration
In terminal run:

```
npm install firebase angularfire2
```

To connect to firebase, you'll need to go to https://console.firebase.google.com/?pli=1 and log in with your google account.

Let's add a database on firebase. Click on add project and write your project name on it. Once created, click on continue. Then click on the </> Logo. This will give you a config variable. Then go to your enviroment.ts and add your config json object. Like this:

```javascript
export const environment = {
    production: false,
    firebase: {
        apiKey: "XXXXXXXXXXXXXXXXXX",
        authDomain: "projectId.firebaseapp.com",
        databaseURL: "https://projectId.firebaseio.com",
        projectId: "projectId",
        storageBucket: "projectId.appspot.com",
        messagingSenderId: "123456789"
    }
};
```
Then in firebase website, click on database and for purposes of this tutotial, we will enable a Realtime Database. So just click enable. To allow CRUDing on the top of the navbar click on Rules and change read and write values to true.

```javascript
{
    "rules": {
        ".read": true,
        ".write": true
    }
}
```

Then click on Publish. Now let's configure our project. Go to app/app.module.ts and write the following:

```javascript
// import firebase at the top of the file
import { AngularFireModule} from 'angularfire2'
import { AngularFireDatabaseModule} from 'angularfire2/database'
import { environment } from '../environments/environment'
```

In imports add those modules, it should look like this:

```javascript
imports: [
    BrowserModule,
    AppRoutingModule,
    AngularFireDatabaseModule,
    AngularFireModule.initializeApp(environment.firebase),
],
```

Congratulations! now you application is connected to firebase. Some of you might of might not have AppRoutingModule, that depends on how you set up you angular application at the beginning. Just don't worry about it for now.