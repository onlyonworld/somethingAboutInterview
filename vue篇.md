### 0、Vue和React的区别 

 （1）监听数据变化的实现原理不同：Vue通过 getter/setter以及一些函数的劫持，能精确知道数据变化；React默认是通过比较引用的方式（diff）进行的，如果不优化可能导致大量不必要的VDOM的重新渲染。React不精确监听数据变化。 

 （2）数据流的不同：Vue1.0中可以实现两种双向绑定；React一直不支持双向绑定，提倡的是单向数据流。 

 （3）组件通信的区别：React 本身并不支持自定义事件，而Vue中子组件向父组件传递消息有两种方式：事件和回调函数，但Vue更倾向于使用事件。在React中我们都是使用回调函数的。 

 （4）模板渲染方式的不同：在表层上，模板的语法不同，React是通过JSX渲染模板。而Vue是通过一种拓展的HTML语法进行渲染，但其实这只是表面现象，毕竟React并不必须依赖JSX；在深层上，模板的原理不同。React是在组件JS代码中，通过原生JS实现模板中的常见语法，比如插值，条件，循环等，都是通过JS语法实现的，更加纯粹更加原生。而Vue是在和组件JS代码分离的单独的模板中，通过指令来实现的，比如条件语句就需要 v-if 来实现对这一点，这样的做法显得有些独特，会把HTML弄得很乱

### 1、谈谈你对MVVM开发模式的理解

​	MVVM分为Model、View、ViewMode三者

​	■Model: 代表数据模型，数据和业务逻辑都是在Model层中定义

​	■View: 代表UI视图，负责对数据的展示

​	■ViewModel: 负责监听Model中数据的改变并控制视图的更新，处理用户交互操作

​	Model和View并无直接关联，而是通过ViewModel来进行联系的，ModeI和ViewModel之间有着双向数据绑定的联系。因此当Model中的数据改变时会触发View层的刷新，View中由于用户交互操作而改变的数据也会在Model中同步。

​	这种模式实现了ModeI和View的数据自动同步，因此开发者只需要专注对数据的维护操作即可，而不需要自己操作dom.

### 2、对于组件通信你了解多少，请描述一下你是怎么完成组件的通信的

​	1）父组件向子组件传值，父组件通过数据绑定，子组件通过props进行接收

​	2）子组件向父组件传值，子组件通过this.$emit，传递一个事件还有需要传给父组件的值，父组件通过触发子组件传过来的事件，可以接收到子组件传过来的值

​	3）非父子之间的传值建立一个空实例进行传值，中央事件总线机制。例如建立一个bus.js空实例，在需要传值的组件中去触发

```js
//原理上就是建立一个公共的js文件， 专门用来传递消息
// bus.js 
import Vue from 'vue'
export default new Vue;

//在需要传递消息的地方引入
import bus from ' . /bus.js '

//传递消息
bus.$emit( 'msg', val) 

//接受消息
bus.$emit( 'msg', val => {
	console.log(val )
})

```

​	4）祖孙之间的传值

​		a.可以利用provide inject模式，在一个组件中通过provide 给孙子组件传值，孙子组件通过inject进行接收，这个模式是在vue2.5版本之后才开始有的

​		b.可以通过 $attrs 传值，借助中间组件的协助

```vue
<!--祖组件：通过属性绑定将值传给父组件，其中a、b、c为data中的数据-->
<parent :msga="a" :msgb="b" :msgc="c"></parent>
<!--父组件：通过v-bind="$attrs" (这个v-bind不能简写为:)，将祖组件的值传给子组件-->
<son v-bind="$attrs"></son>
<!--孙子组件：通过this.$attrs接收这些值-->
console.log(this.$attrs.msga) ;
```

VUEX可以处理上述的每一个情况，通过建立一个数据仓库，可以让数据共享到每一个组件

### 3、v-if和v-show的区别

1）v-if是动态的向DOM树内添加或者删除DOM元素，v-show是通过设置DOM元素的display样式属性控制显示或者隐藏

2）编译过程中，v-if是惰性的，如果一开始条件为false，则什么都不做，即不进行编译，到将其条件切换为true时，才进行编译，如果在变为true之后又切换为false，那么这时候需要进行局部卸载；而v-show则不管初始条件是否为true都进行编译，然后缓存起来，而且DOM元素被保留，所以v-if有更高的切换消耗，而v-show有更高的初始渲染消耗

### 4、关于单页应用首屏加载速度慢，出现白屏时间过长问题你怎么处理

​	1）将公用的JS库通过script标签在index.html进行外部引入，减少我们打包出来的js文件的大小，让浏览器并行下载资源文件，提高下载速度

​	2）在配置路由的时候进行路由的懒加载，在调用到该路由时再加载次路由相对应的js文件

​	3）加一个首屏loading图或骨架屏，提高用户的体验

​	4）尽可能使用CSS Sprites和字体图标库

​	5）图片的懒加载等

### 6、关于修改了数据，视图不更新的理解和处理方式

​	Vue中给data中的对象属性添加一个新的属性时会发生什么

​	经过打印发现数据是已经改变了，但是由于在Vue实例创建时，新添加的属性并未声明，因此就没有被Vue转换为响应式的属性，自然就不会触发视图的更新，这时就需要使用Vue的全局api  --> set()
​	$set()使用方法： $set(需要修改的对象, “对象的属性", 值)

注意：在$set()中对象属性需要双引号包起来

```html
<div id="app">
        <ul>
            <li v-for="(item, index) in obj" :key="index">{{item}}</li>
        </ul>
        <button @click="addAttr1">添加属性d</button>
        <button @click="addAttr2">添加属性e</button>
</div>
<script>
    var vm = new Vue({
       el:'#app',
       data:{
           obj: {
               'a': 'I am a',
               'b': 'I am b',
               'c': 'I am c'
           }
       },
       methods:{
           addAttr1(){
               this.obj.d = 'I am d'
               console.log(this.obj)
               /*
                   此时在页面打印出来的依旧只有：
                   I am a 
                   I am b
                   I am c
                   
                   而此时控制打印出来的有：
                   a: "I am a"
                   b: "I am b"
                   c: "I am c"
                   d: "I am d"  
               */
           },
           addAttr2(){
               this.$set(this.obj, 'e', 'I am e')
               console.log(this.obj)
               /*
                   此时在页面打印出来的依旧只有：
                   I am a 
                   I am b
                   I am c
                   I am e

                   而此时控制打印出来的有：
                   a: "I am a"
                   b: "I am b"
                   c: "I am c"
                   e: "I am e"  
               */
           }
       }
    });
</script>
```

### 7、在Vue中如何做数据的监听

​	1）watch里面监听

​		■第一种写法：监听简单数据类型

```vue
watch:{
	obj(newval,oldval){
		console.log(newval,oldval)
	},
}
```

​		■第二种写法可设置deep为true对数据进行深层遍历监听，如果监听的是复杂数组类型例如数组、对象等

```vue
watch:{
	obj:{
		handler(newval,oldval){
			console.log(222)
			console.log(newval,oldval)
		}，
		deep: true
	}
}
```

​	2）computed里面监听

computed里面的依赖改变时，所计算的属性或作出事实的改变

### 8、watch中的deep：true是如何实现的

​        当用户指定了`watch`中的deep属性为`true`时，如果当前监控的值是数组或者对象类型，会对数组或对象中的每一项进行求值，此时会将当前`watcher`存入到对应属性的依赖中，这样数组中对象发生变化时也会通知数据更新

### 9、为何vue采用异步渲染

​        因为如果不采用异步更新，那么每次更新数据都会对当前组件进行重新渲染，如果数据更新次数多性能会很差。所以为了**性能考虑**，Vue会在本轮数据更新完成后，再去异步更新视图，即只根据最后更新完的新数据进行渲染

### 10、nextTick实现原理

<img src="D:\typora笔记\面试必看\面试复习题.assets\image-20201202213443227.png" alt="image-20201202213443227" style="zoom:50%;" />

### 11、vue中computed的特点

​        计算属性在引用的时候，不要加()去调用，当成普通的变量用就好；        

​        计算属性的求值结果，会被缓存起来，方便下次继续使用；如果计算属性方法中，所依赖的任何数据，都没有发生过变化，则不会重新对计算属性求值。

​        只要计算属性这个function内部所用到的data中的数据发生了变化，就会立即重新计算这个计算属性的值

### 12、vue组件的生命周期

<img src="D:\typora笔记\面试必看\面试复习题.assets\image-20201202222003888.png" alt="image-20201202222003888" style="zoom:50%;" />

<img src="D:\typora笔记\面试必看\面试复习题.assets\image-20201202222103448.png" alt="image-20201202222103448" style="zoom:50%;" />

vue的生命周期函数可以八部分：beforeCreate、created、beforeMount、mounted、beforeUpdate、updated、beforeDestroy、destroyed

#### 	beforeCreate

​	在beforeCreate生命周期函数执行的时候, data和methods等等中的数据都还没有没初始化，所以此时调用这些数据，只会显示undefined

#### 	created

​	在created中, data和methods都已经被初始化好了！如果要调用methods中的方法，或者操作data中的数据，最早,只能在created中操作

#### 	beforeMount

​	此时模板已经在内存中编辑完成了,但是尚末把模板渲染到页面中，在beforeMount 执行的时候，页面中的元素，还没有被真正替换过来,只是之前写的一些模板字符串

#### 	mounted

​	此时内存中的模板,已经真实的挂载到了页面中,用户已经可以看到渲染好的页面了，mounted是实例创建期间的最后一个生命周期函数,当执行完mounted 就表示,实例已经被完全创建好了，此时如果没有其它操作的话，这个实例就一直存在我们的内存中

beforeUpdate和updated只有在数据被修改的时候才会触发

#### 	beforeUpdate

​	当执行beforeUpdate 的时候，页面中的显示的数据还是旧的，此时data 数据是最新的，页面尚未和最新的数据保持同步

#### 	updated

​	updated执行后，页面和data 数据已经保持同步了，都是最新的

beforeDestroy和destroyed被执行时表示vue实例已经进入销毁阶段，可以在控制台上执行vm.$destroy()

#### 	beforeDestroy

​	销毁之前执行，当beforeDestroy函数执行时，表示vue实例已从运行阶段进入销毁阶段，vue实例身上所有的方法与数据都处于可用状态

#### 	destroyed

​	当destroyed函数执行时，组件中所有的方法与数据已经被完全销毁，不可用

### 13、Vue路由守卫有哪些，怎么设置，使用场景等

```javascript
常用的两个路由守卫：router.beforeEach 和 router.afterEach

每个守卫方法接收三个参数：
	to: Route: 即将要进入的目标 路由对象
	from: Route: 当前导航正要离开的路由
	next: Function: 一定要调用该方法来 resolve 这个钩子。

在项目中，一般在beforeEach这个钩子函数中进行路由跳转的一些信息判断。判断是否登录，是否拿到对应的路由权限等等。
```

### 14、vue的双向绑定原理

采用数据劫持，结合发布者-订阅者模式的方式，通过`Object.defineProperty()`来劫持各个属性的`setter`，`getter`，在数据变动时发布消息给订阅者，触发相应的监听回调。

以下是详解：

<img src="D:\typora笔记\面试必看\面试复习题.assets\image-20201202202523885.png" alt="image-20201202202523885" style="zoom:50%;" />

在vue初始化的时候，会调用一个方法`initData`，通过`vm.$options.data`能够获取到用户传入的数据，然后对数据进行观测；

调用`Observer`方法，将数据传给它，然后判断数据是否被观察过了，如果没有被观测过，就`new`一个`Observer`，对该数据进行观测；

观测时分两种情况：

1、观测的数据是对象(非数组)，则调用`walk`方法；在该方法中，遍历对象属性，使用`defineReactive`定义响应式变化，在`defineReactive`中使用`Object.defineProperty`重新定义数据，当用户获取数据时会调用`get`方法，调用时会进行依赖收集，即收集当前的`watcher`；当数据改变时，触发`set`，此时先判断当前的`value`和新的`value`是否一致，不一致的话调用`notify`方法，触发数据对应的依赖进行更新。

2、如果观测的对象时数组，则先判断当前是否支持原型链，如果支持，将数组的`_proto_`指向我们改写的数组原型方法`arrayMethods`，这个方法主要是拦截数组的七个会改变自身数值的方法，(`push`、`pop`、`shift`、`unshift`、`splice`、`sort`、`reverse`)，当用户调用数组的这些方法时，首先还是会执行原生的方法，还会通知视图更新。如果有新增的数据，还需要对新增的内容进行观测(`observeArray`)，同时还需要对每个数组元素进行观测

<img src="D:\typora笔记\面试必看\面试复习题.assets\image-20201202211953545.png" alt="image-20201202211953545" style="zoom:50%;" />

### 15、如果对vuex里的state状态改变是非常敏感的,可以怎么做来监听状态的改变

在引用vuex的state的组件中，使用computed计算属性定义一个变量，return Vuex中的state，然后使用watch对变量进行监听，如果state发生变化，watch就可以监听到。

### 16、mutations和actions的区别

mutations 必须是同步操作，action则可以是异步操作

action通过调用mutations的方法改变store中的值

```javascript
调用mutations的方法： this.$store.commit('function', 'param')
调用action的方法： this.$store.dispatch('function')
```

### 17、vue中mixin是什么，mixin冲突怎么解决

mixin文件是一个对象，可以包含vue组件的任意成分。是分发Vue组件可复用功能的非常灵活的方式，当mixin被组件使用时，所有minxin里的属性/方法会与组件里的属性/方法混合。

在Vue组件中可以有mixins属性，该属性值类型为数组。将mixin引入，作为mixins数组的元素mixins: [mixin]

当mixin中的数据、方法或任何组件选项与组件中的选项具有相同的名称时，可能会发生组件与其mixin之间的命名冲突。如果发生这种情况，则组件本身的属性将优先。

### 18、vue中使用v-for的key值问题

#### 为什么需要key

**主要是用来提供给DOM的diff算法，因为diff算法会比较节点上的key，如果新的节点在旧的节点中已经存在，即节点的标签和key都相同，但位置发生改变，则将元素复用并进行移动，而不会重新创建或者删除节点；**

举个栗子：有ABCD四个旧节点，经过一些操作后变成DABC，此时先将新节点中的第一个节点D取出来和旧节点进行对比，发现在旧节点中有一个节点的key和D相同，那就复用旧节点的D，并将旧节点的D移动到最前面，而不需要去创建新节点；

**如果没有key值，那么diff算法在比较节点时，就暴力地按照位置进行比对；**

举个栗子：有ABCD四个旧节点，经过一些操作后变成DABC，此时因为没有key值，所以直接拿第一个新节点D与旧节点A对比，如果新节点D与旧节点A的标签一样，但内容不一样，则会操作DOM，将旧节点A修改为新节点D；然后拿第二个新节点A与旧节点B进行比较，如果新节点A与旧节点B标签不一样，则删除旧节点B，新建节点A，以此类推。

**所以key值可以提供性能，减少操作DOM节点**

#### 为什么最好使用列表项的id作为key值而不是用index

使用列表项的id作为key值，当列表项发生删除或者增加时，各个列表项的key不会被重新渲染，如果使用index作为列表项的key，当列表项发生删除或者增加时，增加或者删除的列表项后面key都要被重新渲染一遍，所以使用列表项的id作为key值相比于使用index作为列表项的key值，性能有所提升。

举个栗子：有ABCD四个旧节点，删除节点B之后变成ACD，此时如果以index作为key值，那么新节点CD节点的key要从原来的2、3变为1、2，同时，diff算法比较时，会将旧节点的B/C与新节点C/D进行比较，发现旧节点的B(C)与新节点的C(D)不一样，就会操作DOM，将旧节点B(C)改为新节点的C(D)。最后旧节点的末尾还有一个D，直接删除；如果是以列表项id作为key值，那么新节点ACD都可以在旧节点中找到与之具有相同key值的节点，比较第二个和第三个新节点时，旧节点CD会被依次移动到A后面，此时B被挤到最后面，比较完成后直接被删除。

### 19、何时需要使用beforeDestory

<img src="D:\typora笔记\面试必看\面试复习题.assets\image-20201202222739183.png" alt="image-20201202222739183" style="zoom:50%;" />

### 20、为什么v-for与v-if不能连用

因为v-for的优先级高于v-if。如果同时有v-for和v-if，那么先执行v-for，将列表项全部渲染出来，再给每一个列表项添加v-if = false，这么做性能低下，做了许多无用功。

### 21、虚拟DOM

 		虚拟DOM其实是一个js对象，是使用js对象来模拟DOM，如果在一次操作中有多次对DOM节点的更新，每次都立即更新视图会很影响性能，所以虚拟DOM不会立即去操作DOM，而是将多次更新的diff内容保存到本地一个JS对象中，最终将这个JS对象一次性更新到DOM树上，再进行后续操作，避免大量无谓的计算量。

### 22、keep-alive的作用

 keep-alive用来缓存组件，避免多次加载相应的组件，减少性能消耗。

举个栗子：从页面1跳转到页面2后，从页面2回退到页面1时不用在重新执行页面1的代码，只会从缓存中加载之前已经缓存的页面1，不会触发页面1中的created钩子函数，这样可以减少加载时间及性能消耗，提高用户体验性。

**什么时候使用keep-alive**

如果需要频繁切换路由，这个时候就可以考虑用keep-alive了，来达到避免数据的重复请求的目的。

<img src="D:\typora笔记\面试必看\面试复习题.assets\image-20201211105906824.png" alt="image-20201211105906824" style="zoom:50%;" />