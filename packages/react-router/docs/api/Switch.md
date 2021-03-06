# &lt;Switch>

渲染匹配地址(location)的第一个 [`<Route>`](Route.md) 或者 [`<Redirect>`](Redirect.md)

**这与只使用一堆`<Route>`有什么不同？**

`<Switch>`的独特之处是独它*仅仅*渲染一个路由。相反地，*每一个包含*匹配地址(location)的`<Route>`都会被渲染。思考下面的代码：
```js
<Route path="/about" component={About}/>
<Route path="/:user" component={User}/>
<Route component={NoMatch}/>
```

如果现在的URL是 `/about` ，那么  `<About>`, `<User>`, 还有 `<NoMatch>` 都会被渲染，因为它们都与路径(path)匹配。这种设计，允许我们以多种方式将多个 `<Route>` 组合到我们的应用程序中，例如侧栏(sidebars)，面包屑(breadcrumbs)，bootstrap tabs等等。
然而，偶尔我们只想选择一个`<Route>` 来渲染。如果我们现在处于 `/about` ，我们也不希望匹配 `/:user` （或者显示我们的 "404" 页面 ）。以下是使用 `Switch` 的方法来实现：

```js
import { Switch, Route } from 'react-router'

<Switch>
  <Route exact path="/" component={Home}/>
  <Route path="/about" component={About}/>
  <Route path="/:user" component={User}/>
  <Route component={NoMatch}/>
</Switch>
```


现在，如果我们处于 `/about`, `<Switch>` 将开始寻找匹配的 `<Route>`。 `<Route path="/about"/>` 将被匹配， `<Switch>` 将停止寻找匹配并渲染`<About>`。 同样，如果我们处于 `/michael` ， `<User>` 将被渲染。

这对于过渡动画也是起作用的，因为匹配的 `<Route>` 在与前一个相同的位置被渲染。

```js
<Fade>
  <Switch>
    {/* there will only ever be one child here */}
    {/* 这里只会有一个子节点 */}
    <Route/>
    <Route/>
  </Switch>
</Fade>

<Fade>
  <Route/>
  <Route/>
  {/* there will always be two children here,
      one might render null though, making transitions
      a bit more cumbersome to work out */}
   {/* 这里总是有两个子节点,
      一个可能会渲染为null, 使计算过渡增加了一点麻烦 */}    
</Fade>
```

## children: node


`<Switch>` 的所有子节点应为 `<Route>` 或 `<Redirect>` 元素。只有匹配当前地址(location)的第一个子节点才会被渲染。

`<Route>` 元素使用它们的 `path` 属性匹配，`<Redirect>` 元素使用它们的 `from` 属性匹配。没有 `path` 属性的`<Route>` 或者 没有 `from` 属性的 `<Redirect>` 将总是可以匹配当前的地址(location)


```js
<Switch>
  <Route exact path="/" component={Home}/>

  <Route path="/users" component={Users}/>
  <Redirect from="/accounts" to="/users"/>

  <Route component={NoMatch}/>
</Switch>
```
