#### Utility

##### 01|castArray

*如果不是一个数组，则将提供的值转换为数组。 使用**Array.prototype.isArray（）**来确定val是否为数组并按原样返回或相应地封装在数组中。*

```js
const castArray = val => (Array.isArray(val) ? val : [val]);
```

**Example**

```js
castArray('foo'); // ['foo']
castArray([1]); // [1]
```



##### 02|cloneRegExp

*克隆正则表达式。 使用新的**RegExp（），RegExp.source**和RegExp.flags克隆给定的正则表达式。*

```js
const cloneRegExp = regExp => new RegExp(regExp.source, regExp.flags);
```

**Example**

```js
const regExp = /lorem ipsum/gi;
const regExp2 = cloneRegExp(regExp); // /lorem ipsum/gi
```



##### 03|coalesce

*返回第一个非**null / undefined**参数。 使用**Array.prototype.find（）**返回第一个非**null / undefined**参数。*

```js
const coalesce = (...args) => args.find(_ => ![undefined, null].includes(_));
```

**Example**

```js
coalesce(null, undefined, '', NaN, 'Waldo'); // ""
```



##### 04|coalesceFactory

*返回自定义的**coalesce**函数，该函数返回从提供的参数验证函数返回true的第一个参数。 使用**Array.prototype.find（）**返回从提供的参数验证函数返回true的第一个参数。*

```js
const coalesceFactory = valid => (...args) => args.find(valid);
```

**Example**

```js
const customCoalesce = coalesceFactory(_ => ![null, undefined, '', NaN].includes(_));
customCoalesce(undefined, null, NaN, '', 'Waldo'); // "Waldo"
```



##### 05|extendHex

*将3位颜色代码扩展为6位颜色代码。 使用**Array.prototype.map（），String.prototype.split（）**和**Array.prototype.join（）**连接映射的数组，以将3位RGB标记的十六进制颜色代码转换为6位数的形式。 **Array.prototype.slice（）**用于从字符串start中删除＃，因为它被添加一次。*

```js
const extendHex = shortHex =>
  '#' +
  shortHex
    .slice(shortHex.startsWith('#') ? 1 : 0)
    .split('')
    .map(x => x + x)
    .join('');
```

**Example**

```js
extendHex('#03f'); // '#0033ff'
extendHex('05a'); // '#0055aa'
```



##### 06|getURLParameters

*返回包含当前URL参数的对象。 将**String.match（）**与适当的正则表达式一起使用以获取所有键值对，**Array.prototype.reduce（）**将它们映射并组合成单个对象。将**location.search**作为应用于当前URL的参数。*

```js
onst getURLParameters = url =>
  (url.match(/([^?=&]+)(=([^&]*))/g) || []).reduce(
    (a, v) => ((a[v.slice(0, v.indexOf('='))] = v.slice(v.indexOf('=') + 1)), a),
    {}
  );
```

**Example**

```js
getURLParameters('http://url.com/page?name=Adam&surname=Smith'); // {name: 'Adam', surname: 'Smith'}
getURLParameters('google.com'); // {}
```



##### 07|hexToRGB

*如果提供了alpha值，则将颜色代码转换为**rgb（）**或**rgba（）**字符串。 使用按位右移运算符和掩码位和＆（和）运算符将十六进制颜色代码（带或不带前缀）转换为带有RGB值的字符串。如果是3位数颜色代码，请先转换为6位数版本。如果alpha值与6位十六进制一起提供，则返回rgba（）字符串。*

```js
const hexToRGB = hex => {
  let alpha = false,
    h = hex.slice(hex.startsWith('#') ? 1 : 0);
  if (h.length === 3) h = [...h].map(x => x + x).join('');
  else if (h.length === 8) alpha = true;
  h = parseInt(h, 16);
  return (
    'rgb' +
    (alpha ? 'a' : '') +
    '(' +
    (h >>> (alpha ? 24 : 16)) +
    ', ' +
    ((h & (alpha ? 0x00ff0000 : 0x00ff00)) >>> (alpha ? 16 : 8)) +
    ', ' +
    ((h & (alpha ? 0x0000ff00 : 0x0000ff)) >>> (alpha ? 8 : 0)) +
    (alpha ? `, ${h & 0x000000ff}` : '') +
    ')'
  );
};
```

**Example**

```js
hexToRGB('#27ae60ff'); // 'rgba(39, 174, 96, 255)'
hexToRGB('27ae60'); // 'rgb(39, 174, 96)'
hexToRGB('#fff'); // 'rgb(255, 255, 255)'
```



##### 08|httpGet

*对传递的URL发出GET请求。 使用**XMLHttpRequest web api**向给定的URL发出get请求。处理**onload**事件，通过调用给定的回调**responseText**。通过运行提供的错误函数处理**onerror**事件。省略第三个参数err，默认情况下将错误记录到控制台的错误流中。*

```js
const httpGet = (url, callback, err = console.error) => {
  const request = new XMLHttpRequest();
  request.open('GET', url, true);
  request.onload = () => callback(request.responseText);
  request.onerror = () => err(request);
  request.send();
};
```

**Example**

```js
httpGet(
  'https://jsonplaceholder.typicode.com/posts/1',
  console.log
); /*
Logs: {
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
  "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
}
*/
```



##### 09|httpPost

*对传递的URL发出POST请求。 使用**XMLHttpRequest web api**向给定的URL发出请求。使用**setRequestHeader**方法设置**HTTP**请求标头的值。处理**onload**事件，通过调用给定的回调**responseText**。通过运行提供的错误函数处理**onerror**事件。省略第三个参数**data**，不向提供的**url**发送数据。省略第四个参数**err**，默认情况下将错误记录到控制台的错误流中。*

```js
const httpPost = (url, data, callback, err = console.error) => {
  const request = new XMLHttpRequest();
  request.open('POST', url, true);
  request.setRequestHeader('Content-type', 'application/json; charset=utf-8');
  request.onload = () => callback(request.responseText);
  request.onerror = () => err(request);
  request.send(data);
};
```

**Example**

```js
const newPost = {
  userId: 1,
  id: 1337,
  title: 'Foo',
  body: 'bar bar bar'
};
const data = JSON.stringify(newPost);
httpPost(
  'https://jsonplaceholder.typicode.com/posts',
  data,
  console.log
); /*
Logs: {
  "userId": 1,
  "id": 1337,
  "title": "Foo",
  "body": "bar bar bar"
}
*/
httpPost(
  'https://jsonplaceholder.typicode.com/posts',
  null, // does not send a body
  console.log
); /*
Logs: {
  "id": 101
}
*/
```



##### 10|isBrowser

*确定当前运行时环境是否为浏览器，以便前端模块可以在服务器（节点）上运行而不会抛出错误。 对窗口和文档的值类型使用**Array.prototype.includes（）**（通常只在浏览器环境中提供的全局变量，除非它们是明确定义的），如果其中一个未定义，则返回true。 **typeof**允许检查全局变量是否存在而不抛出**ReferenceError**。如果它们都未定义，则假定当前环境是浏览器。*

```js
const isBrowser = () => ![typeof window, typeof document].includes('undefined');
```

**Example**

```js
isBrowser(); // true (browser)
isBrowser(); // false (Node)
```



##### 11|mostPerformant

*返回执行速度最快的函数数组中函数的索引。 使用**Array.prototype.map（）**生成一个数组，其中每个值是迭代次数后执行函数所用的总时间。使用**performance.now（）**值之前和之后的差异来获得高精度的总时间（以毫秒为单位）。使用**Math.min（）**查找最短执行时间，并返回与最高性能函数的索引相对应的最短时间的索引。省略第二个参数**iterations**，使用默认的**10,000**次迭代。迭代次数越多，结果越可靠，但需要的时间越长。*

```js
const mostPerformant = (fns, iterations = 10000) => {
  const times = fns.map(fn => {
    const before = performance.now();
    for (let i = 0; i < iterations; i++) fn();
    return performance.now() - before;
  });
  return times.indexOf(Math.min(...times));
};
```

**Example**

```js
mostPerformant([
  () => {
    // Loops through the entire array before returning `false`
    [1, 2, 3, 4, 5, 6, 7, 8, 9, '10'].every(el => typeof el === 'number');
  },
  () => {
    // Only needs to reach index `1` before returning false
    [1, '2', 3, 4, 5, 6, 7, 8, 9, 10].every(el => typeof el === 'number');
  }
]); // 1
```



##### 12|nthArg

*创建一个在索引n处获取参数的函数。如果n为负数，则返回结尾的第n个参数。 使用**Array.prototype.slice（）**在索引n处获取所需的参数。*

```js
const nthArg = n => (...args) => args.slice(n)[0];
```

**Example**

```js
const third = nthArg(2);
third(1, 2, 3); // 3
third(1, 2); // undefined
const last = nthArg(-1);
last(1, 2, 3, 4, 5); // 5
```



##### 13|parseCookie

*解析HTTP Cookie标头字符串并返回所有cookie名称 - 值对的对象。 使用**String.prototype.split（';'）**将键值对彼此分开。使用**Array.prototype.map（）**和**String.prototype.split（'='）**将键与每对中的值分开。使用**Array.prototype.reduce（）**和**decodeURIComponent（）**创建具有所有键值对的对象。*

```js
const parseCookie = str =>
  str
    .split(';')
    .map(v => v.split('='))
    .reduce((acc, v) => {
      acc[decodeURIComponent(v[0].trim())] = decodeURIComponent(v[1].trim());
      return acc;
    }, {});
```

**Example**

```js
parseCookie('foo=bar; equation=E%3Dmc%5E2'); // { foo: 'bar', equation: 'E=mc^2' }
```



##### 14|prettyBytes

将字节数转换为人类可读的字符串。 使用基于指数访问的单元的数组字典。使用Number.toPrecision（）将数字截断为一定数量的数字。通过构建它来返回经过修饰的字符串，同时考虑所提供的选项以及它是否为负数。省略第二个参数precision，使用3位数的默认精度。省略第三个参数addSpace，默认情况下在数字和单位之间添加空格。

```js
onst prettyBytes = (num, precision = 3, addSpace = true) => {
  const UNITS = ['B', 'KB', 'MB', 'GB', 'TB', 'PB', 'EB', 'ZB', 'YB'];
  if (Math.abs(num) < 1) return num + (addSpace ? ' ' : '') + UNITS[0];
  const exponent = Math.min(Math.floor(Math.log10(num < 0 ? -num : num) / 3), UNITS.length - 1);
  const n = Number(((num < 0 ? -num : num) / 1000 ** exponent).toPrecision(precision));
  return (num < 0 ? '-' : '') + n + (addSpace ? ' ' : '') + UNITS[exponent];
};
```

**Example**

```js
prettyBytes(1000); // "1 KB"
prettyBytes(-27145424323.5821, 5); // "-27.145 GB"
prettyBytes(123456789, 3, false); // "123MB"
```



##### 15|randomHexColorCode

*生成随机十六进制颜色代码。 使用Math.random生成随机的24位（6x4bits）十六进制数。使用位移，然后使用toString（16）将其转换为十六进制字符串。*

```js
const randomHexColorCode = () => {
  let n = (Math.random() * 0xfffff * 1000000).toString(16);
  return '#' + n.slice(0, 6);
};
```

**Example**

```js
randomHexColorCode(); // "#e34155"
```



##### 16|RGBToHex

*将RGB组件的值转换为颜色代码。 使用按位左移运算符（<<）和toString（16），然后使用**String.padStart（6，'0'）**将给定的RGB参数转换为十六进制字符串，以获得6位十六进制值。*

```js
const RGBToHex = (r, g, b) => ((r << 16) + (g << 8) + b).toString(16).padStart(6, '0');
```

**Example**

```js
RGBToHex(255, 165, 1); // 'ffa501'
```



##### 17|serializeCookie

*将cookie名称 - 值对序列化为Set-Cookie标头字符串。 使用模板文字和**encodeURIComponent（）**来创建适当的字符串。*

```js
const serializeCookie = (name, val) => `${encodeURIComponent(name)}=${encodeURIComponent(val)}`;
```

**Example**

```js
serializeCookie('foo', 'bar'); // 'foo=bar'
```



##### 18|timeTaken

*测量函数执行所花费的时间。 使用console.time（）和console.timeEnd（）来测量开始和结束时间之间的差异，以确定回调执行的时间。*

```js
const timeTaken = callback => {
  console.time('timeTaken');
  const r = callback();
  console.timeEnd('timeTaken');
  return r;
};
```

**Example**

```js
timeTaken(() => Math.pow(2, 10)); // 1024, (logged): timeTaken: 0.02099609375ms
```



##### 19|toCurrency

*取一个数字并返回指定的货币格式。 使用**Intl.NumberFormat**启用国家/货币敏感格式。*

```js
const toCurrency = (n, curr, LanguageFormat = undefined) =>
  Intl.NumberFormat(LanguageFormat, { style: 'currency', currency: curr }).format(n);
```

**Example**

```js
toCurrency(123456.789, 'EUR'); // €123,456.79  | currency: Euro | currencyLangFormat: Local
toCurrency(123456.789, 'USD', 'en-us'); // $123,456.79  | currency: US Dollar | currencyLangFormat: English (United States)
toCurrency(123456.789, 'USD', 'fa'); // ۱۲۳٬۴۵۶٫۷۹ ؜$ | currency: US Dollar | currencyLangFormat: Farsi
toCurrency(322342436423.2435, 'JPY'); // ¥322,342,436,423 | currency: Japanese Yen | currencyLangFormat: Local
toCurrency(322342436423.2435, 'JPY', 'fi'); // 322 342 436 423 ¥ | currency: Japanese Yen | currencyLangFormat: Finnish
```



##### 20|toDecimalMark

*使用**toLocaleString（）**将浮点运算转换为Decimal标记形式。它使用逗号分隔字符串与数字。*

```js
const toDecimalMark = num => num.toLocaleString('en-US');
```

**Example**

```js
toDecimalMark(12305030388.9087); // "12,305,030,388.909"
```



##### 21|toOrdinalSuffix

*为数字添加序数后缀。 使用模运算符（％）查找单个和十位数的值。找出哪些序数模式数字匹配。如果在青少年模式中找到数字，请使用青少年序数。*

```js
const toOrdinalSuffix = num => {
  const int = parseInt(num),
    digits = [int % 10, int % 100],
    ordinals = ['st', 'nd', 'rd', 'th'],
    oPattern = [1, 2, 3, 4],
    tPattern = [11, 12, 13, 14, 15, 16, 17, 18, 19];
  return oPattern.includes(digits[0]) && !tPattern.includes(digits[1])
    ? int + ordinals[digits[0] - 1]
    : int + ordinals[3];
};
```

**Example**

```js
toOrdinalSuffix('123'); // "123rd"
```



##### 22|validateNumber

*如果给定值是数字，则返回**true**，否则返回**false**。 使用**！isNaN（）**与**parseFloat（）**结合使用来检查参数是否为数字。使用**isFinite（）**检查数字是否有限。使用**Number（）**检查强制是否成立。*

```js
const validateNumber = n => !isNaN(parseFloat(n)) && isFinite(n) && Number(n) == n;
```

**Example**

```js
validateNumber('10'); // true
```



##### 23|yesNO

*如果字符串是y / yes，则返回true;如果字符串是n / no，则返回false。 使用RegExp.test（）检查字符串是否为y / yes或n / no。省略第二个参数def，将默认答案设置为no。*

```js
const yesNo = (val, def = false) =>
  /^(y|yes)$/i.test(val) ? true : /^(n|no)$/i.test(val) ? false : def;
```

**Example**

```js
yesNo('Y'); // true
yesNo('yes'); // true
yesNo('No'); // false
yesNo('Foo', true); // true
```

