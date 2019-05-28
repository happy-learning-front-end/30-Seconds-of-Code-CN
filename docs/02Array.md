#### 02|Array(数组)

##### 01|all(所有)

*如果提供的谓词函数对集合中的所有元素返回**true**，则返回**true**，否则返回**false**。 使用**Array.prototype.every（）**来测试集合中的所有元素是否基于**fn**返回**true**。省略第二个参数**fn**，使用**Boolean**作为默认值。*

```js
const all = (arr, fn = Boolean) => arr.every(fn);
```

**Example**

```js
all([4, 2, 3], x => x > 1); // true
all([1, 2, 3]); // true
```



##### 02|allEqual(比较所有的)

*检查数组中的所有元素是否相等。* 

*使用**Array.prototype.every（）**检查数组的所有元素是否与第一个元素相同。*

```js
const allEqual = arr => arr.every(val => val === arr[0]);
```

**Example**

```js
allEqual([1, 2, 3, 4, 5, 6]); // false
allEqual([1, 1, 1, 1]); // true
```



##### 03|any(任何)

*如果提供的谓词函数对集合中的至少一个元素返回**true**，则返回**true**，否则返回**false**。 使用**Array.prototype.some（）**来测试集合中的任何元素是否基于**fn**返回**true**。省略第二个参数**fn**，使用**Boolean**作为默认值。*

```js
const any = (arr, fn = Boolean) => arr.some(fn);
```

**Example**

```js
any([0, 1, 2, 0], x => x >= 2); // true
any([0, 0, 1, 0]); // true
```



##### 04|arrayToCSV(数组转换成为)

*将2D数组转换为逗号分隔值（CSV）字符串。* 

*使用**Array.prototype.map（）***

*和**Array.prototype.join（delimiter）**将各个1D数组（行）组合成字符串。使用**Array.prototype.join（'\ n'）**将所有行组合成CSV字符串，用换行符分隔每一行。省略第二个参数**delimiter**，使用的默认分隔符为**,**。*

```js
const arrayToCSV = (arr, delimiter = ',') =>
  arr
    .map(v => v.map(x => (isNaN(x) ? `"${x.replace(/"/g, '""')}"` : x)).join(delimiter))
    .join('\n');
```

**Example**

```js
arrayToCSV([['a', 'b'], ['c', 'd']]); // '"a","b"\n"c","d"'
arrayToCSV([['a', 'b'], ['c', 'd']], ';'); // '"a";"b"\n"c";"d"'
arrayToCSV([['a', '"b" great'], ['c', 3.1415]]); // '"a","""b"" great"\n"c",3.1415'
```



##### 05|bifurcate(分叉)

*将值拆分为两组。如果**filter**中的元素是真实的，则集合中的对应元素属于第一组;否则，它属于第二组。 使用**Array.prototype.reduce（）**和**Array.prototype.push（）**根据**filter**向组添加元素。*

```js
const bifurcate = (arr, filter) =>
  arr.reduce((acc, val, i) => (acc[filter[i] ? 0 : 1].push(val), acc), [[], []]);
```

**Example**

```js
bifurcate(['beep', 'boop', 'foo', 'bar'], [true, true, false, true]); // [ ['beep', 'boop', 'bar'], ['foo'] ]
```



##### 06|bifurcateBy(分叉)

*根据谓词函数将值拆分为两个组，谓词函数指定输入集合中的元素属于哪个组。如果谓词函数返回**truthy**值，则**collection**元素属于第一个组;否则，它属于第二组。*

 *使用**Array.prototype.reduce（）**和**Array.prototype.push（）**根据**fn**为每个元素返回的值向组添加元素。*

```js
const bifurcateBy = (arr, fn) =>
  arr.reduce((acc, val, i) => (acc[fn(val, i) ? 0 : 1].push(val), acc), [[], []]);
```

**Example**

```js
bifurcateBy(['beep', 'boop', 'foo', 'bar'], x => x[0] === 'b'); // [ ['beep', 'boop', 'bar'], ['foo'] ]
```



##### 07|chunk(块)

*将数组块化为指定大小的较小数组。* 

*使用**Array.from（）**创建一个新数组，该数组符合将生成的块数。使用**Array.prototype.slice（）**将新数组的每个元素映射到一个**size**的块。如果原始数组无法均匀分割，则最终的块将包含其余素。*

```js
onst chunk = (arr, size) =>
  Array.from({ length: Math.ceil(arr.length / size) }, (v, i) =>
    arr.slice(i * size, i * size + size)
  );
```

**Example**

```js
chunk([1, 2, 3, 4, 5], 2); // [[1,2],[3,4],[5]]
```



##### 08|compact(紧凑)

*从数组中删除**falsey**值。*

*使用**Array.prototype.filter（）**过滤掉**falsey**值（**false，null，0，“”，undefined和NaN**）。*

```js
const compact = arr => arr.filter(Boolean);
```

**Example**

```js
compact([0, 1, false, 2, '', 3, 'a', 'e' * 23, NaN, 's', 34]); // [ 1, 2, 3, 'a', 's', 34 ]compact([0, 1, false, 2, '', 3, 'a', 'e' * 23, NaN, 's', 34]); // [ 1, 2, 3, 'a', 's', 34 ]
```



##### 09|countBy(计数)

*根据给定的函数对数组的元素进行分组，并返回每个组中元素的数量。*

*使用**Array.prototype.map（）**将数组的值映射到函数或属性名称。使用**Array.prototype.reduce（）**创建一个对象，其中的键是从映射结果生成的。*

```js
const countBy = (arr, fn) =>
  arr.map(typeof fn === 'function' ? fn : val => val[fn]).reduce((acc, val) => {
    acc[val] = (acc[val] || 0) + 1;
    return acc;
  }, {});
```

**Example**

```js
countBy([6.1, 4.2, 6.3], Math.floor); // {4: 1, 6: 2}
countBy(['one', 'two', 'three'], 'length'); // {3: 2, 5: 1}
```



##### 10|count Occurrences(计算出现次数)

*计算数组中值的出现次数。*

*每次遇到数组内的特定值时，使用**Array.prototype.reduce（）**递增计数器。*

```js
const countOccurrences = (arr, val) => arr.reduce((a, v) => (v === val ? a + 1 : a), 0);
```

**Example**

```js
countOccurrences([1, 1, 2, 1, 2, 3], 1); // 3
```



##### 11|deepFlatten(深度展平)

*深度展平阵列。* 

*使用递归。将**Array.prototype.concat（）**与空数组**（[]）**和扩展运算符**（...）**一起使用以展平数组。递归地展平作为数组的每个元素。*

```js
const deepFlatten = arr => [].concat(...arr.map(v => (Array.isArray(v) ? deepFlatten(v) : v)));
```

**Example**

```js
deepFlatten([1, [2], [[3], 4], 5]); // [1,2,3,4,5]
```



##### 12|difference(区别)

*返回两个数组之间的差异。* 

*从**b**创建一个**Set**，然后在**a**上使用**Array.prototype.filter（）**来保留**b**中不包含的值。*

```js
const difference = (a, b) => {
  const s = new Set(b);
  return a.filter(x => !s.has(x));
};
```

**Example**

```js
difference([1, 2, 3], [1, 2, 4]); // [3]
```



##### 13|differenceBy(差异)

*将提供的函数应用于两个数组的每个数组元素后，返回两个数组之间的差异。* 

*通过将fn应用于**b**中的每个元素来创建一个**Set**，然后使用**Array.prototype.map（）**将**fn**应用于**a**中的每个元素，然后使用**Array.prototype.filter（）***

```js
const differenceBy = (a, b, fn) => {
  const s = new Set(b.map(fn));
  return a.map(fn).filter(el => !s.has(el));
};
```

**Example**

```js
differenceBy([2.1, 1.2], [2.3, 3.4], Math.floor); // [1]
differenceBy([{ x: 2 }, { x: 1 }], [{ x: 1 }], v => v.x); // [2]
```



##### 14|dirrerenceWidth(差异与)

*过滤掉比较器函数未返回**true**的数组中的所有值。*

*使用**Array.prototype.filter（）**和**Array.prototype.findIndex（）**来查找适当的值。*	

```js
const differenceWith = (arr, val, comp) => arr.filter(a => val.findIndex(b => comp(a, b)) === -1);
```

**Example**

```js
ifferenceWith([1, 1.2, 1.5, 3, 0], [1.9, 3, 0], (a, b) => Math.round(a) === Math.round(b)); // [1, 1.2]
```



##### 15|drop(删除)

*返回一个新数组，其中左侧删除了n个元素。 使用**Array.prototype.slice（）**从左侧删除指定数量的元素。*

```js
const drop = (arr, n = 1) => arr.slice(n);
```

**Example**

```js
drop([1, 2, 3]); // [2,3]
drop([1, 2, 3], 2); // [3]
drop([1, 2, 3], 42); // []
```



##### 16|drop Right(向右删除)

*返回一个新数组，其中右侧删除了**n**个元素。* 

*使用**Array.prototype.slice（）**从右侧删除指定数量的元素。*

```js
const dropRight = (arr, n = 1) => arr.slice(0, -n);
```

**Example**

```js
dropRight([1, 2, 3]); // [1,2]
dropRight([1, 2, 3], 2); // [1]
dropRight([1, 2, 3], 42); // []
```



##### 17|drop Right While

*从数组末尾删除元素，直到传递的函数返回**true**。*

*返回数组中的其余元素。 循环遍历数组，使用**Array.prototype.slice（）**删除数组的最后一个元素，直到函数返回的值为**true**。返回剩余的元素。*

```js
const dropRightWhile = (arr, func) => {
  while (arr.length > 0 && !func(arr[arr.length - 1])) arr = arr.slice(0, -1);
  return arr;
};

```

**Example**

```js
dropRightWhile([1, 2, 3, 4], n => n < 3); // [1, 2]
```



##### 18|drop While

*删除数组中的元素，直到传递的函数返回true。*

*返回数组中的其余元素。 循环遍历数组，使用**Array.prototype.slice（）**删除数组的第一个元素，直到函数返回的值为**true**。返回剩余的元素。*

```js
const dropWhile = (arr, func) => {
  while (arr.length > 0 && !func(arr[0])) arr = arr.slice(1);
  return arr;
};
```

**Example**

```js
dropWhile([1, 2, 3, 4], n => n >= 3); // [3,4]
```



##### 19|everyNth

*返回数组中的每个第n个元素。* 

*使用**Array.prototype.filter（）**创建一个包含给定数组的每个第n个元素的新数组。*

```js
const everyNth = (arr, nth) => arr.filter((e, i) => i % nth === nth - 1);
```

**Example**

```js
everyNth([1, 2, 3, 4, 5, 6], 2); // [ 2, 4, 6 ]
```



##### 20|filterFalsy(过滤falsy)

*过滤掉数组中的虚假值。* 

*使用**Array.prototype.filter（）**获取仅包含**truthy**值的数组。*

```js
const filterFalsy = arr => arr.filter(Boolean);
```

**Example**

```js
filterFalsy(['', true, {}, false, 'sample', 1, 0]); // [true, {}, 'sample', 1]
```



##### 21|filterNonUnique(过滤非唯一)

*过滤掉数组中的非唯一值。* 

*将**Array.prototype.filter（）**用于仅包含唯一值的数组。*

```js
const filterNonUnique = arr => arr.filter(i => arr.indexOf(i) === arr.lastIndexOf(i));
```

**Example**

```js
filterNonUnique([1, 2, 2, 3, 4, 4, 5]); // [1, 3, 5]
```



##### 22|filter Non Unique By

根据提供的比较器函数筛选出数组中的非唯一值。 

根据比较器函数**fn**，将**Array.prototype.filter（）**和**Array.prototype.every（）**用于仅包含唯一值的数组。比较器函数有四个参数：被比较的两个元素的值及其索引。

```js
onst filterNonUniqueBy = (arr, fn) =>
  arr.filter((v, i) => arr.every((x, j) => (i === j) === fn(v, x, i, j)));
```

**Exmaple**

```js
ilterNonUniqueBy(
  [
    { id: 0, value: 'a' },
    { id: 1, value: 'b' },
    { id: 2, value: 'c' },
    { id: 1, value: 'd' },
    { id: 0, value: 'e' }
  ],
  (a, b) => a.id == b.id
); // [ { id: 2, value: 'c' } ]
```



##### 23|findLast (查找最后一个)

*返回提供的函数返回**truthy**值的最后一个元素。* 

*使用**Array.prototype.filter（）**删除**fn**返回**falsey**值的元素，**Array.prototype.pop（）**以获取最后一个*。

```js
const findLast = (arr, fn) => arr.filter(fn).pop();
```

**Example**

```js
findLast([1, 2, 3, 4], n => n % 2 === 1); // 3
```



##### 24|findLastIndex（查询最后值的索引）

*返回提供的函数返回**truthy**值的最后一个元素的索引。* 

*使用**Array.prototype.map（）**将每个元素映射到具有索引和值的数组。使用**Array.prototype.filter（）**删除**fn**返回**falsey**值的元素，**Array.prototype.pop（）**以获取最后一个。*

```js
const findLastIndex = (arr, fn) =>
  arr
    .map((val, i) => [i, val])
    .filter(([i, val]) => fn(val, i, arr))
    .pop()[0];
```

**Example**

```js
findLastIndex([1, 2, 3, 4], n => n % 2 === 1); // 2 (index of the value 3)
```



##### 25|flatten

*将数组展平到指定的深度。*

*使用递归，每个深度级别将深度递减1。*

*使用**Array.prototype.reduce（）**和**Array.prototype.concat（）**来合并元素或数组。基本情况，深度等于1停止递归。省略第二个参数，深度仅展平为1（单个展平）。*

```js
const flatten = (arr, depth = 1) =>
  arr.reduce((a, v) => a.concat(depth > 1 && Array.isArray(v) ? flatten(v, depth - 1) : v), []);
```

**Example**

```js
flatten([1, [2], 3, 4]); // [1, 2, 3, 4]
flatten([1, [2, [3, [4, 5], 6], 7], 8], 2); // [1, 2, 3, [4, 5], 6, 7, 8]
```



##### 26|forEachRight

*从数组的最后一个元素开始，为每个数组元素执行一次提供的函数。* 

*使用**Array.prototype.slice（0）**克隆给定的数组，使用**Array.prototype.reverse（）**来反转它，使用**Array.prototype.forEach（）**迭代反向数组。*

```js
const forEachRight = (arr, callback) =>
  arr
    .slice(0)
    .reverse()
    .forEach(callback);
```

**Example**

```js
forEachRight([1, 2, 3, 4], val => console.log(val)); // '4', '3', '2', '1'
```



##### 27|groupBy

*根据给定的函数对数组的元素进行分组。* 

*使用**Array.prototype.map（）**将数组的值映射到函数或属性名称。使用**Array.prototype.reduce（）**创建一个对象，其中的键是从映射结果生成的。*

```js
const groupBy = (arr, fn) =>
  arr.map(typeof fn === 'function' ? fn : val => val[fn]).reduce((acc, val, i) => {
    acc[val] = (acc[val] || []).concat(arr[i]);
    return acc;
  }, {});
```

**Example**

```js
groupBy([6.1, 4.2, 6.3], Math.floor); // {4: [4.2], 6: [6.1, 6.3]}
groupBy(['one', 'two', 'three'], 'length'); // {3: ['one', 'two'], 5: ['three']}
```



##### 28|head(头)

*返回列表的头部。 使用**arr [0]**返回传递的数组的第一个元素。*

```js
const head = arr => arr[0];
```

**Example**

```js
head([1, 2, 3]); // 1
```



##### 29|indexOfAll

*返回数组中**val**的所有索引。如果**val**永远不会发生，则返回**[]**。* 

*使用**Array.prototype.reduce（）**循环元素并存储匹配元素的索引。返回索引数组。*

```js
const indexOfAll = (arr, val) => arr.reduce((acc, el, i) => (el === val ? [...acc, i] : acc), []);
```

**Example**

```js
indexOfAll([1, 2, 3, 1, 2, 3], 1); // [0,3]
indexOfAll([1, 2, 3], 4); // []
```



##### 30|initial(初始化)

*返回除最后一个数组之外的数组的所有元素。 使用**arr.slice（0，-1）**返回除数组的最后一个元素之外的所有元素。*

```js
const initial = arr => arr.slice(0, -1);
```

**Example**

```js
initial([1, 2, 3]); // [1,2]
```



##### 31|initialize2DArray(初始化2维数组)

*初始化给定宽度和高度以及值的2D数组。* 

*使用**Array.prototype.map（）**生成h行，其中每个都是一个大小为w的初始化的新数组。如果未提供该值，则默认为**null**。*

```js
const initialize2DArray = (w, h, val = null) =>
  Array.from({ length: h }).map(() => Array.from({ length: w }).fill(val));
```

**Example**

```js
initialize2DArray(2, 2, 0); // [[0,0], [0,0]]
```



#### 32|initialize Array With Range(初始化数组和范围)

*初始化一个数组，其中包含指定范围内的数字，其中**start**和**end**包含其公共差异**step**。* 

*使用**Array.from（）**创建一个所需长度的数组**（end-start + 1）/ step**，以及一个**map**函数，用给定范围内的所需值填充它。您可以省略开始使用默认值0.您可以省略步骤以使用默认值1。*

```js
const initializeArrayWithRange = (end, start = 0, step = 1) =>
  Array.from({ length: Math.ceil((end - start + 1) / step) }, (v, i) => i * step + start);
```

**Example**

```js
initializeArrayWithRange(5); // [0,1,2,3,4,5]
initializeArrayWithRange(7, 3); // [3,4,5,6,7]
initializeArrayWithRange(9, 0, 2); // [0,2,4,6,8]
```



##### 33|initialize Array With Range Right

*初始化一个数组，其中包含指定范围内的数字（反向），其中**start**和**end**包含其公共差异步骤。* 

*使用**Array.from（Math.ceil（（end + 1-start）/ step））**创建一个所需长度的数组**（元素的数量等于（end-start）/ step或（end + 1-start） ）/ step for inclusive end），Array.prototype.map（）**用于填充范围内的所需值。您可以省略开始使用默认值0.您可以省略步骤以使用默认值1。*

```js
const initializeArrayWithRangeRight = (end, start = 0, step = 1) =>
  Array.from({ length: Math.ceil((end + 1 - start) / step) }).map(
    (v, i, arr) => (arr.length - i - 1) * step + start
  );
```

**Example**

```js
initializeArrayWithRangeRight(5); // [5,4,3,2,1,0]
initializeArrayWithRangeRight(7, 3); // [7,6,5,4,3]
initializeArrayWithRangeRight(9, 0, 2); // [8,6,4,2,0]
```



##### 34|initialize Array With Values

*使用指定的值初始化并填充数组。* 

*使用**Array（n）**创建所需长度的数组，**fill（v）**以使用所需的值填充它。您可以省略**val**以使用默认值**0**。*

```js
const initializeArrayWithValues = (n, val = 0) => Array(n).fill(val);
```

**Example**

```js
initializeArrayWithValues(5, 2); // [2, 2, 2, 2, 2]
```



##### 35|initializeNDArray

*创建具有给定值的n维数组。* 

*使用递归。使用**Array.prototype.map（）**生成行，其中每个行都是使用**initializeNDArray**初始化的新数组。*

```js
onst initializeNDArray = (val, ...args) =>
  args.length === 0
    ? val
    : Array.from({ length: args[0] }).map(() => initializeNDArray(val, ...args.slice(1)));
```

**Example**

```js
initializeNDArray(1, 3); // [1,1,1]
initializeNDArray(5, 2, 2, 2); // [[[5,5],[5,5]],[[5,5],[5,5]]]
```



##### 36|intersection

*返回两个数组中存在的元素列表。* 

*从b创建一个Set，然后在a上使用**Array.prototype.filter（）**只保留b中包含的值。*

```js
const intersection = (a, b) => {
  const s = new Set(b);
  return a.filter(x => s.has(x));
};
```

**Example**

```js
intersection([1, 2, 3], [4, 3, 2]); // [2, 3]
```



##### 37|intersectionBy

*在将提供的函数应用于两个数组的每个数组元素之后，返回两个数组中存在的元素列表。* 

*通过将fn应用于b中的所有元素来创建一个**Set**，然后在a上使用**Array.prototype.filter（）**来仅保留元素，当fn应用于它们时，这些元素将生成包含在b中的值。*

```js
const intersectionBy = (a, b, fn) => {
  const s = new Set(b.map(fn));
  return a.filter(x => s.has(fn(x)));
};
```

**Example**

```js
intersectionBy([2.1, 1.2], [2.3, 3.4], Math.floor); // [2.1]
```



##### 38|intersectionWith

*使用提供的比较器函数返回两个数组中存在的元素列表。 将**Array.prototype.filter（）**和**Array.prototype.findIndex（）**与提供的比较器结合使用以确定相交值。*

```js
const intersectionWith = (a, b, comp) => a.filter(x => b.findIndex(y => comp(x, y)) !== -1);
```

**Example**

```js
intersectionWith([1, 1.2, 1.5, 3, 0], [1.9, 3, 0, 3.9], (a, b) => Math.round(a) === Math.round(b)); // [1.5, 3, 0]
```



##### 39|isSorted

*如果数组按升序排序，则返回1;如果按降序排序，则返回-1;如果未排序，则返回0。 计算前两个元素的排序方向。使用**Object.entries（）**循环遍历数组对象并成对比较它们。如果方向改变则返回0，或者如果到达最后一个元素则返回方向。*

```js
const isSorted = arr => {
  let direction = -(arr[0] - arr[1]);
  for (let [i, val] of arr.entries()) {
    direction = !direction ? -(arr[i - 1] - arr[i]) : direction;
    if (i === arr.length - 1) return !direction ? 0 : direction;
    else if ((val - arr[i + 1]) * direction > 0) return 0;
  }
};
```

**Example**

```js
isSorted([0, 1, 2, 2]); // 1
isSorted([4, 3, 2]); // -1
isSorted([4, 3, 5]); // 0
```



##### 40|join

*将数组的所有元素连接成一个字符串并返回该字符串。使用分隔符和结束分隔符。 使用**Array.prototype.reduce（）**将元素组合成一个字符串。省略第二个参数**separator**，使用'，'的默认分隔符。省略第三个参数end，默认情况下使用相同的值作为分隔符。*

```js
const join = (arr, separator = ',', end = separator) =>
  arr.reduce(
    (acc, val, i) =>
      i === arr.length - 2
        ? acc + val + end
        : i === arr.length - 1
          ? acc + val
          : acc + val + separator,
    ''
  );
```

**Example**

```js
join(['pen', 'pineapple', 'apple', 'pen'], ',', '&'); // "pen,pineapple,apple&pen"
join(['pen', 'pineapple', 'apple', 'pen'], ','); // "pen,pineapple,apple,pen"
join(['pen', 'pineapple', 'apple', 'pen']); // "pen,pineapple,apple,pen"
```



##### 41|JSONtoCSV

*将对象数组转换为逗号分隔值（CSV）字符串，该字符串仅包含指定的列。*

*使用**Array.prototype.join（delimiter）**组合列中的所有名称以创建第一行。使用**Array.prototype.map（）和Array.prototype.reduce（）**为每个对象创建一行，用空字符串替换不存在的值，并仅在列中映射值。使用**Array.prototype.join（'\ n'）**将所有行组合成一个字符串。省略第三个参数**delimiter**，使用的默认分隔符为。*

```js
const JSONtoCSV = (arr, columns, delimiter = ',') =>
  [
    columns.join(delimiter),
    ...arr.map(obj =>
      columns.reduce(
        (acc, key) => `${acc}${!acc.length ? '' : delimiter}"${!obj[key] ? '' : obj[key]}"`,
        ''
      )
    )
  ].join('\n');
```

**Example**

```js
JSONtoCSV([{ a: 1, b: 2 }, { a: 3, b: 4, c: 5 }, { a: 6 }, { b: 7 }], ['a', 'b']); // 'a,b\n"1","2"\n"3","4"\n"6",""\n"","7"'
JSONtoCSV([{ a: 1, b: 2 }, { a: 3, b: 4, c: 5 }, { a: 6 }, { b: 7 }], ['a', 'b'], ';'); // 'a;b\n"1";"2"\n"3";"4"\n"6";""\n"";"7"'
```



##### 42|last

*返回数组中的最后一个元素。* 

*使用**arr.length - 1**计算给定数组的最后一个元素的索引并返回它。*

```js
const last = arr => arr[arr.length - 1];
```

**Example**

```js
last([1, 2, 3]); // 3
```



##### 43|longestItem

*获取具有length属性的任意数量的可迭代对象或对象，并返回最长的属性。如果多个对象具有相同的长度，则将返回第一个对象。如果未提供参数，则返回**undefined**。 使用**Array.prototype.reduce（）**，比较对象的长度以找到最长的对象。*

```js
const longestItem = (...vals) => vals.reduce((a, x) => (x.length > a.length ? x : a));
```

**Example**

```js
longestItem('this', 'is', 'a', 'testcase'); // 'testcase'
longestItem(...['a', 'ab', 'abc']); // 'abc'
longestItem(...['a', 'ab', 'abc'], 'abcd'); // 'abcd'
longestItem([1, 2, 3], [1, 2], [1, 2, 3, 4, 5]); // [1, 2, 3, 4, 5]
longestItem([1, 2, 3], 'foobar'); // 'foobar'
```



##### 44|mapObject

*使用函数将数组的值映射到对象，其中键 - 值对由原始值作为键和映射值组成。* 

*使用匿名内部函数作用域来声明未定义的内存空间，使用闭包来存储返回值。使用新Array存储数组，并在其数据集上使用函数映射，使用逗号运算符返回第二步，而无需从一个上下文移动到另一个上下文（由于闭包和操作顺序）。*

```js
const mapObject = (arr, fn) =>
  (a => (
    (a = [arr, arr.map(fn)]), a[0].reduce((acc, val, ind) => ((acc[val] = a[1][ind]), acc), {})
  ))();
```

**Example**

```js
const squareIt = arr => mapObject(arr, a => a * a);
squareIt([1, 2, 3]); // { 1: 1, 2: 4, 3: 9 }
```



##### 45|MaxN

*返回提供的数组中的n个最大元素。如果n大于或等于提供的数组长度，则返回原始数组（按降序排序）。* 

*使用**Array.prototype.sort（）**与**spread运算符（...）**结合使用，可以创建数组的浅层克隆并按降序对其进行排序。使用**Array.prototype.slice（）**获取指定数量的元素。省略第二个参数n，得到一个单元素数组。*

```js
const maxN = (arr, n = 1) => [...arr].sort((a, b) => b - a).slice(0, n);
```

**Example**

```js
maxN([1, 2, 3]); // [3]
maxN([1, 2, 3], 2); // [3,2]
```



##### 46|MinN

*返回提供的数组中的n个最小元素。如果n大于或等于提供的数组长度，则返回原始数组（按升序排序）。* 

*使用**Array.prototype.sort（）**与**spread运算符（...）**结合使用，可以创建数组的浅表克隆并按升序对其进行排序。使用**Array.prototype.slice（）**获取指定数量的元素。省略第二个参数n，得到一个单元素数组。*

```js
const minN = (arr, n = 1) => [...arr].sort((a, b) => a - b).slice(0, n);
```

**Example**

```js
minN([1, 2, 3]); // [1]
minN([1, 2, 3], 2); // [1,2]
```



##### 47|none

*如果提供的谓词函数对集合中的所有元素返回false，则返回true，否则返回false。 使用**Array.prototype.some（）**来测试集合中的任何元素是否基于fn返回**true**。省略第二个参数fn，使用**Boolean**作为默认值。*

```js
const none = (arr, fn = Boolean) => !arr.some(fn);

```

**Example**

```js
one([0, 1, 3, 0], x => x == 2); // true
none([0, 0, 0]); // true

```



##### 48|nthElement

*返回数组的第n个元素。* 

*使用**Array.prototype.slice（）**获取包含第一个元素的第n个元素的数组。如果索引超出范围，则返回**undefined**。省略第二个参数n，得到数组的第一个元素。*

```js
onst nthElement = (arr, n = 0) => (n === -1 ? arr.slice(n) : arr.slice(n, n + 1))[0];
```

**Example**

```js
nthElement(['a', 'b', 'c'], 1); // 'b'
nthElement(['a', 'b', 'b'], -3); // 'a'
```



##### 49|offset

*将指定数量的元素移动到数组的末尾。* 

*使用**Array.prototype.slice（）**两次来获取指定索引之后的元素以及之前的元素。使用**扩展运算符（...）**将两者合并为一个数组。如果**offset**为负数，则元素将从**end**移动到**start**。*

```js
onst offset = (arr, offset) => [...arr.slice(offset), ...arr.slice(0, offset)];
```

**Example**

```js
offset([1, 2, 3, 4, 5], 2); // [3, 4, 5, 1, 2]
offset([1, 2, 3, 4, 5], -2); // [4, 5, 1, 2, 3]
```



##### 50|partition

*将元素分组为两个数组，具体取决于所提供的函数对每个元素的真实性。* 

*使用**Array.prototype.reduce（）**创建两个数组的数组。使用**Array.prototype.push（）**将fn返回true的元素添加到第一个数组，将fn返回false的元素添加到第二个数组。*

```js
const partition = (arr, fn) =>
  arr.reduce(
    (acc, val, i, arr) => {
      acc[fn(val, i, arr) ? 0 : 1].push(val);
      return acc;
    },
    [[], []]
  );
```

**Example**

```js
const users = [{ user: 'barney', age: 36, active: false }, { user: 'fred', age: 40, active: true }];
partition(users, o => o.active); // [[{ 'user': 'fred',    'age': 40, 'active': true }],[{ 'user': 'barney',  'age': 36, 'active': false }]]
```



##### 51|permutations

*警告：此函数的执行时间随每个数组元素呈指数增长。超过8到10个条目的任何内容都会导致浏览器挂起，因为它会尝试解决所有不同的组合。* 

*生成数组元素的所有排列（包含重复项）。* 

*使用递归。对于给定数组中的每个元素，为其余元素创建所有部分排列。使用**Array.prototype.map（）**将元素与每个部分排列组合，然后使用**Array.prototype.reduce（）**将所有排列组合在一个数组中。基本情况是阵列长度等于2或1。*

```js
const permutations = arr => {
  if (arr.length <= 2) return arr.length === 2 ? [arr, [arr[1], arr[0]]] : arr;
  return arr.reduce(
    (acc, item, i) =>
      acc.concat(
        permutations([...arr.slice(0, i), ...arr.slice(i + 1)]).map(val => [item, ...val])
      ),
    []
  );
};
```

**Example**

```js
permutations([1, 33, 5]); // [ [ 1, 33, 5 ], [ 1, 5, 33 ], [ 33, 1, 5 ], [ 33, 5, 1 ], [ 5, 1, 33 ], [ 5, 33, 1 ] ]
```



##### 52|pull

*改变原始数组以过滤掉指定的值。* 

*使用**Array.prototype.filter（）**和**Array.prototype.includes（）**来提取不需要的值。使用**Array.prototype.length = 0**通过将其长度重置为零来改变传入的数组，并使用**Array.prototype.push（）**仅使用提取的值重新填充它。 （对于不改变原始数组的片段，请参阅 Xprod*

```js
onst pull = (arr, ...args) => {
  let argState = Array.isArray(args[0]) ? args[0] : args;
  let pulled = arr.filter((v, i) => !argState.includes(v));
  arr.length = 0;
  pulled.forEach(v => arr.push(v));
};
```

**Example**

```js
let myArray = ['a', 'b', 'c', 'a', 'b', 'c'];
pull(myArray, 'a', 'c'); // myArray = [ 'b', 'b' ]
```



##### 53|pullAtIndex

*改变原始数组以过滤掉指定索引处的值。* 

*使用**Array.prototype.filter（）**和**Array.prototype.includes（）**来提取不需要的值。使用**Array.prototype.length = 0**通过将其长度重置为零来改变传入的数组，并使用**Array.prototype.push（）**仅使用提取的值重新填充它。使用**Array.prototype.push（）**来跟踪拉取的值*

```js
const pullAtIndex = (arr, pullArr) => {
  let removed = [];
  let pulled = arr
    .map((v, i) => (pullArr.includes(i) ? removed.push(v) : v))
    .filter((v, i) => !pullArr.includes(i));
  arr.length = 0;
  pulled.forEach(v => arr.push(v));
  return removed;
};
```

**Example**

```js
et myArray = ['a', 'b', 'c', 'd'];
let pulled = pullAtIndex(myArray, [1, 3]); // myArray = [ 'a', 'c' ] , pulled = [ 'b', 'd' ]
```



##### 54|pullAtValue 

*改变原始数组以过滤掉指定的值。返回已删除的元素。* 

*使用**Array.prototype.filter（）**和**Array.prototype.includes（）**来提取不需要的值。使用**Array.prototype.length = 0**通过将其长度重置为零来改变传入的数组，并使用**Array.prototype.push（）**仅使用提取的值重新填充它。使用**Array.prototype.push（）**来跟踪拉取的值*

```js
onst pullAtValue = (arr, pullArr) => {
  let removed = [],
    pushToRemove = arr.forEach((v, i) => (pullArr.includes(v) ? removed.push(v) : v)),
    mutateTo = arr.filter((v, i) => !pullArr.includes(v));
  arr.length = 0;
  mutateTo.forEach(v => arr.push(v));
  return removed;
};
```

**Example**

```js
let myArray = ['a', 'b', 'c', 'd'];
let pulled = pullAtValue(myArray, ['b', 'd']); // myArray = [ 'a', 'c' ] , pulled = [ 'b', 'd' ]
```



##### 55|pullBy

*根据给定的迭代器函数，对原始数组进行变换以过滤掉指定的值。* 

*检查函数中是否提供了最后一个参数。使用**Array.prototype.map（）**将迭代器函数fn应用于所有数组元素。使用**Array.prototype.filter（）**和**Array.prototype.includes（）**来提取不需要的值。使用**Array.prototype.length = 0**通过将其长度重置为零来改变传入的数组，并使用**Array.prototype.push（）**仅使用提取的值重新填充它。*

```js
const pullBy = (arr, ...args) => {
  const length = args.length;
  let fn = length > 1 ? args[length - 1] : undefined;
  fn = typeof fn == 'function' ? (args.pop(), fn) : undefined;
  let argState = (Array.isArray(args[0]) ? args[0] : args).map(val => fn(val));
  let pulled = arr.filter((v, i) => !argState.includes(fn(v)));
  arr.length = 0;
  pulled.forEach(v => arr.push(v));
};
```

**Example**

```js
var myArray = [{ x: 1 }, { x: 2 }, { x: 3 }, { x: 1 }];
pullBy(myArray, [{ x: 1 }, { x: 3 }], o => o.x); // myArray = [{ x: 2 }]
```



##### 56|reducedFilter

*根据条件过滤对象数组，同时过滤掉未指定的键。 使用**Array.prototype.filter（）**根据谓词fn过滤数组，以便返回条件返回truthy值的对象。在过滤的数组上，使用**Array.prototype.map（）**返回使用**Array.prototype.reduce（）**的新对象，以过滤掉未作为keys参数提供的键。*

```js
const reducedFilter = (data, keys, fn) =>
  data.filter(fn).map(el =>
    keys.reduce((acc, key) => {
      acc[key] = el[key];
      return acc;
    }, {})
  );
```

**Example**

```js
const data = [
  {
    id: 1,
    name: 'john',
    age: 24
  },
  {
    id: 2,
    name: 'mike',
    age: 50
  }
];

reducedFilter(data, ['id', 'name'], item => item.age > 24); // [{ id: 2, name: 'mike'}]
```



##### 57|reduceSuccessive

*对累加器和数组中的每个元素（从左到右）应用函数，返回连续减少的值的数组。 使用**Array.prototype.reduce（）**将给定函数应用于给定数组，存储每个新结果。*

```js
const reduceSuccessive = (arr, fn, acc) =>
  arr.reduce((res, val, i, arr) => (res.push(fn(res.slice(-1)[0], val, i, arr)), res), [acc]);
```

**Example**

```js
reduceSuccessive([1, 2, 3, 4, 5, 6], (acc, val) => acc + val, 0); // [0, 1, 3, 6, 10, 15, 21]
```



##### 58|reduceWhich

*在应用提供的函数设置比较规则后，返回数组的最小/最大值。* 

*将**Array.prototype.reduce（）**与**comparator**函数结合使用以获取数组中的相应元素。您可以省略第二个参数**comparator**，以使用返回数组中最小元素的默认参数。*



##### 59|reject

*采用谓词和数组，如**Array.prototype.filter（）**，但只保留x，如果**pred（x）=== false**。*

```js
const reject = (pred, array) => array.filter((...args) => !pred(...args));
```

Example

```js
reject(x => x % 2 === 0, [1, 2, 3, 4, 5]); // [1, 3, 5]
reject(word => word.length > 4, ['Apple', 'Pear', 'Kiwi', 'Banana']); // ['Pear', 'Kiwi']
```



##### 60|remove

*从给定函数返回false的数组中删除元素。 使用**Array.prototype.filter（）**查找返回truthy值的数组元素和**Array.prototype.reduce（）**以使用**Array.prototype.splice（）**删除元素。使用三个参数**（value，index，array）**调用func。*

```js
const remove = (arr, func) =>
  Array.isArray(arr)
    ? arr.filter(func).reduce((acc, val) => {
      arr.splice(arr.indexOf(val), 1);
      return acc.concat(val);
    }, [])
    : [];
```

**Example**

```js
remove([1, 2, 3, 4], n => n % 2 === 0); // [2, 4]
```



##### 61|sample

*从数组中返回一个随机元素。* 

*使用**Math.random（）**生成一个随机数，将其乘以长度，并使用**Math.floor（）**将其四舍五入为最接近的整数。此方法也适用于字符串*

```js
const sample = arr => arr[Math.floor(Math.random() * arr.length)];
```

**Example**

```js
sample([3, 7, 9, 11]); // 9
```



##### 62|sampleSize

*从数组到数组大小的唯一键获取n个随机元素。* 

*使用Fisher-Yates算法对阵列进行混洗。使用**Array.prototype.slice（）**获取前n个元素。省略第二个参数，n从数组中随机获得一个元素。*

```js
onst sampleSize = ([...arr], n = 1) => {
  let m = arr.length;
  while (m) {
    const i = Math.floor(Math.random() * m--);
    [arr[m], arr[i]] = [arr[i], arr[m]];
  }
  return arr.slice(0, n);
};
```

**Example**

```js
sampleSize([1, 2, 3], 2); // [3,1]
sampleSize([1, 2, 3], 4); // [2,3,1]
```



##### 63|shank

*具有与**Array.prototype.splice（）**相同的功能，但返回一个新数组而不是改变原始数组。* 

*在删除现有元素和/或添加新元素后，使用**Array.prototype.slice（）**和**Array.prototype.concat（）**获取包含新内容的新数组。省略第二个参数**index**，从0开始。省略第三个参数**delCount**，删除0个元素。省略第四个参数**elements**，以便不添加任何新元素。*

```js
const shank = (arr, index = 0, delCount = 0, ...elements) =>
  arr
    .slice(0, index)
    .concat(elements)
    .concat(arr.slice(index + delCount));
```

**Example**

```js
const names = ['alpha', 'bravo', 'charlie'];
const namesAndDelta = shank(names, 1, 0, 'delta'); // [ 'alpha', 'delta', 'bravo', 'charlie' ]
const namesNoBravo = shank(names, 1, 1); // [ 'alpha', 'charlie' ]
console.log(names); // ['alpha', 'bravo', 'charlie']
```



##### 64|shuffle

*随机化数组值的顺序，返回一个新数组。 使用**Fisher-Yates**算法重新排序数组的元素。*

```js
const shuffle = ([...arr]) => {
  let m = arr.length;
  while (m) {
    const i = Math.floor(Math.random() * m--);
    [arr[m], arr[i]] = [arr[i], arr[m]];
  }
  return arr;
};
```

**Example**

```js
const foo = [1, 2, 3];
shuffle(foo); // [2, 3, 1], foo = [1, 2, 3]
```



##### 65|similarity

*返回两个数组中出现的元素数组。* 

*使用**Array.prototype.filter（）**删除不属于值的值，使用**Array.prototype.includes（）**确定。*

```js
const similarity = (arr, values) => arr.filter(v => values.includes(v));
```

**Example**

```js
similarity([1, 2, 3], [1, 2, 4]); // [1, 2]
```



##### 66|sortedIndex

*返回应将值插入数组的最低索引，以便维护其排序顺序。 检查数组是否按降序排序（松散）。使用**Array.prototype.findIndex（）**查找应插入元素的适当索引。*

```js
const sortedIndex = (arr, n) => {
  const isDescending = arr[0] > arr[arr.length - 1];
  const index = arr.findIndex(el => (isDescending ? n >= el : n <= el));
  return index === -1 ? arr.length : index;
};
```

**Example**

```js
sortedIndex([5, 3, 2, 1], 4); // 1
sortedIndex([30, 50], 40); // 1
```



##### 67|sortedIndexBy

*返回应将值插入数组的最低索引，以便根据提供的迭代器函数维护其排序顺序。* 

*检查数组是否按降序排序（松散）。根据迭代器函数fn，使用**Array.prototype.findIndex（）**查找应插入元素的适当索引。*

```js
const sortedIndexBy = (arr, n, fn) => {
  const isDescending = fn(arr[0]) > fn(arr[arr.length - 1]);
  const val = fn(n);
  const index = arr.findIndex(el => (isDescending ? val >= fn(el) : val <= fn(el)));
  return index === -1 ? arr.length : index;
};
```

**Example**

```js
sortedIndexBy([{ x: 4 }, { x: 5 }], { x: 4 }, o => o.x); // 0
```



##### 68|sortedLastIndex

*返回应将值插入数组的最高索引，以便维护其排序顺序。 检查数组是否按降序排序（松散）。*

*使用**Array.prototype.reverse（）**和**Array.prototype.findIndex（）**来查找应插入元素的相应最后一个索引。*

```js
const sortedLastIndex = (arr, n) => {
  const isDescending = arr[0] > arr[arr.length - 1];
  const index = arr.reverse().findIndex(el => (isDescending ? n <= el : n >= el));
  return index === -1 ? 0 : arr.length - index;
};
```

**Example**

```js
sortedLastIndex([10, 20, 30, 30, 40], 30); // 4
```



##### 69|sortedLastIndexBy

*返回应将值插入数组的最高索引*，以便根据提供的迭代器函数维护其排序顺序。 检查数组是否按降序排序（松散）。*使用**Array.prototype.map（）**将迭代器函数应用于数组的所有元素。根据提供的迭代器函数，使用**Array.prototype.reverse（）**和**Array.prototype.findIndex（）**查找应插入元素的相应最后一个索引。*

```js
onst sortedLastIndexBy = (arr, n, fn) => {
  const isDescending = fn(arr[0]) > fn(arr[arr.length - 1]);
  const val = fn(n);
  const index = arr
    .map(fn)
    .reverse()
    .findIndex(el => (isDescending ? val <= el : val >= el));
  return index === -1 ? 0 : arr.length - index;
};
```

**Example**

```js
sortedLastIndexBy([{ x: 4 }, { x: 5 }], { x: 4 }, o => o.x); // 1
```



##### 70|stableSort

*对数组执行稳定排序，在值相同时保留项的初始索引。不改变原始数组，而是返回一个新数组。* 

*使用**Array.prototype.map（）**将输入数组的每个元素与其对应的索引配对。使用**Array.prototype.sort（）**和比较函数对列表进行排序，如果比较的项目相同，则保留其初始顺序。使用**Array.prototype.map（）**转换回初始数组项。*

```js
const stableSort = (arr, compare) =>
  arr
    .map((item, index) => ({ item, index }))
    .sort((a, b) => compare(a.item, b.item) || a.index - b.index)
    .map(({ item }) => item);
```

**Example**

```js
const arr = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const stable = stableSort(arr, () => 0); // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```



##### 71|symmetricDifference

*返回两个数组之间的对称差异，而不过滤掉重复的值。* 

*从每个数组创建一个Set，然后对每个数组使用**Array.prototype.filter（）**，只保留另一个不包含的值。*

```js
const symmetricDifference = (a, b) => {
  const sA = new Set(a),
    sB = new Set(b);
  return [...a.filter(x => !sB.has(x)), ...b.filter(x => !sA.has(x))];
};
```

**Example**

```js
symmetricDifference([1, 2, 3], [1, 2, 4]); // [3, 4]
symmetricDifference([1, 2, 2], [1, 3, 1]); // [2, 2, 3]
```



##### 72|symmetricDifferenceBy

*将提供的函数应用于两个数组的每个数组元素后，返回两个数组之间的对称差异。* 

*通过将fn应用于每个数组的元素来创建一个**Set**，然后对它们中的每一个使用**Array.prototype.filter（）**以仅保留不包含在另一个中的值。*

```js
const symmetricDifferenceBy = (a, b, fn) => {
  const sA = new Set(a.map(v => fn(v))),
    sB = new Set(b.map(v => fn(v)));
  return [...a.filter(x => !sB.has(fn(x))), ...b.filter(x => !sA.has(fn(x)))];
};
```

**Example**

```js
symmetricDifferenceBy([2.1, 1.2], [2.3, 3.4], Math.floor); // [ 1.2, 3.4 ]
```



##### 73|symmetricDifferenceWith

*使用提供的函数作为比较器返回两个数组之间的对称差异。* 

*使用**Array.prototype.filter（）**和**Array.prototype.findIndex（）**来查找适当的值。*

```js
const symmetricDifferenceWith = (arr, val, comp) => [
  ...arr.filter(a => val.findIndex(b => comp(a, b)) === -1),
  ...val.filter(a => arr.findIndex(b => comp(a, b)) === -1)
];
```

Example

```js
symmetricDifferenceWith(
  [1, 1.2, 1.5, 3, 0],
  [1.9, 3, 0, 3.9],
  (a, b) => Math.round(a) === Math.round(b)
); // [1, 1.2, 3.9]
```



##### 74|tail

*返回数组中除第一个元素之外的所有元素。* 

*如果数组的长度大于1，则返回**Array.prototype.slice（1）**，否则返回整个数组。*

```js
const tail = arr => (arr.length > 1 ? arr.slice(1) : arr);
```

**Example**

```js
tail([1, 2, 3]); // [2,3]
tail([1]); // [1]
```



##### 75|take

*返回一个从开头删除n个元素的数组。* 

*使用**Array.prototype.slice（）**创建一个数组切片，其中包含从头开始的n个元素。*

```js
const take = (arr, n = 1) => arr.slice(0, n);
```

**Example**

```js
take([1, 2, 3], 5); // [1, 2, 3]
take([1, 2, 3], 0); // []
```



##### 76|takeRight

*返回从末尾删除n个元素的数组。 使用**Array.prototype.slice（）**创建一个数组切片，其中包含从末尾获取的n个元素。*

```js
const takeRight = (arr, n = 1) => arr.slice(arr.length - n, arr.length);
```

**Example**

```js
takeRight([1, 2, 3], 2); // [ 2, 3 ]
takeRight([1, 2, 3]); // [3]
```



##### 77|takeRightWhile

*从数组末尾删除元素，直到传递的函数返回true。*

*返回已删除的元素。 循环遍历数组，使用**Array.prototype.reduceRight（）**并在函数返回**falsy**值时累积元素。*

```js
const takeRightWhile = (arr, func) =>
  arr.reduceRight((acc, el) => (func(el) ? acc : [el, ...acc]), []);
```

**Example**

```js
takeRightWhile([1, 2, 3, 4], n => n < 3); // [3, 4]
```



##### 78|takeWhile

*删除数组中的元素，直到传递的函数返回true。返回已删除的元素。 循环遍历数组，在**Array.prototype.entries（）**上使用**for ... of**循环，直到函数返回的值为true。使用**Array.prototype.slice（）**返回已删除的元素。*

```js
const takeWhile = (arr, func) => {
  for (const [i, val] of arr.entries()) if (func(val)) return arr.slice(0, i);
  return arr;
};
```

**Example**

```js
takeWhile([1, 2, 3, 4], n => n >= 3); // [1, 2]
```



##### 79|toHash

将给定的Array减少为值哈希（键控数据存储）。 给定一个**Iterable**或类似**Array**的结构，在提供的对象上调用**Array.prototype.reduce.call（）**来跳过它并返回一个**Object**，由参考值键入。

```js
const toHash = (object, key) =>
  Array.prototype.reduce.call(
    object,
    (acc, data, index) => ((acc[!key ? index : data[key]] = data), acc),
    {}
  );
```

**Example**

```js
toHash([4, 3, 2, 1]); // { 0: 4, 1: 3, 2: 2, 3: 1 }
toHash([{ a: 'label' }], 'a'); // { label: { a: 'label' } }
// A more in depth example:
let users = [{ id: 1, first: 'Jon' }, { id: 2, first: 'Joe' }, { id: 3, first: 'Moe' }];
let managers = [{ manager: 1, employees: [2, 3] }];
// We use function here because we want a bindable reference, but a closure referencing the hash would work, too.
managers.forEach(
  manager =>
    (manager.employees = manager.employees.map(function(id) {
      return this[id];
    }, toHash(users, 'id')))
);
managers; // [ { manager:1, employees: [ { id: 2, first: "Joe" }, { id: 3, first: "Moe" } ] } ]
```



##### 80|union

*返回两个数组中任何一个中存在的每个元素一次。 使用a和b的所有值创建一个**Set**并转换为数组。*

```js
const union = (a, b) => Array.from(new Set([...a, ...b]));
```

**Example**

```js
union([1, 2, 3], [4, 3, 2]); // [1,2,3,4]
```



##### 81|unionBy

*在将提供的函数应用于两者的每个数组元素之后，返回两个数组中任何一个中存在的每个元素。 通过将所有fn应用于a的所有值来创建**Set**。从b中的a和所有元素创建一个**Set**，其值在应用fn后与先前创建的集合中的值不匹配。返回转换为数组的最后一组。*

```js
const unionBy = (a, b, fn) => {
  const s = new Set(a.map(fn));
  return Array.from(new Set([...a, ...b.filter(x => !s.has(fn(x)))]));
};
```

**Example**

```js
unionBy([2.1], [1.2, 2.3], Math.floor); // [2.1, 1.2]
```



##### 82|unionWith

*使用提供的比较器函数返回两个数组中任何一个中存在的每个元素。 使用**Array.prototype.findIndex（）**创建一个包含b的所有值和b的值，比较器在a中找不到匹配项。*

```js
const unionWith = (a, b, comp) =>
  Array.from(new Set([...a, ...b.filter(x => a.findIndex(y => comp(x, y)) === -1)]));
```

**Example**

```js
unionWith([1, 1.2, 1.5, 3, 0], [1.9, 3, 0, 3.9], (a, b) => Math.round(a) === Math.round(b)); // [1, 1.2, 1.5, 3, 0, 3.9]
```



##### 83|uniqueElements

*返回数组的所有唯一值。 使用**ES6 Set**和**... rest**运算符可以丢弃所有重复的值。*

```js
const uniqueElements = arr => [...new Set(arr)];
```

**Example**

```js
uniqueElements([1, 2, 2, 3, 4, 4, 5]); // [1, 2, 3, 4, 5]
```



##### 84|uniqueElementsBy

*根据提供的比较器函数返回数组的所有唯一值。* 

*根据比较器函数fn，将**Array.prototype.reduce（）**和**Array.prototype.some（）**用于仅包含每个值的第一个唯一出现的数组。比较器函数有两个参数：被比较的两个元素的值。*

```js
const uniqueElementsBy = (arr, fn) =>
  arr.reduce((acc, v) => {
    if (!acc.some(x => fn(v, x))) acc.push(v);
    return acc;
  }, []);
```

**Example**

```js
uniqueElementsBy(
  [
    { id: 0, value: 'a' },
    { id: 1, value: 'b' },
    { id: 2, value: 'c' },
    { id: 1, value: 'd' },
    { id: 0, value: 'e' }
  ],
  (a, b) => a.id == b.id
); // [ { id: 0, value: 'a' }, { id: 1, value: 'b' }, { id: 2, value: 'c' } ]
```



##### 85|uniqueElementsByRight

*根据提供的比较器函数返回数组的所有唯一值。 根据比较器函数fn，将**Array.prototype.reduce（）**和**Array.prototype.some（）**用于仅包含每个值的最后一个唯一出现的数组。比较器函数有两个参数：被比较的两个元素的值。*

```js
const uniqueElementsByRight = (arr, fn) =>
  arr.reduceRight((acc, v) => {
    if (!acc.some(x => fn(v, x))) acc.push(v);
    return acc;
  }, []);
```

**Example**

```js
uniqueElementsByRight(
  [
    { id: 0, value: 'a' },
    { id: 1, value: 'b' },
    { id: 2, value: 'c' },
    { id: 1, value: 'd' },
    { id: 0, value: 'e' }
  ],
  (a, b) => a.id == b.id
); // [ { id: 0, value: 'e' }, { id: 1, value: 'd' }, { id: 2, value: 'c' } ]
```



##### 86|uniqueSymmetricDifference

*返回两个数组之间唯一的对称差异，不包含任一数组的重复值。* 

*在每个数组上使用**Array.prototype.filter（）**和**Array.prototype.includes（）**来删除另一个数组中包含的值，然后从结果中创建一个Set，删除重复的值。*

```js
const uniqueSymmetricDifference = (a, b) => [
  ...new Set([...a.filter(v => !b.includes(v)), ...b.filter(v => !a.includes(v))])
];
```

**Example**

```js
uniqueSymmetricDifference([1, 2, 3], [1, 2, 4]); // [3, 4]
uniqueSymmetricDifference([1, 2, 2], [1, 3, 1]); // [2, 3]
```



##### 87|unzip

*创建一个数组数组，取消组合zip生成的数组中的元素。 使用**Math.max.apply（）**获取数组中最长的子数组**Array.prototype.map（）**，使每个元素成为一个数组。使用**Array.prototype.reduce（）**和**Array.prototype.forEach（）**将分组值映射到单个数组。*

```js
const unzip = arr =>
  arr.reduce(
    (acc, val) => (val.forEach((v, i) => acc[i].push(v)), acc),
    Array.from({
      length: Math.max(...arr.map(x => x.length))
    }).map(x => [])
  );
```

**Example**

```js
unzip([['a', 1, true], ['b', 2, false]]); // [['a', 'b'], [1, 2], [true, false]]
unzip([['a', 1, true], ['b', 2]]); // [['a', 'b'], [1, 2], [true]]
```



##### 88|unzipWith

*创建一个元素数组，取消组合zip生成的数组中的元素并应用提供的函数。*

*使用**Math.max.apply（）**获取数组中最长的子数组**Array.prototype.map（）**，使每个元素成为一个数组。使用**Array.prototype.reduce（）**和**Array.prototype.forEach（）**将分组值映射到单个数组。使用**Array.prototype.map（）**和扩展运算符**（...）**将fn应用于每个单独的元素组。*

```js
const unzipWith = (arr, fn) =>
  arr
    .reduce(
      (acc, val) => (val.forEach((v, i) => acc[i].push(v)), acc),
      Array.from({
        length: Math.max(...arr.map(x => x.length))
      }).map(x => [])
    )
    .map(val => fn(...val));
```

**Example**

```js
unzipWith([[1, 10, 100], [2, 20, 200]], (...args) => args.reduce((acc, v) => acc + v, 0)); // [3, 30, 300]
```



##### 89|without

*过滤掉具有指定值之一的数组元素。 使用**Array.prototype.filter（）**创建一个排除（使用**！Array.includes（）**）所有给定值的数组。 （对于改变原始数组的片段，请参阅**pull**）*

```js
const without = (arr, ...args) => arr.filter(v => !args.includes(v));
```

**Example**

```js
without([2, 1, 2, 3], 1, 2); // [3]
```



##### 90|xProd

*通过从数组创建每个可能的对，从提供的两个数组中创建一个新数组。 使用**Array.prototype.reduce（），Array.prototype.map（）和Array.prototype.concat（）**从两个数组的元素中生成每个可能的对，并将它们保存在一个数组中。*

```js
const xProd = (a, b) => a.reduce((acc, x) => acc.concat(b.map(y => [x, y])), []);
```

**Example**

```js
xProd([1, 2], ['a', 'b']); // [[1, 'a'], [1, 'b'], [2, 'a'], [2, 'b']]
```



##### 91|ZIP

*创建一个元素数组，根据原始数组中的位置进行分组。 使用**Math.max.apply（）**获取参数中最长的数组。*

*创建一个长度为返回值的数组，并使用带有**map-function**的**Array.from（）**创建一个分组元素数组。如果参数数组的长度不同，则在未找到值的情况下使用**undefined**。*

```js
const zip = (...arrays) => {
  const maxLength = Math.max(...arrays.map(x => x.length));
  return Array.from({ length: maxLength }).map((_, i) => {
    return Array.from({ length: arrays.length }, (_, k) => arrays[k][i]);
  });
};
```

**Example**

```js
zip(['a', 'b'], [1, 2], [true, false]); // [['a', 1, true], ['b', 2, false]]
zip(['a'], [1, 2], [true, false]); // [['a', 1, true], [undefined, 2, false]]
```



##### 92|ZipObject

*给定有效属性标识符和值数组的数组，返回将属性与值相关联的对象。* 

*由于对象可以具有未定义的值但不具有未定义的属性指针，因此使用**Array.prototype.reduce（）**来使用属性数组来确定结果对象的结构。*

```js
const zipObject = (props, values) =>
  props.reduce((obj, prop, index) => ((obj[prop] = values[index]), obj), {});
```

**Example**

```js
zipObject(['a', 'b', 'c'], [1, 2]); // {a: 1, b: 2, c: undefined}
zipObject(['a', 'b'], [1, 2, 3]); // {a: 1, b: 2}
```



##### 93|Zipwith

*创建一个元素数组，根据原始数组中的位置进行分组，并使用函数作为最后一个值来指定应如何组合分组值。 检查提供的最后一个参数是否为函数。*

*使用**Math.max（）**获取参数中最长的数组。创建一个长度为返回值的数组，并使用带有**map-function**的**Array.from（）**创建一个分组元素数组。如果参数数组的长度不同，则在未找到值的情况下使用**undefined**。使用每个组（...组）的元素调用该函数。*

```js
const zipWith = (...array) => {
  const fn = typeof array[array.length - 1] === 'function' ? array.pop() : undefined;
  return Array.from(
    { length: Math.max(...array.map(a => a.length)) },
    (_, i) => (fn ? fn(...array.map(a => a[i])) : array.map(a => a[i]))
  );
};
```

**Example**

```js
zipWith([1, 2], [10, 20], [100, 200], (a, b, c) => a + b + c); // [111,222]
zipWith(
  [1, 2, 3],
  [10, 20],
  [100, 200],
  (a, b, c) => (a != null ? a : 'a') + (b != null ? b : 'b') + (c != null ? c : 'c')
); // [111, 222, '3bc']
```



