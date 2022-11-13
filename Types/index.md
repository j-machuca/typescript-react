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
  const numbers:number[] = []

  // We can access all the array methods like usual.

  numbers.push(1)

  // passing a function 
  // the num parameter gets inferred correctly by typescript. Type annotating it is still posible.
  numbers.forEach(function(num){
    console.log(num)
  })

  // passing an arrow function
  numbers.forEach((num)=> console.log(num)
  )

  // We can change the value even if we used const since reference to the array is still the same.
  ```

### Objects

- Represents a non-primitive type.
- We can define the object structure before using it by defining *interfaces*, *type aliases* and *classes*.

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


  #### **Interfaces**

  - Defines a type with a collection of property and method definition without the implementation. 

  - Interfaces can reference other interfaces inside it. 

  **Properties**

    ```typescript
    // Creating a Product interface
    interface Product {
      // properties of the object
      name:string;
      unitPrice:number;
    }

    // Creating a OrderDetail interface 
    interface OrderDetail {
      product:Product;
      quantity:number;
    }

    // Creating an Product object

    const table:Product = {
      name:"Table",
      unitPrice:500,
    }

    const tableOrder:OrderDetail = {
      product:table,
      quantity:1

    }
    ```

    **Methods**