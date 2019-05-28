#### Math

#### 01|approximatelyEqual

*检查两个数字是否大致相等。 使用**Math.abs（）**将两个值的绝对差值与epsilon进行比较。省略第三个参数epsilon，使用默认值0.001。*

```js
const approximatelyEqual = (v1, v2, epsilon = 0.001) => Math.abs(v1 - v2) < epsilon;
```

**Example**

```js
approximatelyEqual(Math.PI / 2.0, 1.5708); // true
```



##### 02|average

*返回两个或更多数字的平均值。*

*使用**Array.prototype.reduce（）**将每个值添加到累加器，使用值0初始化，除以数组的长度。*

```js
const average = (...nums) => nums.reduce((acc, val) => acc + val, 0) / nums.length;
```

**Example**

```js
average(...[1, 2, 3]); // 2
average(1, 2, 3); // 2
```



##### 03|averageBy

*使用提供的函数将每个元素映射到值后，返回数组的平均值。 使用**Array.prototype.map（）**将每个元素映射到fn，**Array.prototype.reduce（）**返回的值，以将每个值添加到累加器，使用值0初始化，除以数组的长度。*

```js
const averageBy = (arr, fn) =>
  arr.map(typeof fn === 'function' ? fn : val => val[fn]).reduce((acc, val) => acc + val, 0) /
  arr.length;
```

**Example**

```js
averageBy([{ n: 4 }, { n: 2 }, { n: 8 }, { n: 6 }], o => o.n); // 5
averageBy([{ n: 4 }, { n: 2 }, { n: 8 }, { n: 6 }], 'n'); // 5
```



##### 04|binomialCoefficient

*计算两个整数n和k的二项式系数。 使用Number.isNaN（）检查两个值中的任何一个是否为NaN。检查k是否小于0，大于或等于n，等于1或n - 1并返回适当的结果。检查n - k是否小于k并相应地切换它们的值。从2到k循环并计算二项式系数。使用Math.round（）来计算计算中的舍入误差。*

```js
const binomialCoefficient = (n, k) => {
  if (Number.isNaN(n) || Number.isNaN(k)) return NaN;
  if (k < 0 || k > n) return 0;
  if (k === 0 || k === n) return 1;
  if (k === 1 || k === n - 1) return n;
  if (n - k < k) k = n - k;
  let res = n;
  for (let j = 2; j <= k; j++) res *= (n - j + 1) / j;
  return Math.round(res);
};
```

**Example**

```js
binomialCoefficient(8, 2); // 28
```



##### 05|clampNumber

*在边界值a和b指定的包含范围内钳制num。 如果num落在该范围内，则返回num。否则，返回范围内的最近数字。*

```js
const clampNumber = (num, a, b) => Math.max(Math.min(num, Math.max(a, b)), Math.min(a, b));
```

**Example**

```js
clampNumber(2, 3, 5); // 3
clampNumber(1, -1, -5); // -1
```



##### 06|degreesToRads

*将角度从度数转换为弧度。 使用Math.PI和弧度公式将角度从度数转换为弧度*

```js
const degreesToRads = deg => (deg * Math.PI) / 180.0;
```

**Example**

```js
degreesToRads(90.0); // ~1.5708
```



##### 07|digitize

*将数字转换为数字数组。 将数字转换为字符串，使用扩展运算符（...）构建数组。使用**Array.prototype.map（）**和**parseInt（）**将每个值转换为整数。*

```js
const digitize = n => [...`${n}`].map(i => parseInt(i));
```

**Example**

```js
digitize(123); // [1, 2, 3]
```



##### 08|distance

返回两点之间的距离。 使用Math.hypot（）计算两点之间的欧几里德距离。

```js
const distance = (x0, y0, x1, y1) => Math.hypot(x1 - x0, y1 - y0);
```

**Example**

```js
distance(1, 1, 2, 3); // 2.23606797749979
```



##### 09|elo

*使用Elo评级系统计算两个或更多对手之间的新评级。它需要一系列预先评级并返回包含后评级的数组。阵列应该从最佳表演者到最差表演者（胜利者 - >失败者）订购。 使用指数**运算符和数学运算符来计算预期得分（获胜的机会）。每个对手并计算每个对手的新评级。循环评级，使用每个排列以成对方式计算每个玩家的后Elo评级。省略第二个参数以使用32的默认kFactor。*

```js
const elo = ([...ratings], kFactor = 32, selfRating) => {
  const [a, b] = ratings;
  const expectedScore = (self, opponent) => 1 / (1 + 10 ** ((opponent - self) / 400));
  const newRating = (rating, i) =>
    (selfRating || rating) + kFactor * (i - expectedScore(i ? a : b, i ? b : a));
  if (ratings.length === 2) return [newRating(a, 1), newRating(b, 0)];

  for (let i = 0, len = ratings.length; i < len; i++) {
    let j = i;
    while (j < len - 1) {
      j++;
      [ratings[i], ratings[j]] = elo([ratings[i], ratings[j]], kFactor);
    }
  }
  return ratings;
};
```

**Example**

```js
// Standard 1v1s
elo([1200, 1200]); // [1216, 1184]
elo([1200, 1200], 64); // [1232, 1168]
// 4 player FFA, all same rank
elo([1200, 1200, 1200, 1200]).map(Math.round); // [1246, 1215, 1185, 1154]
/*
For teams, each rating can adjusted based on own team's average rating vs.
average rating of opposing team, with the score being added to their
own individual rating by supplying it as the third argument.
*/
```



##### 10|factorial

*计算数字的阶乘。 使用递归。如果n小于或等于1，则返回1.否则，返回n的乘积和n - 1的阶乘。如果n是负数，则抛出异常。*

```js
const factorial = n =>
  n < 0
    ? (() => {
      throw new TypeError('Negative numbers are not allowed!');
    })()
    : n <= 1
      ? 1
      : n * factorial(n - 1);
```

**Example**

```js
factorial(6); // 720
```



##### 11|fibonacci

*生成一个包含Fibonacci序列的数组，直到第n个项。 创建一个特定长度的空数组，初始化前两个值（0和1）。使用**Array.prototype.reduce（）**将值添加到数组中，使用最后两个值的总和，前两个值除外。*

```js
const fibonacci = n =>
  Array.from({ length: n }).reduce(
    (acc, val, i) => acc.concat(i > 1 ? acc[i - 1] + acc[i - 2] : i),
    []
  );
```

**Example**

```js
fibonacci(6); // [0, 1, 1, 2, 3, 5]
```



##### 12|gcd

*计算两个或多个数字/数组之间的最大公约数。 内部_gcd函数使用递归。基本情况是当y等于0.在这种情况下，返回x。否则，返回y的GCD和除法x / y的余数。*

```js
const gcd = (...arr) => {
  const _gcd = (x, y) => (!y ? x : gcd(y, x % y));
  return [...arr].reduce((a, b) => _gcd(a, b));
};
```

**Example**

```js
gcd(8, 36); // 4
gcd(...[12, 8, 32]); // 4
```



##### 13|geometricProgression

*初始化一个数组，其中包含指定范围内的数字，其中start和end包含两者，两个术语之间的比例为step。如果step等于1，则返回错误。 使用Array.from（），Math.log（）和Math.floor（）创建所需长度的数组Array.prototype.map（）以填充范围中的所需值。省略第二个参数start，使用默认值1.省略第三个参数step，使用默认值2。*

```js
const geometricProgression = (end, start = 1, step = 2) =>
  Array.from({ length: Math.floor(Math.log(end / start) / Math.log(step)) + 1 }).map(
    (v, i) => start * step ** i
  );
```

**Example**

```js
geometricProgression(256); // [1, 2, 4, 8, 16, 32, 64, 128, 256]
geometricProgression(256, 3); // [3, 6, 12, 24, 48, 96, 192]
geometricProgression(256, 1, 4); // [1, 4, 16, 64, 256]
```



##### 14|hammingDistance

*计算两个值之间的汉明距离。 使用XOR运算符（^）查找两个数字之间的位差，使用toString（2）转换为二进制字符串。使用match（/ 1 / g）计算并返回字符串中的1的数量。*

```js
const hammingDistance = (num1, num2) => ((num1 ^ num2).toString(2).match(/1/g) || '').length;
```

**Example**

```js
hammingDistance(2, 3); // 1
```



##### 15|inRange

*检查给定数字是否在给定范围内。 使用算术比较来检查给定数字是否在指定范围内。如果未指定第二个参数end，则认为范围是从0开始。*

```js
const inRange = (n, start, end = null) => {
  if (end && start > end) [end, start] = [start, end];
  return end == null ? n >= 0 && n < start : n >= start && n < end;
};
```

**Example**

```js
inRange(3, 2, 5); // true
inRange(3, 4); // true
inRange(2, 3, 5); // false
inRange(3, 2); // false
```



##### 16|isDivisible

*检查第一个数字参数是否可被第二个数字整除。 使用模运算符（％）检查余数是否等于0。*

```js
const isDivisible = (dividend, divisor) => dividend % divisor === 0;
```

**Example**

```js
isDivisible(6, 3); // true
```



##### 17|isEven

*如果给定的数字是偶数，则返回true，否则返回false。 检查数字是奇数还是使用模数（％）运算符。如果数字是偶数则返回true，如果数字是奇数则返回false。*

```js
const isEven = num => num % 2 === 0;
```

**Example**

```js
isEven(3); // false
```



##### 18|isNegativeZero

*检查给定值是否等于负零（-0）。 检查传递的值是否等于0，如果1除以值等于-Infinity*

```js
const isNegativeZero = val => val === 0 && 1 / val === -Infinity;
```

**Example**

```js
isNegativeZero(-0); // true
isNegativeZero(0); // false
```



##### 19|isPrime

*检查提供的整数是否为素数。 检查从2到给定数字的平方根的数字。如果其中任何一个除以给定的数字，则返回false，否则返回true，除非该数字小于2。*

```js
const isPrime = num => {
  const boundary = Math.floor(Math.sqrt(num));
  for (var i = 2; i <= boundary; i++) if (num % i === 0) return false;
  return num >= 2;
};
```

**Example**

```js
isPrime(11); // true
```



##### 20|lcm

*返回两个或多个数字的最小公倍数。* 

*使用最大公约数（GCD）公式和lcm（x，y）= x * y / gcd（x，y）来确定最小公倍数。 GCD公式使用递归*

```js
const lcm = (...arr) => {
  const gcd = (x, y) => (!y ? x : gcd(y, x % y));
  const _lcm = (x, y) => (x * y) / gcd(x, y);
  return [...arr].reduce((a, b) => _lcm(a, b));
};
```

**Example**

```js
lcm(12, 7); // 84
lcm(...[1, 3, 4, 5]); // 60
```



##### 21|luhnCheck

*用于验证各种识别号码的Luhn算法的实现，例如信用卡号，IMEI号，国家提供商标识号等。 将String.prototype.split（''），Array.prototype.reverse（）和Array.prototype.map（）与parseInt（）结合使用以获取数字数组。使用Array.prototype.splice（0,1）获取最后一位数。使用Array.prototype.reduce（）来实现Luhn算法。如果sum可以被10整除，则返回true，否则返回false。*

```js
const luhnCheck = num => {
  let arr = (num + '')
    .split('')
    .reverse()
    .map(x => parseInt(x));
  let lastDigit = arr.splice(0, 1)[0];
  let sum = arr.reduce((acc, val, i) => (i % 2 !== 0 ? acc + val : acc + ((val * 2) % 9) || 9), 0);
  sum += lastDigit;
  return sum % 10 === 0;
};
```

**Example**

```js
luhnCheck('4485275742308327'); // true
luhnCheck(6011329933655299); //  false
luhnCheck(123456789); // false
```



##### 22|mapNumRange

*将数字从一个范围映射到另一个范围。 从inMin-inMax返回outMin-outMax之间的num映射。*

```js
const mapNumRange = (num, inMin, inMax, outMin, outMax) =>
  ((num - inMin) * (outMax - outMin)) / (inMax - inMin) + outMin;
```

**Example**

```js
mapNumRange(5, 0, 10, 0, 100); // 50
```



##### 23|maxBy

*在使用提供的函数将每个元素映射到值之后，返回数组的最大值。 使用**Array.prototype.map（）**将每个元素映射到**fn，Math.max（）**返回的值以获取最大值。*

```js
const maxBy = (arr, fn) => Math.max(...arr.map(typeof fn === 'function' ? fn : val => val[fn]));
```

**Example**

```js
maxBy([{ n: 4 }, { n: 2 }, { n: 8 }, { n: 6 }], o => o.n); // 8
maxBy([{ n: 4 }, { n: 2 }, { n: 8 }, { n: 6 }], 'n'); // 8
```



##### 24|median

*返回数组数组的中位数。 找到数组的中间部分，使用Array.prototype.sort（）对值进行排序。如果长度为奇数，则返回中点处的数字，否则返回两个中间数字的平均值。*

```js
const median = arr => {
  const mid = Math.floor(arr.length / 2),
    nums = [...arr].sort((a, b) => a - b);
  return arr.length % 2 !== 0 ? nums[mid] : (nums[mid - 1] + nums[mid]) / 2;
};
```

**Example**

```js
median([5, 6, 50, 1, -5]); // 5
```



##### 26|midpoint

*计算两对（x，y）点之间的中点。 构造数组以获得x1，y1，x2和y2，通过将两个端点的总和除以2来计算每个维度的中点。*

```js
const midpoint = ([x1, y1], [x2, y2]) => [(x1 + x2) / 2, (y1 + y2) / 2];
```

**Example**

```js
midpoint([2, 2], [4, 4]); // [3, 3]
midpoint([4, 4], [6, 6]); // [5, 5]
midpoint([1, 3], [2, 4]); // [1.5, 3.5]
```



##### 27|minBy

*使用提供的函数将每个元素映射到值后，返回数组的最小值。 使用**Array.prototype.map（）**将每个元素映射到**fn，Math.min（）**返回的值以获取最小值。*

```js
const minBy = (arr, fn) => Math.min(...arr.map(typeof fn === 'function' ? fn : val => val[fn]));
```

**Example**

```js
minBy([{ n: 4 }, { n: 2 }, { n: 8 }, { n: 6 }], o => o.n); // 2
minBy([{ n: 4 }, { n: 2 }, { n: 8 }, { n: 6 }], 'n'); // 2
```



##### 28|percentile

*使用百分位公式计算给定数组中的数量是否小于或等于给定值。 使用**Array.prototype.reduce（）**计算低于该值的数量以及有多少是相同的值并应用百分位公式。*

```js
const percentile = (arr, val) =>
  (100 * arr.reduce((acc, v) => acc + (v < val ? 1 : 0) + (v === val ? 0.5 : 0), 0)) / arr.length;
```

**Example**

```js
percentile([1, 2, 3, 4, 5, 6, 7, 8, 9, 10], 6); // 55
```



##### 29|powerset

*返回给定数字数组的powerset。 使用**Array.prototype.reduce（）**结合**Array.prototype.map（）**迭代元素并组合成包含所有组合的数组。*

```js
const powerset = arr => arr.reduce((a, v) => a.concat(a.map(r => [v].concat(r))), [[]]);
```

**Example**

```js
powerset([1, 2]); // [[], [1], [2], [2, 1]]
```



##### 30|primes

使用**Eratosthenes的Sieve**生成达到给定数量的素数。 生成从2到给定数字的数组。使用**Array.prototype.filter（）**过滤掉可从2到所提供数字的平方根的任何数字整除的值。

```js
const primes = num => {
  let arr = Array.from({ length: num - 1 }).map((x, i) => i + 2),
    sqroot = Math.floor(Math.sqrt(num)),
    numsTillSqroot = Array.from({ length: sqroot - 1 }).map((x, i) => i + 2);
  numsTillSqroot.forEach(x => (arr = arr.filter(y => y % x !== 0 || y === x)));
  return arr;
};
```

**Example**

```js
primes(10); // [2,3,5,7]
```



##### 31|radsToDegrees

*将角度从弧度转换为度数。 使用Math.PI和弧度度数公式将角度从弧度转换为度数。*

```js
const radsToDegrees = rad => (rad * 180.0) / Math.PI;
```

**Example**

```js
radsToDegrees(Math.PI / 2); // 90
```



##### 32|randomIntArrayInRange

*返回指定范围内的n个随机整数的数组。 使用**Array.from（）**创建一个特定长度的数组，**Math.random（）**生成一个随机数并将其映射到所需的范围，使用**Math.floor（）**使其成为整数。*

```js
const randomIntArrayInRange = (min, max, n = 1) =>
  Array.from({ length: n }, () => Math.floor(Math.random() * (max - min + 1)) + min);
```

**Example**

```js
randomIntArrayInRange(12, 35, 10); // [ 34, 14, 27, 17, 30, 27, 20, 26, 21, 14 ]
```



##### 33|randomIntegerInRange

*返回指定范围内的随机整数。 使用**Math.random（）**生成随机数并将其映射到所需范围，使用**Math.floor（）**使其成为整数。*

```js
const randomIntegerInRange = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;
```

**Example**

```js
randomIntegerInRange(0, 5); // 2
```



##### 34|randomNumberInRange

*返回指定范围内的随机数。 使用**Math.random（）**生成随机值，使用乘法将其映射到所需范围。*

```js
const randomNumberInRange = (min, max) => Math.random() * (max - min) + min;
```

**Example**

```js
randomNumberInRange(2, 10); // 6.0211363285087005
```



##### 35|round

将数字舍入到指定的数字位数。 使用**Math.round（）**和模板文字将数字四舍五入到指定的位数。省略第二个参数，小数以舍入为整数。

```js
const round = (n, decimals = 0) => Number(`${Math.round(`${n}e${decimals}`)}e-${decimals}`);
```

**Example**

```js
round(1.005, 2); // 1.01
```



##### 36|sdbm

*将输入字符串哈希为整数。 使用**String.prototype.split（''）**和**Array.prototype.reduce（）**来创建输入字符串的哈希，利用位移。*

```js
const sdbm = str => {
  let arr = str.split('');
  return arr.reduce(
    (hashCode, currentVal) =>
      (hashCode = currentVal.charCodeAt(0) + (hashCode << 6) + (hashCode << 16) - hashCode),
    0
  );
};
```

**Example**

```js
sdbm('name'); // -3521204949
```



##### 37|standardDeviation

*返回数字数组的标准偏差。 使用**Array.prototype.reduce（）**计算值的均值，方差和方差之和，值的方差，然后确定标准偏差。您可以省略第二个参数以获取样本标准偏差或将其设置为true以获得总体标准偏差。*

```js
const standardDeviation = (arr, usePopulation = false) => {
  const mean = arr.reduce((acc, val) => acc + val, 0) / arr.length;
  return Math.sqrt(
    arr.reduce((acc, val) => acc.concat((val - mean) ** 2), []).reduce((acc, val) => acc + val, 0) /
      (arr.length - (usePopulation ? 0 : 1))
  );
};
```

**Example**

```js
standardDeviation([10, 2, 38, 23, 38, 23, 21]); // 13.284434142114991 (sample)
standardDeviation([10, 2, 38, 23, 38, 23, 21], true); // 12.29899614287479 (population)
```



##### 38|sum

*返回两个或更多数字/数组的总和。 使用**Array.prototype.reduce（）**将每个值添加到累加器，使用值0初始化。*

```js
const sum = (...arr) => [...arr].reduce((acc, val) => acc + val, 0);
```

**Example**

```js
sum(1, 2, 3, 4); // 10
sum(...[1, 2, 3, 4]); // 10
```



##### 39|sumby

在使用提供的函数将每个元素映射到值之后，返回数组的总和。 使用**Array.prototype.map（）**将每个元素映射到fn，**Array.prototype.reduce（）**返回的值，以将每个值添加到累加器，初始化为值0。

```js
const sumBy = (arr, fn) =>
  arr.map(typeof fn === 'function' ? fn : val => val[fn]).reduce((acc, val) => acc + val, 0);
```

**Example**

```js
sumBy([{ n: 4 }, { n: 2 }, { n: 8 }, { n: 6 }], o => o.n); // 20
sumBy([{ n: 4 }, { n: 2 }, { n: 8 }, { n: 6 }], 'n'); // 20
```



##### 40|sumPower

*返回从开始到结束（包括两者）的所有数字的幂的总和。 使用**Array.prototype.fill（）**创建一个包含目标范围中所有数字的数组，**Array.prototype.map（）**和指数运算符（**）以将它们提升到幂并将**Array.prototype.reduce（）**提升到将它们加在一起。省略第二个参数power，使用默认的2次幂。省略第三个参数start，使用默认的起始值1。*

```js
const sumPower = (end, power = 2, start = 1) =>
  Array(end + 1 - start)
    .fill(0)
    .map((x, i) => (i + start) ** power)
    .reduce((a, b) => a + b, 0);
```

**Example**

```js
sumPower(10); // 385
sumPower(10, 3); // 3025
sumPower(10, 3, 5); // 2925
```



##### 41|toSafeInteger

*将值转换为安全整数。 使用Math.max（）和Math.min（）查找最接近的安全值。使用Math.round（）转换为整数。*

```js
const toSafeInteger = num =>
  Math.round(Math.max(Math.min(num, Number.MAX_SAFE_INTEGER), Number.MIN_SAFE_INTEGER));
```

**Example**

```js
toSafeInteger('3.2'); // 3
toSafeInteger(Infinity); // 9007199254740991
```



##### 42|vectorDistance

*返回两个向量之间的距离。 使用**Array.prototype.reduce（）**，**Math.pow（）**和**Math.sqrt（）**计算两个向量之间的欧几里德距离。*

```js
const vectorDistance = (...coords) => {
  let pointLength = Math.trunc(coords.length / 2);
  let sum = coords
    .slice(0, pointLength)
    .reduce((acc, val, i) => acc + Math.pow(val - coords[pointLength + i], 2), 0);
  return Math.sqrt(sum);
};
```

**Example**

```js
vectorDistance(10, 0, 5, 20, 0, 10); // 11.180339887498949
```

