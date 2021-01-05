# expression-eval

[![Latest NPM release](https://img.shields.io/npm/v/expression-eval.svg)](https://www.npmjs.com/package/expression-eval)
[![Minzipped size](https://badgen.net/bundlephobia/minzip/expression-eval)](https://bundlephobia.com/result?p=expression-eval)
[![License](https://img.shields.io/badge/license-MIT-007ec6.svg)](https://github.com/donmccurdy/expression-eval/blob/master/LICENSE)
[![CI](https://github.com/donmccurdy/expression-eval/workflows/CI/badge.svg?branch=master&event=push)](https://github.com/donmccurdy/expression-eval/actions?query=workflow%3ACI)

JavaScript expression parsing and evaluation.

Powered by [jsep](https://github.com/soney/jsep).

## Installation

Install:

```
npm install --save expression-eval
```

Import:

```js
// ES6
import { parse, eval } from 'expression-eval';
// CommonJS
const { parse, eval } = require('expression-eval');
// UMD / standalone script
const { parse, eval } = window.expressionEval;
```

## API

### Parsing

```javascript
import { parse } from 'expression-eval';
const ast = parse('1 + foo');
```

The result of the parse is an AST (abstract syntax tree), like:

```json
{
  "type": "BinaryExpression",
  "operator": "+",
  "left": {
    "type": "Literal",
    "value": 1,
    "raw": "1"
  },
  "right": {
    "type": "Identifier",
    "name": "foo"
  }
}
```

### Evaluation

```javascript
import { parse, eval } from 'expression-eval';
const ast = parse('a + b / c'); // abstract syntax tree (AST)
const value = eval(ast, {a: 2, b: 2, c: 5}); // 2.4
```

Alternatively, use `evalAsync` for asynchronous evaluation.

### Compilation

```javascript
import { compile } from 'expression-eval';
const fn = compile('foo.bar + 10');
fn({foo: {bar: 'baz'}}); // 'baz10'
```

Alternatively, use `compileAsync` for asynchronous compilation.

## Security

Although this package does [avoid the use of `eval()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval#Do_not_ever_use_eval!), it _cannot guarantee that user-provided expressions, or user-provided inputs to evaluation, will not modify the state or behavior of your application_. Always use caution when combining user input and dynamic evaluation, and avoid it where possible.

For example:

```js
const ast = expr.parse('foo[bar](baz)()');
expr.eval(ast, {
  foo: String,
  bar: 'constructor',
  baz: 'console.log("im in ur logs");'
});
// Prints: "im in ur logs"
```

The kinds of expressions that can expose vulnerabilities can be more subtle than this, and are sometimes possible even in cases where users only provide primitive values as inputs to pre-defined expressions.

## License

MIT License.
