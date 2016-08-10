
# reducer

现在我们知道该如何去创建一个存放着应用`state`的Redux实例,接下来我们来聊聊那些
可以改变这个应用`state`的`reducer`函数.

`reducer` VS `store`
你可能已经注意到了,在flux架构的图解中,有`store`而没有redux中的`reducer`,所以
`store`和`reducer`实际上有什么区别吗?这比你想象得还有简单:`store`是用来存放数据而`reducer`并不干这活.
所以在典型的flux架构中,`store`存放着应用的`state`,而在redux中,每次`reducer`被调用都会被传入一个需要被更新的`state`,
这样看来,Redux中的`stores`变成了`stateless stores`(无关状态的store),即`reducer`

```js
// 正如之前所说,创建一个redux实例时需要传入一个`reducer`函数
import { createStore } from 'redux'

var store_0 = createStore(() => {})
// ...现在每当应用中有action被触发时,redux都能调用这个`reducer`函数.
// 把reducer函数作为参数传入createStore(),正是redux给`action`注册`handler`(在01_simple-action-creator中提到过的)的做法

// 让我们在reducer中打印些东西吧
var reducer = function (...args) {
  console.log('Reducer was called with args', args)
}

var store_1 = createStore(reducer)
// 输出: Reducer was called with args [ undefined, { type: '@@redux/INIT' } ]
```

看见了吗?`reducer`的确被调用了尽管我门还没有触发任何的action...
这是因为为了初始化应用的`state`,redux实际上触发一个`init action`({ type: '@@redux/INIT' })

`reducer`被调用时会自动传入两个参数(`state`, `action`),这也说明在应用初始化时的`state`由于还没有初始化,所以是"undefined"

但是在redux触发`init action`之后,应用的`state`又是什么呢?

前往下一章节:[04_get-state]()
