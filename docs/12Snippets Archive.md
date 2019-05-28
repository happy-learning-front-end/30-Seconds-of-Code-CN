#### 12|Snippets Archive(代码片段)

这些片段虽然有用且有趣，但由于具有非常特定的用例或过时而未能完全进入存储库。然而，我们觉得它们对某些读者可能仍然有用，所以他们在这里。

##### 01|binarySearch

*使用递归。与**Array.prototype.indexOf（）**类似，它查找数组中值的索引。不同之处在于此操作仅适用于排序数组，与线性搜索或**Array.prototype.indexOf（）**相比，由于其对数性质而提供了显着的性能提升。 通过重复将搜索间隔分成两半来搜索已排序的数组。从覆盖整个阵列的间隔开始。如果搜索的值小于间隔中间的项目，则递归到下半部分。否则进入上半部分。重复递归，直到找到值为mid或者你已经递归到一个大于长度的点，这意味着该值不存在并返回-1。*

```js
const binarySearch = (arr, val, start = 0, end = arr.length - 1) => {
  if (start > end) return -1;
  const mid = Math.floor((start + end) / 2);
  if (arr[mid] > val) return binarySearch(arr, val, start, mid - 1);
  if (arr[mid] < val) return binarySearch(arr, val, mid + 1, end);
  return mid;
};
```

**Example**

```js
binarySearch([1, 4, 6, 7, 12, 13, 15, 18, 19, 20, 22, 24], 6); // 2
binarySearch([1, 4, 6, 7, 12, 13, 15, 18, 19, 20, 22, 24], 21); // -1
```



##### 02|cleanObj

*删除**JSON**对象中指定的属性之外的任何属性。 使用**Object.keys（）**方法循环给定的JSON对象并删除未包含在给定数组中的键。如果你传递一个特殊的键，**childIndicator**，它将搜索深度应用于内部对象的函数。*

```js
const cleanObj = (obj, keysToKeep = [], childIndicator) => {
  Object.keys(obj).forEach(key => {
    if (key === childIndicator) {
      cleanObj(obj[key], keysToKeep, childIndicator);
    } else if (!keysToKeep.includes(key)) {
      delete obj[key];
    }
  });
  return obj;
};
```

**Example**

```js
const testObj = { a: 1, b: 2, children: { a: 1, b: 2 } };
cleanObj(testObj, ['a'], 'children'); // { a: 1, children : { a: 1}}
```



##### 03|collatz

*应用Collatz算法。 如果n是偶数，则返回n / 2。否则，返回3n + 1。*

```js
const collatz = n => (n % 2 === 0 ? n / 2 : 3 * n + 1);
```

**Example**

```js
collatz(8); // 4
```



##### 04|countVowels

*重新提供所提供字符串中的元音数量。 使用正则表达式计算字符串中元音的数量（A，E，I，O，U）。*

```js
const countVowels = str => (str.match(/[aeiou]/gi) || []).length;
```

**Example**

```js
countVowels('foobar'); // 3
countVowels('gym'); // 0
```



##### 05|factors

*返回给定**num**的因子数组。如果第二个参数设置为**true**，则仅返回**num**的素因子。如果**num**为1或0，则返回一个空数组。如果**num**小于0，则返回-int的所有因子及其加法反转。 使用**Array.from（），Array.prototype.map（）和Array.prototype.filter（）**来查找**num**的所有因子。如果给定的**num**为负数，则使用**Array.prototype.reduce（）**将相加的反转添加到数组中。如果**primes为false**，则返回所有结果，否则使用**isPrime**和**Array.prototype.filter（）**确定并仅返回素因子。省略第二个参数**primes**，默认返回素数和非素数因子。 注意： - 负数不被视为素数。*

```js
const factors = (num, primes = false) => {
  const isPrime = num => {
    const boundary = Math.floor(Math.sqrt(num));
    for (var i = 2; i <= boundary; i++) if (num % i === 0) return false;
    return num >= 2;
  };
  const isNeg = num < 0;
  num = isNeg ? -num : num;
  let array = Array.from({ length: num - 1 })
    .map((val, i) => (num % (i + 2) === 0 ? i + 2 : false))
    .filter(val => val);
  if (isNeg)
    array = array.reduce((acc, val) => {
      acc.push(val);
      acc.push(-val);
      return acc;
    }, []);
  return primes ? array.filter(isPrime) : array;
};
```

**Example**

```js
factors(12); // [2,3,4,6,12]
factors(12, true); // [2,3]
factors(-12); // [2, -2, 3, -3, 4, -4, 6, -6, 12, -12]
factors(-12, true); // [2,3]
```



##### 06|fahrenheitToCelsius

*华氏至摄氏温度转换。 如下所述转换公式是100 =（F - 32）* 5/9。*

```js
const fahrenheitToCelsius = degrees => (degrees - 32) * 5/9;
```

**Example**

```js
fahrenheitToCelsius(32); // 0
```



##### 07|fibonacciCountUntilNum

返回最多为num（0和num）的fibonnacci数。 使用数学公式计算斐波那契数的数量，直到num。

```js
const fibonacciCountUntilNum = num =>
  Math.ceil(Math.log(num * Math.sqrt(5) + 1 / 2) / Math.log((Math.sqrt(5) + 1) / 2));
```

**Example**

```js
fibonacciCountUntilNum(10); // 7
```



##### 08|fibonacciUntilNum

*生成一个包含**Fibonacci**序列的数组，直到第n个项。 创建一个特定长度的空数组，初始化前两个值（0和1）。使用**Array.prototype.reduce（）**将值添加到数组中，使用最后两个值的总和，前两个值除外。使用数学公式计算所需数组的长度。*

```js
const fibonacciUntilNum = num => {
  let n = Math.ceil(Math.log(num * Math.sqrt(5) + 1 / 2) / Math.log((Math.sqrt(5) + 1) / 2));
  return Array.from({ length: n }).reduce(
    (acc, val, i) => acc.concat(i > 1 ? acc[i - 1] + acc[i - 2] : i),
    []
  );
};
```

**Example**

```js
fibonacciUntilNum(10); // [ 0, 1, 1, 2, 3, 5, 8 ]
```



##### 09|heronArea

*仅使用3个边长，Heron公式返回三角形的面积。假设边定义有效三角形。不要以为它是一个直角三角形。 有关Heron公式的更多信息及其工作原理，请访问：**https：//en.wikipedia.org/wiki/Heron%27s_formula。 使用Math.sqrt（）**查找值的平方根。*

```js
const heronArea = (side_a, side_b, side_c) => {
    const p = (side_a + side_b + side_c) / 2
    return Math.sqrt(p * (p-side_a) * (p-side_b) * (p-side_c))
  };
```

**Example**

```js
heronArea(3, 4, 5); // 6
```



##### 10|howManyTimes

*返回**num**可以除以除数（整数或小数）而不获得小数答案的次数。适用于负整数和正整数。 如果除数为-1或1则返回无穷大。如果除数为-0或0，则返回0.否则，保持将**num**除以除数并递增i，结果为整数。返回执行循环的次数，i。*

```js
const howManyTimes = (num, divisor) => {
  if (divisor === 1 || divisor === -1) return Infinity;
  if (divisor === 0) return 0;
  let i = 0;
  while (Number.isInteger(num / divisor)) {
    i++;
    num = num / divisor;
  }
  return i;
};
```

**Example**

```js
howManyTimes(100, 2); // 2
howManyTimes(100, 2.5); // 2
howManyTimes(100, 0); // 0
howManyTimes(100, -1); // Infinity
```



##### 11|httpDelete

*对传递的URL发出DELETE请求。 使用**XMLHttpRequest web api**对给定的URL发出删除请求。通过运行提供的回调函数处理onload事件。通过运行提供的错误函数处理onerror事件。省略第三个参数，错误是默认情况下将请求记录到控制台的错误流中。*

```js
const httpDelete = (url, callback, err = console.error) => {
  const request = new XMLHttpRequest();
  request.open('DELETE', url, true);
  request.onload = () => callback(request);
  request.onerror = () => err(request);
  request.send();
};
```

**Example**

```js
httpDelete('https://website.com/users/123', request => {
  console.log(request.responseText);
}); // 'Deletes a user from the database'
```



##### 12|httpPut

*对传递的URL发出PUT请求。 使用**XMLHttpRequest web api**向给定的URL发出put请求。使用**setRequestHeader**方法设置HTTP请求标头的值。通过运行提供的回调函数处理**onload**事件。通过运行提供的错误函数处理**onerror**事件。省略最后一个参数，错误是默认情况下将请求记录到控制台的错误流中。*

```js
const httpPut = (url, data, callback, err = console.error) => {
  const request = new XMLHttpRequest();
  request.open("PUT", url, true);
  request.setRequestHeader('Content-type','application/json; charset=utf-8');
  request.onload = () => callback(request);
  request.onerror = () => err(request);
  request.send(data);
};
```

**Example**

```js
const password = "fooBaz";
const data = JSON.stringify(password);
httpPut('https://website.com/users/123', data, request => {
  console.log(request.responseText);
}); // 'Updates a user's password in database'
```



##### 13|isArmstrongNumber

*检查给定的号码是否为阿姆斯壮号码。 将给定数字转换为数字数组。使用指数运算符（**）为每个数字获得适当的功率并总结它们。如果总和等于数字本身，则返回true，否则返回false。*

```js
const isArmstrongNumber = digits =>
  (arr => arr.reduce((a, d) => a + parseInt(d) ** arr.length, 0) == digits)(
    (digits + '').split('')
  );
```

**Example**

```js
isArmstrongNumber(1634); // true
isArmstrongNumber(56); // false
```



##### 14|isSimilar

*确定模式是否与str匹配。 使用**String.toLowerCase（）**将两个字符串转换为小写，然后循环遍历str并确定它是否包含pattern的所有字符并且顺序正确。改编自这里。*

```js
onst isSimilar = (pattern, str) =>
  [...str].reduce(
      (matchIndex, char) =>
          char.toLowerCase() === (pattern[matchIndex] || '').toLowerCase()
              ? matchIndex + 1
              : matchIndex,
      0
  ) === pattern.length;
```

**Example**

```js
isSimilar('rt','Rohit'); // true
isSimilar('tr','Rohit'); // false
```



##### 15|JSONToDate

*将JSON对象转换为日期。 使用**Date（）**将JSON格式的日期转换为可读格式（dd / mm / yyyy）。*

```js
const JSONToDate = arr => {
  const dt = new Date(parseInt(arr.toString().substr(6)));
  return `${dt.getDate()}/${dt.getMonth() + 1}/${dt.getFullYear()}`;
};
```

**Exmaple**

```js
JSONToDate(/Date(1489525200000)/); // "14/3/2017"
```



##### 16|kmphToMph

将公里/小时转换为英里/小时。 *将比例常数乘以参数。*

```js
onst kmphToMph = (kmph) => 0.621371192 * kmph;
```

**Example**

```js
kmphToMph(10); // 16.09344000614692
kmphToMph(345.4); // 138.24264965280207
```



##### 17|levenshteinDistance

*计算两个弦之间的**Levenshtein**距离。 计算将**string1**转换为**string2**所需的更改（替换，删除或添加）的数量。也可用于比较两个字符串，如第二个示例所示。*

```js
const levenshteinDistance = (string1, string2) => {
  if (string1.length === 0) return string2.length;
  if (string2.length === 0) return string1.length;
  let matrix = Array(string2.length + 1)
    .fill(0)
    .map((x, i) => [i]);
  matrix[0] = Array(string1.length + 1)
    .fill(0)
    .map((x, i) => i);
  for (let i = 1; i <= string2.length; i++) {
    for (let j = 1; j <= string1.length; j++) {
      if (string2[i - 1] === string1[j - 1]) {
        matrix[i][j] = matrix[i - 1][j - 1];
      } else {
        matrix[i][j] = Math.min(
          matrix[i - 1][j - 1] + 1,
          matrix[i][j - 1] + 1,
          matrix[i - 1][j] + 1
        );
      }
    }
  }
  return matrix[string2.length][string1.length];
};
```

**Example**

```js
levenshteinDistance('30-seconds-of-code','30-seconds-of-python-code'); // 7
const compareStrings = (string1,string2) => (100 - levenshteinDistance(string1,string2) / Math.max(string1.length,string2.length));
compareStrings('30-seconds-of-code', '30-seconds-of-python-code'); // 99.72 (%)
```



##### 18|mphToKmph

*将英里/小时转换为公里/小时。 将比例常数乘以参数。*

```js
const mphToKmph = (mph) => 1.6093440006146922 * mph;
```

**Example**

```js
mphToKmph(10); // 16.09344000614692
mphToKmph(85.9); // 138.24264965280207
```



##### 19|pipeLog

*记录一个值并返回它。 使用console.log记录提供的值，并结合||操作员返回它。*

```js
const pipeLog = data => console.log(data) || data;
```

*Example*

```js
pipeLog(1); // logs `1` and returns `1`
```



##### 20|quickSort

***QuickSort a Array（默认为升序排序）**。 使用递归。使用**Array.prototype.filter**和**spread**运算符**（...）**创建一个数组，其值小于**pivot**的所有元素都在pivot之前，并且值大于**pivot**的所有元素都在它之后。如果参数**desc**为**truthy**，则返回数组按降序排序。*

```js
const quickSort = ([n, ...nums], desc) =>
  isNaN(n)
    ? []
    : [
        ...quickSort(nums.filter(v => (desc ? v > n : v <= n)), desc),
        n,
        ...quickSort(nums.filter(v => (!desc ? v > n : v <= n)), desc)
      ];
```

**Example**

```js
quickSort([4, 1, 3, 2]); // [1,2,3,4]
quickSort([4, 1, 3, 2], true); // [4,3,2,1]
```



##### 21|removeVowels

*返回由repl替换的str中的所有元音。 使用带有正则表达式的**String.prototype.replace（）**来替换str中的所有元音。 Omot repl使用默认值''。*

```js
const removeVowels = (str, repl = '') => str.replace(/[aeiou]/gi, repl);
```

**Example**

```js
removeVowels("foobAr"); // "fbr"
removeVowels("foobAr","*"); // "f**b*r"
```



##### 22|solveRPN

*以反向抛光表示法求解给定的数学表达式。如果存在无法识别的符号或表达式错误，则会引发相应的错误。有效的运算符是： **- +， - ，，/，^，***（^＆是指数符号并且是相同的）。此代码段不支持任何一元运算符。 使用字典，**OPERATORS**指定每个运算符的匹配数学运算。使用带有正则表达式的**String.prototype.replace（）**将^替换为^，**String.prototype.split（）**以标记字符串，使用**Array.prototype.filter（）**来删除空标记。使用**Array.prototype.forEach（）**来解析每个符号，将其计算为数值或运算符并求解数学表达式。数值转换为浮点数并推送到堆栈，而运算符使用**OPERATORS**字典和堆栈中的**pop**元素进行评估以应用操作。*

```js
const solveRPN = rpn => {
  const OPERATORS = {
    '*': (a, b) => a * b,
    '+': (a, b) => a + b,
    '-': (a, b) => a - b,
    '/': (a, b) => a / b,
    '**': (a, b) => a ** b
  };
  const [stack, solve] = [
    [],
    rpn
      .replace(/\^/g, '**')
      .split(/\s+/g)
      .filter(el => !/\s+/.test(el) && el !== '')
  ];
  solve.forEach(symbol => {
    if (!isNaN(parseFloat(symbol)) && isFinite(symbol)) {
      stack.push(symbol);
    } else if (Object.keys(OPERATORS).includes(symbol)) {
      const [a, b] = [stack.pop(), stack.pop()];
      stack.push(OPERATORS[symbol](parseFloat(b), parseFloat(a)));
    } else {
      throw `${symbol} is not a recognized symbol`;
    }
  });
  if (stack.length === 1) return stack.pop();
  else throw `${rpn} is not a proper RPN. Please check it and try again`;
};
```

**Example**

```js
solveRPN('15 7 1 1 + - / 3 * 2 1 1 + + -'); // 5
solveRPN('2 3 ^'); // 8
```



##### 23|speechSynthesis

*执行语音合成（实验）。 使用**SpeechSynthesisUtterance.voice**和**window.speechSynthesis.getVoices（）**将消息转换为语音。使用**window.speechSynthesis.speak（）**播放消息。 了解有关**Web Speech API的SpeechSynthesisUtterance**接口的更多信息。*

```js
const speechSynthesis = message => {
  const msg = new SpeechSynthesisUtterance(message);
  msg.voice = window.speechSynthesis.getVoices()[0];
  window.speechSynthesis.speak(msg);
};
```

**Example**

```js
speechSynthesis('Hello, World'); // // plays the message
```



##### 24|squareSum

*将数字中的每个数字平方，然后将结果相加。 将**Array.prototype.reduce（）**与**Math.pow（）**结合使用，迭代数字并将它们的方块加到累加器中。*

```js
const squareSum = (...args) => args.reduce((squareSum, number) => squareSum + Math.pow(number, 2), 0);
```

**Example**

```js
squareSum(1, 2, 2); // 9
```