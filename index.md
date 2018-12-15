## 日常知识点记录
```markdown
1.template.default.import.funName = function(value){...}  
  funName使用 {|分隔符}
2.var obj = {}
  obj['id_'+ id] = data.id	(存)
  let id = obj['id_'+ id]   (取)
```
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

### 项目记录
```markdown

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
### vue研究记录
```markdown
1.vue跳转传参问题:
  this.$router.push({name:'/',params:{data:data}})  //链接上不显示
  this.$router.push({path:'/',query:{data:data}})   //链接显示
  this.$router.push({name:'/',params:{data:data},query:{data:data}})   //链接显示query.data
```
### 对于JavaScript学习


### 问题反馈
这是在开发中遇到,和日常的记录,如果有错误的地方,希望得到您的指点,
邮箱17633884630@163.com
您的不经意的指点就是我的学习的方向;

