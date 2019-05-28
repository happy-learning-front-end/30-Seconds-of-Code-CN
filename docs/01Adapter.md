#### 01|Adapter(适配器)

##### 01|ary(元)

*创建一个最多接受n个参数的函数，忽略任何其他参数。 使用**Array.prototype.slice（0，n）**和扩展运算符**（...）**，使用最多n个参数调用提供的函数**fn**。*

```js
const ary = (fn, n) => (...args) => fn(...args.slice(0, n));
```

**Example**

```js
const firstTwoMax = ary(Math.max, 2);
[[2, 6, 'a'], [8, 4, 6], [10]].map(x => firstTwoMax(...x)); // [6, 8, 10]
```



##### 02|call(调用)

*给定一个键和一组参数，在给定上下文时调用它们。主要用于组合物。 使用闭包来调用存储的参数的存储键。*

**Example**

```js
const call = (key, ...args) => context => context[key](...args);
```

**Example**

```js
Promise.resolve([1, 2, 3])
  .then(call('map', x => 2 * x))
  .then(console.log); // [ 2, 4, 6 ]
const map = call.bind(null, 'map');
Promise.resolve([1, 2, 3])
  .then(map(x => 2 * x))
  .then(console.log); // [ 2, 4, 6 ]
```



##### 03|collectinto(收集器)

*将接受数组的函数更改为可变参数函数。 给定一个函数，返回一个闭包，它将所有输入收集到一个接受数组的函数中。*

```js
onst collectInto = fn => (...args) => fn(args);
```

**Example**

```js
const Pall = collectInto(Promise.all.bind(Promise));
let p1 = Promise.resolve(1);
let p2 = Promise.resolve(2);
let p3 = new Promise(resolve => setTimeout(resolve, 2000, 3));
Pall(p1, p2, p3).then(console.log); // [1, 2, 3] (after about 2 seconds)
```



##### 04|flip(翻动)

*Flip将函数作为参数，然后将第一个参数作为最后一个参数。 返回一个接受可变输入的闭包，并拼接最后一个参数，使其成为应用其余参数之前的第一个参数。*

```js
const flip = fn => (first, ...rest) => fn(...rest, first);
```

**Example**

```js
let a = { name: 'John Smith' };
let b = {};
const mergeFrom = flip(Object.assign);
let mergePerson = mergeFrom.bind(null, a);
mergePerson(b); // == b
b = {};
Object.assign(b, a); // == b
```



##### 05|over(多余)

*创建一个函数，该函数使用它接收的参数调用每个提供的函数并返回结果。 使用**Array.prototype.map（）**和**Function.prototype.apply（）**将每个函数应用于给定的参数。*

```js
const over = (...fns) => (...args) => fns.map(fn => fn.apply(null, args));
```

**Example**

```js
const minMax = over(Math.min, Math.max);
minMax(1, 2, 3, 4, 5); // [1,5]
```



##### 06|overArgs(多余的参数)

*创建一个函数，通过转换参数调用提供的函数。 使用**Array.prototype.map（）**将变换应用于**args**并与**spread**运算符**（...）**一起将转换后的参数传递给**fn**。*

```js
const over = (...fns) => (...args) => fns.map(fn => fn.apply(null, args));
```

**Example**

```js
const minMax = over(Math.min, Math.max);
minMax(1, 2, 3, 4, 5); // [1,5]
```



##### 07|pipeAsyncFunctions(管道异步功能)

*为异步函数执行从左到右的函数组合。* 

*使用带有扩展运算符**（...）**的**Array.prototype.reduce（）**来使用**Promise.then（）**执行从左到右的函数组合。这些函数可以返回以下组合：简单值，**Promise**，或者它们可以定义为通过**await**返回的**async**。所有功能必须是一元的。*

```js
const pipeAsyncFunctions = (...fns) => arg => fns.reduce((p, f) => p.then(f), Promise.resolve(arg));
```

**Example**

```js
const sum = pipeAsyncFunctions(
  x => x + 1,
  x => new Promise(resolve => setTimeout(() => resolve(x + 2), 1000)),
  x => x + 3,
  async x => (await x) + 4
);
(async() => {
  console.log(await sum(5)); // 15 (after one second)
})();
```



[**Recommended Resource - ES6: The Right Parts**](https://frontendmasters.com/courses/es6-right-parts/)

**推荐资源 - ES6：正确的部件 (对应的教学资源来自Front master!)**



##### 08|pipeFunctions(管道函数)

*执行从左到右的功能组合。 *

*使用带有扩展运算符* *（...）*  **Array.prototype.reduce（）**来执行从左到右的函数组合。第一个（最左边）函数可以接受一个或多个参数;其余的功能必须是一元的。*

```js
const pipeFunctions = (...fns) => fns.reduce((f, g) => (...args) => g(f(...args)));
```

**Example**

```js
const add5 = x => x + 5;
const multiply = (x, y) => x * y;
const multiplyAndAdd5 = pipeFunctions(multiply, add5);
multiplyAndAdd5(5, 2); // 15
```



##### 09|promisify(允诺)

*转换异步函数以返回**promise**。*

*使用currying返回一个函数，返回一个调用原始函数的**Promise**。使用**... rest**运算符传入所有参数。* 

*在Node 8+中，您可以使用**util.promisify***

```js
const promisify = func => (...args) =>
  new Promise((resolve, reject) =>
    func(...args, (err, result) => (err ? reject(err) : resolve(result)))
  );
```

**Example**

```js
const delay = promisify((d, cb) => setTimeout(cb, d));
delay(2000).then(() => console.log('Hi!')); // // Promise resolves after 2s
```



##### 10|rearg

*创建一个函数，该函数调用提供的函数，其参数根据指定的索引排列。 使用**Array.prototype.map（）**根据**indexes**和扩展运算符**（...）**重新排序参数，以将转换后的参数传递给**fn**。*

```js
const rearg = (fn, indexes) => (...args) => fn(...indexes.map(i => args[i]));
```

**Example**

```js
var rearged = rearg(
  function(a, b, c) {
    return [a, b, c];
  },
  [2, 0, 1]
);
rearged('b', 'c', 'a'); // ['a', 'b', 'c']
```



##### 11|spreadOver(传播开来)

*采用可变参数函数并返回一个闭包，该闭包接受一个参数数组以映射到函数的输入。 使用闭包和扩展运算符**（...）**将参数数组映射到函数的输入。*

```js
const spreadOver = fn => argsArr => fn(...argsArr);
```

**Example**

```js
const arrayMax = spreadOver(Math.max);
arrayMax([1, 2, 3]); // 3
```



##### 12|unary(一元)

创建一个最多接受一个参数的函数，忽略任何其他参数。 

仅使用给定的第一个参数调用提供的函数**fn**。

```js
const unary = fn => val => fn(val);
```

**Example**

```js
['6', '8', '10'].map(unary(parseInt)); // [6, 8, 10]
```

