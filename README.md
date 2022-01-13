# TypeScript Modules

This repository discuss how TypeScript is dealing with different JavaScript modules it faced like packages,
And understanding configurations related to the modules it accepts, targets and the context of the destination executer.

## Links

### [Modules](https://www.typescriptlang.org/docs/handbook/modules.html)

### [target](https://www.typescriptlang.org/tsconfig#target)

### [lib](https://www.typescriptlang.org/tsconfig#lib)

### [Interop_Constraints](https://www.typescriptlang.org/tsconfig#Interop_Constraints_6252)

---

### [esModuleInterop](https://www.typescriptlang.org/tsconfig#esModuleInterop)

By default (with esModuleInterop false or not set) TypeScript treats CommonJS/AMD/UMD modules similar to ES6 modules. In doing this, there are two parts in particular which turned out to be flawed assumptions:

- a namespace import like import * as moment from "moment" acts the same as const moment = require("moment")

- a default import like import moment from "moment" acts the same as const moment = require("moment").default

This mis-match causes these two issues:

- the ES6 modules spec states that a namespace import (import * as x) can only be an object, by having TypeScript treating it the same as = require("x") then TypeScript allowed for the import to be treated as a function and be callable. That’s not valid according to the spec.

- while accurate to the ES6 modules spec, most libraries with CommonJS/AMD/UMD modules didn’t conform as strictly as TypeScript’s implementation.

Turning on esModuleInterop will fix both of these problems in the code transpiled by TypeScript. The first changes the behavior in the compiler, the second is fixed by two new helper functions which provide a shim to ensure compatibility in the emitted JavaScript:

```typescript
// source module myModule.ts
import * as fs from "fs";
import _ from "lodash";
fs.readFileSync("file.txt", "utf8");
_.chunk(["a", "b", "c", "d"], 2);
```

```javascript
// output module myModule.js, with no esModuleInterop flag
"use strict";
Object.defineProperty(exports, "__esModule", { value: true });
const fs = require("fs");
const lodash_1 = require("lodash");
fs.readFileSync("file.txt", "utf8");
lodash_1.default.chunk(["a", "b", "c", "d"], 2);
```

Now with `esModuleInterop` flag

```javascript
// output module output when using `esModuleInterop`
"use strict";
var __createBinding = (this && this.__createBinding) || (Object.create ? (function(o, m, k, k2) {
    if (k2 === undefined) k2 = k;
    Object.defineProperty(o, k2, { enumerable: true, get: function() { return m[k]; } });
}) : (function(o, m, k, k2) {
    if (k2 === undefined) k2 = k;
    o[k2] = m[k];
}));
var __setModuleDefault = (this && this.__setModuleDefault) || (Object.create ? (function(o, v) {
    Object.defineProperty(o, "default", { enumerable: true, value: v });
}) : function(o, v) {
    o["default"] = v;
});
var __importStar = (this && this.__importStar) || function (mod) {
    if (mod && mod.__esModule) return mod;
    var result = {};
    if (mod != null) for (var k in mod) if (k !== "default" && Object.prototype.hasOwnProperty.call(mod, k)) __createBinding(result, mod, k);
    __setModuleDefault(result, mod);
    return result;
};
var __importDefault = (this && this.__importDefault) || function (mod) {
    return (mod && mod.__esModule) ? mod : { "default": mod };
};
Object.defineProperty(exports, "__esModule", { value: true });
const fs = __importStar(require("fs"));
const lodash_1 = __importDefault(require("lodash"));
fs.readFileSync("file.txt", "utf8");
lodash_1.default.chunk(["a", "b", "c", "d"], 2);
```

---

[allowSyntheticDefaultImports](https://www.typescriptlang.org/tsconfig#allowSyntheticDefaultImports)

---

### off-topic `"strict mode"`

The `"use strict"` Directive
The `"use strict"` directive was new in ECMAScript version 5.

It is not a statement, but a literal expression, ignored by earlier versions of JavaScript.

The purpose of `"use strict"` is to indicate that the code should be executed in "strict mode".

With strict mode, you can not, for example, use undeclared variables.
