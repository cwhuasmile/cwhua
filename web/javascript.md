

 代码中计算天数的方法

d = parseInt(总秒数/60/60/24);  //计算天数

h = parseInt(总秒数/60/60%24);  //计算小时

m = parseInt(总秒数/60%60);  //计算分数

s = parseInt(总秒数%60);  //计算当前秒数

```js
function countDown(time) {
    var nowTime = +new Date();
    var inputTime = +new Date(time);
    var times = (inputTime - nowTime) / 1000;
    var d = parseInt(times / 60 / 60 / 24);
    d = d < 10 ? '0' + d : d;
    var h = parseInt(times / 60 / 60 % 24);
    h = h < 10 ? '0' + h : h;
    var m = parseInt(times / 60 % 60);
    m = m < 10 ? '0' + m : m;
    var s = parseInt(times % 60);
    s = s < 10 ? '0' + s : s;
    return '距离开奖时间还有：' + d + '天' + h + '时' + m + '分' + s + '秒';
}
console.log(countDown('2020-02-27 14:30:00'))
var date = new Date();
console.log('现在的时间是：' + date);
```

## DOM重点核心

关于dom操作，我们主要针对于元素的操作。主要有创建、增、删、改、查、属性操作、事件操作。

### 1. 创建

 	1. document.write
 	2. inner HTML
 	3. createElement

### 2. 增

 	1. appendChild
 	2. inserBefore

### 3. 删

 	1. removeChild

### 4. 改

​	主要修改dom的元素属性，dom元素的内容、属性、表单的值等。

 	1. 修改元素的属性：src、href、title等
 	2. 修改普通元素内容：innerHTML、innerText
 	3. 修改表单元素：value、type、disabled等
 	4. 修改元素样式：style、className

### 5. 查

​	主要获取查询dom的元素

 	1. DOM提供的API方法：getElementById、getElementsByTagName 古老用法不推荐
 	2. H5提供的新方法：querySelector、querySelectorAll 提倡
 	3. 利用节点操作获取元素：父（parentNode）、子（children）、兄（previousElementSibling、nextElementSibling） 提倡

### 6. 属性操作

​	主要针对于自定义属性。

 	1. setAttribute：设置dom的属性值
 	2. getAttribute：得到dom的属性值
 	3. removeAttribute：移除属性

### 7. 事件操作

​	给元素注册事件，采取 事件源.事件类型 = 事件处理程序

#### 7.1鼠标事件

当使用document.addEventListener('event', function(), 捕获或冒泡)事件监听对象时，参数中的event需要去掉对应事件名称前面的”on“。

| 鼠标事件    | 触发条件         |
| ----------- | ---------------- |
| onclick     | 鼠标点击左键     |
| onmouseover | 鼠标经过触发     |
| onmouseout  | 鼠标离开触发     |
| onfocus     | 获得鼠标焦点触发 |
| onblur      | 失去鼠标焦点触发 |
| onmousemove | 鼠标移动触发     |
| onmouseup   | 鼠标弹起触发     |
| onmousedown | 鼠标按下触发     |
| contextmenu | 鼠标右键菜单     |
| selectstart | 鼠标左键开始选中 |

#### 7.2鼠标事件对象

| 鼠标事件对象 | 说明                                     |
| ------------ | ---------------------------------------- |
| e.clientX    | 返回鼠标相对于浏览器窗口可视区的 X 坐标  |
| e.clientY    | 返回鼠标相对于浏览器窗口可视区的 Y 坐标  |
| e.pageX      | 返回鼠标相对于文档页面的 X 坐标 IE9+支持 |
| e.pageY      | 返回鼠标相对于文档页面的 Y 坐标 IE9+支持 |
| e.screenX    | 返回鼠标相对于电脑屏幕的 X 坐标          |
| e.screenY    | 返回鼠标相对于电脑屏幕的 X 坐标          |

#### 7.3键盘事件

| 键盘事件   | 触发条件                                                     |
| ---------- | ------------------------------------------------------------ |
| onkeyup    | 某个键盘按键被松开时触发                                     |
| onkeydown  | 某个键盘按键被按下时触发                                     |
| onkeypress | 某个键盘按键被按下时触发 `但是它不识别功能键，如 ctrl shift等` |

> 如果使用addEventListener不需要加no
>
> onkeyup和onkeydown不区分字母大小写，onkeypress区分字母大小写

#### 7.4键盘事件对象

| 键盘事件对象 `属性` | 说明                |
| ------------------- | ------------------- |
| keyCode             | 返回`该`键的ASCII值 |

#### 7.5触屏事件

| 触屏touch事件 | 说明                          |
| ------------- | ----------------------------- |
| touchstart    | 手指触摸到一个DOM元素时触发   |
| touchmove     | 手指再一个DOM元素上滑动时触发 |
| touchend      | 手指从一个DOM元素上移开时触发 |

#### 7.6触摸事件对象

| 触摸列表       | 说明                                             |
| -------------- | ------------------------------------------------ |
| touches        | 正在触摸屏幕的所有手指的一个列表                 |
| targetTouches  | 正在触摸当前DOM元素上的手指列表                  |
| changedTouches | 手指状态发生了改变的列表，从无到有，从有到无变化 |

> 因为平时我们都时给元素注册触摸事件，所以重点记住targetTouches

## jQuery重点方法

### 1. jQuery选择器

```js
$("选择器")  //里面选择器直接写CSS选择器即可，但是要加引号
```
#### 1.1 jQuery基本选择器

| 名称       | 用法            | 描述                     |
| ---------- | --------------- | ------------------------ |
| ID选择器   | $("#id")        | 获取指定ID的元素         |
| 全选选择器 | $("*")          | 匹配所有元素             |
| 类选择器   | $(".class")     | 获取同一类class的元素    |
| 标签xzq    | $("div")        | 获取同一类标签的所有元素 |
| 并集选择器 | $("div,p,li")   | 获取多个元素             |
| 交集选择器 | $("li.current") | 交集元素                 |

#### 1.2 jQuery层级选择器

| 名称       | 用法       | 描述                                                         |
| ---------- | ---------- | ------------------------------------------------------------ |
| 子代选择器 | $("ul>li") | 使用>号，获取亲儿子层级的元素；注意，并不会获取孙子层级的元素 |
| 后代选择器 | $("ul li") | 使用空格，代表后代选择器，获取ul下的所有li元素，包括孙子等   |

#### 1.3jQuery筛选选择器

| 语法       | 用法          | 描述                                                      |
| ---------- | ------------- | --------------------------------------------------------- |
| :first     | $("li:first") | 获取第一个li元素                                          |
| :last      | $("li:last")  | 获取最后一个li元素                                        |
| :eq(index) | $("li:eq(2)") | 获取到的li元素中，选择器引号为2的元素，所以好index从0开始 |
| :odd       | $("li:odd")   | 获取到的li元素中，选择器引号为奇数的元素                  |
| :even      | $("li:even")  | 获取到的li元素中，选择器引号为偶数的元素                  |

#### 1.4jQuery筛选方法（重点）

| 语法               | 用法                            | 说明                                                   |
| ------------------ | ------------------------------- | ------------------------------------------------------ |
| parent()           | $("li").parent();               | 查找父级                                               |
| children(selector) | $("ul").children("li");         | 相当于`$("ul>li")`，最近一级（亲儿子）                 |
| find(selector)     | $("ul").find("li");             | 相当于`$("ul li")`，后代选择器                         |
| siblings(selector) | $(".first").siblings("li");     | 查找兄弟节点，不包括自己本身                           |
| nextAll([expr])    | $(".first").nextAll("");        | 查找当前元素之后所有的同辈元素                         |
| prevtAll([expr])   | $(".last").prevAll();           | 查找当前元素之前所有的同辈元素                         |
| hasClass(class)    | $("div").hasClass("protected"); | 检查当前的元素是否含有某个特定的类，如果有，则返回true |
| eq(index)          | $("li").eq(2);                  | 相当于`$("li:eq(2)")`，index从0开始                    |

### 2. jQuery效果

| 显示隐藏 | 滑动          | 淡入淡出             | 自定义动画 |
| -------- | ------------- | -------------------- | ---------- |
| show()   | slideDown()   | fadeIn()             | animate()  |
| hide()   | slideUp()     | fadeOut()            |            |
| toggle() | slideToggle() | fadeToggle()         |            |
|          |               | fadeTo()  切换透明度 |            |

>  解决动画排队问题用stop()，例如：`$(this).siblings().stop().fadeTo(400,0.5)`

## JavaScript函数进阶内容

### 1 JavaScript函数调用方式

#### 1.1 普通函数

```js
function fn() {
    console.log('人生的巅峰')
}
fn();
fn.call();
```

#### 1.2 对象函数

```js
var o = {
    sayHi: function() {
        console.log('人生的巅峰')
    }
}
o.sayHi();
```

#### 1.3 构造函数

```js
function Star() {};
new Star();
```

#### 1.4 绑定事件函数

```js
btn.onclick = function() {};  //点击了按钮就可以调用这个函数
```

#### 1.5 定时器函数

```js
setInterval(function() {}, 1000);  //这个函数时定时器自动1秒钟调用一次
```

#### 1.6 立即执行函数

```js
(function() {
    console.log('人生的巅峰');
})();  //立即执行函数时自动调用
```

### 2 this指向

#### 2.1函数内this的指向

这些this的指向，是当我们调用函数的时候确定的。调用方式的不同决定了this的指向不同，一般指向我们的调用者。

| 调用方式     | this指向                                  |
| ------------ | ----------------------------------------- |
| 普通函数调用 | window                                    |
| 构造函数调用 | 实例对象 原型对象里面的方法也指向实例对象 |
| 对象方法调用 | 该方法所属对象                            |
| 事件绑定方法 | 绑定事件对象                              |
| 定时器函数   | window                                    |
| 立即执行函数 | window                                    |

### 3 严格模式

1. 我们的标量名必须先声明再使用
2. 我们不能随意删除已经声明好的变量
3. 严格模式下全局作用域中函数中的this是undefined
4. 严格模式下，如果构造函数不加new调用，this指向的是undefined，如果给他赋值则会报错
5. 定时器this还是指向window
6. 严格模式下函数里面的参数不允许有重名
7. 函数的声明必须在顶层，新版本的JavaScript会引入“块级作用域”（ES6中已引入）。为了与新版本接轨，不允许再非函数的代码块内声明函数。

