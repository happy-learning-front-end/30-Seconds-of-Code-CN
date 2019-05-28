#### Type

##### 01|getType

*返回值的本机类型。 返回值的小写构造函数名称，如果**value**未定义或为**null**，则返回“**undefined**”或“**null**”。*

```js
const getType = v =>
  v === undefined ? 'undefined' : v === null ? 'null' : v.constructor.name.toLowerCase();
```

**Example**

```js
getType(new Set([1, 2, 3])); // 'set'
```



##### 02|is

*检查提供的值是否为指定类型。 使用**Array.prototype.includes（）**确保值未定义或为**null**，并将值的构造函数属性与type进行比较，以检查提供的值是否为指定类型。*

```js
const is = (type, val) => ![, null].includes(val) && val.constructor === type;
```

**Example**

```js
is(Array, [1]); // true
is(ArrayBuffer, new ArrayBuffer()); // true
is(Map, new Map()); // true
is(RegExp, /./g); // true
is(Set, new Set()); // true
is(WeakMap, new WeakMap()); // true
is(WeakSet, new WeakSet()); // true
is(String, ''); // true
is(String, new String('')); // true
is(Number, 1); // true
is(Number, new Number(1)); // true
is(Boolean, true); // true
is(Boolean, new Boolean(true)); // true
```



##### 03|isArrayLike

*检查提供的参数是否类似于数组（即可迭代）。 检查提供的参数是否为null，并且其**Symbol.iterator**属性是否为函数。*

```js
const isArrayLike = obj => obj != null && typeof obj[Symbol.iterator] === 'function';
```

**Example**

```js
isArrayLike(document.querySelectorAll('.className')); // true
isArrayLike('abc'); // true
isArrayLike(null); // false
```



##### 04|isBoolean

*检查给定的参数是否是本机布尔元素。 使用typeof检查值是否归类为布尔基元。*

```js
const isBoolean = val => typeof val === 'boolean';
```

**Example**

```js
isBoolean(null); // false
isBoolean(false); // true
```



##### 05|isEmpty

*如果a值是空对象，集合，没有可枚举属性或任何不被视为集合的类型，则返回true。 检查提供的值是否为null或其长度是否等于0。*

```js
const isEmpty = val => val == null || !(Object.keys(val) || val).length;
```

**Example**

```js
isEmpty([]); // true
isEmpty({}); // true
isEmpty(''); // true
isEmpty([1, 2]); // false
isEmpty({ a: 1, b: 2 }); // false
isEmpty('text'); // false
isEmpty(123); // true - type is not considered a collection
isEmpty(true); // true - type is not considered a collection
```



##### 06|isFunction

*检查给定的参数是否是函数。 使用typeof检查值是否归类为函数原语。*

```js
const isFunction = val => typeof val === 'function';
```

**Example**

```js
isFunction('x'); // false
isFunction(x => x); // true
```



##### 07|isNil

*如果指定的值为**null**或未定义，则返回**true**，否则返回**false**。 使用**strict equality**运算符检查val和val的值是否等于**null**或**undefined**。*

```js
const isNil = val => val === undefined || val === null;
```

**Example**

```js
isNil(null); // true
isNil(undefined); // true
```



##### 08|isNull

*如果指定的值为**null**，则返回**true**，否则返回**false**。 使用**strict equality**运算符检查**val**和**val**的值是否等于**null**。*

```js
const isNull = val => val === null;
```

**Example**

```js
isNull(null); // true
```



##### 09|isNumber

*检查给定的参数是否为数字。 使用typeof检查值是否归类为数字原语。为了防止NaN，检查**val === val**（因为**NaN**的**typeof**等于**number**并且是唯一不等于它自己的值）。*

```js
const isNumber = val => typeof val === 'number' && val === val;
```

**Example**

```js
isNumber(1); // true
isNumber('1'); // false
isNumber(NaN); // false
```



##### 10|isObject

*返回一个布尔值，确定传递的值是否为对象。 使用Object构造函数为给定值创建对象包装器。如果值为null或未定义，则创建并返回一个空对象。否则，返回与给定值对应的类型的对象。*

```js
const isObject = obj => obj === Object(obj);
```

**Example**



```js
isObject([1, 2, 3, 4]); // true
isObject([]); // true
isObject(['Hello!']); // true
isObject({ a: 1 }); // true
isObject({}); // true
isObject(true); // false
```



##### 11|isObjectLike

*检查值是否类似于对象。 检查提供的值是否为null，其typeof是否等于'object'。*

```js
const isObjectLike = val => val !== null && typeof val === 'object';
```

**Exmaple**



```js
isObjectLike({}); // true
isObjectLike([1, 2, 3]); // true
isObjectLike(x => x); // false
isObjectLike(null); // false
```



##### 12|isPlainObject

*检查提供的值是否是**Object**构造函数创建的对象。 检查提供的值是否真实，使用**typeof**检查它是否是对象，使用**Object.constructor**确保构造函数等于Object。*

```js
const isPlainObject = val => !!val && typeof val === 'object' && val.constructor === Object;
```

**Example**



```js
isPlainObject({ a: 1 }); // true
isPlainObject(new Map()); // false
```



##### 13|isPrimitive

返回一个布尔值，确定传递的值是否为原始值。 从val创建一个对象并将其与val进行比较，以确定传递的值是否为原始值（即不等于创建的对象）。

```js
const isPrimitive = val => Object(val) !== val;
```

**Example**

```js
isPrimitive(null); // true
isPrimitive(50); // true
isPrimitive('Hello!'); // true
isPrimitive(false); // true
isPrimitive(Symbol()); // true
isPrimitive([]); // false
```



##### 14|isPromiseLike

*如果对象看起来像**Promise**，则返回true，否则返回**false**。 检查对象是否为**null**，其**typeof**与对象或函数匹配，以及是否具有.**then**属性，该属性也是函数。*

```js
const isPromiseLike = obj =>
  obj !== null &&
  (typeof obj === 'object' || typeof obj === 'function') &&
  typeof obj.then === 'function';
```

**Example**



```js
isPromiseLike({
  then: function() {
    return '';
  }
}); // true
isPromiseLike(null); // false
isPromiseLike({}); // false
```



##### 15|isString

*检查给定的参数是否为字符串。仅适用于字符串基元。 使用**typeof**检查值是否归类为字符串原语。*

```js
const isString = val => typeof val === 'string';
```

**Example**

```js
isString('10'); // true
```



##### 16|isSymbol

*检查给定的参数是否为符号。 使用typeof检查值是否归类为符号原语。*

```js
const isSymbol = val => typeof val === 'symbol';
```

**Example**

```js
isSymbol(Symbol('x')); // true
```



##### 17|isUndefined

*如果指定的值未定义，则返回**true**，否则返回**false**。 使用**strict equality**运算符检查**val**和**val**的值是否等于**undefined**。*

```js
const isUndefined = val => val === undefined;
```

**Example**

```js
isUndefined(undefined); // true
```



##### 18|isValidJSON

*检查提供的字符串是否是有效的JSON。 使用JSON.parse（）和try ... catch块来检查提供的字符串是否是有效的JSON。*

```js
const isValidJSON = str => {
  try {
    JSON.parse(str);
    return true;
  } catch (e) {
    return false;
  }
};
```

**Example**

```js
isValidJSON('{"name":"Adam","age":20}'); // true
isValidJSON('{"name":"Adam",age:"20"}'); // false
isValidJSON(null); // true
```

