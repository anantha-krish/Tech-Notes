Basics

## Setup
Updating NodeJS:

Go to nodejs.org and download the latest version - uninstall (all) installed versions on your machine first.

Updating npm:

Run [sudo] npm install -g npm  (sudo  is only required on Mac/ Linux)

Updating the CLI
```
[sudo] npm uninstall -g angular-cli @angular/cli 

npm cache verify 

[sudo] npm install -g @angular/cli 
```
Here are some common issues & solutions:


## How Angular loads your component

Ex: app-root tag should load AppComponent
```html
<body>
  <app-root></app-root>
</body>
```

1. Angular first checks main.ts file

```ts
platformBrowserDynamic().bootstrapModule(AppModule)
//this informs there is an AppModule
```

2. Then it checks the app.module.ts

```ts
  bootstrap: [AppComponent],
      //it will find a list of bootstrap array
```
3. It will Scan the App component

```js
@Component({
    //finds app-root
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
})
export class AppComponent {}

```
It will then inject corresponding template

Adding new component

```
ng generate component server

or 

ng g c server
```

confirm the component was added to module
```js
 declarations: [AppComponent, ServerComponent],
```  
## Selector tags

```
//as element
selector: 'app-root'

Html -> <app-root></app-root>

//as attribute
selector: '[app-root]'

Html -> <div app-root></div>

//as class
selector: '.app-root'

Html -> <div class="app-root"></div>
```


## String Interpolation & Property binding

```html
<!-- // String Interpolation-> data is converted to string  using {{data}} -->
<div>
  Hi this is server no: {{ serveNum }} now
  {{ isOffline ? "Offline" : "Online" }}
</div>
<!-- //property binding -> [attribute] = "data" -->
<button class="btn-primary" [disabled]="isOffline">Add Server</button>

```


## Event Binding

```jsx
<!-- //event binding -> (eventType) = "method()" -->
<button class="btn-primary" (click)="onCreateServer()">Create Server</button>
<br />
{{ serverCreationStatus }}

//component.ts
export class ServerComponent implements OnInit {
  serveNum: number = 10;
  isOffline: boolean = false;
  serverCreationStatus: string = 'No server was created';
  constructor() {
    setTimeout(() => {
      this.isOffline = true;
    }, 5000);
  }

  ngOnInit(): void {}

  onCreateServer() {
    this.serverCreationStatus = 'Server has been created successfully!';
  }
}

```
## Passing Event in methods
// right approach of ignore target value error
```jsx
  <!-- //event binding with passing event -> (eventType) = "function($event)" -->
  <input type="text" (input)="onUpdateServerName($event)" />
  {{ serverName }}


 onUpdateServerName(event: any) {
    this.serverName = (<HTMLInputElement>event.target).value;
  }
```            
## Two way binding
Here we will demonstrate using ngModel directive

step1 : import FormsModule into app.module.ts
```js
@NgModule({
  declarations: [AppComponent, ServerComponent],
  //example
    imports: [BrowserModule, FormsModule],
 ``` 
step 2: restart the server


step 3: Implement in template
```jsx
<!-- // One -way binding event binding with passing event -> (eventType) = "function($event)" -->
<div>
  <label for="one-way-input">One way Input</label>
  <input id="one-way-input" type="text" (input)="onUpdateServerName($event)" />
</div>

<!-- //Two way binding by using directive-> [(ngModel)] = "data" -->
<div>
  <label for="two-way-input">Two way Input</label>
  <input id="two-way-input" type="text" [(ngModel)]="serverName" />
</div>

Ouptut: {{ serverName }}
```
### If we enter in one-way bind
![](image-khbiotzp.png)


### If we enter in two way bind
![](image-khbiseis.png)
    
## Directives
Directives are instructions in DOM

Structural Directive

starts with * : Either it will be present or not present in DOM

Ex: *ngIf

```jsx
<p *ngIf="isServerCreated">{{ serverCreationStatus }}</p>
```

Ex2: if & else condition
```jsx
<p *ngIf="isServerCreated; else noServer">{{ serverCreationStatus }}</p>
<ng-template #noServer>
  <p>No server was created !!</p>
</ng-template>
```

### Dynamic Class binding
ng class accepts key value pair where key is classname and  valuew is boolean expression

```jsx
<p   
  [ngClass]="{ online: isServerCreated }"
  *ngIf="isServerCreated; else noServer"
>
  {{ serverCreationStatus }}
</p>
```
### *ngFor/ for loop in angular
```html
<p *ngFor="let server of servers">Server added: {{ server }}</p>
```

## Components Communication

Passing Properties to child
```jsx
//child ts
@Input() element: { type: string; name: string; content: string };

//parent ts 
serverObj = { type: 'server', name: 'Test Server', content: 'just Test' }

//parent html
<app-child [element]="serverObj"></app-child>
```

Passing data to Parent component

can only be achieved by parent
```ts
//child component setup

// create event emitter objects
 @Output() serverCreated = new EventEmitter<{
    serverName: string;
    serverContent: string;
  }>();

  @Output() blueprintCreated = new EventEmitter<{
    serverName: string;
    serverContent: string;
  }>();

//emit the data to pass
  onAddServer() {
    this.serverCreated.emit({
      serverName: this.newServerName,
      serverContent: this.newServerContent,
    });
  }

  onAddBlueprint() {
    this.blueprintCreated.emit({
      serverName: this.newServerName,
      serverContent: this.newServerContent,
    });
  }
}


// Parent setup

//html
<app-cockpit
(serverCreated)="onServerAdded($event)"
(blueprintCreated)="onBlueprintAdded($event)">
</app-cockpit>


// ts
onServerAdded(serverData: { serverName: string; serverContent: string }) {
this.serverElements.push({
  type: 'server',
  name: serverData.serverName,
  content: serverData.serverContent,
});
}

onBlueprintAdded(serverData: { serverName: string; serverContent: string }) {
this.serverElements.push({
  type: 'blueprint',
  name: serverData.serverName,
  content: serverData.serverContent,
});
}
```
## View Encapsulation

Angular by default provides view encapsulation,
only css linked to the component will be able to style the component. No other style from other components will be applied.

by Default
```ts
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
  encapsulation: ViewEncapsulation.Emulated //default & optional
})

// if we want component to apply styles from other components
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
  encapsulation: ViewEncapsulation.None
})
```