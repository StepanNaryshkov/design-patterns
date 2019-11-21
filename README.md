
# Design patterns
*The notes are an important attempt to strengthen results in design patterns (JavaScript)*

[What is a design pattern?](#Design%20pattern%20is%20reisable%20solution%20to%20commonly%20occuring%20problems%20in%20sowftawe%20problems.)

Design pattern is reusable solution to commonly occuring problems in software design. In mostly cases you can not copy pattern in your project because pattern is not a piece of code and an exact solution but it is general concept for solving particular problems. Design patterns have some benefits:

 - Pattern is reusable solution;
 - Pattern is proven solution;
 - Pattern is expressive solution;
 - Patters solves particular problem;
 
 We consider three main group of design patterns:

- **Creational**. Provide object creational mechanisms that increase flebility and reuse of existing code.
- **Sctructural**. Explain how to assemble objects and classes into larger stuctures, while keeping the structure flexiable and efficiant.
- **Behavioral**. Take care of effective communication and the assignment of responsibilities between objects.

Creational:

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

**Prototype** is creational design pattern which lets you copy existing objects without maling your code dependent on their classes

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
