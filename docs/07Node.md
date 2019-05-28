#### Node

##### 01|atob

*解码使用base-64编码编码的数据字符串。 使用base-64编码为给定字符串创建缓冲区，并使用**Buffer.toString（'binary'）**返回已解码的字符串。*

```js
const atob = str => Buffer.from(str, 'base64').toString('binary');
```

**Example**

```js
atob('Zm9vYmFy'); // 'foobar'
```



##### 02|btoa

*从String对象创建base-64编码的ASCII字符串，其中字符串中的每个字符都被视为二进制数据的字节。* 

*使用二进制编码为给定字符串创建缓冲区，并使用Buffer.toString（'base64'）返回编码字符串。*

```js
const btoa = str => Buffer.from(str, 'binary').toString('base64');
```

**Example**

```js
btoa('foobar'); // 'Zm9vYmFy'
```



##### 03|colorrize

*在文本中添加特殊字符以在控制台中以彩色打印（与console.log（）结合使用）。 使用模板文字和特殊字符将适当的颜色代码添加到字符串输出中。对于背景颜色，添加一个特殊字符，用于重置字符串末尾的背景颜色。*

```js
const colorize = (...args) => ({
  black: `\x1b[30m${args.join(' ')}`,
  red: `\x1b[31m${args.join(' ')}`,
  green: `\x1b[32m${args.join(' ')}`,
  yellow: `\x1b[33m${args.join(' ')}`,
  blue: `\x1b[34m${args.join(' ')}`,
  magenta: `\x1b[35m${args.join(' ')}`,
  cyan: `\x1b[36m${args.join(' ')}`,
  white: `\x1b[37m${args.join(' ')}`,
  bgBlack: `\x1b[40m${args.join(' ')}\x1b[0m`,
  bgRed: `\x1b[41m${args.join(' ')}\x1b[0m`,
  bgGreen: `\x1b[42m${args.join(' ')}\x1b[0m`,
  bgYellow: `\x1b[43m${args.join(' ')}\x1b[0m`,
  bgBlue: `\x1b[44m${args.join(' ')}\x1b[0m`,
  bgMagenta: `\x1b[45m${args.join(' ')}\x1b[0m`,
  bgCyan: `\x1b[46m${args.join(' ')}\x1b[0m`,
  bgWhite: `\x1b[47m${args.join(' ')}\x1b[0m`
});
```

**Example**

```js
console.log(colorize('foo').red); // 'foo' (red letters)
console.log(colorize('foo', 'bar').bgBlue); // 'foo bar' (blue background)
console.log(colorize(colorize('foo').yellow, colorize('foo').green).bgWhite); // 'foo bar' (first word in yellow letters, second word in green letters, white background for both)
```



##### 04|createDirIfNotExists

*创建目录（如果该目录不存在）。 使用**fs.existsSync（）**检查目录是否存在，**fs.mkdirSync（）**来创建它。*

```js
const fs = require('fs');
const createDirIfNotExists = dir => (!fs.existsSync(dir) ? fs.mkdirSync(dir) : undefined);
```

**Example**



```js
createDirIfNotExists('test'); // creates the directory 'test', if it doesn't exist
```



##### 05|hasFlags

*检查当前进程的参数是否包含指定的标志。 使用**Array.prototype.every（）**和**Array.prototype.includes（）**检查**process.argv**是否包含所有指定的标志。使用正则表达式来测试指定的标志是否以 - 或 - 作为前缀，并相应地加上前缀。*

```js
const hasFlags = (...flags) =>
  flags.every(flag => process.argv.includes(/^-{1,2}/.test(flag) ? flag : '--' + flag));
```

**EXAMPLES**



```js
// node myScript.js -s --test --cool=true
hasFlags('-s'); // true
hasFlags('--test', 'cool=true', '-s'); // true
hasFlags('special'); // false
```



##### 06|hasNode

*使用SHA-256算法为值创建哈希。返回一个promise。 使用crypto API为给定值创建散列，使用**setTimeout**防止对长操作进行阻塞，使用**Promise**为其提供熟悉的接口。*

```js
const crypto = require('crypto');
const hashNode = val =>
  new Promise(resolve =>
    setTimeout(
      () =>
        resolve(
          crypto
            .createHash('sha256')
            .update(val)
            .digest('hex')
        ),
      0
    )
  );
```

**Example**

```js
hashNode(JSON.stringify({ a: 'a', b: [1, 2, 3, 4], foo: { c: 'bar' } })).then(console.log); // '04aa106279f5977f59f9067fa9712afc4aedc6f5862a8defc34552d8c7206393'
```



##### 07|isDuplexStream

*检查给定参数是否为双工（可读写）流。 检查值是否与null不同，使用typeof检查值是否为object类型，并且pipe属性是**function**类型。另外，检查**read，write**和**readableState**，**writableState**属性的类型是否分别是函数和对象。*

```js
const isDuplexStream = val =>
  val !== null &&
  typeof val === 'object' &&
  typeof val.pipe === 'function' &&
  typeof val._read === 'function' &&
  typeof val._readableState === 'object' &&
  typeof val._write === 'function' &&
  typeof val._writableState === 'object';
```

**Example**



```js
const Stream = require('stream');
isDuplexStream(new Stream.Duplex()); // true
```



##### 08|isReadableStream

*检查给定参数是否为可读流。 检查值是否与null不同，使用typeof检查值是否为object类型，并且pipe属性是**function**类型。另外，检查**read和readableState**属性的类型是否分别是**function和object**。*

```js
const isReadableStream = val =>
  val !== null &&
  typeof val === 'object' &&
  typeof val.pipe === 'function' &&
  typeof val._read === 'function' &&
  typeof val._readableState === 'object';
```

**Example**



```js
const fs = require('fs');
isReadableStream(fs.createReadStream('test.txt')); // true
```



##### 09|isStream

*检查给定的参数是否是流。 检查值是否与**null**不同，使用typeof检查值是否为**object**类型，并且**pipe**属性是**function**类型。*

```js
const isStream = val => val !== null && typeof val === 'object' && typeof val.pipe === 'function';
```

**Example**



```js
const fs = require('fs');
isStream(fs.createReadStream('test.txt')); // true
```



##### 10|isTravisCI

*检查当前环境是否为Travis CI。*

*检查当前环境是否具有TRAVIS和CI环境变量（参考）。*

```js
const isTravisCI = () => 'TRAVIS' in process.env && 'CI' in process.env;
```

**Example**



```js
isTravisCI(); // true (if code is running on Travis CI)
```



##### 11|isWritableStream

*检查给定的参数是否是可写流。*

*检查值是否与null不同，使用typeof检查值是否为object类型，并且pipe属性是function类型。另外，检查write和writableState属性的类型是否分别是函数和对象。*

```js
const isWritableStream = val =>
  val !== null &&
  typeof val === 'object' &&
  typeof val.pipe === 'function' &&
  typeof val._write === 'function' &&
  typeof val._writableState === 'object';
```

**Example**

```js
const fs = require('fs');
isWritableStream(fs.createWriteStream('test.txt')); // true
```



##### 12|JSONToFile

*将JSON对象写入文件。* 

*使用**fs.writeFile（）**，模板文字和**JSON.stringify（）**将json对象写入.json文件。*

```js
const fs = require('fs');
const JSONToFile = (obj, filename) =>
  fs.writeFile(`${filename}.json`, JSON.stringify(obj, null, 2));
```

**Example**

```js
JSONToFile({ test: 'is passed' }, 'testJsonFile'); // writes the object to 'testJsonFile.json'
```



##### 13|readFileLines

*返回指定文件中的行数组。 在fs **node**包中使用**readFileSync**函数从文件创建Buffer。使用**toString**（编码）函数将缓冲区转换为字符串。通过逐行拆分文件内容（每个\ n）从文件内容创建一个数组。*

```js
const fs = require('fs');
const readFileLines = filename =>
  fs
    .readFileSync(filename)
    .toString('UTF8')
    .split('\n');
```

**Example**

```js
/*
contents of test.txt :
  line1
  line2
  line3
  ___________________________
*/
let arr = readFileLines('test.txt');
console.log(arr); // ['line1', 'line2', 'line3']
```



##### 14|untildify

*将波形路径转换为绝对路径。 将**String.prototype.replace（）**与正则表达式和**OS.homedir（）**一起使用，用主目录替换路径开头的〜。*

```js
const untildify = str => str.replace(/^~($|\/|\\)/, `${require('os').homedir()}$1`);
```

**Example**

```js
untildify('~/node'); // '/Users/aUser/node'
```



##### 15|UUIDGeneratorNode

*在Node.JS中生成UUID。 使用crypto API生成符合RFC4122版本4的UUID。*

```js
const crypto = require('crypto');
const UUIDGeneratorNode = () =>
  ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, c =>
    (c ^ (crypto.randomBytes(1)[0] & (15 >> (c / 4)))).toString(16)
  );
```

**Example**

```js
UUIDGeneratorNode(); // '79c7c136-60ee-40a2-beb2-856f1feabefc'
```

