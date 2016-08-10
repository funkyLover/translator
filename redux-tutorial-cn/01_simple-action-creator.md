
# action creator

我们从上节开始就简单讲了一下`action`,但是`action creator`实际上又是什么呢?而和`actions`又有什么关系呢?

实际上非常简单的几行代码就能完全解释清楚

```js
// `action creator`只是一个..
var actionCreator = function() {
  // ..生产及返回`action`的函数(对了,从`action creator`的名字看来也是非常的明显)
  return {
    type: 'AN_ACTION'
  }
}
// 这就完了吗?对的.
```

然而还需要注意一件事,就是函数返回的`action`的格式.flux架构中约定`action`是一个包含`type`属性的对象,
这个`type`属性用于区分那些回调来处理该`action`.当然了,`action`也能包含其他任何你想传递的属性数据.

我们之后将会看到`action creator`实际上也是可以返回其他非`action`的东西,例如可以返回一个函数,
这对于处理`异步action`非常的有用(详见[dispatch-async-action]()).

```js
// 我们调用上面的`action creator`函数能得到一个符合预期的`action`
console.log(actionCreator())
// 输出: { type: 'AN_ACTION' }
```

好的,尽管这能跑得通但却没干什么实事..
我们需要的是把这个`action`发送给那些对其感兴趣的东西让它们知道"发生了什么"及"要做什么".
我们把这个过程叫做`dispatch`一个`action`

为了去`dispatch`一个`action`,我们需要....一个`dispatch`函数(相当6666).
及为了让其它希望响应`action`的部分被知会,我们需要一个去注册`handlers`的手段.一般而言,在flux架构中`action`的`handler`被叫做`store`.
我们将在下一节中讲解它们在redux中又分别代表了什么.

到目前为止,在我们的应用中有以下的数据流向
> actionCreator -> Action

前往下一章节:[02_about-state-and-meet-redux]()


