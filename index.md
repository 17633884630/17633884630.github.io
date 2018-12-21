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
2.简单的清空对象
   $.each(obj,(k,v)=>{
     obj[k] = null
   })
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
        我在子组件中呀:
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
19.2 构造函数
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
19.5
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
20.
```