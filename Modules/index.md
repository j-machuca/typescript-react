# Modules3

## Index

---

1. [Structuring code into Modules](#modules)



## Modules

Typescript genereted code executes in the global scope meaning that code from one file is available in another file. 

1. **Asynchronous Module Definitions** (AMD)
    1. Commonly used in code that is targeted for the browser and uses a `define` fucntion to define modules.
2. **CommonJS**
    1. This format is used in **Node.js** programs.
    1. Uses `module.exports` to define modules.
    1. Uses `require` to define dependencies.
3. **Universal Module Definition** (UMD)
    1. This can be used for both browser apps and **Node.js** programs.
4. **ES6**
    1. This is the native Javascript module format.
    2. Uses the `export` keyword to define modules
    3. Uses `import` to define dependencies. 

### **Exporting**

Exporting code from a module allows it to be used by other modules. 

Can be used to export:

1. Interfaces
2. Type aliases
3. Clases
4. Functions
5. Constants


#### **Product.ts**
```typescript
// Export interface directly
export interface Product {
    name:string;
    unitPrice:number;
}

// Another option is to use the export statement beneath item declarations

interface A {
    // implementation goes here
}

interface B{
    // implementation goes here
}

// export after declaration
export {A}

// or we can rename the items to be exported

export {A as Product}

```

### **Importing**

Importing allows us to import items from and exported module. 

```typescript
// import an exported function/constant or class implicitly
import { Product } from './product';

// we can rename the imported item using the "as" keyword

import { Product as P} from './product';

```

### Default exports

We can specify a single item to be exported as default using the `default` keyword.

**Product.js**
```typescript
export default interface {
    name:string;
    unitPrice:number;
}
```

**OrderDetail.js**

```typescript
import Product from '/product';
```