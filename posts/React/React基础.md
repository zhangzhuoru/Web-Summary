# React基础 #

## React-JSX ##

**所谓的 `JSX` 其实就是 `JavaScript` 对象。**

从`JSX`到页面经历的过程：

![](https://i.imgur.com/dBOd580.png)

**总结：**

1. `JSX` 是 `JavaScript` 语言的一种语法扩展；
2. `React.js` 可以用 `JSX` 来描述你的组件；
3. `JSX` 在编译的时候会变成相应的 `JavaScript` 对象描述；
4. `react-dom` 负责把这个用来描述 `UI` 信息的 `JavaScript` 对象变成 `DOM` 元素，并且渲染到页面上。

## 组件的 `render` 方法 ##

一个组件类必须要实现一个 `render` 方法，这个 `render` 方法必须要返回一个 `JSX` 元素。必须要用一个外层的 `JSX` 元素把所有内容包裹起来，返回并列多个 `JSX` 元素是不合法的。

**表达式插入**

在 `JSX` 当中你可以插入 `JavaScript` 的表达式，表达式返回的结果会相应地渲染到页面上。表达式用 `{}` 包裹。

`{}` 内可以放任何 `JavaScript` 的代码，包括**变量**、**表达式计算**、**函数执行**等。

表达式插入不仅仅可以用在标签内部，也可以用在标签的属性上。

**条件返回**

可以在 `render` 函数内部根据不同条件返回不同的 `JSX`。

**`JSX` 元素变量**

## 组件的组合、嵌套和组件树 ##

**自定义的组件都必须要用大写字母开头，普通的 `HTML` 标签都用小写字母开头。**

## 事件监听 ##

在 `React.js` 不需要手动调用浏览器原生的 `addEventListener` 进行事件监听。`React.js` 帮我们封装好了一系列的 `on*` 的属性，当你需要为某个元素监听某个事件的时候，只需要简单地给它加上 `on*` 就可以了。而且你不需要考虑不同浏览器兼容性的问题，`React.js` 都帮我们封装好这些细节了。

这些事件属性名都必须要用驼峰命名法。

没有经过特殊处理的话，**这些 `on*` 的事件监听只能用在普通的 `HTML` 的标签上，而不能用在组件标签上**。

**`event` 对象**

`React.js` 中的 `event` 对象并不是浏览器提供的，而是它自己内部所构建的。`React.js` 将浏览器原生的 `event` 对象封装了一下，对外提供统一的 `API` 和属性，这样你就不用考虑不同浏览器的兼容性问题。

**关于事件中的 `this`**

    handleClickOnTitle (e) {
        console.log(this) // => null or undefined
    }
   
上面的 `handleClickOnTitle` 中把 `this` 打印出来，你会看到 `this` 是 `null` 或者 `undefined`。

这是因为 `React.js` 调用你所传给它的方法的时候，并不是通过对象方法的方式调用（`this.handleClickOnTitle`），而是直接通过函数调用 （`handleClickOnTitle`），所以事件监听函数内并不能通过 `this` 获取到实例。    

如果你想在事件函数当中使用当前的实例，你需要手动地将实例方法 `bind` 到当前实例上再传入给 `React.js`。

`bind` 会把实例方法绑定到当前实例上，然后我们再把绑定后的函数传给 `React.js` 的 `onClick` 事件监听。

你也可以在 `bind` 的时候给事件监听函数传入一些参数。

这种 `bind` 模式在 `React.js` 的事件监听当中非常常见，`bind` 不仅可以帮我们把事件监听方法中的 `this` 绑定到当前组件实例上；还可以帮助我们在在渲染列表元素的时候，把列表元素传入事件监听函数当中。

## 组件的 `state` 和 `setState` ##

**state**

一个组件的显示形态是可以由它数据状态和配置参数决定的。

**`setState` 接受对象参数**

`setState` 方法由父类 `Component` 所提供。**当我们调用这个函数的时候，`React.js` 会更新组件的状态 `state` ，并且重新调用 `render` 方法，然后再把 `render` 方法所渲染的最新的内容显示到页面上**。

当我们要改变组件的状态的时候，不能直接用 `this.state = xxx` 这种方式来修改，如果这样做 `React.js` 就没办法知道你修改了组件的状态，它也就没有办法更新页面。所以，一定要使用 `React.js` 提供的 `setState` 方法，它接受一个对象或者函数作为参数。

**`setState` 接受函数参数**

当你调用 `setState` 的时候，**`React.js` 并不会马上修改 `state`**。而是把这个对象放到一个更新队列里面，稍后才会从队列当中把新的状态提取出来合并到 `state` 当中，然后再触发组件更新。

`setState` 的第二种使用方式，可以接受一个函数作为参数。`React.js` 会把上一个 `setState` 的结果传入这个函数，你就可以使用该结果进行运算、操作，然后返回一个对象作为更新 `state` 的对象：

**`setState` 合并**

上面我们进行了三次 `setState`，但是实际上组件只会重新渲染一次，而不是三次；这是因为在 `React.js` 内部会把 `JavaScript` 事件循环中的消息队列的同一个消息中的 `setState` 都进行合并以后再重新渲染组件。

在使用 `React.js` 的时候，并不需要担心多次进行 `setState` 会带来性能问题。

## 配置组件的 `props` ##

如何让组件能适应不同场景下的需求，我们就要让组件具有一定的“**可配置**”性。

`React.js` 的 `props` 就可以帮助我们达到这个效果。每个组件都可以接受一个 `props` 参数，它是一个对象，包含了所有你对这个组件的配置。

从 `render` 函数可以看出来，组件内部是通过 `this.props` 的方式获取到组件的参数的，如果 `this.props` 里面有需要的属性我们就采用相应的属性，没有的话就用默认的属性。

**在使用一个组件的时候，可以把参数放在标签的属性当中，所有的属性都会作为 `props` 对象的键值：**

**默认配置 `defaultProps`**

上面的组件默认配置我们是通过 `||` 操作符来实现。这种需要默认配置的情况在 `React.js` 中非常常见，所以 `React.js` 也提供了一种方式 `defaultProps`，可以方便的做到默认配置。 

`defaultProps` 作为点赞按钮组件的类属性，里面是对 `props` 中各个属性的默认配置。这样我们就不需要判断配置属性是否传进来了：如果没有传进来，会直接使用 `defaultProps` 中的默认属性。 所以可以看到，在 `render` 函数中，我们会直接使用 `this.props` 而不需要再做判断。

**`props` 不可变**

`props` 一旦传入进来就不能改变。

组件的使用者可以主动地通过重新渲染的方式把新的 `props` 传入组件当中，这样这个组件中由 `props` 决定的显示形态也会得到相应的改变。

我们把 `Index` 的 `state` 中的 `likedText` 和 `unlikedText` 传给 `LikeButton` 。`Index` 还有另外一个按钮，点击这个按钮会通过 `setState` 修改 `Index` 的 `state` 中的两个属性。

由于 `setState` 会导致 `Index` 重新渲染，所以 `LikedButton` 会接收到新的 `props`，并且重新渲染，于是它的显示形态也会得到更新。这就是通过重新渲染的方式来传入新的 `props` 从而达到修改 `LikedButton` 显示形态的效果。

**总结**

1. 为了使得组件的可定制性更强，在使用组件的时候，可以在标签上加属性来传入配置参数。
2. 组件可以在内部通过 `this.props` 获取到配置参数，组件可以根据 `props` 的不同来确定自己的显示形态，达到可配置的效果。
3. 可以通过给组件添加类属性 `defaultProps` 来配置默认参数。
4. `props` 一旦传入，你就不可以在组件内部对它进行修改。但是你可以通过父组件主动重新渲染的方式来传入新的 `props`，从而达到更新的效果。

## `state` vs `props` ##

`state` 的主要作用是用于组件保存、控制、修改自己的可变状态。`state` 在组件内部初始化，可以被组件自身修改，而外部不能访问也不能修改。你可以认为 `state` 是一个局部的、只能被组件自身控制的数据源。`state` 中状态可以通过 `this.setState` 方法进行更新，`setState` 会导致组件的重新渲染。

`props` 的主要作用是让使用该组件的父组件可以传入参数来配置该组件。它是外部传进来的配置参数，组件内部无法控制也无法修改。除非外部组件主动传入新的 `props`，否则组件的 `props` 永远保持不变。

`state` 和 `props` 有着千丝万缕的关系。它们都可以决定组件的行为和显示形态。一个组件的 `state` 中的数据可以通过 `props` 传给子组件，一个组件可以使用外部传入的 `props` 来初始化自己的 `state`。但是它们的职责其实非常明晰分明：`state` 是让组件控制自己的状态，`props` 是让外部对组件自己进行配置。

**尽量少地用 `state`，尽量多地用 `props`。**

没有 `state` 的组件叫无状态组件，设置了 `state` 的叫做有状态组件。因为状态会带来管理的复杂性，我们尽量多地写无状态组件，尽量少地写有状态的组件。这样会降低代码维护的难度，也会在一定程度上增强组件的可复用性。

`React.js` 非常鼓励无状态组件，在 `0.14` 版本引入了函数式组件——一种定义不能使用 `state` 组件。

以前一个组件是通过继承 `Component` 来构建，一个子类就是一个组件。而用函数式的组件编写方式是一个函数就是一个组件，你可以和以前一样通过 `<HellWorld />` 使用该组件。不同的是，函数式组件只能接受 `props` 而无法像跟类组件一样可以在 `constructor` 里面初始化 `state`。你可以理解函数式组件就是一种只能接受 `props` 和提供 `render` 方法的类组件。

## 渲染列表数据 ##

### 渲染存放 `JSX` 元素的数组 ###

**如果你往 `{}` 放一个数组，`React.js` 会帮你把数组里面一个个元素罗列并且渲染出来。**

### 使用 `map` 渲染列表数据 ###

这里把负责展示用户数据的 `JSX` 结构抽离成一个组件 `User` ，并且通过 `props` 把 `user` 数据作为组件的配置参数传进去；这样改写 `Index` 就非常清晰了，看一眼就知道负责渲染 `users` 列表，而用的组件是 `User`。

<p class="codepen" data-height="265" data-theme-id="0" data-default-tab="js" data-user="whjin" data-slug-hash="KJgzKP" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="渲染列表数据">
  <span>See the Pen <a href="https://codepen.io/whjin/pen/KJgzKP/">
  渲染列表数据</a> by whjin (<a href="https://codepen.io/whjin">@whjin</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### `key` ###

`React.js` 的是非常高效的，它高效依赖于所谓的 `Virtual-DOM` 策略。简单来说，能复用的话 `React.js` 就会尽量复用，没有必要的话绝对不碰 `DOM`。对于列表元素来说也是这样，但是处理列表元素的复用性会有一个问题：元素可能会在一个列表中改变位置。

现在只需要记住一个简单的规则：**对于用表达式套数组罗列到页面上的元素，都要为每个元素加上 `key` 属性，这个 `key` 必须是每个元素唯一的标识。**一般来说，`key` 的值可以直接后台数据返回的 `id`，因为后台的 `id` 都是唯一的。

    return (
        <div>
            {users.map((user, index) =>
                <User user={user} key={index}/>
            )}
        </div>
    )

记住一点：在实际项目当中，如果你的数据顺序可能发生变化，标准做法是最好是后台数据返回的 `id` 作为列表元素的 `key`。


