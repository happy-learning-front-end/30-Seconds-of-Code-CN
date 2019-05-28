#### Function

##### 01|attempt(尝试) 

*尝试使用提供的参数调用函数，返回结果或捕获的错误对象。*

*使用try ... catch块返回函数的结果或适当的错误。*

```js
const attempt = (fn, ...args) => {
  try {
    return fn(...args);
  } catch (e) {
    return e instanceof Error ? e : new Error(e);
  }
};
```

**Example**

```js
var elements = attempt(function(selector) {
  return document.querySelectorAll(selector);
}, '>_>');
if (elements instanceof Error) elements = []; // elements = []
```



##### 02|bind(绑定)

*创建一个使用给定上下文调用fn的函数，可选地将任何其他提供的参数添加到参数的开头。*

*返回一个使用**Function.prototype.apply（）**的函数将给定的上下文应用于fn。使用**Array.prototype.concat（）**将任何其他提供的参数添加到参数中。*

```js
const bind = (fn, context, ...boundArgs) => (...args) => fn.apply(context, [...boundArgs, ...args]);
```

**Example**

```js
function greet(greeting, punctuation) {
  return greeting + ' ' + this.user + punctuation;
}
const freddy = { user: 'fred' };
const freddyBound = bind(greet, freddy);
console.log(freddyBound('hi', '!')); // 'hi fred!'
```



##### 03|bindKey

*创建一个函数，该函数在对象的给定键处调用该方法，可选地将任何其他提供的参数添加到参数的开头。*

*返回一个使用**Function.prototype.apply（）**将**context [fn]**绑定到context的函数。使用扩展运算符（...）将任何其他提供的参数添加到参数中。*

```js
const bindKey = (context, fn, ...boundArgs) => (...args) =>
  context[fn].apply(context, [...boundArgs, ...args]);
```

**Example**

```js
const freddy = {
  user: 'fred',
  greet: function(greeting, punctuation) {
    return greeting + ' ' + this.user + punctuation;
  }
};
const freddyBound = bindKey(freddy, 'greet');
console.log(freddyBound('hi', '!')); // 'hi fred!'
```



##### 04|chainAsync 链接异步

链接异步函数。 循环遍历包含异步事件的函数数组，在每个异步事件完成时调用next

```js
onst chainAsync = fns => {
  let curr = 0;
  const last = fns[fns.length - 1];
  const next = () => {
    const fn = fns[curr++];
    fn === last ? fn() : fn(next);
  };
  next();
};
```

**Example**

```js
chainAsync([
  next => {
    console.log('0 seconds');
    setTimeout(next, 1000);
  },
  next => {
    console.log('1 second');
    setTimeout(next, 1000);
  },
  () => {
    console.log('2 second');
  }
]);
```



##### 05|checkProp

*给定谓词函数和prop字符串，这个curried函数将通过调用属性并将其传递给谓词来检查对象。 在obj上召唤prop，将其传递给提供的谓词函数并返回一个屏蔽的布尔值。*

```js
const checkProp = (predicate, prop) => obj => !!predicate(obj[prop]);
```

**Example**

```js
const lengthIs4 = checkProp(l => l === 4, 'length');
lengthIs4([]); // false
lengthIs4([1,2,3,4]); // true
lengthIs4(new Set([1,2,3,4])); // false (Set uses Size, not length)

const session = { user: {} };
const validUserSession = checkProps(u => u.active && !u.disabled, 'user');

validUserSession(session); // false

session.user.active = true;
validUserSession(session); // true

const noLength(l => l === undefined, 'length');
noLength([]); // false
noLength({}); // true
noLength(new Set()); // true
```



##### 06|compose

*执行从右到左的功能组合。*

*使用**Array.prototype.reduce（）**执行从右到左的函数组合。最后（最右边）函数可以接受一个或多个参数;其余的功能必须是一元的。*

```js
const compose = (...fns) => fns.reduce((f, g) => (...args) => f(g(...args)));
```

**Example**

```js
const add5 = x => x + 5;
const multiply = (x, y) => x * y;
const multiplyAndAdd5 = compose(
  add5,
  multiply
);
multiplyAndAdd5(5, 2); // 15
```



##### 07|composeRight

*执行从左到右的功能组合。*

*使用**Array.prototype.reduce（）**执行从左到右的函数组合。第一个（最左边）函数可以接受一个或多个参数;其余的功能必须是一元的。*

```js
const composeRight = (...fns) => fns.reduce((f, g) => (...args) => g(f(...args)));
```

**Example**

```js
const add = (x, y) => x + y;
const square = x => x * x;
const addAndSquare = composeRight(add, square);
addAndSquare(1, 2); // 9
```



##### 09|converge

*接受聚合函数和分支函数列表，并返回将每个分支函数应用于参数的函数，并将分支函数的结果作为参数传递给聚合函数。* 

*使用**Array.prototype.map（）和Function.prototype.apply（）**将每个函数应用于给定的参数。使用**扩展运算符（...）**使用所有其他函数的结果调用coverger。*

```js
const converge = (converger, fns) => (...args) => converger(...fns.map(fn => fn.apply(null, args)));
```

**Example**

```js
const average = converge((a, b) => a / b, [
  arr => arr.reduce((a, v) => a + v, 0),
  arr => arr.length
]);
average([1, 2, 3, 4, 5, 6, 7]); // 4
```



##### 10|curry

*提供功能。* 

*使用递归。如果提供的参数（args）数量足够，则调用传递的函数fn。否则，返回一个**curried函数fn**，它需要其余的参数。如果你想要一个接受可变数量的参数的函数（一个可变参数函数，例如**Math.min（））**，你可以选择将参数个数传递给第二个参数arity。*

```js
const curry = (fn, arity = fn.length, ...args) =>
  arity <= args.length ? fn(...args) : curry.bind(null, fn, arity, ...args);
```

**Example**

```js
curry(Math.pow)(2)(10); // 1024
curry(Math.min, 3)(10)(50)(2); // 2
```



##### 11|debounce

*创建一个去抖动函数，该函数延迟调用所提供的函数，直到自上次调用它起至少经过ms毫秒。* *每次调用**debounced**函数时，使用**clearTimeout（）**清除当前的挂起超时，并使用**setTimeout（）**创建一个新的超时，延迟调用函数，直到至少ms毫秒为止。使用**Function.prototype.apply（）**将此上下文应用于函数并提供必要的参数。省略第二个参数ms，将超时设置为默认值0 ms*

```js
const debounce = (fn, ms = 0) => {
  let timeoutId;
  return function(...args) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => fn.apply(this, args), ms);
  };
};
```

**Example**

```js
window.addEventListener(
  'resize',
  debounce(() => {
    console.log(window.innerWidth);
    console.log(window.innerHeight);
  }, 250)
); // Will log the window dimensions at most every 250ms
```



##### 12|defer

*延迟调用函数，直到当前调用堆栈清除为止。 使用超时为**1ms**的**setTimeout（）**将新事件添加到浏览器事件队列，并允许呈现引擎完成其工作。使用**spread（...）**运算符为函数提供任意数量的参数。*

```js
const defer = (fn, ...args) => setTimeout(fn, 1, ...args);
```

**Example**

```js
// Example A:
defer(console.log, 'a'), console.log('b'); // logs 'b' then 'a'

// Example B:
document.querySelector('#someElement').innerHTML = 'Hello';
longRunningFunction(); // Browser will not update the HTML until this has finished
defer(longRunningFunction); // Browser will update the HTML then run the function
```



##### 13|delay

*在等待毫秒后调用提供的函数。 使用**setTimeout（）**来延迟执行fn。使用spread（...）运算符为函数提供任意数量的参数。*

```js
const delay = (fn, wait, ...args) => setTimeout(fn, wait, ...args);
```

**Example**

```js
delay(
  function(text) {
    console.log(text);
  },
  1000,
  'later'
); // Logs 'later' after one second.
```



##### 14|functionName

*记录函数的名称。* 

*使用**console.debug（）**和传递方法的name属性将方法的名称记录到控制台的调试通道。*

```js
const functionName = fn => (console.debug(fn.name), fn);
```

**Example**

```js
functionName(Math.max); // max (logged in debug channel of console)
```



##### 15|hz

*返回每秒执行函数的次数。 hz是赫兹的单位，频率单位定义为每秒一个周期。 使用**performance.now（）**来获取迭代循环之前和之后的毫秒差异，以计算执行函数迭代次数所经过的时间。通过将毫秒转换为秒并将其除以经过的时间来返回每秒的周期数。省略第二个参数**iterations**，使用默认的**100**次迭代。*

```js
const hz = (fn, iterations = 100) => {
  const before = performance.now();
  for (let i = 0; i < iterations; i++) fn();
  return (1000 * iterations) / (performance.now() - before);
};
```

**Example**

```js
// 10,000 element array
const numbers = Array(10000)
  .fill()
  .map((_, i) => i);

// Test functions with the same goal: sum up the elements in the array
const sumReduce = () => numbers.reduce((acc, n) => acc + n, 0);
const sumForLoop = () => {
  let sum = 0;
  for (let i = 0; i < numbers.length; i++) sum += numbers[i];
  return sum;
};

// `sumForLoop` is nearly 10 times faster
Math.round(hz(sumReduce)); // 572
Math.round(hz(sumForLoop)); // 4784
```



##### 16|memoize

*返回memoized（缓存）函数。* 

*通过实例化新的Map对象来创建空缓存。返回一个函数，它通过首先检查函数的特定输入值的输出是否已经被缓存，将一个参数提供给memoized函数，如果没有则存储并返回它。必须使用function关键字，以便允许memoized函数在必要时更改其上下文。允许通过将其设置为返回函数的属性来访问缓存。*

```js
const memoize = fn => {
  const cache = new Map();
  const cached = function(val) {
    return cache.has(val) ? cache.get(val) : cache.set(val, fn.call(this, val)) && cache.get(val);
  };
  cached.cache = cache;
  return cached;
};
```

**Example**

```js
// See the `anagrams` snippet.
const anagramsCached = memoize(anagrams);
anagramsCached('javascript'); // takes a long time
anagramsCached('javascript'); // returns virtually instantly since it's now cached
console.log(anagramsCached.cache); // The cached anagrams map
```



##### 17|negate

否定谓词函数。 获取谓词函数并使用其参数将not运算符（！）应用于它。

```js
const negate = func => (...args) => !func(...args);
```

**Example**

```js
[1, 2, 3, 4, 5, 6].filter(negate(n => n % 2 === 0)); // [ 1, 3, 5 ]
```



##### 18|once

*确保只调用一次函数。 使用一个闭包，使用一个被调用的标志，并在第一次调用该函数时将其设置为**true**，防止再次调用它。为了允许函数更改其上下文（例如在事件侦听器中），必须使用**function**关键字，并且所提供的函数必须应用上下文。允许使用**rest / spread（...）**运算符为函数提供任意数量的参数。*

```js
const once = fn => {
  let called = false;
  return function(...args) {
    if (called) return;
    called = true;
    return fn.apply(this, args);
  };
};
```

**Example**

```js
const startApp = function(event) {
  console.log(this, event); // document.body, MouseEvent
};
document.body.addEventListener('click', once(startApp)); // only runs `startApp` once upon click
```



##### 19|partial

*创建一个函数，该函数调用fn，并在其接收的参数前加上partial。 使用扩展运算符（...）将partials添加到fn的参数列表中。*

```js
onst partial = (fn, ...partials) => (...args) => fn(...partials, ...args);
```

**Example**

```js
const greet = (greeting, name) => greeting + ' ' + name + '!';
const greetHello = partial(greet, 'Hello');
greetHello('John'); // 'Hello John!'
```



##### 20|partalRight

*创建一个函数，该函数调用fn，并将partials附加到它接收的参数。*

*使用spread运算符（...）将partials附加到fn的参数列表中。*

```js
const partialRight = (fn, ...partials) => (...args) => fn(...args, ...partials);
```

**Example**

```js
const greet = (greeting, name) => greeting + ' ' + name + '!';
const greetJohn = partialRight(greet, 'John');
greetJohn('Hello'); // 'Hello John!'
```



##### 21|runPromisesInSeries

*运行一系列承诺串联。* 

*使用Array.prototype.reduce（）创建一个promise链，其中每个promise在解析时返回下一个promise。*

```js
const runPromisesInSeries = ps => ps.reduce((p, next) => p.then(next), Promise.resolve());
```

**Example**

```js
const delay = d => new Promise(r => setTimeout(r, d));
runPromisesInSeries([() => delay(1000), () => delay(2000)]); // Executes each promise sequentially, taking a total of 3 seconds to complete
```



##### 22|sleep

*延迟异步函数的执行。 延迟执行异步函数的一部分，通过将其置于休眠状态，返回Promise*。

```js
const sleep = ms => new Promise(resolve => setTimeout(resolve, ms));
```

EXAMPLES

```js
async function sleepyWork() {
  console.log("I'm going to sleep for 1 second.");
  await sleep(1000);
  console.log('I woke up after 1 second.');
}
```



##### 23|throttle

*创建一个限制函数，每隔几毫秒只调用一次提供的函数 使用**setTimeout（）**和**clearTimeout（）**来限制给定的方法fn。使用**Function.prototype.apply（）**将此上下文应用于函数并提供必要的参数。使用**Date.now（）**来跟踪上次调用限制函数的时间。省略第二个参数**wait**，将超时设置为默认值0 ms。*

```js
onst throttle = (fn, wait) => {
  let inThrottle, lastFn, lastTime;
  return function() {
    const context = this,
      args = arguments;
    if (!inThrottle) {
      fn.apply(context, args);
      lastTime = Date.now();
      inThrottle = true;
    } else {
      clearTimeout(lastFn);
      lastFn = setTimeout(function() {
        if (Date.now() - lastTime >= wait) {
          fn.apply(context, args);
          lastTime = Date.now();
        }
      }, Math.max(wait - (Date.now() - lastTime), 0));
    }
  };
};
```

**Example**

```js
window.addEventListener(
  'resize',
  throttle(function(evt) {
    console.log(window.innerWidth);
    console.log(window.innerHeight);
  }, 250)
); // Will log the window dimensions at most every 250ms
```



##### 24|times

迭代回调n次 使用**Function.call（）**调用fn n次或直到它返回false。省略最后一个参数**context**，以使用未定义的对象（或非严格模式下的全局对象）。

```js
const times = (n, fn, context = undefined) => {
  let i = 0;
  while (fn.call(context, i) !== false && ++i < n) {}
};
```

**Example**

```js
var output = '';
times(5, i => (output += i));
console.log(output); // 01234
```



##### 25|uncurry

*解决深度为n的函数。 返回一个可变函数。*

*在提供的参数上使用**Array.prototype.reduce（）**来调用函数的每个后续curry级别。如果提供的参数的长度小于n则抛出错误。否则，使用**Array.prototype.slice（0，n）**使用适当数量的参数调用fn。省略第二个参数n，以保证深度为1。*

```js
const uncurry = (fn, n = 1) => (...args) => {
  const next = acc => args => args.reduce((x, y) => x(y), acc);
  if (n > args.length) throw new RangeError('Arguments too few!');
  return next(fn)(args.slice(0, n));
};
```

**EXAMPLES**

```js
const add = x => y => z => x + y + z;
const uncurriedAdd = uncurry(add, 3);
uncurriedAdd(1, 2, 3); // 6
```



##### 26|unfold

使用迭代器函数和初始种子值构建数组。 使用while循环和Array.prototype.push（）重复调用该函数，直到它返回false。迭代器函数接受一个参数（种子），并且必须始终返回一个包含两个元素（[value，nextSeed]）或false的数组以终止。

```js
onst unfold = (fn, seed) => {
  let result = [],
    val = [null, seed];
  while ((val = fn(val[1]))) result.push(val[0]);
  return result;
};
```

**Example**

```js
var f = n => (n > 50 ? false : [-n, n + 10]);
unfold(f, 10); // [-10, -20, -30, -40, -50]
```



##### 27|when

针对谓词函数测试值x。如果为true，则返回fn（x）。否则，返回x。 返回一个期望单个值x的函数，它返回基于pred的适当值。

```js
const when = (pred, whenTrue) => x => (pred(x) ? whenTrue(x) : x);
```

**Example**

```js
const doubleEvenNumbers = when(x => x % 2 === 0, x => x * 2);
doubleEvenNumbers(2); // 4
doubleEvenNumbers(1); // 1
```