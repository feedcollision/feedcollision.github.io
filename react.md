react被设计为单独的view层的框架，优点在于可以拓展到RN等其他平台进行渲染，然而同时导致其并非一个完整的前端框架
其本身能够和其他控制数据model的框架进行组合，比如和backbone进行组合。
backbone本身的view层也属于比较薄的一层架构，那么使用其的model层和collection层控制状态，就可以构建一个基准的mvc框架。

这样将collection的增删改查触发react的forceUpdate，感觉过于暴力。
而点击事件等通过直接更改组件本身对应传入的collection的内容，完成事件到页面视图的更新。
需要注意的是
由于react只比较render后返回的虚拟dom中的dom内容，在某些情况下直接打印state中的内容将会导致页面不更新，此行为比较特殊，需要单独编写组件的shouldComponentUpdate(nextProps, nextState)生命周期来进行控制，只有强行返回true才会更新到页面。


回过头来看vdom，其本质是一个可以带children的js对象，在普通对象上增加_isReactElement这一属性既可作为常规的react组件挂载。
在原始的react组件上，_isReactElement这一属性是挂在__proto__上，作为其从react继承的内容。All React elements require an additional $$typeof: Symbol.for('react.element') field declared on the object for security reasons.
https://github.com/facebook/react/pull/4832  这个论述还需要看看。


vdom的更新遵从以下原则，从而将更新算法简化为O(n)的算法
1.两个不同类型的元素将产生不同的树。
2.通过渲染器附带key属性，开发者可以示意哪些子元素可能是稳定的。

那么，如果更新时直接更改了元素的类型，那么其下方叶节点等的树都需要重新更新。
当渲染时附带了专门唯一的key时，可以帮助react识别出需要更新与不需要更新的内容，从而删除掉不需要的dom。
这就要求key的稳定唯一，本身react就是默认使用index来作为key的，然而index基本在更新时不起决定作用。
所以用index做key是没有必要的。


当不同组件有相同的功能，可以使用mixins，类似于切片，然而这一功能不能和class共存，并且不推荐在ES6中使用这一功能。
createReactClass({

mixins:[setSomeThing]
})
如果一个组件有多个混入，且其中几个混入中定义了相同的生命周期方法（比如都会在组件被摧毁的时候执行），那么这些生命周期方法是一定会被调用的。通过混入定义的方法，执行顺序也与定义时的顺序一致，且会在组件上的方法执行之后再执行。
这就导致了组件的在某些生命周期中的执行先后顺序，在抽象mixins方法时需要注意。



在JSX中绑定事件，形如<button onClick={(e) => this.handleClick(e)}>，在每次渲染时都会生成不同的回调，在高频次渲染或者将其作为props传入低阶的组件时时会有性能问题。然而当点击事件需要传递参数时，这又是不可避免的，形如onClick={this.deleteRow.bind(this, id)，通过bind传递参数时，e将作为最后的参数，在传递的参数的后面，即偏函数
