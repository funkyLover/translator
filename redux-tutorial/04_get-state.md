# get state

我们要怎么在redux实例中拿到state?

```js
import { createStore } from 'redux'

var reducer_0 = function (state, action) {
    console.log('reducer_0 was called with state', state, 'and action', action)
}

var store_0 = createStore(reducer_0)
// 输出: reducer_0 was called with state undefined and action { type: '@@redux/INIT' }

// To get the state that Redux is holding for us, you call getState
// 可以调用`getState`来获取被存放在redux实例中的`state`

console.log('store_0 state after initialization:', store_0.getState())
// 输出: store_0 state after initialization: undefined

// 应用的`state`在初始化后依然是"undefined"?这当然了,因为`reducer`还没有任何操作,
// 记得我们在 `02_about-state-and-meet-redux`中是如何描述的`reducer`的吗?
// `reducer`其实就是一个接受当前应用的`state`和`action`,而后返回一个经改动的新的`state`的函数
// 我们的reducer还没有返回任何东西,而应用的state就是由reducer()返回的,所以依然是"undefined"

// 现在让我们试试当作为参数传入reducer的state为"undefined"时,返回一个state的初始值给应用

var reducer_1 = function (state, action) {
    console.log('reducer_1 was called with state', state, 'and action', action)
    if (typeof state === 'undefined') {
        return {}
    }

    return state;
}

var store_1 = createStore(reducer_1)
// 输出: reducer_1 was called with state undefined and action { type: '@@redux/INIT' }

console.log('store_1 state after initialization:', store_1.getState())
// 输出: store_1 state after initialization: {}

// 正如我们所期望的,在初始化之后state的值为{}

// 多亏了ES6,有一种更加清晰的做法来实现上述模式
var reducer_2 = function (state = {}, action) {
    console.log('reducer_2 was called with state', state, 'and action', action)

    return state;
}

var store_2 = createStore(reducer_2)
// 输出: reducer_2 was called with state {} and action { type: '@@redux/INIT' }

console.log('store_2 state after initialization:', store_2.getState())
// 输出: store_2 state after initialization: {}

// 或许你还发现了,当我们给reducer_2的state参数设置默认值后,reducer的函数体内state将不会在是"undefined"

// 现在我们回想起来reducer只会为响应被触发的action而调用,
// 让我们假设为了响应type为'SAY_SOMETHING'的action,会对state做以下改动

var reducer_3 = function (state = {}, action) {
    console.log('reducer_3 was called with state', state, 'and action', action)

    switch (action.type) {
        case 'SAY_SOMETHING':
            return {
                ...state,
                message: action.value
            }
        default:
            return state;
    }
}

var store_3 = createStore(reducer_3)
// 输出: reducer_3 was called with state {} and action { type: '@@redux/INIT' }

console.log('store_3 state after initialization:', store_3.getState())
// 输出: redux state after initialization: {}

```

在最后一例中state依然没有任何变化,因为我们还没有触发action.但是在最后一例中还有几个需要留意的点:

0. 我假设action对象包含一个type和value属性,其中type属性更多的是flux架构中action对象约定俗成的部分,value属性则是其他数据
1. 你将会频繁的看到这种通过使用switch来对reducer接收到的action进行适当的响应
2. 使用switch时,千万要记得要有"default: return state",如果不这样的话,reducer就有可能会返回undefined(以及丢失state)
3. 注意我们是如何返回了一个合并了当前state和{ message: action.value }的新的state,这都要归功于惊人的ES7特性(Object Spread): { ...state, message: action.value }
4. 也要记住这个ES7特性(Object Spread)适用于上例是因为其背后在state中对{ message: action.value }进行了浅拷贝(这意味为着state的第一层属性因为{ message: action.value }的缘故被完全重写,而不是优雅的合并).但是如果是要操作一个复杂的的数据结构的话,你或许需要选择其他不同的方式去更新你的state
  - 使用[Immutable.js]((https://facebook.github.io/immutable-js/)
  - 使用[Object.assign](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
  - 手动进行合并
  - 或其他任何符合你需求及适合你应用中的state的结构的策略,因为redux并没有这方面的要求(记住,redux只是一个`state container`).

现在我们可以开始在reducer中响应action了,接下来让我们聊聊有多个不同reducer的情况及如何把它们合并起来.

前往下一章节:[05_combine-reducers](https://github.com/funkyLover/article/blob/master/redux-tutorial/05_combine-reducers.md)
