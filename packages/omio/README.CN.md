[English](./README.md) | 简体中文

# Omio

> 兼容老浏览器的 Omi 版本(支持到IE8+和移动端浏览器)

---

## 立即使用

```bash
$ npm i omi-cli -g             
$ omi init-o my-app   
$ cd my-app           
$ npm start                     
$ npm run build               
```

### 与 omi 不同之处

omio 拥有 omi一样的语法，但是也有一些差异需要注意：

* omio 支持 `staticCss` 来提高渲染性能，omi 有 shadow dom 隔离不需要支持也支持不了

`css` 和 `staticCss` 的区别是 ? 例如：

``` js
render() {
  return (
    <div>
      <my-ele name={this.name}></my-ele>
      <my-ele name={this.name}></my-ele>
      <my-ele name={this.name}></my-ele>
    </div>
  )
}
```

如上面的例子,`css`方法会渲染三次，并插入到 head，而`staticCss` 只会渲染一次。
当你 update 组件或者 setState 时候，`css`方法会渲染三次，并更新head里对应三个地方的样式，`staticCss` 不再渲染。

* Omio 不支持 slot, 请使用 `props.children` 代替，像 react 一样
* Omio 支持 store，但不会去监听 store.data 变化，store 仅仅只是从根节点注入

## 在 Omi 项目中使用

``` bash
npm i omio
```
### Webpack Alias

如果你想在已经存在的 omi 项目下使用 omio，你可以使用下面配置，不用任何代码更改:

```js
module.exports = {
  //...
  resolve: {
    alias: {
      omi: 'omio'
    }
  }
};
```


## 兼容 IE8

```js
import { render, WeElement, define } from '../../src/omi'

define('my-counter', class extends WeElement {
  //ie8 不能使用 observe
  //static observe = true

  data = {
    count: 1
  }

  sub = () => {
    this.data.count--
    //手动 update
    this.update()
  }

  add = () => {
    this.data.count++
    //手动 update
    this.update()
  }

  render() {
    return (
      <div>
        <button onClick={this.sub}>-</button>
        <span>{this.data.count}</span>
        <button onClick={this.add}>+</button>
      </div>
    )
  }
})

render(<my-counter />, 'body')
```

如果你不需要兼容 IE8，只需要兼容 IE9+，你可以使用 `static observe = true` 进行数据监听自动更新视图。

## License

MIT © [dntzhang](https://github.com/dntzhang)
