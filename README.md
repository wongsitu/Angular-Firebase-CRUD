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