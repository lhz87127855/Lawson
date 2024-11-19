# 1. 选型

- `Taro UI`

  - 很多组件不全
  - `Modal`组件会在 IOS 上出现内容泄漏的情况
  - 无 `table`
  - 无 `form` 校验

- `NutUI`

  - 组件比较全
  - 改主题色比较麻烦
  - 需要安装`@tarojs/plugin-html`插件，会导致包变大，而且开发的基础组件变成了 div，而不是 view，和其他小程序 UI 格格不入

- `@antmjs/vantui`
  - 组件比较全
  - 主题色修改和 `antd` 类似
  - `form` 校验也和 `antd` 类似
  - 有原始的 `table` 组件
  - `PullToRefresh` 不好用，自己结合scroll-view，来开发
