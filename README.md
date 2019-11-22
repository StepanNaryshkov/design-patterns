
# Design patterns
*The notes are an important attempt to strengthen results in design patterns (JavaScript)*

[What is a design pattern?](#Design%20pattern%20is%20reisable%20solution%20to%20commonly%20occuring%20problems%20in%20sowftawe%20problems.)

Design pattern is reusable solution to commonly happen problems in software design. In mostly cases you can not copy pattern in your project because pattern is not a piece of code and an exact solution but it is general concept for solving particular problems. Design patterns have some benefits:

 - Pattern is reusable solution;
 - Pattern is proven solution;
 - Pattern is expressive solution;
 - Patters solves particular problem;
 
 We consider three main group of design patterns:

- **Creational**. Provide object creational mechanisms that increase flebility and reuse of existing code.
- **Sctructural**. Explain how to assemble objects and classes into larger stuctures, while keeping the structure flexiable and efficiant.
- **Behavioral**. Take care of effective communication and the assignment of responsibilities between objects.

***Creational:***

**Factory method** is creational design pattern which provides an interface for creating object in superclass but it delegates object creational to subclasses to change the type of objects that will be created. It means that we don't know exactly which types of objects we want to create, but we know that these objects will have the same structure and differnt data.

<details><summary>Show code</summary>
<p>

```
index.js
class AbstractProduct {
  constructor({ model, price, owner }) {
    this.model = model;
    this.price = price;
    this.owner = owner;
  }
}

class Creator {
  create(props) {
    switch (props.type) {
      case "Auto":
        return new Auto(props);
        break;
      case "Motorcycle":
        return new Motorcycle(props);
        break;
      default:
        throw "Parameter is not a product!";
    }
  }
}

const factory = new Creator(); // superclass

class Auto extends AbstractProduct {}
class Motorcycle extends AbstractProduct {}

const X5 = factory.create({
  // subclasse
  type: "Auto",
  model: "X5",
  price: "50000",
  owner: "Mike"
});
const CX500 = factory.create({
  // subclasse
  type: "Motorcycle",
  model: "Honda CX 500",
  price: "20000",
  owner: "Bred"
});

```

</p>
</details>

**Abstract method** is creational design pattern which provide an interface for creating objects that are related by a common theme. The main goal is encapsulate a group of factories with a common goal

<details><summary>Show code</summary>
<p>

```
class AbstractProduct {
  constructor({ brand, price, owner, type }) {
    this.brand = brand;
    this.price = price;
    this.owner = owner;
    this.type = type;
  }
}

class ProductModernFactory {
  create(props) {
    switch (props.type) {
      case "Auto":
        return new Auto({ ...props, type: "modern" });
        break;
      case "Motorcycle":
        return new Motorcycle({ ...props, type: "modern" });
        break;
      default:
        throw "Parameter is not a product!";
    }
  }
}

class ProductRetroFactory {
  create(props) {
    switch (props.type) {
      case "Auto":
        return new Auto({ ...props, type: "retro" });
        break;
      case "Motorcycle":
        return new Motorcycle({ ...props, type: "retro" });
        break;
      default:
        throw "Parameter is not a product!";
    }
  }
}

const productModernFactory = new ProductModernFactory();
const productRetroFactory = new ProductRetroFactory();
class Auto extends AbstractProduct {}
class Motorcycle extends AbstractProduct {}

function AbstractFactory(props) {
  switch (props.kind) {
    case "modern":
      return productModernFactory.create(props);
      break;
    case "retro":
      return productRetroFactory.create(props);
      break;
    default:
      throw "Parameter is not a factory!";
  }
}

const myAuto = AbstractFactory({
  kind: "modern",
  type: "Auto",
  brand: "BMW",
  price: "10000",
  owner: "Stepan"
});

console.log(myAuto);

```

</p>
</details>

**Builder** is creational design pattern which lets you build  complex objects step by step. It allows you to produce different types and representations of an object using the same code.

<details><summary>Show code</summary>
<p>
 
```
class House {
  constructor() {
    this.foundation = true;
  }
}

class HouseBuilder {
  constructor() {
    this.house = new House();
  }

  addDoors(doors) {
    this.house.doors = doors;
    return this;
  }

  addWalls(walls) {
    this.house.walls = walls;
    return this;
  }

  addWindows(windows) {
    this.house.windows = windows;
    return this;
  }

  addRoof(roof) {
    this.house.roof = roof;
    return this;
  }

  build() {
    return this.house;
  }
}

const myHome = new HouseBuilder()
  .addDoors(true)
  .addWalls(true)
  .addRoof(true)
  .build();
console.log(myHome);
```

</p>
</details>

**Prototype** is creational design pattern which lets you copy existing objects without making your code dependent on their classes

<details><summary>Show code</summary>
<p>
 
```
class Car {
  constructor(model, price) {
    this.model = model;
    this.price = price;
  }

  clone() {
    return new Car(this.model, this.price);
  }
}

const prototypeCar = new Car("BMW X5", 50000);

const car1 = prototypeCar.clone();
const car2 = prototypeCar.clone();
const car3 = prototypeCar.clone();

OR

const car = {
  model: 'BMW',
  price: 50000
};

const car1 = Object.create(car);
const car2 = Object.create(car);
car2.price = 99999;
```

</p>
</details>

**Singleton** is creational design pattern which lets you create class with only one instance and providing a global access to this instance

<details><summary>Show code</summary>
<p>

```
class Singleton {
  constructor(name) {
    if (Singleton.instance) {
      return Singleton.instance;
    }
    Singleton.instance = this;
    this.name = name;
    return Singleton.instance;
  }

  getName() {
    return this.name;
  }
}

const singleton = new Singleton("example");
const singletonSecondary = new Singleton("another example");
console.log(singleton.getName()); // example
console.log(singletonSecondary.getName()); // example
```

 </p>
</details>

***Sctructural design patterns***

**Adapter** is structural design pattern which lets objects with incompatible interfaces to collaborate. 

<details><summary>Show code</summary>
<p>
 
```
const myApi = {
  getCars: () => ["BMW", "AUDI", "TESLA"]
};

const anotherApi = {
  getCars: () => "HAMMER,HONDA"
};

const responseAdapter = data => data.split(",");

class Cars {
  constructor() {
    this._data = [];
  }

  get data() {
    return this._data;
  }

  set data(newCars) {
    this._data = [...this._data, ...newCars];
  }
}

const cars = new Cars();

cars.data = myApi.getCars();
cars.data = responseAdapter(anotherApi.getCars());

console.log(cars.data);
```

 </p>
</details>

**Bridge** is structural design pattern that lets you split a large class or a set of closely related classes into two separate system: abstraction and implementation.

<details><summary>Show code</summary>
<p>
 
 ```
 class Computer { // abstraction
  constructor(brand) {
    this.brand = brand;
  }
}

class Brand {
  constructor(type) {
    this.type = type;
  }

  getName() {
    return this.type;
  }
}

class Asus extends Brand {
  constructor() {
    super("Asus");
  }
}

class Laptop extends Computer { // implementation
  constructor(brand) {
    super(brand);
  }

  greeting() {
    return `Laptop - ${this.brand.getName()}`;
  }
}

const myLaptop = new Laptop(new Asus());
console.log(myLaptop.greeting());

EXAMPLE #2
class About {
  constructor(theme) {
    this.theme = theme;
  }

  getContent() {
    return `This is About page in ${this.theme.getColor()} theme`;
  }
}

class DarkTheme {
  getColor() {
    return "Dark";
  }
}

const aboutPage = new About(new DarkTheme());
console.log(aboutPage.getContent());

 ```
 
  </p>
</details>
Also, we can add one more example, css variables are abstractation, and when we use them in our code it will be like implementation

**Composite** is structural design pattern which composes objects into tree structures and then work with these sctructures as if they were individual objects


<details><summary>Show code</summary>
<p>
 
 ```
 class Sprint {
  addTask(task) {
    this.task = task;
  }
}

class Composite extends Sprint {
  constructor() {
    super();
    this.tasks = [];
  }

  add(task) {
    this.tasks.push(task);
  }

  getInfo() {
    return this.tasks.map(i => i.task);
  }
}

class CreatePage extends Composite {
  constructor() {
    super();
    this.addTask("create page");
  }
}

class TestPage extends Composite {
  constructor() {
    super();
    this.addTask("need to test the created page");
  }
}

const sprint = new Composite();
sprint.add(new CreatePage());
sprint.add(new TestPage());
console.log(sprint.getInfo());

 ```
 
  </p>
</details>
