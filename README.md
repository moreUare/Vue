# Vue
Some knowledge points of Vue
## Prop类型
以字符串数组形式列出Prop
```props: ['title', 'likes', 'isPublished', 'commentIds', 'author']```
但是，通常你希望每个prop都有指定的**值类型**。这时，你可以以对象形式列出prop，这些属性的名称和值分别是prop各自的名称和类型
	props: {
		title: String,
		likes: Number,
		isPublished: Boolean,
		commentIds: Array,
		author: Object
	}
## 传递静态或动态Prop
给prop传递一个静态的值
```<blog-post title="My journey with Vue"></blog-post>```
亦可以通过v-bind动态复制
```
	<!-- 动态赋予一个变量的值 -->
	<blog-post v-bind:title="post.title"></blog-post>
	
	<!-- 动态赋予一个复杂表达式的值 -->
	<blog-post v-bind:title="post.title + 'by' + post.author.name"></blog-post>
```
## 传入一个数字
```
	<!-- 即便`42`是静态的，但我们想要要传递的是number型而不是string类型，因此我们需要使用`v-bind` -->
	<blog-post v-bind:likes="42"></blog-post>
	
	<!-- 也可以通过一个变量来赋值 -->
	<blog-post v-bind:likes="post.likes"></blog-post>
```