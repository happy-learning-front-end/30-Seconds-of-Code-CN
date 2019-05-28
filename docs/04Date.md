#### 04|Date

##### 01|dayOfYear

*从Date对象获取一年中的某一天。 使用新的Date（）和**Date.prototype.getFullYear（）**将一年中的第一天作为Date对象获取，从提供的日期中减去它并除以每天的毫秒数以获得结果。使用**Math.floor（）**将生成的日计数适当地舍入为整数。*

```js
const dayOfYear = date =>
  Math.floor((date - new Date(date.getFullYear(), 0, 0)) / 1000 / 60 / 60 / 24);
```

**Example**

```js
dayOfYear(new Date()); // 272
```



##### 02|formatDuration

*返回给定毫秒数的人类可读格式。 将ms除以适当的值，以获得白天，小时，分钟，秒和毫秒的适当值。将**Object.entries（）**与**Array.prototype.filter（）**一起使用以仅保留非零值。使用**Array.prototype.map（）**为每个值创建字符串，并进行适当的多元化。使用**String.prototype.join（'，'）**将值组合成一个字符串。*

```js
const formatDuration = ms => {
  if (ms < 0) ms = -ms;
  const time = {
    day: Math.floor(ms / 86400000),
    hour: Math.floor(ms / 3600000) % 24,
    minute: Math.floor(ms / 60000) % 60,
    second: Math.floor(ms / 1000) % 60,
    millisecond: Math.floor(ms) % 1000
  };
  return Object.entries(time)
    .filter(val => val[1] !== 0)
    .map(([key, val]) => `${val} ${key}${val !== 1 ? 's' : ''}`)
    .join(', ');
};
```

**Example**

```js
formatDuration(1001); // '1 second, 1 millisecond'
formatDuration(34325055574); // '397 days, 6 hours, 44 minutes, 15 seconds, 574 milliseconds'
```



##### 03|getColonTimeFromDate

*从Date对象返回**HH：MM：SS**形式的字符串。 使用**Date.prototype.toTimeString（）**和**String.prototype.slice（）**来获取给定**Date**对象的**HH：MM：SS**部分。*

```js
const getColonTimeFromDate = date => date.toTimeString().slice(0, 8);
```

**Example**

```js
getColonTimeFromDate(new Date()); // "08:38:00"
```



##### 04|getDaysDiffBetweenDates

*返回两个日期之间的差异（以天为单位）。 计算两个Date对象之间的差异（以天为单位）。*

```js
const getDaysDiffBetweenDates = (dateInitial, dateFinal) =>
  (dateFinal - dateInitial) / (1000 * 3600 * 24);
```

**Example**



```js
getDaysDiffBetweenDates(new Date('2017-12-13'), new Date('2017-12-22')); // 9
```



##### 05|getMeridiemSuffixOfInteger

*将整数转换为后缀字符串，根据其值添加**am**或**pm**。 使用模运算符（％）和条件检查将整数转换为带有**meridiem**后缀的字符串化12小时格式。*

```js
const getMeridiemSuffixOfInteger = num =>
  num === 0 || num === 24
    ? 12 + 'am'
    : num === 12
      ? 12 + 'pm'
      : num < 12
        ? (num % 12) + 'am'
        : (num % 12) + 'pm';
```

**Example**

```js
getMeridiemSuffixOfInteger(0); // "12am"
getMeridiemSuffixOfInteger(11); // "11am"
getMeridiemSuffixOfInteger(13); // "1pm"
getMeridiemSuffixOfInteger(25); // "1pm"
```



##### 06|isAfterDate

*检查日期是否在另一个日期之后。 使用大于运算符（>）检查第一个日期是否在第二个日期之后。*

```js
const isAfterDate = (dateA, dateB) => dateA > dateB;
```

**Example**

```js
isAfterDate(new Date(2010, 10, 21), new Date(2010, 10, 20)); // true
```



##### 07|isBeforeDate

*检查日期是否在另一个日期之前。 使用小于运算符（<）来检查第一个日期是否在第二个日期之前。*

```js
const isBeforeDate = (dateA, dateB) => dateA < dateB;
```

**Example**



```js
isBeforeDate(new Date(2010, 10, 20), new Date(2010, 10, 21)); // true
```



##### 08|isSameDate

*检查日期是否与另一个日期相同。 使用**Date.prototype.toISOString（）**和严格相等性检查**（===）**来检查第一个日期是否与第二个日期相同。*

```js
const isSameDate = (dateA, dateB) => dateA.toISOString() === dateB.toISOString();
```

**Example**



```js
isSameDate(new Date(2010, 10, 20), new Date(2010, 10, 20)); // true
```



##### 09|maxDate

*返回给定日期的最大值。 使用**Math.max.apply（）**查找最大日期值，使用新的Date（）将其转换为Date对象*

```js
const maxDate = (...dates) => new Date(Math.max.apply(null, ...dates));
```

**Example**



```js
const array = [
  new Date(2017, 4, 13),
  new Date(2018, 2, 12),
  new Date(2016, 0, 10),
  new Date(2016, 0, 9)
];
maxDate(array); // 2018-03-11T22:00:00.000Z
```



##### 10|minDate

*返回给定日期的最小值。 使用**Math.min.apply（）**查找最小日期值，使用新的Date（）将其转换为Date对象。 **const minDate =（... dates）=> new Date（Math.min.apply（null，... dates））;***

```js
const minDate = (...dates) => new Date(Math.min.apply(null, ...dates));
```

**Example**



```js
const array = [
  new Date(2017, 4, 13),
  new Date(2018, 2, 12),
  new Date(2016, 0, 10),
  new Date(2016, 0, 9)
];
minDate(array); // 2016-01-08T22:00:00.000Z
```



##### 11|tomorrow

*结果以明天的日期的字符串表示。 使用新的**Date（）**获取当前日期，使用**Date.getDate（）**递增1，并使用**Date.setDate（）**将值设置为结果。使用**Date.prototype.toISOString（）以yyyy-mm-dd**格式返回字符串。*

```js
const tomorrow = () => {
  let t = new Date();
  t.setDate(t.getDate() + 1);
  return t.toISOString().split('T')[0];
};
```

**Example**



```js
tomorrow(); // 2018-10-19 (if current date is 2018-10-18)
```

