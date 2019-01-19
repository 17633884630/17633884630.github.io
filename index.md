## 日常知识点记录 || (常用)
```markdown
1.template.default.import.funName = function(value){...}  
  funName使用 {|分隔符}
2.var obj = {}
  obj['id_'+ id] = data.id	(存)
  let id = obj['id_'+ id]   (取)
3.vuex
  this.$store.state.data = data;(存)
  this.$store.state.Cache(文件名).data (存)
4.localStorage(存对象)
  localStorage.setItem("data", JSON.stringify(data));(存)
  JSON.parse(localStorage.getItem("data"));(取)
5.对于{}怎么判断;
  $.isEmptyObject({})	true;
  $.isEmptyObject({name:"name"})	false;
6.对于vue中拼接(动态)
  :title="`主题导航(${name})`"  !!!v-bind
  :title="'字符串' + xx"
  :href="'/forum/list/?id=' + id"
7.富文本框不获取焦点
  E.prototype.initSelection = function(line) {}
8.unescape(str) 函数
  str --->要解码或反转义的字符串
9.解决ztree拦截问题
  $.ajaxSetup({
    async: true,
    type: 'GET',
    dataType: 'json',
    cache: false,
    beforeSend: function(xhr) {
      var login = $cookies.get("token");
      xhr.setRequestHeader('Authorization', "Bearer " + login.access_token);
    },
  });
10:http 和 https 
  例如-->饿了么(element-ui)在本地出现--->无法访问 --->检查下是不是使用的是http(是的话) -->改成https 就可以正常访问
  http ---> 网络协议
  https --> 是以安全为目标的http通道,就是在http加入SSL层.
11.函数分流(函数防抖)
  函数防抖:就是多次触发事件后,事件处理函数只执行一次,而且是在事件触发操作停止的时候;
 -->延迟处理函数,如果设定的时间到来之前,又一次触发了事件,就清除上一次的定时器;
  obj.onmousemove = function () {
    clearTimeout(myFun.timer);
    myFun.timer = setTimeout(()=>{},50);
  }
  函数分流:函数分流的思想就是计时;
12,import,export的使用;(在template里使用还没有研究);
  定义变量:
  -->export const str = "hello"  --> export const arr = ["arr","str","obj"] --> export const obj = {
    str:"str",
    arr:["arr","str","obj"],
    obj:{}
  }
  引用变量:
  import {str,arr,obj} from xx" -->xx(路径);
13.数组删除指定的变量:
  let arr = ["arr","str","obj"]
   Array.prototype.remove = function (val) {
    var index = this.indexOf(val);
    if (index > -1) {
        this.splice(index, 1);
    }
   };
   arr.remove("arr") --> arr -->["str","obj"]
15:使用powerShell,cd之后使用 文件第一个字母 -->tab能自动补全路径;
```
### 项目记录
```markdown
1.使用iview-ui:
    在iview中event时全局变量:使用直接在函数中
   let e = event.target;
1.1在style修改ui样式不起作用问题(可能)是在style中加了作用域scoped
    可以使用 .red >>> ui 
1.2在table中使用render
  render:(h,params)=>{
    return h('span',params.row == true ? '是' : '否')
  }
1.3.在form表单中重置(重置表单)
    this.$refs['ruleForm'].resetFields(); //移除校验结果并重置字段值(element,iview ui)
    this.$refs['ruleForm'].clearValidate(); //移除校验结果(element ui^2.4.3)
在外部：
   this.$nextTick(() => {
      this.$refs['ruleForm'].resetFields()
   })
1.4,验证必须为数字
   num:[
     {type:"integer",required: true,message: "必须是数字"}
   ]
2.简单的清空对象
   $.each(obj,(k,v)=>{
     obj[k] = null
   })
3.数组去重排序
   Array.from(new Set(xx)).sort((a,b)=>{return b - a})
4.对象去重
   var hash = {},str = []; 
   str = str.reduce((item, next) =>{
      hash[next.xx]  ? '' : hash[next.xx] = true && item.push(next);  ---> xx-->key
      return item
    }, [])
5.数组合并
   str.concat(str) --> str -->合并,拼接
6.数组转字符串
  join(',') ---> 默认可以填空
7.字符串转数组
  split('') --->默认填空会所有分割
8.删除数据(项目)
  let num = 0;
  $.each(this.list, (d, i) => {
    if(d.success){
      num++;
      if (num >= this.list.length) {
       this.list.length = 0;
      }
     } 
  })
```
### es6 
```markdown
1.对象合并拷贝 
    var obj = Object.assign({},obj1,obj2) 
    Object.assign(obj,obj1,obj2) ---> obj
1.1 jq对象合并
    $.extend(true,obj,obj1)
2.对于定义变量:var,const,let 的区别
    var --> 定义的变量,没有块的概念,可以跨块访问,不能跨函数访问;
    let --> 定义的变量,只能在块作用域里访问,不能跨块访问,也不能跨函数访问
    const > 定义常量,使用时必须初始化(必须赋值),只能在块作用域里访问,而且不能修改;
    const obj --> Missing initializer in const declaration --> const声明中缺少初始化器(const定义必须初始化)
    const obj = {};
    var obj;let obj; --> Identifier 'obj' has already been declared --> 标识符“ob”已经声明(报错)
    let str;
    var str;--> Identifier 'str' has already been declared --> let,const定义的var都不能声明 -->不然会报错(xx已经声明)
2.1.JS作用域 
    全局变量的作用域:作用域是全局的,在代码的任何地方都是有定义的,然而函数的参数和局部变量只在函数体内有定义,另外局部变量的优先级要高于同名的全局变量,也就是说当局部变量与全局变量重名时,局部变量会覆盖全局变量;声明局部变量时一定要定义,不能解释权会将该变量当做全局对象window的属性;
    函数作用域:(let块级作用域)--->在变量声明的代码段之外时不可见的,我们通常称为块级作用域;
    链式调用的原理 ---> return this 
    作用域的关键:函数的内部环境可以通过作用域链访问到所有的外部环境,但是外部环境却不可以访函数内部环境,这就是作用域的关键
    闭包 --> 允许使用内部函数(即函数定义和函数表达式位于另一个函数的函数体内);而且,这些内部函数可以访问他们所在外部函数中声明的所有局部变量,参数和声明的其他内部函数,当其中一个这样的内部函数在包含他们的外部函数之外调用时,就会形成闭包.即内部函数会在外部函数返回后被执行,而当这个内部函数执行时,它仍然必须访问其外部函数的局部变量,参数以及其他内部函数.这些局部变量,参数和函数声明(最初时)值时外部函数返回时的值,但也会受到内部函数的影响; --->(锋利的jquery)

```
### 移动端记录
```markdown
1.rem,em,vw,vh,px
    px:绝对单位;
    rem:是基于html元素的字体大小来决定;	html元素16px,10rem将等于160px
    em:根据使用它的元素大小决定;使用他的元素有18px,10em将等于 180px
    vw:相对于视窗的宽度;视窗宽度是100vw
    vh:相对于视窗的高度;视窗高度是100vh
```

### VUE记录
```markdown
1.vue跳转传参问题:
  this.$router.push({name:'/',params:{data:data}})  //链接上不显示
  this.$router.push({path:'/',query:{data:data}})   //链接显示
  this.$router.push({name:'/',params:{data:data},query:{data:data}})   //链接显示query.data
	在跳转接受值时候注意:
	this.$route.query  不是 this.$router.query 要注意多一个r!!!;
	
2.axios请求使用表单(form)提交
   let data = new FormData();
   data.append('data',data)
   const cofig={
   headers:{
     "Content-Type": "multipart/form-data"
    }
   }
   axios.post('/url',data,cofig)  
3.在vue中 v-for="item in obj" ---> 会报错key的错,
  (报错之后可以直接写:key="item.xx") --> xx 不能为字符串即可
  ---> 而不用v-for="(item,index) in obj" --> :key="index"
4.$.ajax提交数据是如果是  -- 表单提交时 -- 提交数组时,会自动在所设定的参数后面增加中括号 []
   如str[]:1
   原因:$.ajax会调用$.param序列化参数,$.param(obj,traditional) 默认的话，traditional为false;
   --> 导致后端springboot中的@RequestParam获取不到参数。
   解决办法:ajax请求时增加：traditional: true 就可以正常提交了
5.v-for和v-if同时使用时,如果报错key-->undefined是;就把v-if和v-for不同级就可以;
6.对象的取值方式:
   var obj = {a:{b:1}};
   obj.a.b -- 相等 -- obj["a"]["b"]
   但是((如果不确定值,还没有初始化定义))的话:使用obj["a"]["b"]这种方式就取不到值了
   -->解决办法
   obj[item.model][item.objStr];
   [item.model] --> 对象
   [item.objStr] -->对象属性(xx自定义)

```
### 关于template模拟实现js和html代码分离
```markdown
	<!DOCTYPE html>
	<html>
		<head>
			<title>artTemplate js和html模板分离</title>
			<meta charset="UTF-8"/>
			<script src="jquery.min.js"></script><script src="template-web.js" ></script>
		</head>
		<body><div id="content"></div></body>
	</html>
	<script>
		$(function(){
			var obj = {title: '标签1',list: ['文艺', '博客', '摄影', '电影', '民谣', '旅行', '吉他'],isAdmin:true};		
			var html = $.post('test.html',function (data) {
				var source = data.getElementById('test'); //获取文件 -->获取文件中的一个模板			
		   		var render = template.compile(source.innerHTML);  //-->template.compile(data) data -->字符串
		    	document.getElementById('content').innerHTML = render(obj); //render()将返回渲染结果。
			})
	})
	test.html:
	<script id="test" type="text/html">
	 {{if isAdmin}}
		<h1>{{title}}</h1>
		<ul>
			{{each list as value i}}
			<li>索引 {{i + 1}} ：{{value}}</li>
			{{/each}}
		</ul>
	{{/if}}
</script>
</script>
```
### VUE ---> 关于Vue父子组件间传值解析
```markdown
在vue中对于父子组件传值(v-bind,@on,$emit) ---> 
  父组件:
  <template>
    <div class="hello">
        ----------------父组件--------------------
        我是父组件的钱:
        <input type="text" v-model="msgs" class="inputName">
        我是子组件的钱:
        <input type="text" v-model="inputs" class="inputName" >
        ---------------子组件------------------
        :msgs ---> 父组件 ---> 子组件   @input  子组件 ---> 
        <looks ref="chlidren" :msgs="msgs" @input="input" :testOne="inputs"></looks>      
    </div>
  </template>
  export default {
    components: { looks },
    name: 'hello',
    data() {
        return {
            // msgs:undefined,  ---> 值为undefind 可触发默认值
            msgs: "给子组件100块钱",
            inputs:""
        };
    },
    methods: {
        input(html) {
            this.inputs = html  ----> 子传父组件的值
        }
    }
  }
  子组件:
  <template>
    <div>     
        我在子组件中呀:{{names}}
        <input type="text" v-model="msgs" class="inputName">
        我在子组件中呀:
        <input type="text" class="inputName" v-model="testOne">
    </div>
  </template>
  <script>
  export default {
    name: 'look',
    // props:['msg'],
    props: {
        //验证
        // msgs:String,  //验证数据规格, ---> 字符串,数值,布尔值,函数,对象,数组,symbol(表示独一无二的值)--->原始类型的值  symbol()
        // required: true
        //默认值
        // msgs:{
        //  type: String,
        //  default:"这是100块钱的默认值",
        //  // default:()=>{return '函数返回--->这是100块钱的默认值'}
        // }
        // 自定义验证函数
        // msgs: {
        // type: String,
        // validator: function(t) {
        //     // 这个值必须匹配下列字符串中的一个
        //     return t === 'fade' || t === 'slide'  ---> 这这测试有点问题 欢迎解答--->17633884630@163.com
        // },
        // defalut: 'slide'
        // }
        msgs: {
            type: String,
            required: true
        },
        testOne: {
            type: String,
            required: true
        }
    },
    data() {
        return {
            names: "我是子组件,我是直接在子组件里面写的值",
        }
    },
    watch: {
        msgs(cur, old) {
            this.$emit('input', this.msgs) ---> 检测msgs的值的变化事实更新数据
        }
    },
    mounted() {
        this.$emit('input', this.msgs) ---> 初始话
    }
  }
```
### 对于JavaScript,JQuery记录
```markdown
1.js数据类型
  字符串(String),数值(Number),布尔值(Boolean),对象(Object),null,undefined;
 ES6引入了一种新的原始数据类型Symbol,表示独一无二.
 let str = Symbol();
 typeof str  //symbol
 typeof null  //object
 typeof undefined  //undefined
 null == undefined //true
2.$.extend() 函数将一个或者多个对象的内容合并到目标对象;
3.对于event的使用:
 event.target.nodeName  　　 //获取事件触发元素标签name
 event.target.id　　　　　　　//获取事件触发元素id
 event.target.className　　  //获取事件触发元素classname
 event.target.innerHTML　　  //获取事件触发元素的内容
```

### 对于CSS,CSS3记录
```markdown
1.文字超出隐藏
   text-overflow:ellipsis;
   white-space:nowrap;
   overflow:hidden;
2.css去除ul,li,a下划线,默认样式
   list-style：none;
   text-decoration none;
```

### 问题反馈
这是在开发中遇到,和日常的记录,如果有错误的地方,希望得到您的指点,
邮箱17633884630@163.com
您的不经意的指点就是我的学习的方向;

### 项目常用代码片段记录

### Markdown
Markdown是一种轻量级且易于使用的语法，用于为您的文章设计样式。它包括以下约定
```markdown
语法高亮代码块

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

有关更多细节，请参见[GitHub风味Markdown](https://guides.github.com/features/mastering-markdown/)。
### 插件
```markdown
sublime 中文插件 ---> Ctrl + Shift + P ---> package control:install package ---> localiza
                格式化插件    ---> HTML-CSS-JS prettify ---> Ctrl + Shift + H
```
### 读书笔记
```markdown
1.javaScript是一种区分大小写的语言;
2.html不区分大小写
3.直接量:就是程序中直接显示除了的数据值;(数字,字符串,---)
4.""; 空串 他的字符数是0; 
5.字符串和数组一样都是以0开始进行索引;
6.布尔值,在必要时会将true转换成1,false转换成0;
7.在js中函数时一个真正的数据类型;
8.JavaScript是无类型的
9.JS中作用域有：全局作用域、函数作用域。没有块作用域的概念
9.1.var 声明的变量是永久性的,可以修改,如果不初始化会输处undefined,不会报错
    const定义的变量不可以修改
    let是块级作用域,函数内部使用let顶以后,对函数外部无影响
9.2.函数作用域(局部变量)作用域是局部性;在函数体内部,局部变量的优先级比同名的全局变量高;
10.基本类型 (数值,布尔值,null,undefined) 在内存中具有固定大小
11.引用类型 (对象,数组,函数)
12.字符串特例(基本类型,引用类型)
13.window对象代表浏览窗口,在客户端js中
14.在执行一个函数时,函数参数和局部变量是作为调用对象的属性而存储的
15.当函数引用一个变量时,首先检查的是调用对象(局部作用域),其次才检查全局对象(全局作用域)
16.var str = (function(x){return x*x})(5) //str 25
17.函数arguments.length
    callee属性:用来引用当前正在执行的函数;
18.call()和apply()的第一个参数都是要调用的函数的对象;在函数体内这一参数是关键字this的值;
    call()第二参数:可以接收任意个参数;
    apply()第二参数:必须是一个数组,或者类数组;
19.对象  key:value;
19.1 for/in for(k in obj) k ---> key  obj[k] ---> value;
19.2 构造函数 new Function("return str")  ---> ()
    function str(a,b){this.width = a;this.height = b;}
    var arr = new str(2,5);
    console.log(arr) ---> str {width: 2, height: 5} 
    console.log(arr.width) ---> 2
    构造函数通常没有返回值,它们只是初始化由this值传递进来的对象,并且什么也不返回.但是,构造函数可以返回一个对象值,如果这样做,被返回的对象就成了new表达式的值,在这种情况下,this值所引用的对象就被丢弃了
    function calculate(){return this.a * this.b}
    function str(a,b){
    //初始化对象属性;
    this.a = a;this.b = b;
    //定义对象的方法
      this.calculate = calculate
    }
    var arr = new str(2,3);
    console.log(arr.calculate()) ---> 6
19.3,继承 js中所有函数都有prototype属性;
   每个对象都有原型对象,原型对象的所有属性是以它为原型的对象的属性,每个对象都继承原型对象的所有属性;
    function b(x,y,r){this.x = x;this.y = y;this.r = r;}
    new b(0,0,0)  --->创建并舍弃初始b对象
    b.prototype.pi = 3.14159		--->定义常量
    b.prototype.e = function(){return 2 * this.pi * this.r;}	--->定义方法
    b.prototype.t = function(){return  this.pi * this.r * this.r}
    var c = new b(0.0,0.0,1.0)  --->创建实例调用它的方法
    var a = c.t()  ---> 3.14159
    var p = c.e() ---> 6.28318
19.4.不只是我们定义的类具有原型对象,像String等这样内部类用于具有原型对象;
    String.prototype.e = function(e){return e == this.charAt(this.length-1)} --->str.e(length-1) --->true
19.5 Complex --> Complex.prototype --> Object.prototype (查询路径)
    function Complex(x,y){this.x = x;this.y = y;}
    Complex.prototype.magnitude = function(){return Math.sqrt(this.x * this.x + this.y * this.y)}
    Complex.prototype.negative = function(){return new Complex(-this.x,-this.y);}
    Complex.prototype.toString = function(){return "{" + this.x + "," + this.y + "}"} 
    Complex.prototype.valueOf = function(){return this.x;}
    Complex.add  = function(a,b){
      return new Complex( a.x + b.y , a.x + b.y)  ---> !!!2个参数;
    }
    Complex.subtract  = function(a,b){
      return new Complex( a.x - b.x , a.y - b.y)
    }
    Complex.multiply  = function(a,b){
      return new Complex( a.x * b.x - a.y * b.y ,  a.x * b.y - a.y * b.x)
    }
    var v = new Complex(5,6)
    var b = new Complex(5,6)
    console.log(v.toString(5,5))
    console.log(Complex.add(v,b)) ---->调用需要new 2个数据;
20.Object(所有内部类的超类,所有类都继承了Object的基本方法)
21.关联数组对象:
   例子: const obj = {a:1};
   obj.a === obj['a']  -->ture
   (在编写程序时,[]的属性名是一个字符串值(该值是动态的,可以在运行时改变),而不是一个标识符(它时情态的,在程序中必须对其进行硬编码))
   关联数组:它是一种数据结构,允许你动态地将任意数组和任意字符串关联在一起  --> for/in,$.each -->把属性名从关联数组中抽取出来
22.constructor(每个对象都具有 constructor 属性,它引用的是用来初始化该对象的构造函数)
   function Complex(x,y){this.x=x;this.y=y} let con = new Complex(2,2) 
   console.log(con.constructor) ---> Complex  --> con.constructor === Complex --> ture
   验证对象局部的非继承的属性 (hasOwnProperty()):
   --> con.hasOwnProperty("x") -->true --> 
   --> con.hasOwnProperty("xx") -->true
   验证调用对象是否是实际参数指定的对象的原型对象
   --> Complex.prototype.isPrototypeOf(con) --> true
23 [object object(class)]
   class 是对象的内部类型,通常对应于该对象的构造函数名, ---> Array对象的class为 Array
24. 数组Array(返回的新数组 --> 使用新的变量接受)
   Array.reverse() --> 颠倒数组元素并返回新数组;
   Array.join()    --> 数组转字符串
   Array.sort()    --> 数组元素进行排序
   Array.concat()  --> 合并数组
   Array.slice(start,end)   --> 返回指定数组的一个片段  --> 方法可从已有的数组中返回选定的元素。
   -->  参数值为负数(从数组的最后一个元素数起)
   Array.splice()
   1.当splice(a) 一个值的时候是:
   var str = [0,1,2,3,4,5,6,7,8,9];
   var arr = str.splice(4) --> str:[0,1,2,3],arr:[4,5,6,7,8,9] 
   2.当splice(a,b) 二个值的时候是: b--> 个数
   var arr = str.splice(1,2) --> str:[0,3,4,5,6,7,8,9],arr:[1,2]
   3.当splice(a,b,c) 三个值的时候是:
   var arr = str.splice(1,0,2) --> 当b为0时,时直接添加2,b为1时,删除一个在添加2;
   ---------
   String.slice(start,end)  -->start:位置,end:下标 --> 提取字符串的片断，并在新的字符串中返回被提取的部分
   String.concat(stringX,stringX,...,stringX)  --> 方法用于连接两个或多个字符串。
   String.substring(start,stop) --> start 在string的位置; stop:位置   --> 方法用于提取字符串中介于两个指定下标之间的字符。
   String.substr(start,length) --> start:下标 length:字串的数 --> 方法可在字符串中抽取从 start 下标开始的指定数目的字符
   ---
   String.charAt(index) ---> 方法可返回指定位置的字符
   String.split(separator,howmany)  --> 方法用于把一个字符串分割成字符串数组。
   ---
   array对象方法:http://www.w3school.com.cn/jsref/jsref_obj_array.asp
   string对象方法:http://www.w3school.com.cn/jsref/jsref_obj_string.asp
25.push,pop,unshift,shift
   push(向后添加),unshift(向头部添加),pop(删除后的),shift(删除头部的)
26.toString() -->将数组转化成字符串
   var str = [{name:"san"},true,5,[1,2],"字符串",null,undefined]
   --> "[object Object],true,5,1,2,字符串,," 
   --> toString() 不能把数组中的对象的值转换出来;
   --> join();
正则表达式:RegExp()
1.正则的创建; 
   let arr = new RegExp('s$')
   let arr = /s$/
2.在正则表达式中,具有特殊的含义的符号
   ^ $ . * + ? = ! : | \ / {} [] () 
   ^ --> 否定字符类
   [] --> 匹配里面的值
   ? --> 非贪婪的重复
   | --> 分割(选择,匹配左或右边)
   search()
   replace() -->替换
   match()
   split()
JavaScript数据类型间的转换:
    1.显示转换:
    1.1把一个值强制转换成布尔值: --> ! --> false  !! --> true;
    1.2把数字-->字符串的转换
    --> String()
    2.字符串转数字
    2.1Number();
    2.2parseInt() --> 整数
    2.2parseFloat() --> 整数,浮点数
    3.引用,基本类型
JavaScript在Web上展示;
    1.window对象的引用:window和self
    -->alert(),confirm()和prompt()获取用户的响应
    -->close() 关闭窗口
    -->open() 打开新窗口 --> 4个参数,默认是false,改为true能替换当前浏览器
    2.每个window对象都包含一个document属性 -->document属性都有一个forms[]数组 -->每个from对象都有一个elements[]数组(数组包含出现在表单种的各种HTML表单元素)
    3.window对象还包含一个frames[]数组--->每个窗口种还含有一个parent属性
    4.事件驱动
    5.JS延迟加载  
    --> defer 属性
    --> async 属性
    --> 动态创建DOM属性
    --> 使用jq的getScript()方法--> $.getScript("xx.js",function(){脚本加载完成});
    --> 使用setTimeout 
    --> 放在页面的底部最后加载
    6.html语义化:正确的标签做正确的事请,能够便于开发者阅读和写出优雅的代码,同时让网络爬虫很好的理解
    -->网络爬虫:按照一定的规则,自动的抓取万维网信息的程序或脚本
    -->SEO优化
    7.setTimeout(),clearTimeout()
    8.navigator对象(包含web浏览器总体信息)
    9.screen对象(提供用户显示器的大小,可以颜色数量的信息)
    10.location对象(文档的URl)
JavaScript的Document对象
    表单:
    type="hidden" --->隐藏
    cookie:
    生存期,可见性,安全性,
Jq的学习:
    加载:window.onload  $(()=>{}) --> onload 慢于 ready;
  :has(selector)  --> 匹配含有选择器所匹配的元素的元素  $("div:has(p)");
    从新定义jq --> jQuery.noConflict();  --> var $jq = jQuery.noConflict();
  CSS选择器找到元素添加样式,jQuery选择找到元素添加行为;
  jQuery对象转DOM对象
  var domObj = $("div:has(p)")[0];
  var domObj = $("div:has(p)").get(0);
  find() -->在元素内匹配元素;

  attr()  -->获取属性,设置属性		-->设置类名时,会把本身自带的覆盖掉
  removeAttr()	-->删除属性

  addClass()	-->添加类名					-->设置类名时,会追加类名
  removeClass() -->删除类名
  
  toggleClass()	-->添加,删除类名  -->当类名存在时,即为删除,反之,即为添加;
  hasClass()		-->判断是否含有个类名,有返回true,反之false
  
  getAttribute(); --> 获取属性
  setAttribute(); --> 设置属性 
  append()	-->向每个匹配的元素内部追加内容
  after()	 --> 在每个匹配元素之后插入内容
  before()	-->向每个匹配的元素之前插入内容
  remove() --> 删除(所包含的所有后代节点将同时被删除)
  detach()	--> 删除(但是保留所有的事件,附加的数据)
  empty()		-->清空节点();
  clone()   -->复制clone(true)复制绑定事件
  
  html()	类似于--> innerHTML
  text()	类似于--> innerText
  val()		类似于--> value
  focus()	类似于--> onfocus()
  blur()	类似于--> onblur()
  this.defaultValue --> <input value="defaultValue"> -->默认的value值(注意this,指向当前的文本框);
  
    遍历:
  	children() -->获取匹配元素的所有子元素的个数(值考虑子元素);
  	next()		-->取得匹配元素后面紧邻的同辈元素
  	prev()		-->取得匹配元素前面紧邻的同辈元素
  	siblings()	-->取得匹配元素前后所有的同辈元素
  	parent()		-->取得匹配元素的父级元素
  	parents()		-->取得匹配元素的祖先元素(一直找到最处的节点);
  	
  DOM操作 --> 获取,设置
  	css();
  	height();
  	width();
  	offset()		-->获取元素在当前视窗的相对偏移
  	position()	-->获取元素相对对于最近一个position样式属性设置为relative,absolute的祖父节点的相对偏移
  	scrollTop(),scrollLeft()	--> 获取元素的滚动条距顶端,距左侧的距离
  	is(":visible")
  	is(":hidden")
  	
  Jquery对于事件
  	$(document) --> $() -->不带参数时,默认参数时document
  	事件绑定bind():
  	hover(enter,leave) -->enter光标移动到元素上触发,光标移除元素触发leave
  	toggle(fn1,fn2...fnN); -->模拟连续单击事件
    toggleClass() --> 添加,取消类名
  	事项冒泡:
      会按照DOM的层次结构想水泡一样不断向上直至顶端(冒泡事件从下到上);事项捕获(是从上到下)
    阻止事件冒泡 , 阻止默认行为(a链接跳转,表单提交)
      event --> event.stopPropagation();
      event --> event.preventDefault();
      return false;
      event.pageX,event.pageY --> 获取光标相对于页面的x,y坐标
      event.which --> 搭配mousedown使用,获取鼠标的左,中,右键;
                  --> 搭配keyup使用,获取键盘按键数
      event.metaKey -->获取ctrl按键
    移除事件
      unbind() --> 移除所有事件 -->多个移除 unbind().unbind()...
      bind('click.plugin') --> 移除多个 --> unbind(".plugin") !!! 点.
      trigger(click!)  --> 不执行.plugin的click 
      unbind(type,函数名) -->移除指定的函数 type --> click...
      One() --> 只执行一次,之后就会销毁
      off() --> 通常用于移除通过on()方法添加的事件处理程序 bind()也可以移除
    模拟操作
      trigger(type)  --> 直接执行事件
      trigger(type,[data]) --> [data]要传递给事件处理函数的附加数据(多个事件,以第一个为主,附加数据先加载);
      triggerHandler() -->不会执行浏览器默认操作
      bind() --> 绑定两个事件,先执行第一个,后执行第二个
      delegate() --> 为指定的元素,添加一个或者多个事件
      undelegate()	-->删除使用delegate()
    动画
      hide() --> display:none
      show() --> display:block --> show("slow") --动画600毫秒，normal,fast 400毫秒，200毫秒 -->show(1000);
      fadeln() 
      fadeOut() --> 改变透明度
      slideUp()
      slideDown() -->改变高度
      fadeTo(600,0.2) --> 只改变不透明度
      animate() --> 自定义动画
      animate({left:"-=500px"},10000) --> -=,+=左,右 默认为右
      animate({left:"-=500px"},10000，function(){}) --> -=,+=左,右 默认为右 -->function() 回调函数
      animate().animate()
      stop(bool,bool) -->停止动画
      is(":animated") --> 判断动画
      delay(time) --> 延迟动画
      tiggle() --> 隐藏就显示,显示就隐藏  
      slideToggle()	--> 隐藏就显示,显示就隐藏,改变高度
      fadeToggle() --> 隐藏就显示,显示就隐藏,改变透明度
    

```