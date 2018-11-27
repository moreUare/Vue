# Vue
Some knowledge points of Vue
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