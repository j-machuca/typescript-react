# Modules

## Index

---

1. [Structuring code into Modules](#modules)
2. [Exporting](#exporting)
3. [Importing](#importing)
4. [Default Exports](#default-exports)
5. [Compilation](#configuring-compilation)
6. [The tsconfig.json file](#tsconfigjson)


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

### Configuring Compilation

#### Common Options

1. `--target` 
    - **ES3** defaults to 
    - **ESNext** Latest supported features of the ECMAScript.
2. `--outDir` 
    - By default the transpiled JS files are created in the same directory.
3. `--module` 
    - Defaults to **CommonJS** (check [Modules](#modules)) for other options.
4. `--allowJS` 
    - Tells the TypeScript compiler to process Javascript files as well.
5. `--watch` 
    - allows for constant recompiling of the code.
6. `--noImplicityAny`
    - Forces to specify to either specify a type or explicitly use the `any` keyword. In this case Typescript will not infer the `any` type.  
7. `--noImplicitReturns`
    - Forces us to return a value in all branches of a function unless `void` is explicitly defined.
8. `--sourceMap` 
    - Creates a `*.map` during the transpilation process. 
9. `--moduleResolution` 
    - Tells Typescript how to resolve modules.
    - Common values are `classic` or `node`. Using the node value for module resolution tells the compiler to look for modules in `node_modules`.
10. `--strictNullChecks` 
    - when set to `false`, `null` and `undefined` are ignored by typescript.
    - when set to `true`, `null` and `undefined` have their own distinct types and will produce a type error where a concrete value is expected.


### **tsconfig.json**

Instead of specifying the different flags everytime we need to compile we can specify them once in the `tsconfig.json` file.

```json
// Basic Configuration 
{
 "compilerOptions": {
    "target": "esnext",
    "outDir": "dist",
    "module": "es6",
    "moduleResolution": "node",
    "sourceMap": true,
    "noImplicitReturns": true,
    "noImplicitAny": true
    }      
}
```

We can specify the files and folders to be checked or ignored by the compiler as well. 

```json
{
    "compilerOptions":{
        ...
    },
    "include":["src/**/*"]
}
```

## **TSLint**

The TSLint extensions allows us to define certain linting rules.

**Installation**

```bash
npm install -g tslint
```

**Set up the Extension**

Go to the IDE and install the extension.

**Configure the rules**

the `tslint` module and the IDE extension will check the `tslint.json` for the rules.

Full list of [rules](https://palantir.github.io/tslint/rules/). 

**tslint.json**

```json
{
    "rules":{
        // foces us to declare access modifiers
        "member-access" true
    }
}
```
**Built-in rules**

We can extend the default `tslint` built-in rulesets.

**tslint.json**
```json
{
    // Extending built in rules
    "extends" : ["tslint:recommended"],
    // Our own rules
    "rules" : {
        "interface-name":false,
    }
}
```

**Excluding Files**

We can exclude files from the linting process. 

*Useful for 3rd party code*

```json
{
    "extends":["tslint:recommended"],
    "linterOptions":{
        "exclude":[
            "node_modules/**/*.ts"
            ]
    }
}
```