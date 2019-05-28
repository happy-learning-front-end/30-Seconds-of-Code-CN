#### Object

##### 01|bindAll

*将对象的方法绑定到对象本身，覆盖现有方法。 使用**Array.prototype.forEach（）**返回一个函数，该函数使用**Function.prototype.apply（）**将指定的上下文（obj）应用于指定的每个函数的fn。*

```js
const bindAll = (obj, ...fns) =>
  fns.forEach(
    fn => (
      (f = obj[fn]),
      (obj[fn] = function() {
        return f.apply(obj);
      })
    )
  );
```

**Example**

```js
var view = {
  label: 'docs',
  click: function() {
    console.log('clicked ' + this.label);
  }
};
bindAll(view, 'click');
jQuery(element).on('click', view.click); // Logs 'clicked docs' when clicked.
```



##### 02|deepClone

*创建对象的深层克隆。 使用递归。使用**Object.assign（）**和空对象（{}）创建原始的浅层克隆。使用**Object.keys（）**和**Array.prototype.forEach（）**来确定需要深度克隆的键值对。*

```js
const deepClone = obj => {
  let clone = Object.assign({}, obj);
  Object.keys(clone).forEach(
    key => (clone[key] = typeof obj[key] === 'object' ? deepClone(obj[key]) : obj[key])
  );
  return Array.isArray(obj) && obj.length
    ? (clone.length = obj.length) && Array.from(clone)
    : Array.isArray(obj)
      ? Array.from(obj)
      : clone;
};
```

**Example**

```js
const a = { foo: 'bar', obj: { a: 1, b: 2 } };
const b = deepClone(a); // a !== b, a.obj !== b.obj
```



##### 03|DeepFreeze

*深度冻结一个物体。 对作为**instanceof**对象的传递对象的所有未冻结属性递归调用**Object.freeze（obj）。***

```js
onst deepFreeze = obj =>
  Object.keys(obj).forEach(
    prop =>
      !(obj[prop] instanceof Object) || Object.isFrozen(obj[prop]) ? null : deepFreeze(obj[prop])
  ) || Object.freeze(obj);
```

**Example**

```js
'use strict';

const o = deepFreeze([1, [2, 3]]);

o[0] = 3; // not allowed
o[1][0] = 4; // not allowed as well
```



##### 04|deepGet

*基于**keys**数组返回嵌套JSON对象中的目标值。 将嵌套**JSON**对象中所需的键与**Array**进行比较。使用**Array.prototype.reduce（）**逐个从嵌套的JSON对象中获取值。如果密钥存在于object中，则返回目标值，否则返回null。*

```js
const deepGet = (obj, keys) => keys.reduce((xs, x) => (xs && xs[x] ? xs[x] : null), obj);
```

EXAMPLES

```js
let index = 2;
const data = {
  foo: {
    foz: [1, 2, 3],
    bar: {
      baz: ['a', 'b', 'c']
    }
  }
};
deepGet(data, ['foo', 'foz', index]); // get 3
deepGet(data, ['foo', 'bar', 'baz', 8, 'foz']); // null
```



##### 05|deepMapKeys

*深度映射对象键。 使用与提供的对象相同的值创建一个对象，并通过为每个键运行提供的函数生成密钥。 使用**Object.keys（obj）**迭代对象的键。使用**Array.prototype.reduce（）**使用fn创建具有相同值和映射键的新对象。*

```js
const deepMapKeys = (obj, f) =>
  Array.isArray(obj)
    ? obj.map(val => deepMapKeys(val, f))
    : typeof obj === 'object'
      ? Object.keys(obj).reduce((acc, current) => {
        const val = obj[current];
        acc[f(current)] =
            val !== null && typeof val === 'object' ? deepMapKeys(val, f) : (acc[f(current)] = val);
        return acc;
      }, {})
      : obj;
```

**Example**

```js
const obj = {
  foo: '1',
  nested: {
    child: {
      withArray: [
        {
          grandChild: ['hello']
        }
      ]
    }
  }
};
const upperKeysObj = deepMapKeys(obj, key => key.toUpperCase());
/*
{
  "FOO":"1",
  "NESTED":{
    "CHILD":{
      "WITHARRAY":[
        {
          "GRANDCHILD":[ 'hello' ]
        }
      ]
    }
  }
}
*/
```



##### 06|default

*为未定义的对象中的所有属性分配默认值。 使用Object.assign（）创建一个新的空对象并复制原始对象以维护键顺序，使用**Array.prototype.reverse（）**和**spread**运算符...从左到右组合默认值，最后使用obj再次覆盖最初具有值的属性。*

```js
const defaults = (obj, ...defs) => Object.assign({}, obj, ...defs.reverse(), obj);
```

**Example**

```js
defaults({ a: 1 }, { b: 2 }, { b: 6 }, { a: 3 }); // { a: 1, b: 2 }
```



##### 07|dig

*基于给定的键返回嵌套JSON对象中的目标值。 使用in运算符检查obj中是否存在目标。如果找到，则返回obj [target]的值，否则使用**Object.values（obj）**和**Array.prototype.reduce（）**以递归方式在每个嵌套对象上调用dig，直到找到第一个匹配的键/值对。*

```js
const dig = (obj, target) =>
  target in obj
    ? obj[target]
    : Object.values(obj).reduce((acc, val) => {
      if (acc !== undefined) return acc;
      if (typeof val === 'object') return dig(val, target);
    }, undefined);
```

**Example**

```js
const data = {
  level1: {
    level2: {
      level3: 'some data'
    }
  }
};
dig(data, 'level3'); // 'some data'
dig(data, 'level4'); // undefined
```



##### 08|equals

*在两个值之间执行深度比较以确定它们是否相等。 检查这两个值是否相同，如果它们都是具有相同时间的Date对象，则使用**Date.getTime（）**或者它们都是具有等效值的非对象值（严格比较）。检查是否只有一个值为null或未定义，或者它们的原型是否不同。如果没有满足上述条件，请使用**Object.keys（）**检查两个值是否具有相同数量的键，然后使用**Array.prototype.every（）**检查第一个值中的每个键是否都存在于第二个值中如果通过递归调用此方法它们是等效的*

```js
const equals = (a, b) => {
  if (a === b) return true;
  if (a instanceof Date && b instanceof Date) return a.getTime() === b.getTime();
  if (!a || !b || (typeof a !== 'object' && typeof b !== 'object')) return a === b;
  if (a === null || a === undefined || b === null || b === undefined) return false;
  if (a.prototype !== b.prototype) return false;
  let keys = Object.keys(a);
  if (keys.length !== Object.keys(b).length) return false;
  return keys.every(k => equals(a[k], b[k]));
};
```

**Example**

```js
equals({ a: [2, { e: 3 }], b: [4], c: 'foo' }, { a: [2, { e: 3 }], b: [4], c: 'foo' }); // true
```



##### 09|findKey

*返回满足提供的测试函数的第一个键。否则返回**undefined**。 使用**Object.keys（obj）**获取对象的所有属性**Array.prototype.find（）**以测试每个键值对的提供函数。回调接收三个参数 - 值，键和对象。*

```js
const findKey = (obj, fn) => Object.keys(obj).find(key => fn(obj[key], key, obj));
```

**Example**

```js
findKey(
  {
    barney: { age: 36, active: true },
    fred: { age: 40, active: false },
    pebbles: { age: 1, active: true }
  },
  o => o['active']
); // 'barney'
```



##### 10|findLastKey

*返回满足提供的测试函数的最后一个键。否则返回**undefined**。 使用**Object.keys（obj）**获取对象的所有属性，**Array.prototype.reverse（）**以反转它们的顺序，使用**Array.prototype.find（）**来测试每个键值对提供的函数。回调接收三个参数 - 值，键和对象。*

```js
const findLastKey = (obj, fn) =>
  Object.keys(obj)
    .reverse()
    .find(key => fn(obj[key], key, obj));
```

**Example**

```js
findLastKey(
  {
    barney: { age: 36, active: true },
    fred: { age: 40, active: false },
    pebbles: { age: 1, active: true }
  },
  o => o['active']
); // 'pebbles'
```



##### 11|flattenObject

*使用键的路径展平对象。 使用递归。使用**Object.keys（obj）**与**Array.prototype.reduce（）**结合将每个叶节点转换为展平路径节点。如果键的值是对象，则该函数使用适当的前缀调用自身以使用**Object.assign（）**创建路径。否则，它会将相应的前缀键值对添加到累加器对象。您应该总是省略第二个参数prefix，除非您希望每个键都有一个前缀。*

```js
const flattenObject = (obj, prefix = '') =>
  Object.keys(obj).reduce((acc, k) => {
    const pre = prefix.length ? prefix + '.' : '';
    if (typeof obj[k] === 'object') Object.assign(acc, flattenObject(obj[k], pre + k));
    else acc[pre + k] = obj[k];
    return acc;
  }, {});
```

**Example**

```js
flattenObject({ a: { b: { c: 1 } }, d: 1 }); // { 'a.b.c': 1, d: 1 }
```



##### 12|forOwn

*迭代对象的所有属性，为每个属性运行回调。 使用**Object.keys（obj）**获取对象的所有属性**Array.prototype.forEach（）**以为每个键值对运行提供的函数。回调接收三个参数 - 值，键和对象。*

```js
const forOwn = (obj, fn) => Object.keys(obj).forEach(key => fn(obj[key], key, obj));
```

**Example**

```js
forOwn({ foo: 'bar', a: 1 }, v => console.log(v)); // 'bar', 1
```



##### 13|forOwnRight

*反向迭代对象的所有属性，为每个属性运行回调。 使用Object.keys（obj）获取对象的所有属性，**Array.prototype.reverse（）**以反转它们的顺序，使用**Array.prototype.forEach（）**来为每个键值对运行提供的函数。回调接收三个参数 - 值，键和对象。*

```js
const forOwnRight = (obj, fn) =>
  Object.keys(obj)
    .reverse()
    .forEach(key => fn(obj[key], key, obj));
```

**Example**

```js
forOwnRight({ foo: 'bar', a: 1 }, v => console.log(v)); // 1, 'bar'
```



##### 14|functions

从对象的自身（以及可选继承）可枚举属性返回函数属性名称数组。 使用**Object.keys（obj）**迭代对象自己的属性。如果**inherited**为true，则使用**Object.get.PrototypeOf（obj）**来获取对象的继承属性。使用**Array.prototype.filter（）**仅保留那些作为函数的属性。省略继承的第二个参数，默认情况下不包括继承的属性。

```js
const functions = (obj, inherited = false) =>
  (inherited
    ? [...Object.keys(obj), ...Object.keys(Object.getPrototypeOf(obj))]
    : Object.keys(obj)
  ).filter(key => typeof obj[key] === 'function');
```

**Exmaple**

```js
function Foo() {
  this.a = () => 1;
  this.b = () => 2;
}
Foo.prototype.c = () => 3;
functions(new Foo()); // ['a', 'b']
functions(new Foo(), true); // ['a', 'b', 'c']
```



##### 15|get

从对象中检索给定选择器指示的一组属性。 对每个选择器使用**Array.prototype.map（）**，**String.prototype.replace（）**用点替换方括号，**String.prototype.split（'。'）**拆分每个选择器，**Array.prototype.filter（）**删除空值和**Array.prototype.reduce（）**来获取它指示的值。

```js
const get = (from, ...selectors) =>
  [...selectors].map(s =>
    s
      .replace(/\[([^\[\]]*)\]/g, '.$1.')
      .split('.')
      .filter(t => t !== '')
      .reduce((prev, cur) => prev && prev[cur], from)
  );
```

**Example**

```js
const obj = { selector: { to: { val: 'val to select' } }, target: [1, 2, { a: 'test' }] };
get(obj, 'selector.to.val', 'target[0]', 'target[2].a'); // ['val to select', 1, 'test']
```



##### 16|invertKeyValues

*反转对象的键值对，而不改变它。每个反转键的相应反转值是负责产生反转值的键阵列。如果提供了一个功能，它将应用于每个反转的键。 使用**Object.keys（）**和**Array.prototype.reduce（）**反转对象的键值对并应用提供的函数（如果有）。省略第二个参数fn，得到反转键而不对它们应用函数。*

```js
const invertKeyValues = (obj, fn) =>
  Object.keys(obj).reduce((acc, key) => {
    const val = fn ? fn(obj[key]) : obj[key];
    acc[val] = acc[val] || [];
    acc[val].push(key);
    return acc;
  }, {});
```

**Example**

```js
invertKeyValues({ a: 1, b: 2, c: 1 }); // { 1: [ 'a', 'c' ], 2: [ 'b' ] }
invertKeyValues({ a: 1, b: 2, c: 1 }, value => 'group' + value); // { group1: [ 'a', 'c' ], group2: [ 'b' ] }
```



##### 17|lowercaseKeys

*从指定对象创建一个新对象，其中所有键都是小写的。 使用**Object.keys（）**和**Array.prototype.reduce（）**从指定对象创建新对象。使用**String.toLowerCase（）**将原始对象中的每个键转换为小写。*

```js
const lowercaseKeys = obj =>
  Object.keys(obj).reduce((acc, key) => {
    acc[key.toLowerCase()] = obj[key];
    return acc;
  }, {});
```

**Example**

```js
const myObj = { Name: 'Adam', sUrnAME: 'Smith' };
const myObjLower = lowercaseKeys(myObj); // {name: 'Adam', surname: 'Smith'};
```



##### 18|mapKeys

*使用为每个键运行提供的函数以及与提供的对象相同的值生成的密钥创建一个对象。 使用**Object.keys（obj）**迭代对象的键。使用**Array.prototype.reduce（）**使用fn创建具有相同值和映射键的新对象。*

```js
const mapKeys = (obj, fn) =>
  Object.keys(obj).reduce((acc, k) => {
    acc[fn(obj[k], k, obj)] = obj[k];
    return acc;
  }, {});
```

**Exmaple**

```js
mapKeys({ a: 1, b: 2 }, (val, key) => key + val); // { a1: 1, b2: 2 }
```



##### 19|mapValues

*使用与提供的对象相同的键创建对象，并通过为每个值运行提供的函数生成值。 使用**Object.keys（obj）**迭代对象的键。使用**Array.prototype.reduce（）**使用fn创建具有相同键和映射值的新对象。*

```js
const mapValues = (obj, fn) =>
  Object.keys(obj).reduce((acc, k) => {
    acc[k] = fn(obj[k], k, obj);
    return acc;
  }, {});
```

**Example**

```js
const users = {
  fred: { user: 'fred', age: 40 },
  pebbles: { user: 'pebbles', age: 1 }
};
mapValues(users, u => u.age); // { fred: 40, pebbles: 1 }
```



##### 20|matches

*比较两个对象以确定第一个对象是否包含与第二个对应的属性值。 使用**Object.keys（source）**获取第二个对象的所有键，然后使用**Array.prototype.every（）**，**Object.hasOwnProperty（）**和严格比较来确定第一个对象中是否存在所有键并具有相同的值。*

```js
const matches = (obj, source) =>
  Object.keys(source).every(key => obj.hasOwnProperty(key) && obj[key] === source[key]);
```

**Example**

```js
matches({ age: 25, hair: 'long', beard: true }, { hair: 'long', beard: true }); // true
matches({ hair: 'long', beard: true }, { age: 25, hair: 'long', beard: true }); // false
```



##### 21|matchesWith

*根据提供的函数，比较两个对象以确定第一个对象是否包含与第二个对应的属性值。 使用**Object.keys（source）**获取第二个对象的所有键，然后使用**Array.prototype.every（），Object.hasOwnProperty（）**和提供的函数来确定第一个对象中是否存在所有键并具有等效值。如果未提供任何功能，则将使用相等运算符比较值。*

```js
const matchesWith = (obj, source, fn) =>
  Object.keys(source).every(
    key =>
      obj.hasOwnProperty(key) && fn
        ? fn(obj[key], source[key], key, obj, source)
        : obj[key] == source[key]
  );
```

**Exmaple**

```js
const isGreeting = val => /^h(?:i|ello)$/.test(val);
matchesWith(
  { greeting: 'hello' },
  { greeting: 'hi' },
  (oV, sV) => isGreeting(oV) && isGreeting(sV)
); // true
```



##### 22|merge

*从两个或多个对象的组合中创建新对象。 使用**Array.prototype.reduce（）**结合**Object.keys（obj）**迭代所有对象和键。使用**hasOwnProperty（）**和**Array.prototype.concat（）**为多个对象中存在的键附加值。*

```js
onst merge = (...objs) =>
  [...objs].reduce(
    (acc, obj) =>
      Object.keys(obj).reduce((a, k) => {
        acc[k] = acc.hasOwnProperty(k) ? [].concat(acc[k]).concat(obj[k]) : obj[k];
        return acc;
      }, {}),
    {}
  );
```

**Example**

```js
const object = {
  a: [{ x: 2 }, { y: 4 }],
  b: 1
};
const other = {
  a: { z: 3 },
  b: [2, 3],
  c: 'foo'
};
merge(object, other); // { a: [ { x: 2 }, { y: 4 }, { z: 3 } ], b: [ 1, 2, 3 ], c: 'foo' }
```



##### 23|nest

*给定彼此链接的平面对象数组，它将递归地嵌套它们。用于嵌套注释，例如reddit.com上的注释。 使用递归。使用**Array.prototype.filter（）**过滤id与链接匹配的项目，然后使用**Array.prototype.map（）**将每个项目映射到具有子属性的新对象，该属性以递归方式嵌套项目为基础当前项目的子项。省略第二个参数id，使其默认为null，表示该对象未链接到另一个（即它是顶级对象）。省略第三个参数link，使用'parent_id'作为默认属性，通过其id将对象链接到另一个对象。*

```js
const nest = (items, id = null, link = 'parent_id') =>
  items
    .filter(item => item[link] === id)
    .map(item => ({ ...item, children: nest(items, item.id) }));
```

**Example**

```js
// One top level comment
const comments = [
  { id: 1, parent_id: null },
  { id: 2, parent_id: 1 },
  { id: 3, parent_id: 1 },
  { id: 4, parent_id: 2 },
  { id: 5, parent_id: 4 }
];
const nestedComments = nest(comments); // [{ id: 1, parent_id: null, children: [...] }]
```



##### 24|objectFromPairs

根据给定的键值对创建对象。 使用**Array.prototype.reduce（）**创建和组合键值对。

```js
const objectFromPairs = arr => arr.reduce((a, [key, val]) => ((a[key] = val), a), {});
```

**Example**

```js
objectFromPairs([['a', 1], ['b', 2]]); // {a: 1, b: 2}
```



##### 25|objectToPairs

从对象创建键值对数组的数组。 使用**Object.keys（）**和**Array.prototype.map（）**迭代对象的键并生成具有键值对的数组。

```js
const objectToPairs = obj => Object.keys(obj).map(k => [k, obj[k]]);
```

**Example**

```js
objectToPairs({ a: 1, b: 2 }); // [ ['a', 1], ['b', 2] ]
```



##### 26|omit

*从对象中省略与给定键对应的键值对。 使用**Object.keys（obj）**，**Array.prototype.filter（）**和**Array.prototype.includes（）**删除提供的键。使用**Array.prototype.reduce（）**将过滤后的键转换回具有相应键值对的对象。*

```js
const omit = (obj, arr) =>
  Object.keys(obj)
    .filter(k => !arr.includes(k))
    .reduce((acc, key) => ((acc[key] = obj[key]), acc), {});
```

**Example**

```js
omit({ a: 1, b: '2', c: 3 }, ['b']); // { 'a': 1, 'c': 3 }
```



##### 27|omitBy

*创建一个由给定函数返回falsey的属性组成的对象。使用两个参数调用该函数：（value，key）。 使用**Object.keys（obj）**和**Array.prototype.filter（）**删除fn返回truthy值的键。使用**Array.prototype.reduce（）**将过滤后的键转换回具有相应键值对的对象。*

```js
const omitBy = (obj, fn) =>
  Object.keys(obj)
    .filter(k => !fn(obj[k], k))
    .reduce((acc, key) => ((acc[key] = obj[key]), acc), {});
```

**Example**

```js
omitBy({ a: 1, b: '2', c: 3 }, x => typeof x === 'number'); // { b: '2' }
```



##### 28|orderBy

*返回按属性和顺序排序的排序对象数组。 在props数组上使用**Array.prototype.sort（）**，**Array.prototype.reduce（）**，默认值为0，使用数组解构根据传递的顺序交换属性位置。如果没有传递orders数组，则默认情况下按'asc'排序。*

```js
const orderBy = (arr, props, orders) =>
  [...arr].sort((a, b) =>
    props.reduce((acc, prop, i) => {
      if (acc === 0) {
        const [p1, p2] = orders && orders[i] === 'desc' ? [b[prop], a[prop]] : [a[prop], b[prop]];
        acc = p1 > p2 ? 1 : p1 < p2 ? -1 : 0;
      }
      return acc;
    }, 0)
  );
```

**Example**

```js
const users = [{ name: 'fred', age: 48 }, { name: 'barney', age: 36 }, { name: 'fred', age: 40 }];
orderBy(users, ['name', 'age'], ['asc', 'desc']); // [{name: 'barney', age: 36}, {name: 'fred', age: 48}, {name: 'fred', age: 40}]
orderBy(users, ['name', 'age']); // [{name: 'barney', age: 36}, {name: 'fred', age: 40}, {name: 'fred', age: 48}]
```



##### 29|pick

*从对象中挑选与给定键对应的键值对。 如果对象中存在键，则使用**Array.prototype.reduce（）**将已过滤/拾取的键转换回具有相应键值对的对象。*

```js
const pick = (obj, arr) =>
  arr.reduce((acc, curr) => (curr in obj && (acc[curr] = obj[curr]), acc), {});
```

**Example**

```js
pick({ a: 1, b: '2', c: 3 }, ['a', 'c']); // { 'a': 1, 'c': 3 }
```



##### 30|pickBy

*创建一个由给定函数返回truthy的属性组成的对象。使用两个参数调用该函数：（value，key）。 使用Object.keys（obj）和**Array.prototype.filter（）**删除fn返回falsey值的键。使用**Array.prototype.reduce（）**将过滤后的键转换回具有相应键值对的对象。*

```js
const pickBy = (obj, fn) =>
  Object.keys(obj)
    .filter(k => fn(obj[k], k))
    .reduce((acc, key) => ((acc[key] = obj[key]), acc), {});
```

**Example**

```js
pickBy({ a: 1, b: '2', c: 3 }, x => typeof x === 'number'); // { 'a': 1, 'c': 3 }
```



##### 31|renameKeys

*使用提供的值替换多个对象键的名称。 将**Object.keys（）**与**Array.prototype.reduce（）**和spread运算符（...）结合使用以获取对象的键并根据keysMap重命名它们。*

```js
const renameKeys = (keysMap, obj) =>
  Object.keys(obj).reduce(
    (acc, key) => ({
      ...acc,
      ...{ [keysMap[key] || key]: obj[key] }
    }),
    {}
  );
```

**Example**

```js
const obj = { name: 'Bobo', job: 'Front-End Master', shoeSize: 100 };
renameKeys({ name: 'firstName', job: 'passion' }, obj); // { firstName: 'Bobo', passion: 'Front-End Master', shoeSize: 100 }
```



##### 32|shallowClone

*创建对象的浅层克隆。 使用**Object.assign（）**和空对象（{}）创建原始的浅层克隆。*

```js
const shallowClone = obj => Object.assign({}, obj);
```

**Example**

```js
const a = { x: true, y: 1 };
const b = shallowClone(a); // a !== b
```



##### 33|size

获取数组，对象或字符串的大小。 获取val的类型（数组，对象或字符串）。使用数组的长度属性。使用长度或大小值（如果可用）或对象的键数。使用从val为字符串创建的Blob对象的大小。 使用split（''）将字符串拆分为字符数组并返回其长度。

```js
const size = val =>
  Array.isArray(val)
    ? val.length
    : val && typeof val === 'object'
      ? val.size || val.length || Object.keys(val).length
      : typeof val === 'string'
        ? new Blob([val]).size
        : 0;
```

**Example**

```js
size([1, 2, 3, 4, 5]); // 5
size('size'); // 4
size({ one: 1, two: 2, three: 3 }); // 3
```



##### 34|transform

*对累加器和对象中的每个键应用一个函数（从左到右）。 使用**Object.keys（obj）**迭代对象中的每个键，**Array.prototype.reduce（）**以调用对指定累加器应用指定的函数。*

```js
const transform = (obj, fn, acc) => Object.keys(obj).reduce((a, k) => fn(a, obj[k], k, obj), acc);
```

**Example**

```js
transform(
  { a: 1, b: 2, c: 1 },
  (r, v, k) => {
    (r[v] || (r[v] = [])).push(k);
    return r;
  },
  {}
); // { '1': ['a', 'c'], '2': ['b'] }
```



##### 35|truthCheckCollection

*检查谓词（第二个参数）是否对集合的所有元素（第一个参数）都是真实的。 使用**Array.prototype.every（）**检查每个传递的对象是否具有指定的属性，以及它是否返回truthy值。*

```js
const truthCheckCollection = (collection, pre) => collection.every(obj => obj[pre]);
```

**Example**

```js
truthCheckCollection([{ user: 'Tinky-Winky', sex: 'male' }, { user: 'Dipsy', sex: 'male' }], 'sex'); // true
```



##### 36|unflattenObject

*使用键的路径取消对象的展开。 使用**Object.keys（obj）**结合**Array.prototype.reduce（）**将展平路径节点转换为叶节点。如果键的值包含点分隔符（。），则使用**Array.prototype.split（'。'）**，字符串转换和**JSON.parse（）**创建对象，然后使用**Object.assign（）**创建叶节点。否则，将相应的键值对添加到累加器对象。*

```js
const unflattenObject = obj =>
  Object.keys(obj).reduce((acc, k) => {
    if (k.indexOf('.') !== -1) {
      const keys = k.split('.');
      Object.assign(
        acc,
        JSON.parse(
          '{' +
            keys.map((v, i) => (i !== keys.length - 1 ? `"${v}":{` : `"${v}":`)).join('') +
            obj[k] +
            '}'.repeat(keys.length)
        )
      );
    } else acc[k] = obj[k];
    return acc;
  }, {});
```

**Example**

```js
unflattenObject({ 'a.b.c': 1, d: 1 }); // { a: { b: { c: 1 } }, d: 1 }
```

