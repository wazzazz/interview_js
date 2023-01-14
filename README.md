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


