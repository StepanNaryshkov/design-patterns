
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
```
index.js

class AbstractProduct {
    constructor({model, price, owner}) {
    this.model = model;
    this.price = price;
    this.owner = owner;
  }
}

class Creator {
  create(props) {
    switch(props.type) {
      case 'Auto':
        return new Auto(props);
        break;
      case 'Motorcycle':
        return new Motorcycle(props);
        break;
      default: 
        throw 'Parameter is not a product!'
    }
  }
}

const factory = new Creator(); // superclass

class Auto extends AbstractProduct {}
class Motorcycle extends AbstractProduct {}

const X5 = factory.create({ // subclasse
 type: 'Auto',
 model: 'X5',
 price: '50000',
 owner: 'Mike'
})
const CX500 = factory.create({ // subclasse
 type: 'Motorcycle',
 model: 'Honda CX 500',
 price: '20000',
 owner: 'Bred'
})
```
```
class AbstractProduct {
    constructor({model, price, owner, type}) {
    this.model = model;
    this.price = price;
    this.owner = owner;
    this.type = type;
  }
}

class ProductModernFactory {
  create(props) {
    switch(props.type) {
      case 'Auto':
        return new Auto({...props, type: 'modern'});
        break;
      case 'Motorcycle':
        return new Motorcycle({...props, type: 'modern'});
        break;
      default: 
        throw 'Parameter is not a product!'
    }
  }
}

class ProductRetroFactory {
  create(props) {
    switch(props.type) {
      case 'Auto':
        return new Auto({...props, type: 'retro'});
        break;
      case 'Motorcycle':
        return new Motorcycle({...props, type: 'retro'});
        break;
      default: 
        throw 'Parameter is not a product!'
    }
  }
}

const productModernFactory = new ProductModernFactory();
const productRetroFactory = new ProductRetroFactory();

class Auto extends AbstractProduct {}
class Motorcycle extends AbstractProduct {}

const X5 = productModernFactory.create({
 type: 'Auto',
 model: 'X5',
 price: '50000',
 owner: 'Mike'
})
const CX500 = productRetroFactory.create({
 type: 'Motorcycle',
 model: 'Honda CX 500',
 price: '20000',
 owner: 'Bred'
})

console.log(X5);
console.log(CX500);
```
