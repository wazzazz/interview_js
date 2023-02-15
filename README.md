# CRA vs Vite vs Astro/Gatsby/Next.js

Vite is much more faster then CRA in developer and production mode. Ready out of the box for bunch of features (typescript, postcss). Cons: not for SSR
Astro/Gatsby great for SSG (Static Site Generator or prerender. Blog or etc.). Astro can work not only with React and maybr more easier.
Next.js great for SSR in React and for huge projects.


# Service worker/ PWA

### Service worker is a heart of PWA. As a standalone worker can intersect all requests, used for cache assets to work better with low internet. Works in parallels and use only HTTPS. Also similar to middleware


# Nginx (EngineEx)

+ Proxy the traffic
+ Scales quickly
+ Light weight/Speed
+ Host Web Sites
+ Use it as a proxy server
+ COnfiguring several virtual hosts.

Is an open-source web server that, since its initial success as a web server, is now also used as a reverse proxy, HTTP cache, and load balancer.

# Event Loop, Hoisting, Scope, JS Engine, Prototypal Ingeritance, Generators and Iterators, Promises (microtasks + macrostasks)

### Event Loop https://dev.to/lydiahallie/javascript-visualized-event-loop-3dif
### Hoisting https://dev.to/lydiahallie/javascript-visualized-hoisting-478h
### Scope https://dev.to/lydiahallie/javascript-visualized-scope-chain-13pd
### JS Engine https://dev.to/lydiahallie/javascript-visualized-the-javascript-engine-4cdf
### Prototypal Ingeritance https://dev.to/lydiahallie/javascript-visualized-prototypal-inheritance-47co
### Generators and Iterators https://dev.to/lydiahallie/javascript-visualized-generators-and-iterators-e36
### Promises (microtasks + macrostasks) https://dev.to/lydiahallie/javascript-visualized-promises-async-await-5gke

# Private vs '#'

Without typescript we can use # (this.#counter++) in order to prevent access outside the class (through instance)

# SOLID

### Single responsibility
#### A class should do one thing and therefore it should have only a single reason to change

<details>
  <summary>Code example</summary>
      
```typescript
  
// Single Responsibility Principle
class HttpClient {
  get(url: string) {}
  post() {}
  put() {}
  delete() {}
}

class UserService {
  client: HttpClient;
  constructor(client) {
      this.client = client;
  }
  getOneUser(id: number) {}
  getAllUsers() {}
}

class RequisitesService {
  createRequisites() {}
  getRequisites() {}
  updateRequisites() {}
} 
 
```
</details>

### Open-closed principle
#### Classes should be open for extension and closed to modification.
Need to write code which doesn't impact on others parts of existing code. e.g. Reduce cohesion and if clauses.

<details>
  <summary>Code example</summary>
  
 ```typescript

interface Attacker {
  attack: () => void;
}
class Weapon implements Attacker {
  damage: number; // 0 - 100;
  range: number; // 0 - 100;

  constructor( damage: number, range: number) {
    this.damage = damage;
    this.range = range;
  }

  attack() {}
}

class Sword extends Weapon {
  attack() {
    console.log('Удар мечом с уроном ' + this.damage)
  }
}

class Crossbow extends Weapon {
  attack() {
    console.log('Выстрел из арбалета с уроном ' + this.damage)
  }
}

class Knife extends Weapon {
  attack() {
    console.log('Удар ножом с уроном ' + this.damage)
  }
}

class Character {
  name: string;
  weapon: Weapon;

  constructor(name: string, weapon: Weapon) {
    this.name = name;
    this.weapon = weapon;
  }

  changeWeapon(newWeapon: Weapon) {
    this.weapon = newWeapon;
  }

  attack() {
    this.weapon.attack();
  }
}

const sword = new Sword(15, 2);
const character = new Character('Warrior', sword);
character.attack()

const crossbow = new Crossbow(40, 100);
character.changeWeapon(crossbow);
character.attack()
 
```
</details>

### Liskov substitution
#### Subclasses should be substitutable for their base classes.

Inheritable class must add and not replace class

<details>
  <summary>Code example</summary>
      
```typescript

 class Database {
  connect() {}
  read() {}
  write() {}
}

class SQLDatabase extends Database {
  connect() {}
  read() {}
  write() {}
  joinTables() {}
}

class NOSQLDatabase extends Database {
  connect() {}
  read() {}
  write() {}
  createIndex() {}
}

class MySQLDatabase extends SQLDatabase {
  connect() {}
  read() {}
  write() {}
  joinTables() {}
}

class MongoDatabase extends NOSQLDatabase {
  connect() {}
  read() {}
  write() {}
  createIndex() {}
  mergeDocuments() {}
}


function startApp(database: Database) {
  database.connect()
}
startApp(new MongoDatabase())
startApp(new MySQLDatabase())

  
```
</details>

### Interface segregatinon
#### Segregation means keeping things separated, and the Interface Segregation Principle is about separating the interfaces

Similar to LSP. Interface mustn't have method thad he couldn't realise

<details>
  <summary>Code example</summary>
      
```typescript
  
interface HttpRequest {
  get: () => void;
  post: () => void;
  put: () => void;
  delete: () => void;
  
  addToken: () => void; // need to move to another interface, because not of clients need it.
}
  
```
</details>

### Dependency inversion
#### Our classes should depend upon interfaces or abstract classes instead of concrete classes and functions.

<details>
  <summary>Code example</summary>
      
```typescript

interface MusicApi {
  getTracks: () => void;
}

class YandexMusicApi implements MusicApi {
  getTracks(): void {}
}

class SpotifyApi implements MusicApi {
  getTracks(): void {}
}

class VKMusicApi implements MusicApi {
  getTracks(): void {}
}

class MusicClient implements MusicApi {
  client: MusicApi;

  constructor(client: MusicApi) {
    this.client = client;
  }

  getTracks() {
    this.client.getTracks();
  }
}

const MusicApp = () => {
  const API = new MusicClient(new SpotifyApi())

  API.getTracks()
}


interface HttpClient {

}


class Axios implements HttpClient {
  request() {
    fetch
    XMLHttpRequest()
    node-fetch
    node http module
  }
}


```
</details>

# Webpack, Gulp, CommonJs vs ES Modules

A module system allows us to split up our code in different parts or to include code written by other developers.
Since the very beginning of NodeJS, the CommonJS module system is the default module system within the ecosystem. However, recently a new module system was added to NodeJS - ES modules.

https://reflectoring.io/nodejs-modules-imports/ <br />
https://medium.com/nuances-of-programming/%D0%B2%D0%B2%D0%B5%D0%B4%D0%B5%D0%BD%D0%B8%D0%B5-%D0%B2-webpack-%D0%B4%D0%BB%D1%8F-%D0%BD%D0%BE%D0%B2%D0%B8%D1%87%D0%BA%D0%BE%D0%B2-6cafbf562386 <br />
https://rdevelab.ru/blog/no-category/post/typescript-build-gulp-webpack-and-lint-with-eslint#anchor-babel-configuration <br />
https://medium.com/computed-comparisons/commonjs-vs-amd-vs-requirejs-vs-es6-modules-2e814b114a0b <br />

# JS simple tasks and codes

https://github.com/lydiahallie/javascript-questions <br />
https://github.com/sudheerj/javascript-interview-questions <br />
https://github.com/sudheerj/javascript-interview-questions#coding-exercise <br />
https://github.com/learning-zone/javascript-interview-questions <br />
https://github.com/Adespinoza/javascript-interview-cheatsheet <br />

# JS - 2 reactive examples

<details>
  <summary>Code example - start issue 1</summary>
      
```javascript
  
/* 1. Init obj with defineProperty instructions */
/* 2. When first autoRun start, runningReaction becomes callback function with console.log and obj.a assigning */
/* 3. After assigning get operator do its work and save fn instruction for key "a" */
/* 4. The same for autoRun for obj.b */
/* PROFIT (works on closure and global variable) */
let runningReaction = null;

const obj = reactive({
  a: 0,
  b: 1,
});

autoRun(() => {
  console.log("obj.a reactive", obj.a);
});

autoRun(() => {
  console.log("obj.b reactive", obj.b);
});

function reactive(obj) {
  return Object.entries(obj).reduce((acc, [key, val]) => {
    let value = val;
    const deps = new Set();
    Object.defineProperty(acc, key, {
      get() {
        if (runningReaction && !deps.has(runningReaction)) {
          deps.add(runningReaction);
        }
        return value;
      },
      set(newValue) {
        if (hasChanged(value, newValue)) {
          value = newValue;
          deps.forEach(f => f());
        }
      },
      enumerable: true,
    });
    return acc;
  }, {});
}

function hasChanged(newVal, oldVal) {
  return newVal !== oldVal && (newVal === newVal || oldVal === oldVal);
}

function autoRun(fn) {
  runningReaction = fn;
  fn();
  runningReaction = null;
}

obj.a = 6;
obj.b = 10;
```
</details>

<details>
  <summary>Code example - start issue 2</summary>
      
```javascript
  
class Reactive {
  constructor (obj) {
    this.contents = obj;
    this.listeners = {};
    this.makeReactive(obj);
  }
  makeReactive(obj) {
    Object.keys(obj).forEach(prop => this.makePropReactive(obj, prop));
  }

  makePropReactive(obj, key) {
    let value = obj[key];

    // Gotta be careful with this here
    const that = this;

    Object.defineProperty(obj, key, {
        get () {
          return value;
        },
        set (newValue) {
          value = newValue;
          that.notify(key)
        }
    });
  }

  listen(prop, handler) {
    if (!this.listeners[prop]) this.listeners[prop] = [];

    this.listeners[prop].push(handler);
  }

  notify(prop) {
    this.listeners[prop].forEach(listener => listener(this.contents[prop]));
  }

}

const data = new Reactive({
  foo: 'bar'
});

data.listen('foo', (change) => console.log('Change: ' + change));

data.contents.foo = 'baz';

```
</details>

# Object-oriented programming

Object Oriented programming (OOP) is a programming paradigm that relies on the concept of classes and objects. It is used to structure a software program into simple, reusable pieces of code blueprints (usually called classes), which are used to create individual instances of objects. 

Note: JavaScript is not an object-oriented language. Neither is it completely a functional language. JavaScript is a prototype-based procedural language. It supports both functional and object-oriented patterns of programming.

#### Benefits of Object Oriented Programming

+ Easier debuging
+ Reuse of code through inheritance
+ Flexibility through polymorphism
+ Effective problem solving
+ Project decoupling (Separate project into groups)

### Encapsulation - Encapsulation puts the data variables and the data functions together inside a box. Encapsulation ensures that data can only be accessed using the data functions defined inside the class, and abstraction ensures not anyone outside this encapsulated box can access it.

### Abstraction - Class objects can only share public properties and hide private properties. This creates a great way of controlling its behaviour.

### Inheritance - We can inherit other classes and extend all there public properties.

<details>
  <summary>Code example</summary>
      
```javascript

// "class" declaration
function Car(make, model) {
  this.make = make;
  this.model = model;

  function start() {
    console.log('vroom');
  }

  function toString() {
    console.log('Car - ' + this.make + ' - ' + this.model);
  }

  return { make, model, start, toString }
}

// inheritance example
function SportsCar(make, model, turbocharged) {
  Car.call(this, make, model);
  this.turbocharged = turbocharged;
}

// actual inheritance logic
SportsCar.prototype = Object.create(Car.prototype);
SportsCar.prototype.constructor = SportsCar;

// overriding the start method
SportsCar.prototype.start = function() {
  console.log('VROOOOM');
}

// Now testing the classes
var car = new Car('Nissan', 'Sunny');
car.start(); // vroom
console.log(car.make); // Nissan

var sportsCar = new SportsCar('Subaru', 'BRZ', true);
sportsCar.start(); // VROOOOM
console.log(car.turbocharged); // true

```
</details>

### Polymorphism - Objects can take various behaviour depending on the context.

+ Overloading (ad-hoc polymorphism)
+ Structural Subtyping (Structural Polymorphism)

Instead of using classes we can also use functions.

<details>
  <summary>Code example</summary>
      
```typescript

interface DbCommon {
  getDetails: () => DbDetails[];
}

interface DbDetails {
  name: string;
  age: number;
}

class MongoDb implements DbCommon {
  // Structural Polymorphism
  getDetails = (): DbDetails[] => {
    return [
      {
        name: "MongoDb",
        age: 5
      }
    ];
  };

  // Polymorphism Overloading

  // getDetails = (num: number): DbDetails[]  => {
  //   return [{
  //     name: "MongoDb",
  //     age: 5 + num
  //   }];
  // };
}

class Postgres implements DbCommon {
  // Structural Polymorphism
  getDetails = (): DbDetails[] => {
    return [
      {
        name: "Postgres",
        age: 15
      }
    ];
  };
}

class Database {
  db1: DbCommon;
  db2: Postgres;

  // Aggregation
  constructor(db: DbCommon) {
    this.db1 = db;
    // Composition
    this.db2 = new Postgres();
  }

  getUsersAggregation() {
    return this.db1.getDetails();
  }

  getUsersComposition() {
    return this.db2.getDetails();
  }
}

// Depedency injection MongoDb
const db = new Database(new MongoDb());
console.log(db.getUsersAggregation());
console.log(db.getUsersComposition());

```
</details>


