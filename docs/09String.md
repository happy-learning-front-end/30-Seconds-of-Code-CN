#### String

##### 01|byteSize

*以字节为单位返回字符串的长度。 将给定字符串转换为Blob对象并查找其大小。*

```js
const byteSize = str => new Blob([str]).size;
```

**Example**

```js
byteSize('😀'); // 4
byteSize('Hello World'); // 11
```



##### 02|capitalize

*大写字符串的第一个字母。 使用数组解构和**String.prototype.toUpperCase（）**来大写第一个字母，**... rest**来获取第一个字母后的字符数组，然后使用**Array.prototype.join（''）**再次使它成为一个字符串。省略**lowerRest**参数以保持字符串的其余部分不变，或将其设置为true以转换为小写。*

```js
const capitalize = ([first, ...rest], lowerRest = false) =>
  first.toUpperCase() + (lowerRest ? rest.join('').toLowerCase() : rest.join(''));
```

**Example**

```js
capitalize('fooBar'); // 'FooBar'
capitalize('fooBar', true); // 'Foobar'
```



##### 03|capitalizeEveryWord

*将字符串中每个单词的首字母大写。 使用**String.prototype.replace（）**匹配每个单词的第一个字符和**String.prototype.toUpperCase（）**来大写它。*

```js
const capitalizeEveryWord = str => str.replace(/\b[a-z]/g, char => char.toUpperCase());
```

**Example**

```js
capitalizeEveryWord('hello world!'); // 'Hello World!'
```



##### 04|compactWhitespace

*返回压缩空格的字符串。 将**String.prototype.replace（）**与正则表达式一起使用，以使用单个空格替换所有出现的2个或更多空格字符。*

```js
const compactWhitespace = str => str.replace(/\s{2,}/g, ' ');
```

**Example**

```js
compactWhitespace('Lorem    Ipsum'); // 'Lorem Ipsum'
compactWhitespace('Lorem \n Ipsum'); // 'Lorem Ipsum'
```



##### 05|CSVToArray

*将逗号分隔值（CSV）字符串转换为2D数组。 如果**omitFirstRow**为**true**，则使用**Array.prototype.slice（）**和**Array.prototype.indexOf（'\ n'）**删除第一行（标题行）。使用**String.prototype.split（'\ n'）**为每一行创建一个字符串，然后使用**String.prototype.split（delimiter）**来分隔每一行中的值。省略第二个参数**delimiter**，使用的默认分隔符为。省略第三个参数**omitFirstRow**，以包含**CSV**字符串的第一行（标题行）。*

```js
const CSVToArray = (data, delimiter = ',', omitFirstRow = false) =>
  data
    .slice(omitFirstRow ? data.indexOf('\n') + 1 : 0)
    .split('\n')
    .map(v => v.split(delimiter));
```

**Example**

```js
CSVToArray('a,b\nc,d'); // [['a','b'],['c','d']];
CSVToArray('a;b\nc;d', ';'); // [['a','b'],['c','d']];
CSVToArray('col1,col2\na,b\nc,d', ',', true); // [['a','b'],['c','d']];
```



##### 06|CSVToJSON

*将逗号分隔值（CSV）字符串转换为2D对象数组。字符串的第一行用作标题行。 使用**Array.prototype.slice（）**和**Array.prototype.indexOf（'\ n'）**和**String.prototype.split（分隔符）**将第一行（标题行）分隔为值。使用**String.prototype.split（'\ n'）**为每一行创建一个字符串，然后使用**Array.prototype.map（）**和**String.prototype.split（delimiter）**来分隔每行中的值。使用**Array.prototype.reduce（）**为每行的值创建一个对象，并从标题行解析键。省略第二个参数**delimiter**，使用的默认分隔符为。*

```js
const CSVToJSON = (data, delimiter = ',') => {
  const titles = data.slice(0, data.indexOf('\n')).split(delimiter);
  return data
    .slice(data.indexOf('\n') + 1)
    .split('\n')
    .map(v => {
      const values = v.split(delimiter);
      return titles.reduce((obj, title, index) => ((obj[title] = values[index]), obj), {});
    });
};
```

**Example**

```js
CSVToJSON('col1,col2\na,b\nc,d'); // [{'col1': 'a', 'col2': 'b'}, {'col1': 'c', 'col2': 'd'}];
CSVToJSON('col1;col2\na;b\nc;d', ';'); // [{'col1': 'a', 'col2': 'b'}, {'col1': 'c', 'col2': 'd'}];
```



##### 07|decapitalize

*对字符串的第一个字母进行去除资本化。 使用数组解构和**String.toLowerCase（）**对第一个字母进行**decapitalize**，**... rest**以获取第一个字母后的字符数组，然后使用**Array.prototype.join（''）**使其再次成为字符串。省略**upperRest**参数以保持字符串的其余部分不变，或将其设置为**true**以转换为大写。*

```js
const decapitalize = ([first, ...rest], upperRest = false) =>
  first.toLowerCase() + (upperRest ? rest.join('').toUpperCase() : rest.join(''));
```

**Example**

```js
decapitalize('FooBar'); // 'fooBar'
decapitalize('FooBar', true); // 'fOOBAR'
```



##### 08|escapeHTML

*转义字符串以在HTML中使用。 将**String.prototype.replace（）**与regexp一起使用，该regexp与需要转义的字符匹配，使用回调函数使用字典（对象）将每个字符实例替换为其关联的转义字符。*

```js
const escapeHTML = str =>
  str.replace(
    /[&<>'"]/g,
    tag =>
      ({
        '&': '&amp;',
        '<': '&lt;',
        '>': '&gt;',
        "'": '&#39;',
        '"': '&quot;'
      }[tag] || tag)
  );
```

**Example**

```js
escapeHTML('<a href="#">Me & you</a>'); // '&lt;a href=&quot;#&quot;&gt;Me &amp; you&lt;/a&gt;'
```



##### 10|escapeRegExp

*转义要在正则表达式中使用的字符串。 使用**String.prototype.replace（）**来转义特殊字符。*

```js
const escapeRegExp = str => str.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
```

**Example**

```js
escapeRegExp('(test)'); // \\(test\\)
```



##### 11|fromCamelCase

*从**camelcase**转换字符串。 使用**String.prototype.replace（）**删除下划线，连字符和空格，并将单词转换为**camelcase**。省略第二个参数以使用_的默认分隔符。*

```js
const fromCamelCase = (str, separator = '_') =>
  str
    .replace(/([a-z\d])([A-Z])/g, '$1' + separator + '$2')
    .replace(/([A-Z]+)([A-Z][a-z\d]+)/g, '$1' + separator + '$2')
    .toLowerCase();
```

**Example**

```js
fromCamelCase('someDatabaseFieldName', ' '); // 'some database field name'
fromCamelCase('someLabelThatNeedsToBeCamelized', '-'); // 'some-label-that-needs-to-be-camelized'
fromCamelCase('someJavascriptProperty', '_'); // 'some_javascript_property'
```



##### 12|indentString

*缩进提供的字符串中的每一行。 使用**String.replace**和正则表达式在每行的开头添加由缩进计数次数指定的字符。省略第三个参数**indent**，使用''的默认缩进字符。*

```js
const indentString = (str, count, indent = ' ') => str.replace(/^/gm, indent.repeat(count));
```

**Example**

```js
indentString('Lorem\nIpsum', 2); // '  Lorem\n  Ipsum'
indentString('Lorem\nIpsum', 2, '_'); // '__Lorem\n__Ipsum'
```



##### 13|isAbsoluteURL

*如果给定的字符串是绝对URL，则返回true，否则返回false。 使用正则表达式来测试字符串是否是绝对URL。*

```js
const isAbsoluteURL = str => /^[a-z][a-z0-9+.-]*:/.test(str);
```

**Example**

```js
isAbsoluteURL('https://google.com'); // true
isAbsoluteURL('ftp://www.myserver.net'); // true
isAbsoluteURL('/foo/bar'); // false
```



##### 14|isAnagram

*检查字符串是否是另一个字符串的字谜（不区分大小写，忽略空格，标点符号和特殊字符）。 使用带有适当正则表达式的**String.toLowerCase（），String.prototype.replace（）**来删除不必要的字符，**String.prototype.split（''），Array.prototype.sort（）**和**Array.prototype.join（'' ）**在两个字符串上规范化它们，然后检查它们的规范化形式是否相等。*

```js
const isAnagram = (str1, str2) => {
  const normalize = str =>
    str
      .toLowerCase()
      .replace(/[^a-z0-9]/gi, '')
      .split('')
      .sort()
      .join('');
  return normalize(str1) === normalize(str2);
};
```

**Example**

```js
isAnagram('iceman', 'cinema'); // true
```



##### 15|isLowerCase

*检查字符串是否为小写。 使用**String.toLowerCase（）**将给定字符串转换为小写，并将其与原始字符串进行比较。*

```js
const isLowerCase = str => str === str.toLowerCase();
```

**Example**

```js
isLowerCase('abc'); // true
isLowerCase('a3@$'); // true
isLowerCase('Ab4'); // false
```



##### 16|isUpperCase

*检查字符串是否为大写。 使用**String.prototype.toUpperCase（）**将给定字符串转换为大写，并将其与原始字符串进行比较。*

```js
const isUpperCase=str=>str===str.toUpperCase();
```

**Example**

```js
isUpperCase('ABC'); // true
isLowerCase('A3@$'); // true
isLowerCase('aB4'); // false
```



##### 17|mapString

*创建一个新字符串，其结果是在调用字符串中的每个字符上调用提供的函数。 使用**String.prototype.split（''）和Array.prototype.map（）**为str中的每个字符调用提供的函数fn。使用**Array.prototype.join（''）**将字符数组重组为字符串。回调函数fn接受三个参数（当前字符，当前字符的索引和字符串**mapString**被调用）。*

```js
const mapString = (str, fn) =>
  str
    .split('')
    .map((c, i) => fn(c, i, str))
    .join('');
```

**Example**

```js
mapString('lorem ipsum', c => c.toUpperCase()); // 'LOREM IPSUM'
```



##### 18|mask

*使用指定的掩码字符替换除最后一个字符数之外的所有字符。 使用**String.prototype.slice（）**来获取将保持未屏蔽的字符部分，并使用**String.padStart（）**以掩码字符填充字符串的开头，直到原始长度。省略第二个参数**num**，以保持默认的4个字符未被屏蔽。如果**num**为负数，则未屏蔽的字符将位于字符串的开头。省略第三个参数**mask**，为掩码使用默认字符'*'。*

```js
const mask = (cc, num = 4, mask = '*') => `${cc}`.slice(-num).padStart(`${cc}`.length, mask);
```

**Example**

```js
mask(1234567890); // '******7890'
mask(1234567890, 3); // '*******890'
mask(1234567890, -4, '$'); // '$$$$567890'
```



##### 19|pad

*如果它短于指定的长度，则在指定字符的两侧填充一个字符串。 使用**String.padStart（）**和**String.padEnd（）**来填充给定字符串的两侧。省略第三个参数char，使用空格字符作为默认填充字符。*

```js
const pad = (str, length, char = ' ') =>
  str.padStart((str.length + length) / 2, char).padEnd(length, char);
```

**Example**

```js
pad('cat', 8); // '  cat   '
pad(String(42), 6, '0'); // '004200'
pad('foobar', 3); // 'foobar'
```



##### 20|palindrome

*如果给定的字符串是回文结构，则返回true，否则返回false。 将字符串转换为**String.prototype.toLowerCase（）**并使用**String.prototype.replace（）**从中删除非字母数字字符。然后，使用扩展运算符（...）将字符串拆分为单个字符**Array.prototype.reverse（），String.prototype.join（''）**并将其转换为原始的非反转字符串，将其转换为**String.prototype.toLowerCase（）。***

```js
const palindrome = str => {
  const s = str.toLowerCase().replace(/[\W_]/g, '');
  return s === [...s].reverse().join('');
};
```

**Example**

```js
palindrome('taco cat'); // true
```



##### 21||pluralize

*根据输入数字返回单词或复数形式的单词。如果第一个参数是一个对象，它将使用一个闭包，它返回一个函数，该函数可以自动复制单词，如果提供的字典包含单词，则该单词不会简单地以s结尾。 如果num为-1或1，则返回单词的单数形式。如果num是任何其他数字，则返回复数形式。省略第三个参数以使用单数词+ s的默认值，或在必要时提供自定义复数词。如果第一个参数是一个对象，则通过返回一个函数来利用一个闭包，该函数可以使用提供的字典来解析单词的正确复数形式。*

```js
const pluralize = (val, word, plural = word + 's') => {
  const _pluralize = (num, word, plural = word + 's') =>
    [1, -1].includes(Number(num)) ? word : plural;
  if (typeof val === 'object') return (num, word) => _pluralize(num, word, val[word]);
  return _pluralize(val, word, plural);
};
```

**Example**

```js
pluralize(0, 'apple'); // 'apples'
pluralize(1, 'apple'); // 'apple'
pluralize(2, 'apple'); // 'apples'
pluralize(2, 'person', 'people'); // 'people'

const PLURALS = {
  person: 'people',
  radius: 'radii'
};
const autoPluralize = pluralize(PLURALS);
autoPluralize(2, 'person'); // 'people'
```



##### 22|removeNonASCll

**删除不可打印的ASCII字符。 使用正则表达式删除不可打印的ASCII字符。**

```js
const removeNonASCII = str => str.replace(/[^\x20-\x7E]/g, '');
```

**Example**

```js
removeNonASCII('äÄçÇéÉêlorem-ipsumöÖÐþúÚ'); // 'lorem-ipsum'
```



##### 23|reverseString

*反转一个字符串。 使用**spread**运算符**（...）**和**Array.prototype.reverse（）**来反转字符串中字符的顺序。使用**String.prototype.join（''）**组合字符以获取字符串。*

```js
const reverseString = str => [...str].reverse().join('');
```

**Example**

```js
reverseString('foobar'); // 'raboof'
```



##### 24|sortCharactersInString

*按字母顺序对字符串中的字符进行排序。 使用spread运算符**（...）**，**Array.prototype.sort（）**和**String.localeCompare（）**对str中的字符进行排序，使用**String.prototype.join（''）**重新组合。*

```js
const sortCharactersInString = str => [...str].sort((a, b) => a.localeCompare(b)).join('');
```

**Example**

```js
sortCharactersInString('cabbage'); // 'aabbceg'
```



##### 25|splitLines

*将多行字符串拆分为一行数组。 使用**String.prototype.split（）**和正则表达式来匹配换行符并创建一个数组。*

```js
const splitLines = str => str.split(/\r?\n/);
```

**Example**

```js
splitLines('This\nis a\nmultiline\nstring.\n'); // ['This', 'is a', 'multiline', 'string.' , '']
```



##### 26|stringPermutations

*警告：此函数的执行时间随每个字符呈指数增长。任何超过8到10个字符的内容都会导致浏览器挂起，因为它会尝试解决所有不同的组合。 生成字符串的所有排列（包含重复项）。 使用递归。对于给定字符串中的每个字母，为其余字母创建所有部分排列。使用**Array.prototype.map（）**将字母与每个部分排列组合，然后使用**Array.prototype.reduce（）**将所有排列组合在一个数组中。基本情况是字符串长度等于2或1。*

```js
const stringPermutations = str => {
  if (str.length <= 2) return str.length === 2 ? [str, str[1] + str[0]] : [str];
  return str
    .split('')
    .reduce(
      (acc, letter, i) =>
        acc.concat(stringPermutations(str.slice(0, i) + str.slice(i + 1)).map(val => letter + val)),
      []
    );
};
```

**Example**

```js
stringPermutations('abc'); // ['abc','acb','bac','bca','cab','cba']
```



##### 27|stripHTMLTags

*从字符串中删除HTML / XML标记。 使用正则表达式从字符串中删除HTML / XML标记。*

```js
const stripHTMLTags = str => str.replace(/<[^>]*>/g, '');
```

**Example**

```js
stripHTMLTags('<p><em>lorem</em> <strong>ipsum</strong></p>'); // 'lorem ipsum'
```



##### 28|toCamelCase

*将字符串转换为**camelcase**。 将字符串分解为单词并将它们组合起来，使用正则表达式将每个单词的第一个字母大写。*

```js
const toCamelCase = str => {
  let s =
    str &&
    str
      .match(/[A-Z]{2,}(?=[A-Z][a-z]+[0-9]*|\b)|[A-Z]?[a-z]+[0-9]*|[A-Z]|[0-9]+/g)
      .map(x => x.slice(0, 1).toUpperCase() + x.slice(1).toLowerCase())
      .join('');
  return s.slice(0, 1).toLowerCase() + s.slice(1);
};
```

**Example**

```js
toCamelCase('some_database_field_name'); // 'someDatabaseFieldName'
toCamelCase('Some label that needs to be camelized'); // 'someLabelThatNeedsToBeCamelized'
toCamelCase('some-javascript-property'); // 'someJavascriptProperty'
toCamelCase('some-mixed_string with spaces_underscores-and-hyphens'); // 'someMixedStringWithSpacesUnderscoresAndHyphens'
```



##### 29|toKebabCase

*将字符串转换为烤肉串案例。 将字符串分解为单词并将它们组合添加 - 作为分隔符，使用正则表达式。*

```js
const toKebabCase = str =>
  str &&
  str
    .match(/[A-Z]{2,}(?=[A-Z][a-z]+[0-9]*|\b)|[A-Z]?[a-z]+[0-9]*|[A-Z]|[0-9]+/g)
    .map(x => x.toLowerCase())
    .join('-');
```

**Example**

```js
toKebabCase('camelCase'); // 'camel-case'
toKebabCase('some text'); // 'some-text'
toKebabCase('some-mixed_string With spaces_underscores-and-hyphens'); // 'some-mixed-string-with-spaces-underscores-and-hyphens'
toKebabCase('AllThe-small Things'); // "all-the-small-things"
toKebabCase('IAmListeningToFMWhileLoadingDifferentURLOnMyBrowserAndAlsoEditingSomeXMLAndHTML'); // "i-am-listening-to-fm-while-loading-different-url-on-my-browser-and-also-editing-xml-and-html"
```



##### 30|toSnakeCase

*将字符串转换为蛇案例。 将字符串分解为单词并将它们组合使用正则表达式将_作为分隔符添加。*

```js
const toSnakeCase = str =>
  str &&
  str
    .match(/[A-Z]{2,}(?=[A-Z][a-z]+[0-9]*|\b)|[A-Z]?[a-z]+[0-9]*|[A-Z]|[0-9]+/g)
    .map(x => x.toLowerCase())
    .join('_');
```

**Example**

```js
toSnakeCase('camelCase'); // 'camel_case'
toSnakeCase('some text'); // 'some_text'
toSnakeCase('some-mixed_string With spaces_underscores-and-hyphens'); // 'some_mixed_string_with_spaces_underscores_and_hyphens'
toSnakeCase('AllThe-small Things'); // "all_the_smal_things"
toSnakeCase('IAmListeningToFMWhileLoadingDifferentURLOnMyBrowserAndAlsoEditingSomeXMLAndHTML'); // "i_am_listening_to_fm_while_loading_different_url_on_my_browser_and_also_editing_some_xml_and_html"
```



##### 31|toTitleCase

*将字符串转换为标题大小写。 使用正则表达式将字符串分解为单词，并将它们组合成大写每个单词的第一个字母并在它们之间添加空格。*

```js
const toTitleCase = str =>
  str
    .match(/[A-Z]{2,}(?=[A-Z][a-z]+[0-9]*|\b)|[A-Z]?[a-z]+[0-9]*|[A-Z]|[0-9]+/g)
    .map(x => x.charAt(0).toUpperCase() + x.slice(1))
    .join(' ');
```

**Example**

```js
toTitleCase('some_database_field_name'); // 'Some Database Field Name'
toTitleCase('Some label that needs to be title-cased'); // 'Some Label That Needs To Be Title Cased'
toTitleCase('some-package-name'); // 'Some Package Name'
toTitleCase('some-mixed_string with spaces_underscores-and-hyphens'); // 'Some Mixed String With Spaces Underscores And Hyphens'
```



##### 32|truncateString

*截断指定长度的字符串。 确定字符串的长度是否大于**num**。将截断的字符串返回到所需长度，并在末尾或原始字符串后附加“...”。*

```js
const truncateString = (str, num) =>
  str.length > num ? str.slice(0, num > 3 ? num - 3 : num) + '...' : str;
```

**Example**

```js
truncateString('boomerang', 7); // 'boom...'
```



##### 33|unescapeHTML

**Unescapes**转义了HTML字符。 将**String.prototype.replace（）**与正则表达式匹配，该正则表达式与需要非转义的字符匹配，使用回调函数使用字典（对象）将每个转义的字符实例替换为其关联的非转义字符。*

```js
const unescapeHTML = str =>
  str.replace(
    /&amp;|&lt;|&gt;|&#39;|&quot;/g,
    tag =>
      ({
        '&amp;': '&',
        '&lt;': '<',
        '&gt;': '>',
        '&#39;': "'",
        '&quot;': '"'
      }[tag] || tag)
  );
```

**Example**

```js
unescapeHTML('&lt;a href=&quot;#&quot;&gt;Me &amp; you&lt;/a&gt;'); // '<a href="#">Me & you</a>'
```



##### 34|URLJoin

*将所有给定的URL段连接在一起，然后规范化生成的URL。 使用**String.prototype.join（'/'）**组合URL段，然后使用各种regexp调用一系列**String.prototype.replace（）**来规范化生成的**URL**（删除双斜杠，为协议添加适当的斜杠，删除斜杠之前参数，将参数与'＆'组合并规范化第一个参数分隔符）。*

```js
const URLJoin = (...args) =>
  args
    .join('/')
    .replace(/[\/]+/g, '/')
    .replace(/^(.+):\//, '$1://')
    .replace(/^file:/, 'file:/')
    .replace(/\/(\?|&|#[^!])/g, '$1')
    .replace(/\?/g, '&')
    .replace('&', '?');
```

**Example**

```js
URLJoin('http://www.google.com', 'a', '/b/cd', '?foo=123', '?bar=foo'); // 'http://www.google.com/a/b/cd?foo=123&bar=foo'
```



##### 35|words

*将给定的字符串转换为单词数组。 将**String.prototype.split（）**与提供的模式（默认为非alpha作为**regexp**）一起使用，以转换为字符串数组。使用**Array.prototype.filter（）**删除任何空字符串。省略第二个参数以使用默认的regexp。*

```js
const words = (str, pattern = /[^a-zA-Z-]+/) => str.split(pattern).filter(Boolean);
```

**Example**

```js
words('I love javaScript!!'); // ["I", "love", "javaScript"]
words('python, javaScript & coffee'); // ["python", "javaScript", "coffee"]
```