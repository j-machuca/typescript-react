# Types

## Index

1. [Type Basics](#type-basics)
2. [Type Inference](#type-inference)
3. [Primitive Types](#primitive-types)
4. [Typescript Specific types](#typescript-specific-types)
5. [Arrays](#arrays)
6. [Objects](#objects)
7. [Object Types](#object-types)
   1. [Interfaces](#interfaces)
   2. [Type Alias](#type-alias)
   3. [Classes](#classes)

---

## Type Basics

- Javascript variables are determined at runtime and can also change.

- **Typescript** allows us to type annotate our code. The compiler will check for errors during compilation.

## Type Inference

- Typescript has the ability to infere the type of variables.

### Primitive types

1. string
2. sumber
3. boolean
4. undefined
5. null

### Typescript Specific Types

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

### Arrays

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

### Objects

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

### **Interfaces**

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

### Type alias

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

### Classes

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
