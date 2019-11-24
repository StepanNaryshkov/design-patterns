
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
<p></p>

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

**Decorator** is structural design pattern which lets you dynamically change behavior of an object by wrapping in object of decorator class.

<details><summary>Show code</summary>
<p>
 
```
class Coffee {
  getPrice() {
    return 10
  }
}


class Latte {
  constructor(coffee) {
    this.coffee = coffee
  }

  getPrice() {
    return this.coffee.getPrice() + 4;
  }
}


let order = new Coffee();
order = new Latte(order);
console.log(order.getPrice())
```

  </p>
</details>

**Facade** is structural design pattern which provides simplified interface to complex subsystem (framework, library, api)

<details><summary>Show code</summary>
<p>
 
```
class Coffee {
  findFreeWaiter() {
    console.log('find a waiter')
  }
  
  prepareCoffeeMachine() {
    console.log('prepare coffee machine')
  }
  
  
  makeCoffee() {
    console.log('make a cup of coffee')
  }
}


class Facade {
  constructor(coffee){
    this.coffee = coffee
  }
  
  getCoffee(){
    this.coffee.findFreeWaiter();
    this.coffee.prepareCoffeeMachine()
    this.coffee.makeCoffee()
  }
}


const order = new Facade(new Coffee());
order.getCoffee()
```

  </p>
</details>

**Flyweight** is structural design pattern which reduces cost of creating large number of similar objects. Also it uses to minimize memory usage by sharing as much as possible to similar objects. 

<details><summary>Show code</summary>
<p>
 
```
class Coffee {
  constructor() {
    this.types = []
  }
  
  createCoffee(type) {
    if (this.types.includes(type)) {
      return type;
    } else {
      return this.types.push(type); // here flyweight
    }
  }
  
  
  getCoffee(type) {
    const ind = this.types.indexOf(type);
    if (ind !== -1) {
      return this.types[ind];
    } else {
      return "Not found"
    }
  }
  
  getAllCoffee() {
    return this.types;
  }
}

const coffee = new Coffee();
coffee.createCoffee("Latte");
coffee.createCoffee("Espresso");
coffee.createCoffee("Latte");
coffee.createCoffee("Cacao");

console.log(coffee.getCoffee("Latte"));
console.log(coffee.getAllCoffee());
```

  </p>
</details>

**Proxy** is structural design pattern which provide surrogate or a placeholder for another object to control access to it.

<details><summary>Show code</summary>
<p>
 
```
class CoffeeProxy {
 constructor(info) {
    this.info = info;
  }
  
  getCoffee(money) {
    if (money < 10) {
      return "Sorry, but it costs 10$"
    } else {
      return this.info.getCoffee()
    }
  }
}

class Coffee {
  getCoffee() {
    return "here is your coffee";
  }
}

const coffee = new CoffeeProxy(new Coffee());
console.log(coffee.getCoffee(7));
console.log(coffee.getCoffee(10));
```

  </p>
</details>

***Behavior***

**Chain of responsibility** is behavioral design pattern which helps building of objects, request enters from one and keeps going from object to object till finds the suitable handler or object can pass it to next object, it depends on your case.

<details><summary>Show code</summary>
<p>
 
```
class Account {
  setNext(account) {
    this.successor = account;
  }

  pay(amountToPay) {
    if (this.canPay(amountToPay)) {
      console.log(`Paid ${amountToPay} using ${this.name}`);
    } else if (this.successor) {
      console.log(`Cannot pay using ${this.name}. Proceeding...`);
      this.successor.pay(amountToPay);
    } else {
      console.log("None of the accounts have enough balance");
    }
  }

  canPay(amount) {
    return this.balance >= amount;
  }
}

class Bank extends Account {
  constructor(balance) {
    super();
    this.name = "bank";
    this.balance = balance;
  }
}

class Paypal extends Account {
  constructor(balance) {
    super();
    this.name = "Paypal";
    this.balance = balance;
  }
}

class Bitcoin extends Account {
  constructor(balance) {
    super();
    this.name = "bitcoin";
    this.balance = balance;
  }
}

const bank = new Bank(100); // Bank with balance 100
const paypal = new Paypal(200); // Paypal with balance 200
const bitcoin = new Bitcoin(300); // Bitcoin with balance 300

bank.setNext(paypal);
paypal.setNext(bitcoin);

// Let's try to pay using the first priority i.e. bank
bank.pay(259);
```

  </p>
</details>

**Comand** is behavioral design pattern which allows you encapsulate actions in objects (like redux actions). The main idea is to provide means to decouple client to receiver

<details><summary>Show code</summary>
<p>

```
// Receiver
class Bulb {
  turnOn() {
    console.log("Bulb has been lit");
  }

  turnOff() {
    console.log("Darkness!");
  }
}

/*
Command interface :

    execute()
    undo()
    redo()
*/

// Command
class TurnOnCommand {
  constructor(bulb) {
    this.bulb = bulb;
  }

  execute() {
    this.bulb.turnOn();
  }

  undo() {
    this.bulb.turnOff();
  }

  redo() {
    this.execute();
  }
}

class TurnOffCommand {
  constructor(bulb) {
    this.bulb = bulb;
  }

  execute() {
    this.bulb.turnOff();
  }

  undo() {
    this.bulb.turnOn();
  }

  redo() {
    this.execute();
  }
}

// Invoker
class RemoteControl {
  submit(command) {
    command.execute();
  }
}

const bulb = new Bulb();

const turnOn = new TurnOnCommand(bulb);
const turnOff = new TurnOffCommand(bulb);

const remote = new RemoteControl();
remote.submit(turnOn); // Bulb has been lit!
remote.submit(turnOff); // Darkness!
```

  </p>
</details>

**Iterator** is behavior design pattern which accessess the elements of the objects sequentially without exposing its undrelying representaional.

<details><summary>Show code</summary>
<p>

```
class RadioStation {
  constructor(frequency) {
    this.frequency = frequency;
  }

  getFrequency() {
    return this.frequency;
  }
}

class StationList {
  constructor() {
    this.stations = [];
  }

  addStation(station) {
    this.stations.push(station);
  }

  removeStation(toRemove) {
    const toRemoveFrequency = toRemove.getFrequency();
    this.stations = this.stations.filter(station => {
      return station.getFrequency() !== toRemoveFrequency;
    });
  }
}

const stationList = new StationList();

stationList.addStation(new RadioStation(89));
stationList.addStation(new RadioStation(101));
stationList.addStation(new RadioStation(102));
stationList.addStation(new RadioStation(103.2));

stationList.stations.forEach(station => console.log(station.getFrequency()));

stationList.removeStation(new RadioStation(89)); // Will remove station 89

```

  </p>
</details>

**Mediator** is behavior design pattern which provides cetral object over a group of objects by encapsulating how these objects interact. From real world, it is network, when you call somebody.

<details><summary>Show code</summary>
<p>

```
// Mediator
class ChatRoom {
  showMessage(user, message) {
    const time = new Date();
    const sender = user.getName();

    console.log(time + "[" + sender + "]:" + message);
  }
}

class User {
  constructor(name, chatMediator) {
    this.name = name;
    this.chatMediator = chatMediator;
  }

  getName() {
    return this.name;
  }

  send(message) {
    this.chatMediator.showMessage(this, message);
  }
}

const mediator = new ChatRoom();

const john = new User("John Doe", mediator);
const jane = new User("Jane Doe", mediator);

john.send("Hi there!");
jane.send("Hey!");


```

  </p>
</details>


**Memento** is behavior design pattern which provides the ability to restore an object to its previous state

<details><summary>Show code</summary>
<p>

```
class EditorMemento {
  constructor(content) {
    this._content = content;
  }

  getContent() {
    return this._content;
  }
}

class Editor {
  constructor() {
    this._content = "";
  }

  type(words) {
    this._content = this._content + " " + words;
  }

  getContent() {
    return this._content;
  }

  save() {
    return new EditorMemento(this._content);
  }

  restore(memento) {
    this._content = memento.getContent();
  }
}

const editor = new Editor();

// Type some stuff
editor.type("This is the first sentence.");
editor.type("This is second.");

// Save the state to restore to : This is the first sentence. This is second.
const saved = editor.save();

// Type some more
editor.type("And this is third.");

// Output: Content before Saving
console.log(editor.getContent()); // This is the first sentence. This is second. And this is third.

// Restoring to last saved state
editor.restore(saved);

console.log(editor.getContent()); // This is the first sentence. This is second.

```

  </p>
</details>

**Strategy** is behavioral design pattern which allows you switch between strategy based upon the situation.

<details><summary>Show code</summary>
<p>

```
class ShoppingCart {
  constructor(discount) {
    this.discount = discount;
    this.amount = 0;
  }

  checkout() {
    return this.discount(this.amount);
  }

  setAmount(amount) {
    this.amount = amount;
  }
}

function guestStrategy(amount) {
  return amount;
}

function regularStrategy(amount) {
  return amount * 0.9;
}

function premiumStrategy(amount) {
  return amount * 0.8;
}

const regularUser = new ShoppingCart(regularStrategy);
regularUser.setAmount(100);
console.log(regularUser.checkout());

```

  </p>
</details>

**State** is behavioral design pattern which lets you change the behavior of a class when state changes. For example offline and online mode on website
<details><summary>Show code</summary>
<p>

```
const upperCase = inputString => inputString.toUpperCase();
const lowerCase = inputString => inputString.toLowerCase();
const defaultTransform = inputString => inputString;

class TextEditor {
  constructor(transform) {
    this._transform = transform;
  }

  setTransform(transform) {
    this._transform = transform;
  }

  type(words) {
    console.log(this._transform(words));
  }
}

const editor = new TextEditor(defaultTransform);

editor.type("First line");

editor.setTransform(upperCase);

editor.type("Second line");
editor.type("Third line");

editor.setTransform(lowerCase);

editor.type("Fourth line");
editor.type("Fifth line");

```

  </p>
</details>


**Visitor** is behavioral design pattern which lets you add futher operations to object without having modify them.

<details><summary>Show code</summary>
<p>

```
class Monkey {
  shout() {
    console.log("Ooh oo aa aa!");
  }

  accept(operation) {
    operation.visitMonkey(this);
  }
}

class Lion {
  roar() {
    console.log("Roaaar!");
  }

  accept(operation) {
    operation.visitLion(this);
  }
}

class Dolphin {
  speak() {
    console.log("Tuut tuttu tuutt!");
  }

  accept(operation) {
    operation.visitDolphin(this);
  }
}

const speak = {
  visitMonkey(monkey) {
    monkey.shout();
  },
  visitLion(lion) {
    lion.roar();
  },
  visitDolphin(dolphin) {
    dolphin.speak();
  }
};

const monkey = new Monkey();
const lion = new Lion();
const dolphin = new Dolphin();

monkey.accept(speak); // Ooh oo aa aa!
lion.accept(speak); // Roaaar!
dolphin.accept(speak);

```

  </p>
</details>

**Observer** is behavioral design pattern which defines one to many dependency between objects, so when one changes, all its dependets  are notified by automaticly

<details><summary>Show code</summary>
<p>

```
const JobPost = title => ({
  title: title
});

class JobSeeker {
  constructor(name) {
    this._name = name;
  }

  notify(jobPost) {
    console.log(
      this._name,
      "has been notified of a new posting :",
      jobPost.title
    );
  }
}

class JobBoard {
  constructor() {
    this._subscribers = [];
  }

  subscribe(jobSeeker) {
    this._subscribers.push(jobSeeker);
  }

  addJob(jobPosting) {
    this._subscribers.forEach(subscriber => {
      subscriber.notify(jobPosting);
    });
  }
}

const jonDoe = new JobSeeker("John Doe");
const janeDoe = new JobSeeker("Jane Doe");
const kaneDoe = new JobSeeker("Kane Doe");

// Create publisher and attach subscribers
const jobBoard = new JobBoard();
jobBoard.subscribe(jonDoe);
jobBoard.subscribe(janeDoe);

// Add a new job and see if subscribers get notified
jobBoard.addJob(JobPost("Software Engineer"));

```

  </p>
</details>

**Template method** is behavioral design pattern which defines the skeleton of an algorithm in an operation, deferring some steps to subclasses. 

<details><summary>Show code</summary>
<p>

```
class Builder {
  // Template method
  build() {
    this.test();
    this.lint();
    this.assemble();
    this.deploy();
  }
}
class AndroidBuilder extends Builder {
  test() {
    console.log("Running android tests");
  }

  lint() {
    console.log("Linting the android code");
  }

  assemble() {
    console.log("Assembling the android build");
  }

  deploy() {
    console.log("Deploying android build to server");
  }
}

class IosBuilder extends Builder {
  test() {
    console.log("Running ios tests");
  }

  lint() {
    console.log("Linting the ios code");
  }

  assemble() {
    console.log("Assembling the ios build");
  }

  deploy() {
    console.log("Deploying ios build to server");
  }
}

const androidBuilder = new AndroidBuilder();
androidBuilder.build();

// Output:
// Running android tests
// Linting the android code
// Assembling the android build
// Deploying android build to server

const iosBuilder = new IosBuilder();
iosBuilder.build();

```

  </p>
</details>
