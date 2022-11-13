# Types

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

## Objects

- Represents a non-primitive type.

  ```typescript
  // Creating an Object just like JS. Typescript can infer types.
  const customer = {
    name: "Me",
    turnover: 100000,
    active: true,
  };

  // We can change the value even if we used const since reference to object is still the same.

  customer.turnover = 2000000;
  ```
