
# combine reducers

我们已经开始掌握`reducer`的个中深意..

```js
var reducer_0 = function (state = {}, action) {
    console.log('reducer_0 was called with state', state, 'and action', action)

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

// 但在我们进行下一步前,我们先来看看当我们要处理10种不同的action时我们的reducer会是什么一个样子

var reducer_1 = function (state = {}, action) {
    console.log('reducer_1 was called with state', state, 'and action', action)

    switch (action.type) {
        case 'SAY_SOMETHING':
            return {
                ...state,
                message: action.value
            }
        case 'DO_SOMETHING':
            // ...
        case 'LEARN_SOMETHING':
            // ...
        case 'HEAR_SOMETHING':
            // ...
        case 'GO_SOMEWHERE':
            // ...
        // etc.
        default:
            return state;
    }
}

// 可以看出要用一个reducer来处理应用中的所有action事不现实的(尽管可能可以做到,但却会使得程序变得不可维护)
// 相当幸运的是,redux并没有规定我们只能使用一个reducer或是十个reducer,甚至在有多个reducer的情况下它还能帮我们把其合并起来.

// 我们来试试定义两个reducer函数
var userReducer = function (state = {}, action) {
    console.log('userReducer was called with state', state, 'and action', action)

    switch (action.type) {
        // etc.
        default:
            return state;
    }
}
var itemsReducer = function (state = [], action) {
    console.log('itemsReducer was called with state', state, 'and action', action)

    switch (action.type) {
        // etc.
        default:
            return state;
    }
}

// 我想你肯定注意到了上面reducer函数的state的初始值:
// userReducer的state初始值为一个空对象({})
// itemsReducer的state初始值为一个空数组([])
// 这只是说明reducer可以处理任何数据结构.而要使用什么数据结构(对象,数组,布尔值,字符串,不可变数据结构)完全取决于你自己的需求

// 当有多个reducer时,每个reducer只会处理应用的state中相对应的部分
// 但是据我们所知,createStore只接收一个reducer函数作为其参数

// 我们应该怎么去合并我们的reducers呢?还有该如何告诉redux每个reducer只会处理state中的对应的部分?
// 这简直太简单了,我们使用redux的`combineReducers`函数
// combineReducers会给reducer打上hash及返回一个最终的reducer函数,这个最终的reducer函数可以调用各个子reducer,处理state中相应的部分,并把它们的结果重新合并成到redux中存放的state对象中去.

// 长话短说,下面演示如何用多个reducers来创建redux实例
import { createStore, combineReducers } from 'redux'

var reducer = combineReducers({
    user: userReducer,
    items: itemsReducer
})

// 输出:
// userReducer was called with state {} and action { type: '@@redux/INIT' }
// userReducer was called with state {} and action { type: '@@redux/PROBE_UNKNOWN_ACTION_9.r.k.r.i.c.n.m.i' }
// itemsReducer was called with state [] and action { type: '@@redux/INIT' }
// itemsReducer was called with state [] and action { type: '@@redux/PROBE_UNKNOWN_ACTION_4.f.i.z.l.3.7.s.y.v.i' }
var store_0 = createStore(reducer)
// 输出:
// userReducer was called with state {} and action { type: '@@redux/INIT' }
// itemsReducer was called with state [] and action { type: '@@redux/INIT' }

```
