
# state & redux

有时在应用中我们需要处理的`action`不仅会告知我们发生了什么,还会提醒我们需要更新数据.

实际上对于每一个应用,这都是相当麻烦的点.

- 在应用运行期间我应该吧所有的数据存放在何处?
- 又改如何处理每一份数据的改动呢?
- 以及如何把数据改动带来的影响分发到应用中的其他部分呢(`View`或者`Model`)?

接下来就看redux的了

`[Redux](https://github.com/rackt/redux)` 是一个"JavaScript的可预测化的状态容器"

让我们回顾一下上面的问题并使用`Redex`中的概念来解决它们:

> 在应用运行期间我应该吧所有的数据存放在何处?

就以你喜欢的形式来存放数据(js对象,数据,不可变结构体...).而你应用中数据被称为`state`(状态),
这个叫法似乎相当合理因为在应用中的数据无时无刻都在发生变化,而数据的变化正正代表了应用的状态.
不过你得把这个`state`交予redux来进行管理(redux是一个`state container`(状态容器))

> 又改如何处理每一份数据的改动呢?

使用`reducers`(对应flux架构中的`stores`),
一个`reducer`是`actions`的订阅者
`reducer`其实就是一个接受当前应用的`state`和`action`,而后返回一个经改动的新的`state`的函数

> 以及如何把数据改动带来的影响分发到应用中的其他部分呢(`View`或者`Model`)?

那就只要监听着应用`state`的变化就可以了.

Redux已经为你把所有部件都连接起来了.
概括起来,Redux会提供给你:

1. 存放应用`state`的地方
2. 一种分发`action`和改动应用`state`的手段 - `reducers`
3. 监听应用状态更新的方法

```js
// redux实例被称作store,可以像下面一样创建redux实例
import { createStore } from 'redux'

var store = createStore()
// 但是运行上面代码你却会发现报了一个错误
// Error: Invariant Violation: Expected the reducer to be a function.
// 这是因为createStore()接收一个函数作为其参数,而这个函数用于改变你应用的`state`
```

``` js
// 下面才是正确示范
import { createStore } from 'redux'

var store = createStore(() => {})
// 这就能正常运行了
```

前往下一章节:[03_simple-reducer]()
