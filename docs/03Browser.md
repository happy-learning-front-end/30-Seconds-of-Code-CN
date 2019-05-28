#### 03|Browser(浏览器)

##### 01|arrayToHtmlList

*将给定的数组元素转换为<li>标记，并将它们附加到给定id的列表中。 使用**Array.prototype.map（），document.querySelector（）**和**匿名内部闭包**来创建html标记列表。*

```js
const arrayToHtmlList = (arr, listID) =>
  (el => (
    (el = document.querySelector('#' + listID)),
    (el.innerHTML += arr.map(item => `<li>${item}</li>`).join(''))
  ))();
```

**Example**

```js
arrayToHtmlList(['item 1', 'item 2'], 'myListID');
```



##### 02|bottomVisible

*如果页面底部可见则返回true，否则返回false。* 

*使用**scrollY，scrollHeight和clientHeight**确定页面底部是否可见。*

```js
const bottomVisible = () =>
  document.documentElement.clientHeight + window.scrollY >=
  (document.documentElement.scrollHeight || document.documentElement.clientHeight);
```

**Example**

```js
bottomVisible(); // true
```



##### 03|copyToClipboard

*注意：使用新的异步剪贴板API可以轻松实现相同的功能，该API仍然是实验性的，但应该在将来使用，而不是使用此代码段。在这里了解更多相关信息。 将字符串复制到剪贴板。仅适用于用户操作的结果（即在单击事件侦听器内）。 创建一个新的<textarea>元素，用提供的数据填充它并将其添加到HTML文档中。使用**Selection.getRangeAt（）**存储选定的范围（如果有）。使用**document.execCommand（'copy'）**复制到剪贴板。从HTML文档中删除<textarea>元素。最后，使用**Selection（）。addRange（）**来恢复原始的选定范围（如果有的话）。*

```js
const copyToClipboard = str => {
  const el = document.createElement('textarea');
  el.value = str;
  el.setAttribute('readonly', '');
  el.style.position = 'absolute';
  el.style.left = '-9999px';
  document.body.appendChild(el);
  const selected =
    document.getSelection().rangeCount > 0 ? document.getSelection().getRangeAt(0) : false;
  el.select();
  document.execCommand('copy');
  document.body.removeChild(el);
  if (selected) {
    document.getSelection().removeAllRanges();
    document.getSelection().addRange(selected);
  }
};
```

**Example**

```js
copyToClipboard('Lorem ipsum'); // 'Lorem ipsum' copied to clipboard.
```



##### 04|counter

*创建具有指定选择器的指定范围，步长和持续时间的计数器。 检查步骤是否有正确的符号并相应更改。将**setInterval（）**与**Math.abs（）**和**Math.floor（）**结合使用，可以计算每次新文本绘制之间的时间。使用**document.querySelector（）**。**innerHTML**更新所选元素的值。省略第四个参数step，使用默认步骤1.省略第五个参数**duration**，使用默认持续时间**2000ms**。*

```js
const counter = (selector, start, end, step = 1, duration = 2000) => {
  let current = start,
    _step = (end - start) * step < 0 ? -step : step,
    timer = setInterval(() => {
      current += _step;
      document.querySelector(selector).innerHTML = current;
      if (current >= end) document.querySelector(selector).innerHTML = end;
      if (current >= end) clearInterval(timer);
    }, Math.abs(Math.floor(duration / (end - start))));
  return timer;
};
```

**Example**

```js
counter('#my-id', 1, 1000, 5, 2000); // Creates a 2-second timer for the element with id="my-id"
```



##### 05|createElement

*从字符串创建元素（不将其附加到文档）。如果给定的字符串包含多个元素，则只返回第一个元素。 使用**document.createElement（）**创建新元素。将**innerHTML**设置为作为参数提供的字符串。使用**ParentNode.firstElementChild**返回字符串的元素版本。*

```js
const createElement = str => {
  const el = document.createElement('div');
  el.innerHTML = str;
  return el.firstElementChild;
};
```

**Example**

```jsx
const el = createElement(
  `<div class="container">
    <p>Hello!</p>
  </div>`
);
console.log(el.className); // 'container'
```



##### 06|createEventHub

*使用emit，on和off方法创建pub / sub（发布 - 订阅）事件中心。* 

*使用**Object.create（null）**创建一个不从**Object.prototype**继承属性的空集线器对象。对于**emit**，根据事件参数解析处理程序数组，然后通过传入数据作为参数，使用**Array.prototype.forEach（）**运行每个处理程序。对于**on**，如果该事件尚不存在，则为该事件创建一个数组，然后使用**Array.prototype.push（）**将该处理程序添加到该数组中。对于off，使用**Array.prototype.findIndex（）**查找事件数组中处理程序的索引，并使用**Array.prototype.splice（）**将其删除。*

```js
const createEventHub = () => ({
  hub: Object.create(null),
  emit(event, data) {
    (this.hub[event] || []).forEach(handler => handler(data));
  },
  on(event, handler) {
    if (!this.hub[event]) this.hub[event] = [];
    this.hub[event].push(handler);
  },
  off(event, handler) {
    const i = (this.hub[event] || []).findIndex(h => h === handler);
    if (i > -1) this.hub[event].splice(i, 1);
  }
});
```

**Example**

```js
onst handler = data => console.log(data);
const hub = createEventHub();
let increment = 0;

// Subscribe: listen for different types of events
hub.on('message', handler);
hub.on('message', () => console.log('Message event fired'));
hub.on('increment', () => increment++);

// Publish: emit events to invoke all handlers subscribed to them, passing the data to them as an argument
hub.emit('message', 'hello world'); // logs 'hello world' and 'Message event fired'
hub.emit('message', { hello: 'world' }); // logs the object and 'Message event fired'
hub.emit('increment'); // `increment` variable is now 1

// Unsubscribe: stop a specific handler from listening to the 'message' event
hub.off('message', handler);
```



##### 07|currentURL

*返回当前URL。 使用**window.location.href**获取当前URL。*

```js
const currentURL = () => window.location.href;
```

**Example**

```js
currentURL(); // 'https://google.com'
```



##### 08|detectDeviceType

*检测网站是否在移动设备或台式机/笔记本电脑中打开。 使用正则表达式测试**navigator.userAgent**属性，以确定设备是移动设备还是台式机/笔记本电脑。*

```js
const detectDeviceType = () =>
  /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent)
    ? 'Mobile'
    : 'Desktop';
```

**Example**

```js
detectDeviceType(); // "Mobile" or "Desktop"
```



##### 09|elementContains

*如果父元素包含子元素，则返回**true**，否则返回**false**。 检查父元素是否与子元素不同，使用**parent.contains（child）**检查父元素是否包含子元素。*

```js
const elementContains = (parent, child) => parent !== child && parent.contains(child);
```

**Example**

```js
elementContains(document.querySelector('head'), document.querySelector('title')); // true
elementContains(document.querySelector('body'), document.querySelector('body')); // false
```



##### 10|elementIsVisibleInViewport

*如果指定的元素在视口中可见，则返回**true**，否则返回**false**。 使用**Element.getBoundingClientRect（）和window.inner（Width | Height）**值来确定给定元素在视口中是否可见。省略第二个参数以确定元素是否完全可见，或指定true以确定元素是否部分可见。*

```js
const elementIsVisibleInViewport = (el, partiallyVisible = false) => {
  const { top, left, bottom, right } = el.getBoundingClientRect();
  const { innerHeight, innerWidth } = window;
  return partiallyVisible
    ? ((top > 0 && top < innerHeight) || (bottom > 0 && bottom < innerHeight)) &&
        ((left > 0 && left < innerWidth) || (right > 0 && right < innerWidth))
    : top >= 0 && left >= 0 && bottom <= innerHeight && right <= innerWidth;
};
```

**Example**

```js
// e.g. 100x100 viewport and a 10x10px element at position {top: -1, left: 0, bottom: 9, right: 10}
elementIsVisibleInViewport(el); // false - (not fully visible)
elementIsVisibleInViewport(el, true); // true - (partially visible)
```



##### 11|formToObject

*将一组表单元素编码为对象。* 

*使用FormData构造函数将HTML表单转换为**FormData，Array.from（）**以转换为数组。使用**Array.prototype.reduce（）**从数组中收集对象。*

```js
const formToObject = form =>
  Array.from(new FormData(form)).reduce(
    (acc, [key, value]) => ({
      ...acc,
      [key]: value
    }),
    {}
  );
```

**Example**

```js
formToObject(document.querySelector('#form')); // { email: 'test@email.com', name: 'Test Name' }
```





##### 12|getImages

*从元素中获取所有图像并将它们放入数组中 使用**Element.prototype.getElementsByTagName（）**获取所提供元素中的所有<img>元素，**Array.prototype.map（）**以映射其各自<img>元素的每个src属性，然后创建一个**Set**以消除重复并返回阵列。*

```js
const getImages = (el, includeDuplicates = false) => {
  const images = [...el.getElementsByTagName('img')].map(img => img.getAttribute('src'));
  return includeDuplicates ? images : [...new Set(images)];
};
```

**Example**

```js
getImages(document, true); // ['image1.jpg', 'image2.png', 'image1.png', '...']
getImages(document, false); // ['image1.jpg', 'image2.png', '...']
```



##### 13|getScrollPosition

*返回当前页面的滚动位置。 如果已定义，则使用**pageXOffset和pageYOffset**，否则使用**scrollLeft和scrollTop**。您可以省略el以使用窗口的默认值。*

```js
const getScrollPosition = (el = window) => ({
  x: el.pageXOffset !== undefined ? el.pageXOffset : el.scrollLeft,
  y: el.pageYOffset !== undefined ? el.pageYOffset : el.scrollTop
});
```

**Example**

```js
getScrollPosition(); // {x: 0, y: 200}
```

##### 

##### 14|getStyle

*返回指定元素的CSS规则的值。 使用**Window.getComputedStyle（）**获取指定元素的CSS规则的值。*

```js
const getStyle = (el, ruleName) => getComputedStyle(el)[ruleName];
```

**Example**

```js
getStyle(document.querySelector('p'), 'font-size'); // '16px'
```



##### 15|hasClass

*如果元素具有指定的类，则返回true，否则返回false。 使用**element.classList.contains（）**检查元素是否具有指定的类*。

```js
const hasClass = (el, className) => el.classList.contains(className);
```

**Example**

```js
hasClass(document.querySelector('p.special'), 'special'); // true
```



##### 16|hashBrowser

*使用**SHA-256**算法为值创建哈希。返回一个**promise**。 使用SubtleCrypto API为给定值创建哈希。*

```js
const hashBrowser = val =>
  crypto.subtle.digest('SHA-256', new TextEncoder('utf-8').encode(val)).then(h => {
    let hexes = [],
      view = new DataView(h);
    for (let i = 0; i < view.byteLength; i += 4)
      hexes.push(('00000000' + view.getUint32(i).toString(16)).slice(-8));
    return hexes.join('');
  });
```

**Example**

```js
hashBrowser(JSON.stringify({ a: 'a', b: [1, 2, 3, 4], foo: { c: 'bar' } })).then(console.log); // '04aa106279f5977f59f9067fa9712afc4aedc6f5862a8defc34552d8c7206393'
```



##### 17|hide

*隐藏指定的所有元素。 使用**NodeList.prototype.forEach（）**将**display：none**应用于指定的每个元素。*

```js
const hide = (...el) => [...el].forEach(e => (e.style.display = 'none'));
```

**Example**

```js
hide(document.querySelectorAll('img')); // Hides all <img> elements on the page
```



##### 18|httpsRedirect

*如果页面当前处于HTTP状态，则将页面重定向到HTTPS。此外，按下后退按钮不会将其恢复到HTTP页面，因为它已在历史记录中替换。 使用**location.protocol**获取当前正在使用的协议。如果它不是**HTTPS**，请使用**location.replace（）**将现有页面替换为页面的**HTTPS**版本。使用**location.href**获取完整地址，使用**String.prototype.split（）**拆分它并删除**URL**的协议部分。*

```js
const httpsRedirect = () => {
  if (location.protocol !== 'https:') location.replace('https://' + location.href.split('//')[1]);
};
```

**Example**

```js
const httpsRedirect = () => {
  if (location.protocol !== 'https:') location.replace('https://' + location.href.split('//')[1]);
};
```



##### 19|insertAfter

*在指定元素结束后插入HTML字符串。 使用位置为'**afterend**'的**el.insertAdjacentHTML（）**来解析**htmlString**并在**el**结束后插入它。*

```js
const insertAfter = (el, htmlString) => el.insertAdjacentHTML('afterend', htmlString);
```

**Example**

```js
insertAfter(document.getElementById('myId'), '<p>after</p>'); // <div id="myId">...</div> <p>after</p>
```



##### 20|insertBefore

*在指定元素的开头之前插入HTML字符串。 使用位于'**beforebegin**'位置的**el.insertAdjacentHTML（）**来解析**htmlString**并在el开始之前插入它。*

```js
const insertBefore = (el, htmlString) => el.insertAdjacentHTML('beforebegin', htmlString);
```

**Example**

```js
insertBefore(document.getElementById('myId'), '<p>before</p>'); // <p>before</p> <div id="myId">...</div>
```



##### 21|isBrowserTabFocused

*如果页面的浏览器选项卡是聚焦的，则返回**true**，否则返回**false**。 使用**Page Visibility API**引入的**Document.hidden**属性来检查页面的浏览器选项卡是可见还是隐藏。*

```js
const isBrowserTabFocused = () => !document.hidden;
```

**Example**

```js
isBrowserTabFocused(); // true
```



##### 22|nodeListToArray

*将NodeList转换为数组。 在新数组中使用spread运算符将NodeList转换为数组。*

```js
const nodeListToArray = nodeList => [...nodeList];
```

**Example**

```js
nodeListToArray(document.childNodes); // [ <!DOCTYPE html>, html ]
```



##### 23|observeMutations

*返回一个新的**MutationObserver**，并为指定元素上的每个变异运行提供的回调。 使用**MutationObserver**观察给定元素的突变。使用**Array.prototype.forEach（）**为每个观察到的突变运行回调。省略第三个参数**options**，以使用默认选项（全部为**true**）。*

```js
const observeMutations = (element, callback, options) => {
  const observer = new MutationObserver(mutations => mutations.forEach(m => callback(m)));
  observer.observe(
    element,
    Object.assign(
      {
        childList: true,
        attributes: true,
        attributeOldValue: true,
        characterData: true,
        characterDataOldValue: true,
        subtree: true
      },
      options
    )
  );
  return observer;
};
```

**Example**

```js
const obs = observeMutations(document, console.log); // Logs all mutations that happen on the page
obs.disconnect(); // Disconnects the observer and stops logging mutations on the page
```



##### 24|off

*从元素中删除事件侦听器。 使用**EventTarget.removeEventListener（）**从元素中删除事件侦听器。省略第四个参数opts使用false或根据添加事件侦听器时使用的选项指定它。*

```js
const off = (el, evt, fn, opts = false) => el.removeEventListener(evt, fn, opts);
```

**Example**

```js
const fn = () => console.log('!');
document.body.addEventListener('click', fn);
off(document.body, 'click', fn); // no longer logs '!' upon clicking on the page
```



##### 25|on

*向具有使用事件委派功能的元素添加事件侦听器。 使用**EventTarget.addEventListener（）**向元素添加事件侦听器。如果有一个target属性提供给**options**对象，请确保事件目标与指定的目标匹配，然后通过提供正确的此上下文来调用回调。返回对自定义委托者函数的引用，以便可以使用off。省略选择默认为非委托行为和事件冒泡。*

```js
const on = (el, evt, fn, opts = {}) => {
  const delegatorFn = e => e.target.matches(opts.target) && fn.call(e.target, e);
  el.addEventListener(evt, opts.target ? delegatorFn : fn, opts.options || false);
  if (opts.target) return delegatorFn;
};
```

**Example**

```js
const fn = () => console.log('!');
on(document.body, 'click', fn); // logs '!' upon clicking the body
on(document.body, 'click', fn, { target: 'p' }); // logs '!' upon clicking a `p` element child of the body
on(document.body, 'click', fn, { options: true }); // use capturing instead of bubbling
```



##### 26|onUserInputChange

*每当用户输入类型更改（鼠标或触摸）时运行回调。用于根据输入设备启用/禁用代码。该过程是动态的并且适用于混合设备（例如，触摸屏笔记本电脑）。 使用两个事件侦听器。最初假设鼠标输入并将touchstart事件侦听器绑定到文档。在**touchstart**上，使用**performance.now（）**添加一个**mousemove**事件侦听器，以侦听在20ms内触发的两个连续**mousemove**事件。在任何一种情况下，使用输入类型作为参数运行回调。*

```js
const onUserInputChange = callback => {
  let type = 'mouse',
    lastTime = 0;
  const mousemoveHandler = () => {
    const now = performance.now();
    if (now - lastTime < 20)
      (type = 'mouse'), callback(type), document.removeEventListener('mousemove', mousemoveHandler);
    lastTime = now;
  };
  document.addEventListener('touchstart', () => {
    if (type === 'touch') return;
    (type = 'touch'), callback(type), document.addEventListener('mousemove', mousemoveHandler);
  });
};
```

**Example**

```js
onUserInputChange(type => {
  console.log('The user is now using', type, 'as an input method.');
});
```



##### 27|prefix

*返回浏览器支持的CSS属性的前缀版本（如果需要）。 在供应商前缀字符串数组上使用**Array.prototype.findIndex（）**来测试**document.body**是否在其**CSSStyleDeclaration**对象中定义了其中一个，否则返回null。使用**String.prototype.charAt（）**和**String.prototype.toUpperCase（）**来大写属性，该属性将附加到供应商前缀字符串。*

```js
const prefix = prop => {
  const capitalizedProp = prop.charAt(0).toUpperCase() + prop.slice(1);
  const prefixes = ['', 'webkit', 'moz', 'ms', 'o'];
  const i = prefixes.findIndex(
    prefix => typeof document.body.style[prefix ? prefix + capitalizedProp : prop] !== 'undefined'
  );
  return i !== -1 ? (i === 0 ? prop : prefixes[i] + capitalizedProp) : null;
};
```

**Example**

```js
prefix('appearance'); // 'appearance' on a supported browser, otherwise 'webkitAppearance', 'mozAppearance', 'msAppearance' or 'oAppearance'
```



##### 28|recordAnimationFrames

*在每个动画帧上调用提供的回调。 使用递归。如果**running**为**true**，则继续调用**window.requestAnimationFrame（）**，该窗口调用提供的回调。使用两种方法启动和停止返回一个对象，以允许手动控制录制。省略第二个参数**autoStart**，在调用函数时隐式调用start。*

```js
const recordAnimationFrames = (callback, autoStart = true) => {
  let running = true,
    raf;
  const stop = () => {
    running = false;
    cancelAnimationFrame(raf);
  };
  const start = () => {
    running = true;
    run();
  };
  const run = () => {
    raf = requestAnimationFrame(() => {
      callback();
      if (running) run();
    });
  };
  if (autoStart) start();
  return { start, stop };
};
```

**Example**

```js
const cb = () => console.log('Animation frame fired');
const recorder = recordAnimationFrames(cb); // logs 'Animation frame fired' on each animation frame
recorder.stop(); // stops logging
recorder.start(); // starts again
const recorder2 = recordAnimationFrames(cb, false); // `start` needs to be explicitly called to begin recording frames
```



##### 29|redirect

*重定向到指定的URL。 使用**window.location.href**或**window.location.replace（）**重定向到**url**。传递第二个参数来模拟链接**单击（true - 默认）或HTTP重定向（false）**。*

```js
const redirect = (url, asLink = true) =>
  asLink ? (window.location.href = url) : window.location.replace(url);
```

**Example**

```js
redirect('https://google.com');
```



##### 30|runAsync

*通过使用**Web Worker**在单独的线程中运行函数，允许长时间运行的函数不阻止UI。* 

*使用Blob对象URL创建新**Worker**，其内容应为所提供函数的字符串化版本。立即发布回调函数的返回值。返回一个承诺，监听**onmessage**和**onerror**事件并解析从工作人员发回的数据，或者抛出错误。*

```js
const runAsync = fn => {
  const worker = new Worker(
    URL.createObjectURL(new Blob([`postMessage((${fn})());`]), {
      type: 'application/javascript; charset=utf-8'
    })
  );
  return new Promise((res, rej) => {
    worker.onmessage = ({ data }) => {
      res(data), worker.terminate();
    };
    worker.onerror = err => {
      rej(err), worker.terminate();
    };
  });
};
```

**Example**

```js
const runAsync = fn => {
  const worker = new Worker(
    URL.createObjectURL(new Blob([`postMessage((${fn})());`]), {
      type: 'application/javascript; charset=utf-8'
    })
  );
  return new Promise((res, rej) => {
    worker.onmessage = ({ data }) => {
      res(data), worker.terminate();
    };
    worker.onerror = err => {
      rej(err), worker.terminate();
    };
  });
};
```



##### 31|scrollToTop

*平滑滚动到页面顶部。 使用**document.documentElement.scrollTop**或**document.body.scrollTop**与顶层保持距离。滚动距离顶部一小部分距离。使用**window.requestAnimationFrame（）**为滚动设置动画。*

```js
const scrollToTop = () => {
  const c = document.documentElement.scrollTop || document.body.scrollTop;
  if (c > 0) {
    window.requestAnimationFrame(scrollToTop);
    window.scrollTo(0, c - c / 8);
  }
};
```

**Example**

```js
scrollToTop();
```



##### 32|serializeForm

*将一组表单元素编码为查询字符串。 使用**FormData**构造函数将HTML表单转换为**FormData，Array.from（）**以转换为数组，并将map函数作为第二个参数传递。使用**Array.prototype.map（）**和**window.encodeURIComponent（）**来编码每个字段的值。将**Array.prototype.join（）**与适当的参数一起使用以生成适当的查询字符串。*

```js
const serializeForm = form =>
  Array.from(new FormData(form), field => field.map(encodeURIComponent).join('=')).join('&');
```

**Example**

```js
serializeForm(document.querySelector('#form')); // email=test%40email.com&name=Test%20Name
```



##### 33|setStyle

*设置指定元素的CSS规则的值。 使用**element.style**将指定元素的CSS规则值设置为val。*

```js
const setStyle = (el, ruleName, val) => (el.style[ruleName] = val);
```

**Example**

```js
setStyle(document.querySelector('p'), 'font-size', '20px'); // The first <p> element on the page will have a font-size of 20px
```



##### 34|show

显示指定的所有元素。 使用spread运算符**（...）**和**Array.prototype.forEach（）**清除指定的每个元素的display属性。

```js
const show = (...el) => [...el].forEach(e => (e.style.display = ''));
```

**Example**

```js
show(...document.querySelectorAll('img')); // Shows all <img> elements on the page
```



##### 35|smoothScroll

*平滑地将调用它的元素滚动到浏览器窗口的可见区域。 使用**.scrollIntoView**方法滚动元素。将**{behavior：'smooth'}**传递给**.scrollIntoView**，使其顺畅滚动。*

```js
const smoothScroll = element =>
  document.querySelector(element).scrollIntoView({
    behavior: 'smooth'
  });
```

**Example**

```js
smoothScroll('#fooBar'); // scrolls smoothly to the element with the id fooBar
smoothScroll('.fooBar'); // scrolls smoothly to the first element with a class of fooBar
```



##### 36|toggleClass

*切换元素的类。 使用**element.classList.toggle（）**切换元素的指定类。*

```js
const toggleClass = (el, className) => el.classList.toggle(className);
```

**Example**

```js
toggleClass(document.querySelector('p.special'), 'special'); // The paragraph will not have the 'special' class anymore
```



##### 37|triggerEvent

*触发给定元素上的特定事件，可选地传递自定义数据。 使用新的**CustomEvent（）**从指定的**eventType**和详细信息创建事件。使用**el.dispatchEvent（）**在给定元素上触发新创建的事件。如果您不想将自定义数据传递给触发事件，请省略第三个参数**detail**。*

```js
const triggerEvent = (el, eventType, detail) =>
  el.dispatchEvent(new CustomEvent(eventType, { detail }));
```

**Example**

```js
triggerEvent(document.getElementById('myId'), 'click');
triggerEvent(document.getElementById('myId'), 'click', { username: 'bob' });
```



##### 38|UUIDGeneratorBrowser

*在浏览器中生成**UUID**。 使用**crypto API**生成符合**RFC4122**版本4的UUID。*

```js
const UUIDGeneratorBrowser = () =>
  ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, c =>
    (c ^ (crypto.getRandomValues(new Uint8Array(1))[0] & (15 >> (c / 4)))).toString(16)
  );
```

**Example**

```js
UUIDGeneratorBrowser(); // '7982fcfe-5721-4632-bede-6000885be57d'
```

