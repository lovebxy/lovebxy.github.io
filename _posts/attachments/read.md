[TOC]



## 1.script 元素

1.1 主要的作用：将js代码插入到HTML中。



1.2 script标签中的属性

- async : 可选.表示应该立即开始下载脚本，但不能阻止其他页面动作
- charset : 可选，使用src属性指定的代码字符集
- crossorigin : 可选。配置相关请求的cors（跨资源共享）设置。默认不使用。`crossorigin="anonymous"`配置文件请求不必设置凭据标志，`crossorigin="use-credentials"`设置凭据标志，意味着出战请求会包含凭据
- defer ：可选。表示脚本可以延迟到文档完全被解析和显示之后再执行
- integrity ：允许比对接收到的资源和指定的加密签名以验证资源完整性
- src ：可选，表示包含要执行的代码的外部文件
- type ：可选，代替language，表示代码块中脚本语言的内容类型，如果这个值是module，则代码块会被当成es6模块，而且只有这个时候代码中才能出现import和export关键字



1.3使用script的方式有两种：

1.通过它直接在网页中嵌入js代码。

```javascript
<script>
  function sayHi(){
  	console.log("Hi!");
	}
</script>
```

包含在script内的代码会从上到下解释。被解释的是一个函数定义，并且该函数会保存在解释器环境中。在script元素中的代码被计算完成之前，页面的其余内容不会被加载，也不会被显示。

注意 ：在使用行内js代码时，要注意代码中不能出现字符串`script` 。浏览器在解析行内脚本的方式决定了它在看到字符串`</script>`时，会将其当成结束的`</script>`标签。想避免这个问题，只需要`\`即可

```javascript
<script>
	function sayScript(){
  	console.log("<\/script>")
	}
</script>
```

2.引入外部文件的js

必须使用src

```javascript
<script src="example.js" />
```

文件本身只需包含要放在`<script>`的起始及结束标签中间的js代码。与解释行内js一样，在解释外部js文件时，页面也会阻塞。（阻塞时间也包含下载文件的时间）

使用了src属性的`<script>`元素不应该再在`<script>`标签中包含其他js代码，如果两者都提供的话，则浏览器只会下载并执行脚本文件，从而忽略行内代码

`<script>`最为强大、同时也备受争议的地方在于，他可以包含来自外部域的js文件。跟`<img>`很像。

```javascript
<script src="http://www.baidu.com/api.js" />
```

浏览器在解析这个资源时，会向src属性指定的路径发送一个GET请求，以取得相应资源。但要注意，要引用自己的域，防止引用其他域的恶意攻击。

不管包含的什么代码，浏览器都会按照`script`出现的顺序依次解释它们，前提是没有使用 defer 和 async 属性和。之后的script标签，必须得等待前一个执行之后，才可以继续执行



1.4 标签的位置

过去所有的`<script>`元素都放在页面的head标签内，这样做法的目的就是把外部的 css 和 js 文件都集中放在一起。这样就意味着只有将所有的js代码都下载，解析和解释完成之后，才能开始渲染页面。对于需要很多js的页面，这个会导致页面渲染的明显延迟。在此期间浏览器窗口完全空白。

现代web应用程序通常将所有的js引用放在 body 元素中的页面内容后面，解决页面加载空窗期的问题，



1.5 推迟执行脚本

defer：这个属性表示脚本在执行的时候不会改变页面的结构，也就是，脚本会被延迟到整个页面都解析完毕后在运行。设置上这个属性之后，就相当于告诉浏览器立即下载，但延迟执行。考虑到这点，还是要把推迟执行的脚本放在页面底部比较好。

注意：defer属性只对外部脚本文件才有效。



1.6 异步执行脚本

在改变脚本处理方式上看，async属性与defer类似。它们两者都是只适合用于外部脚本，都会告诉浏览器立即开始下载。与defer不同的是，标记为async的脚本并不保证能按照它们出现的次序执行



1.7 动态加载脚本

除了script标签外，还有其他方式可以加载脚本。因为js可以使用DOM API,所以通向DOM中动态添加script元素同样可以加载指定的脚本。只要创建一个sript元素并添加到DOM即可

```javascript
let script = document.createElement('script');
script.src = 'a.js';
script.async = false; //默认情况是异步，所以为了统一动态脚本的加载行为，可以设置成同步
document.head.appendChild(script);
```





