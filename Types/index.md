# Types

## Index

1. [Type Basics](#type-basics)
2. [Type Inference](#type-inference)
3. [Primitive Types](#primitive-types)
4. [Typescript Specific types](#typescript-specific-types)
5. [Arrays](#arrays)
6. [Tuples](#tuples)
7. [Objects](#objects)
8. [Object Types](#object-types)
   1. [Interfaces](#interfaces)
   2. [Type Alias](#type-alias)
   3. [Classes](#classes)
9. [Access Modifiers](#access-modifiers)
10. [Property Setters and Getters](#property-setters-and-getters)
11. [Static](#static)
12. [Type checking](#type-checking)
---

## Type Basics

- Javascript variables are determined at runtime and can also change.

- **Typescript** allows us to type annotate our code. The compiler will check for errors during compilation.

## Type Inference

- Typescript has the ability to infere the type of variables.

## Primitive types

1. string
2. sumber
3. boolean
4. undefined
5. null

## Typescript Specific Types

1. any
   - If a variable is defined and no type is specified typescript assigns the type `any`
2. void
   - Used to represent a function that does not return any value.
3. never
   - Used to represent something that would never occur.
   - typically used to specify unreachable areas of code.
4. enum

   - Used to declare a meaningful set of friendly names that a variable can be set to.

     ```typescript
     // numeric values for enum start at 0
     enum OrderStatus {
       Paid,
       Shipped,
       Completed,
       Cancelled,
     }
     // can set numerical value to start at 1
     enum OrderStatusB {
       Paid = 1,
       Shipped,
       Completed,
       Cancelled,
     }

     // Explicit values can be set as well
     enum OrderStatusB {
       Paid = 1,
       Shipped = 2,
       Completed = 3,
       Cancelled = 0,
     }

     let status = OrderStatus.Shipped;
     ```
  5. unknown

    - the key difference between `any` and `unknown` is that typescript will not perform type checking if we set type to `any`. 


## Arrays

- Represent the same structure available in Javascript.

  ```typescript
  // Define an array and type annotate
  const numbers: number[] = [];

  // We can access all the array methods like usual.

  numbers.push(1);

  // passing a function
  // the num parameter gets inferred correctly by typescript. Type annotating it is still posible.
  numbers.forEach(function (num) {
    console.log(num);
  });

  // passing an arrow function
  numbers.forEach((num) => console.log(num));

  // We can change the value even if we used const since reference to the array is still the same.
  ```

## Tuples

- Is a finite ordered list of elements.

```typescript
// Defining the tuple
let product: [string,number];

// implementing
product = ["table",500];

// To Access values in the tuple
console.log(product[0]) 
console.log(product[1]) 

// we can iterate over tuples using a for loop

for (let element in product){
  console.log(product[element]);
}

// or the forEach method

product.forEach(function(element){
  console.log(element);
})

```

**Open ended tuples**

- We can define open ended tuples using the `...`(rest) operator. 

```typescript
// Type definition
type Scores = [string,...number[]];

const billyScores:Scores = ['Billy',50,60,70];

const sallyScores:Scores = ['Sally',50,60,70];
```

**Tuple function parameters**

- We can define strongly-types `...`(rest) parameters in functions

```typescript
// Using rest operator
function logScores(...scores:[...numbers[]]){
  console.log(scores);
}

// Using open-ended tuples

type Scores = [string,...number[]];

function logNameAndScores (...scores:Scores){
  console.log(scores);
}

logNameAndScores("Sally",60,70,75,70);
```

**Spread expressions**

- Typescript allows us to use tuples with spread expressions.

```typescript

function logScore(score1:number,score2:number,score3:number) {
  console.log(score1 + ', ' + score2 + ', ' + score3)
}
// fixed tuples
const scores:[number,number,number] = [75,65,80];

logScore(...scores);
```

**Empty tuples**

- We can also set empty tuples. Not much use by itself but comes in handy when using `union` type.

```typescript
const empty:Empty = [];
```

**Optional tuple elements**

- we can now define optional tuple elements

```typescript
type Scores = [number,number?,number?];

const samScores:Scores = [55];
const bobScores:Scores = [55,65];
const jayScores:Scores = [55,65,65];
```

## Objects

- Represents a non-primitive type.
- We can define the object structure before using it by defining _interfaces_, _type aliases_ and _classes_.

  ```typescript
  // Creating an Object just like JS. Typescript can infer types.
  const customer = {
    name: "Me",
    turnover: 100000,
    active: true,
  };

  // We can change the value even if we used const since reference to  the object is still the same.

  customer.turnover = 2000000;
  ```

## **Interfaces**

- Defines a type with a collection of property and method definition without the implementation.

- Interfaces can reference other interfaces inside it.

**Properties**

```typescript
// Creating a Product interface
interface Product {
  // properties can be readonly
  readonly name: string;
  unitPrice: number;
}

// Creating a OrderDetail interface
interface OrderDetail {
  product: Product;
  quantity: number;
  // Optional properties can be specified by adding a ?
  dateAdded?: Date;
}

// Creating an Product object

const table: Product = {
  name: "Table",
  unitPrice: 500,
};

// Creating an OrderDetail Object
const tableOrder: OrderDetail = {
  product: table,
  quantity: 1,
};
```

**Methods**

```typescript
// Interface definition

interface OrderDetail {
  product: Product;
  quantity: number;
  // can specify optional function parameters by adding a ? to function signatures
  getTotal(discount?: number): number;
}

// Interface Implementation

const tableOrder: OrderDetail = {
  product: table,
  quantity: 1,

  getTotal(discount: number): number {
    const priceWithoutDiscount = this.product.unitPrice * this.quantity;
    const discountAmount = priceWithoutDiscount * (discount || 0);
    return priceWithoutDiscount - discountAmount;
  },
};
```

**Extending Interfaces**

Interaces can extend other interfaces by using the `extends` keyword.

```typescript
interface Product {
  name: string;
  unitPrice: number;
}

interface DiscountCode {
  code: string;
  percentage: number;
}

interface ProductWithDiscountcodes extends Product {
  discountCodes: DiscountCode[];
}

const table: ProductWithDiscountcodes = {
  name: "Table",
  unitPrice: 500,
  discountCodes: [
    { code: "SUMMER10", percentage: 0.1 },
    { code: "BFRI", percentage: 0.2 },
  ],
};
```

## Type alias

- Type aliases allow us to create a new `type`.
- Type aliases also allow us to define the shape of the object.

```typescript
// function signature using a type alias
type GetTotal = (discount: number) => number;

interface OrderDetail {
  product: Product;
  quantity: number;
  getTotal: GetTotal;
}

// Defining the Product type alias
type Product = {
  name: string;
  unitPrice: number;
};

// Implementing the Product type

type OrderDetail = {
  product: Product;
  quantity: number;
  getTotal: (discount: number) => number;
};
```

## Classes

- Classes have more features thatn `type` aliases and `interfaces`. One of these features is the ability to define the implementation of the methods in that class.

  ```typescript
  class Product {
    name: string;
    unitPrice: number;
  }

  // type implementation - table type is implied from the Product class
  const table = new Product();
  table.name = "Table";
  table.unitPrice = 500;
  ```

  **Refactoring OrderDetail**

  ```typescript
  // class definition
  class OrderDetail {
    product:Product;
    quantity:number;
    getTotal(discount:number):number{
      const priceWithoutDiscount = this.product.unitPrice * this.quantity;
      const discountAmount = priceWithoutdiscount 8 discount;
      return priceWithoutDiscount - discountAmount;
    }
  }
  // Class implementation

  const table = new Product();
  table.name = "Table";
  table.unitPrice = 500;

  const orderDetail = new OrderDetail();
  orderDetail.product = table;
  orderDetail.quantity = 2;

  const total = orderDetail.getTotal(0.1);
  console.log(total);

  ```

  **Implementing Interfaces**

  ```typescript
  // Defining the Interface
  interface IOrderDetail {
    product: Product;
    quantity: number;
    getTotal(discount: number): number;
  }

  // Implementing the Interface
  class OrderDetail implements IOrderDetail {
    product: Product;
    quantity: number;
    getTotal(discount: number): number {
      const priceWithoutDiscount = this.product.unitPrice * this.quantity;
      const discountAmount = priceWithoutDiscount * discount;
      return priceWithoutDiscount - discountAmount;
    }
  }
  ```

  **Constructors**

  Constructors are functions that perform the initialization of new instances of a class.

  ```typescript
  class OrderDetail implements IOrderDetail {
    product: Product;
    quantiy: number;

    constructor(product: Product, quantity: number) {
      this.product = product;
      this.quantity = quantity;
    }

    getTotal(discount: number): number {
      // implementation goes here
    }
  }

  // creating a new instance of OrderDetail
  const orderDetail = new OrderDetail(table, 2);
  ```

  We can also set default values by modifying our constructor function

  ```typescript
  constructor(product:Product,quantity:number){
    this.product = product;
    this.quantity = quantity;
  }

  // creating a new instance
  new orderDetail = new OrderDetail(table);
  ```

  By adding the `public` keyword before the parameters in the constructor we can let the **Typescript compiler** implement the `product` and `quantity` properties.

  ```typescript
  class OrderDetail implements IOrderDetail {
    constructor(public product: Product, public quantity: number = 1) {
      this.product = product;
      this.quantity = quantity;
    }

    getTotal(discount: number): number {
      // Implementation goes here
    }
  }
  ```

  **Extending Classes**

  `Class`es can extend other `Class`es like we do with `interfaces`.

  ```typescript
  // Create Base classes
  class Product {
    name:string;
    unitPrice:number;
  }

  interface DiscountCode {
    code:string;
    percentage:number;
  }
  // Extending base classes
  class ProductWithDiscountCodes extends Product {
    discountCodes:DiscountCode[];
  }

  // Creating a class instance
  const table = new ProductWithDiscountCodes();
  table.name = "Table";
  table.unitPrice = 500;
  table.discountCodes = [
    { 
      code: "SUMMER10",
      percentage: 0.1 
    },
    { 
      code: "BFRI",
      percentage: 0.2
    }
  ]

  // if the the parent class has a constructor class then we need to use the super() method in the constructor

  class Product {
    constructor(public name:string,public unitPrice:number){}
  }

  // .....  //

  class ProductWithDiscountcodes extends Product {
    constructor(public name:string,public unitPrice:number){
      super(name,unitPrice)
    }
    discountCodes:DiscountCode[]
  }

  ```

  **Abstract classes**

  Abstract classes can only be inherited from but not instanciated. 

  ```typescript
  // Define the abstract class
  abstract class Product{
    name:string;
    unitPrice:number;
    abstract delete():void;
  }
  // Extend the abstract class
  class Food extends Product {
    deleted:boolean;
    constructor(public bestBefore:Date){
      super();
    }
    delete(){
      this.deleted = false;
    }
  }

  // create an instance of the Food class
  const bread = new Food(new Date(2022,11,14));
  ```
## Access Modifiers

  The `public` keyword allows us to access and interact with properties and methods inside the class. 

  On the other hand the `private` allows the member to only be available to interact withing the class and not on instances or child classes.

  Last there is a `protected` keyword that allows us to interact with inside the class and on child classes but not on instances. 

  ```typescript
  class OrderDetail {
    public product:Product;
    public quantity:number;
    private deleted:boolean;

    public delete(): void {
      this.deleted = true;
    }
  }

  const orderDetail = new OrderDetail();
  // cannot access the deleted property this will make the compiler flag this as an error.
  orderDetail.deleted = true;
  ```

## Property setters and getters

For more complex scenarios we can implement property *setters* and *getters*.

Usually you store the property in a `private` property.

- **Getters** : is a function that returns the value of a `private` property. 
  
  *The convention for getter functions are to name it `get_property`.*

- **Setters** : is a function that receives a single paramenter that will set the `private` property.

  *The convention for setter functions are to name it `set_property`.*

- The `private` property is commonly named the same as the `getter` and `setter` with an `_` infront of the property.

```typescript
class Product {
  name:string;

  private _unitPrice:number;
  get unitPrice():number {
    return this._unitPrice || 0;
  }

  set unitPrice(value:number){
    if (value< 0) {
      value = 0;
    }
    this._unitPrice = value;
  }
}

//  Implementation
const table = new Product();
table.name = "Table";
console.log(table.unitPrice);
table.unitPrice = -10;
console.log(table.unitPrice);
```

## Static

`static` properties and methods are held in the class itself and not in the class instance.

```typescript
class OrderDetail {
  product: Product;
  quantity:number;


  static getTotal(unitPrice:number,quantity:number,discount:number):number {
    const priceWithoutDiscount = unitPrice * quantity;
    const discountAmount = priceWithoutDiscount * discount;
    return priceWithoutDiscount - discountAmount;
  }
}

const total = OrderDetail.getTotal(500,2,0.1);
console.log(total);
```

## Type Checking

In order to check types

```typescript
    // defining the value as unknown will make the compiler detect the error
    function logScores(scores:unknown){
      console.log(scores.firstName);
      console.log(scores.scores);
    }

    // Trying to excecute the function 
    logScores({
      name:"Billy",
      scores:[60,70,75]
    })

    // Typechecking in the code using type predicates

    const scoresCheck = (scores:any): scores is {name:string;scores:number[]} =>{
      return 'name' in scores && 'scores' in scores;
    }

    // new funtion definition

    function logScores(scores:unknown){
      if(scoresCheck(scores)){
        // will produce an error
        console.log(scores.firstName);
        // will pass type checking
        console.log(scores.name);
        console.log(scores.scores);
      }
    }

    // a better implementation would be 

    type Scores = {name:string;scores:number[]}
    const scoresCheck = (scores:any):scores is Scores => {
      return 'name' in scores && 'scores' in scores;
    }

```

**Type Assertion**

- We can do type assertion using the `as` keyword.

```typescript
// creating the type alias
type Scores = {
  name:string;
  scores:number[]
};

// using the as keyword to tell the compiler what type to expect
function logScores(scores:unknown){
  // this will produce an error when trying to access firstName property
  console.log((scores as Scores).firstName);
  console.log((scores as Scores).scores);
}
```