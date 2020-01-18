[中文](./doc/README_CN.md)

Since V4.0.0, Steward has componentized and rasterized newtab, providing APIs and official repositories

## Component development
You can add help documentation for your component at [this repository](https://github.com/Steward-launcher/steward-documents)

### Vue component development
Please refer to [FranckFreiburger/http-vue-loader: load .vue files from your html/js](https://github.com/FranckFreiburger/http-vue-loader)

### Components APIs
#### Vue prototype APIs
- $dayjs -- Date library
[https://github.com/iamkun/dayjs](https://github.com/iamkun/dayjs)

- $http -- http library
[https://github.com/axios/axios](https://github.com/axios/axios)

#### Global StewardApp object
For details, please view the source code or use `console.log`

```js
window.stewardApp = {
  // Some plugin helpers
  helpers,
  // chrome apis
  chrome,
  util,
  constant,
  Toast,
  md5,
  browser,
  // user configuration for steward
  config
}
```

### Configure the component in [data.json](../data.json)
```js
{
  // required
  "version": "0.1.0",
  // required，unique
  "id": "shortcuts",
  "icon": "https://i.imgur.com/4NVFlf3.png",
  // required
  "title": "Shortcuts",
  "subtitle": "Shortcuts",
  // required
  "source": "https://raw.githubusercontent.com/Steward-launcher/steward-newtab-components/master/components/shortcuts/0.1.0/index.vue",
  // your github ID
  "author": "solobat",
  // The default grid configuration, the format is [x coordinate, y coordinate, grid width, grid height], and the grid is a layout of 24 * 13
  // required
  "grid": [0, 0, 10, 1]
},
```

#### Grid configuration
Please refer to [jbaysolutions/vue-grid-layout: A draggable and resizable grid layout, for Vue.js.](https://github.com/jbaysolutions/vue-grid-layout)