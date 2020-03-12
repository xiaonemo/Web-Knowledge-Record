## React常用的生命周期函数

### 挂载

#### constructor()
``` javascript
constructor(props) {
  super(props);
}
```
##### 如果不初始化 state 或不进行方法绑定，则不需要为 React 组件实现构造函数。
通常，在react中使用constructor方法有两种情况，1.初始化props的值给this.state。2.为事件处理函数绑定实例。

#### render()
``` javascript
render(){
  return()
}
```
render() 方法是 class 组件中唯一必须实现的方法。
render()是纯输出DOM函数，应避免在此处处理setState，如需处理state可在componentDidMount等生命周期函数中。

#### componentDidMount()
``` javascript
componentDidMount()
```
componentDidMount()是在组件挂载后立即调用，依赖于DOM节点的初始化应该放到这里。请求网络接口数据时，此方法也是最好的位置。

### 更新

#### componentDidUpdate()
``` javascript
componentDidUpdate(prevProps, prevState, snapshot)
```
componentDidUpdate() 会在更新后会被立即调用。首次渲染不会执行此方法。
* 组件更新后，可以在此处操作DOM。
* 组件更新后，可以在此处比较porps值是否改变。
* 组件更新后，可以在此处处理网络请求。

``` javascript
componentDidUpdate(prevProps) {
  // 典型用法（不要忘记比较 props）：
  if (this.props.userID !== prevProps.userID) {
    this.fetchData(this.props.userID);
  }
}
```
在 componentDidUpdate() 中直接调用 setState()，但请注意它必须被包裹在一个条件语句里。

### 卸载

#### componentWillUnmount()
``` javascript
componentWillUnmount()
```
componentWillUnmount() 会在组件卸载及销毁之前直接调用。在此方法中执行必要的清理操作，例如，清除 timer，取消网络请求或清除在 componentDidMount() 中创建的订阅等。