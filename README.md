# Vue
Some knowledge points of Vue
v-bind:is=""
## Prop
### Prop类型
以字符串数组形式列出Prop  
```props: ['title', 'likes', 'isPublished', 'commentIds', 'author']```  
但是，通常你希望每个prop都有指定的**值类型**。这时，你可以以对象形式列出prop，这些属性的名称和值分别是prop各自的名称和类型
```	
props: {
	title: String,
	likes: Number,
	isPublished: Boolean,
	commentIds: Array,
	author: Object
}
```
### 传递静态或动态Prop
给prop传递一个静态的值
```<blog-post title="My journey with Vue"></blog-post>```
亦可以通过v-bind动态复制
```
<!-- 动态赋予一个变量的值 -->
<blog-post v-bind:title="post.title"></blog-post>
	
<!-- 动态赋予一个复杂表达式的值 -->
<blog-post v-bind:title="post.title + 'by' + post.author.name"></blog-post>
```
### 传入一个数字
```
<!-- 即便`42`是静态的，但我们想要要传递的是number型而不是string类型，因此我们需要使用`v-bind` -->
<blog-post v-bind:likes="42"></blog-post>
	
<!-- 也可以通过一个变量来赋值 -->
<blog-post v-bind:likes="post.likes"></blog-post>
```
### 传递一个布尔值


### 传递一个数组


### 传入一个对象

### 传入一个对象的所有属性
如果你想要将一个对象的所有属性都作为prop传入,你可以使用不带参数的v-bind(取代v-bind:prop-name).
```
post: {
	id: 1,
	title: 'My Journey with Vue'
}
```
下面的模板
```<blog-post v-bind="post"></blog-post>``` <!-- 试了一下没有效,v-bind:post="post"有效 -->
等价于:
```
<blog-post
	v-bind:id="post.id"
	v-bind:title="post.title"
></blog-post>
```
## 单向数据流
所有的prop都使的其父子prop之间形成了一个单向下行绑定:父级prop的更新会向下流动到子组件中,但反过来则不行,这样防止了子组件意外改变父级组件的状态,从而导致你的应用的数据流向难以理解.
## Prop验证
```
Vue.component('my-component',{
	Props: {
		<!-- [^_^]: # (基础的类型检查(`null`匹配任何类型)) -->
		propA: Number,
		//多个可能的类型
		propB: [String, Number],
		//必填的字符串
		propC: {
			type: String,
			required: true
		},
		//带有默认的数字
		propD: {
			type: Number,
			default: 100
		},
		//带有默认的对象
		propE: {
			type: Object,
			default: function(){
				return {message: 'hello'}
			}
		},
		//自定义验证函数
		propF: {
			validator: function(value){
				//这个值必须匹配下列字符串中的一个
				return ['success', 'warning', 'danger'].indexOf(value) !== -1
			}
		}	
	}
})
```
`type`可以是下列原生构造函数中的一个
- String
- Number
- Boolean
- Array
- Object
- Date
- Function
- Symbol
另外,`type`还可以是一个自定义的构造函数,并且通过instanceof来进行检查确认.例如下列构造函数
```
function Person(firstname, lastname){
	this.firstName = firstName
	this.lastName = lastName
}
```
## 非Prop的特性

## 自定义事件 (***)
### 事件名
推荐使用kebab-case的事件名**HTML大小写不敏感**

### 自定义组件的v-model
```
Vue.component('base-checkbox',{
	model: {
		prop: 'checked',
		event: 'change'
	},
	props: {
		checked: Boolean
	},
	template:`
		<input
			type="checkbox"
			v-bind:checked="checked"
			v-on:change="$emit('change',$event.target.checked)"
		>
	`
})
```
  现在在这个组件上使用v-model的时候:  
```<base-checkbox v-model="lovingVue"></base-checkbox>```
这里的 `lovingVue` 的值将会传入这个名为 `checked` 的 `prop`。同时当 <base-checkbox> 触发一个 `change` 事件并附带一个新的值的时候，这个 `lovingVue` 的属性将会被更新。
### 将原生事件绑定到组件 
可以使用v-on的.native修饰符  
.native -监听组件根元素的原生事件。 主要是给自定义的组件添加原生事件。
.......

### `.sync`修饰符
  我们可能需要对一个prop进行“双向绑定”。但真正的双向数据绑定会带来维护上的问题，因为子组件可以修改父组件，且在父组件和子组件都没有明显的改动来源。
所以推荐以`update:myPropName`的模式触发事件取而代之。
```
this.$emit('update:title', newTitle)
然后父组件可以监听那个事件并根据需要更新一个本地的数据属性。
<text-document
	v-bind:title="doc.title"
	v-on:update:title="doc.title = $event"
></text-document>
为了方便 给这种模式提供一个缩写模式，即`.sync`修饰符
<text-document v-bind:title.sync="doc.title"></text-document>
```
> 注意带有.sync修饰符的v-bind不能和表达式一起（例如v-bind:title.sync="doc.title + '!' "是无效的）。取而代之的是，你只能提供你想要绑定的属性名，类似v-model  
当我们用一个对象同时设置多个prop的时候，也可以将这个.sync修饰符和v-bind配合使用。  
>将v-bind.sync用在一个字面量的对象上，例如`v-bind.sync="{ title: doc.title }"`,是无法正常工作的，因为在解析一个像这样的复杂表达式的时候，有很多边缘情况需要考虑。  

## 插槽
### 具名插槽
`<slot>`元素有一个特殊的特性：name.这个特性可以定义额外的插槽：
假设在一个<base-layout>组件的模板中
```
<div class="container">
	<header>
		<slot name="header"></slot>
	</header>
	<main>
		<slot></slot>
	</main>
	<footer>
		<slot name="footer"></slot>
	</footer>
</div>
在向具名插槽提供内容的时候，我们可以在一个父组件的<template>元素上使用slot特性：
<base-layout>
	<template slot="header">
		<h1>Here might be a page title</h1>
	<template>
	
	<p>A paragraph for the main content.</p>
	<p>And another one.<p>
	
	<template slot="footer">
		<p>Here's some contact info</p>
	</template>
</base-layout>
另一种slot特性的用法是直接在一个普通元素上：
<base-layout>
	<h1 slot="header">Here might be a page title</h1>
	
	<p>A paragraph for the main content.</p>
	<p>And another one.</p>
	
	<p slot="footer">Here's some contact info</p>
</base-layout>
我们可以保留一个未命名插槽，这个插槽是默认插槽。
```
### 插槽的默认内容
有时候为插槽提供默认的内容是很有用的。例如，一个 <submit-button> 组件可能希望这个按钮的默认内容是“Submit”，但是同时允许用户覆写为“Save”、“Upload”或别的内容。
```
<button type="submit">
	<slot>Submit</slot>
</button>
```
如果父组件为这个插槽提供内容，则默认的内容会被替换。
### 编译作用域
父组件模板的所有东西都会在父级作用域内编译；子组件模板的所有东西都会在子级作用域内编译。
### 作用域插槽
我认为作用域插槽的template里就是做了一个循环的模板，在外部调用时，将外部的数据按模板中遍历的方法遍历，然后显示出来,再在子组件中插入东西
作用域插槽最具代表性的用例是`列表组件`
```
<div id="app">
	<my-list :books='books'>
		//`slot-scope`不再限制在<template>元素上使用，而可以用在插槽内的任何元素或组件上。
		<template slot='book' slot-scope='props'>  
			<li>{{props.bookName}}</li>
		</template>
	</my-list>
</app>
<script>
	Vue.component('my-list',{
		prop: {
			books {
				type: Array,
				default: function(){
					return [];
				}
			}
		},
		template:`
		<ul>
			<slot name="book" v-for='book in books' :bookName='book.name'></slot>
		</ul>
		`
	})
	let app = new Vue({
		el: '#app',
		data: {
			books: [
				{name: '<<Vue js 实战>>'},
				{name: '<<JavaScript 高级程序设计>>'},
				{name: '<<深入浅出>>'}
			]
		}
	})
</script>
```
#### 解构slot-scope
如果一个JavaScript表达式在一个函数定义的参数位置有效，那么这个表达式实际上就可以被slot-scope接受。

## 动态组件与异步组件
<component v-bind:is="currentTabComponent"></component>
每次切换新标签的时候，Vue都创建了一个新的currentTabComponent实例。   

重新创建动态组件的行为通常是非常有用的，但是在这个案例中，我们更希望那些标签的组件实例能够被在它们第一次被创建的时候缓存下来。
为了解决这个问题，我们可以用一个<keep-alive>元素将其动态组件包裹起来。
```
<!-- 失去的组件会被缓存 -->
<keep-alive>
	<component v-bind:is="currentTabComponent"></component>
</keep-alive>
```



# Vue中的父组件和子组件
组件通过`prop`属性接受它的父级的数据，那么当前这个组件就是相对的子组件了，提供数据的就是符组件。
```
<div id='app'>
	<my-component message='来自父组件的数据'></my-component>
</div>
<script>
	Vue.component('my-component',{
		props: ['message'],
		template: `<div>{{message}}</div>`
	})
	let app = new Vue({
		el: '#app'
	})
```
**父子组件是相对的，不是绝对的**
子组件：my-component
父组件：app
