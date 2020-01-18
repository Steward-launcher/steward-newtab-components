从 V4.0.0 以后，Steward 将 newtab 进行了组件化以及栅格化，提供 api 以及官方的仓库

## 组件开发说明
你可以在[这个仓库](https://github.com/Steward-launcher/steward-documents)添加你的组件的帮助文档

### 单文件 Vue 组件开发
请参考 [FranckFreiburger/http-vue-loader: load .vue files from your html/js](https://github.com/FranckFreiburger/http-vue-loader)

### 组件可用的 API
#### Vue 原型上的 API
- $dayjs -- 日期库
[https://github.com/iamkun/dayjs](https://github.com/iamkun/dayjs)

- $http -- http 库
[https://github.com/axios/axios](https://github.com/axios/axios)

#### 全局的 StewardApp 对象
具体请通过源码或 `console.log` 查看
```js
window.stewardApp = {
  // 一些插件的 helper
  helpers,
  // chrome 对象
  chrome,
  util,
  // 常量
  constant,
  Toast,
  md5,
  browser,
  // steward 的用户配置
  config
}
```

### 在[data.json](../data.json)中配置组件
```json
{
  // required
  "version": "0.1.0",
  // required，唯一
  "id": "shortcuts",
  "icon": "https://i.imgur.com/4NVFlf3.png",
  // required
  "title": "Shortcuts",
  "subtitle": "Shortcuts",
  // required
  "source": "https://raw.githubusercontent.com/Steward-launcher/steward-newtab-components/master/components/shortcuts/0.1.0/index.vue",
  // 你的 github ID
  "author": "solobat",
  // 默认的栅格配置, 格式为 [x坐标, y坐标, 栅格宽, 栅格高], 栅格为 24 * 13 的布局
  // required
  "grid": [0, 0, 10, 1]
},
```

#### 栅格化配置
请参考 [jbaysolutions/vue-grid-layout: A draggable and resizable grid layout, for Vue.js.](https://github.com/jbaysolutions/vue-grid-layout)