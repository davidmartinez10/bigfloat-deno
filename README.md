[![BigFloat](https://raw.githubusercontent.com/davidmartinez10/bigfloat-esnext/master/bigfloat.jpg)](https://github.com/davidmartinez10/bigfloat-deno)

A library for arbitrary precision decimal floating point arithmetic that can exactly represent all decimal fractions,
unlike JavaScript's number data type which is 64-bit binary floating point.

Based on the original work by Douglas Crockford.
This implementation is built upon ES native bigints.

This library provides three ways to make bigfloat operations:
  - A set of functions for a functional style approach
  - A BigFloat class with an API similar to that of Decimal.js
  - An evaluate() function that parses and resolves an expression

# Basic usage
### Functional style
```typescript
import { make, string, sqrt } from "https://deno.land/x/bigfloat/mod.ts";

string(sqrt(make("2")));               // "1.4142"
```

### Class based
```typescript
import { BigFloat } from "https://deno.land/x/bigfloat/mod.ts";

new BigFloat("2").sqrt().toString();   // "1.4142"
```

### The evaluate() function
```typescript
evaluate(expression: string, precision?: number): string | boolean
```
>The first argument can be any valid arithmetic or relational expression, including scientific e-notation.
>Precision should be a negative integer. Default is -4.
```typescript
import { evaluate } from "https://deno.land/x/bigfloat/mod.ts";

0.1 + 0.2 === 0.3;                     // false
evaluate("0.1 + 0.2 == 0.3");          // true

0.1 + 0.2;                             // 0.30000000000000004
evaluate("0.1 + 0.2");                 // "0.3"

1 + Number.EPSILON / 2;                // 1
evaluate(`1 + ${Number.EPSILON / 2}`); // "1.00000000000000011102230246251565"

evaluate("1 + 2.220446049250313e-16"); // "1.0000000000000002220446049250313"

evaluate(`4 >= ${Math.PI}`);           // true
```

Valid tokens:
  - Parenthesis: (,)
  - Number: Decimal, integer or scientific e-notation
  - Operator: Arithmetic +,-,/,*,** Relational =\=\=,=\=,!==,!=,<,>,<=,>=

### Change precision
```typescript
import { BigFloat, set_precision } from "https://deno.land/x/bigfloat/mod.ts";

new BigFloat(2).sqrt().toString();     // "1.4142"
set_precision(-10);
new BigFloat(2).sqrt().toString();     // "1.4142135623"
```

### The bigfloat object
```typescript
interface IBigFloat {
  coefficient: bigint;
  exponent: number;
}
```

Valid bigfloat made from primitives:
```typescript
const bigfloat: IBigFloat {
  coefficient: 522299n,
  exponent: -4
};
```
