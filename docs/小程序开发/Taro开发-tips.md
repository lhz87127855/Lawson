# 路由

- 所有的路由，需要在 `app.config.ts` 中注册，否则无法识别
  - 不会自动识别 `index` ，因此需要加上 `index`
- 不支持嵌套路由
- 不支持动态路由
- `app.ts`每当路由变动，都会进入此文件
  - 此组件不会渲染任何内容，因此无法实现统一逻辑处理来达到嵌套路由效果
  - 目前只能根据 `accessToken` 是否有效来判定是否是`游客模式`
- 路由参数，通过`？`的方式缀在路由后面，类似与web上的searchParams
  ```ts
  Taro.navigateTo({
    url: '/pages/page/path/name?id=2&type=test',
  })
  ```
- 使用`Taro.navigateTo`进行路由跳转，不会销毁组件，只是在本图层上多了另外一层图层
- 使用`Taro.navigateBack`进行路由回撤，也不会重新创建组件，只是销毁了上层的图层组件，本图层组件的`useEffect(() => {}, [])`不会被触发
- 由于上述两者的原因，若需要路由再次到路由页面时发生动作，则需要使用`useDidShow`来判定触发
- 使用`Taro.getCurrentPages`可以获取到，当前已有多少图层
- 使用`Taro.getCurrentInstance`可以获取到当前应用的实例instance，`router`就在instance上挂载，而 `params` 在 `router` 上
- 通过微信扫码方式进入的页面，`params`中的参数为`q`，内容为跳转的url，需要再次手动解析

  ```ts
  const instance = useStoreGlobal(store => store.instance) // instance存储在store上了
  const { params } = instance?.router || {}
  const { q } = params || {}
  ...
  const realUrl = new URL(decodeURIComponent(q))
  const urlParams = new URLSearchParams(realUrl.search)
  ```

# DOM操作

- 无法动态添加节点，无`createElement`、`createPortal`，不能获取`Page`根节点
- 无法进行按钮操作模拟(安全策略)
- 不支持动态`import`中有变量，哪怕是`字符串模板`也不行

# 导航

- 若要定制化导航栏，需要在`app.config.ts`中配置`window.navigationStyle: 'custom'`
- 若要使得对应页面能进行下拉，则需要在组件顶部定义或外部定义(最好在外部，内部的样式不生效)
  ```ts
  definePageConfig({
    enablePullDownRefresh: true,
  })
  ```

# 样式

- 小程序上无法使用\*，因此`reset.css`在移动端使用不了, PC和小程序的标签不一样，所以如果有小程序和H5，则需要有两套css
- 使用`unocss-preset-weapp`进行taro上unocss开发时，若不喜欢其中的默认配置（比如：不进行rpx宽高重新计算，需要配置whRpx），请参考官网配置
- 闪屏问题，一般是宽高没有定死导致，谨慎使用 `Image` 的 `widthFix`、`heightFix`
- 真机上，`background-image`显示异常
  - 只能支持外部链接的方式，因此需要用到图床
  - 或者使用 `Image` ，然后进行绝对定位

# 组件

- 在`View`中，使用`overflow:auto，padding-bottom`会异常，`ScrollView`中无此问题
- 在真机上，`input` 设置 `opicity: 0` 也会显示
- `scroll-view`中的`refresh-enable`开启时，其内的`fixed`定位失效；且其滚动太快时，安卓上`onScrollToLower`可能不会触发

# 安全区域

- 顶部可以通过`navigationStyle: 'custom'`来自定义头部或使用内置的navigation
- 底部可以通过`screenHeight - (safeArea?.bottom || 0)`来得到高度，然后写一个view来撑开