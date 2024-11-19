# 原生功能

- `button`组件的`open-type`属性
  - `contact`
    - 打开用户与小程序客服的会话。
    - 需要在小程序后台开启客服功能。
  - `share`
    - 微信分享功能
  - `chooseAvatar`
    - 在微信提供的版本中，可以通过这个属性打开微信的头像选择功能
    - 得到的是临时的url，需要手动上传。`Taro.getFileSystemManager().getFileInfo`可以获取图片的`size`，然后和`url`、`size`以及类型就可以组成File类型，进行手动上传

# WebView 渲染外部html

```tsx
<WebView src={src} />
```

- 一般需要结合`字典表功能`来动态配置`src`
- 需要`微信公众平台`开启对应的`业务域名`
- 无法自定义`navigationBar`
- 若出现字体太小的问题，请尝试在目标html上添加

```html
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" />
```

- 若出现word转html的时候的`乱码`情况，请先查看html中的编码方式是否是`utf8`的，如果是`gb2312`等之列的，请使用文本编辑器进行转码，并修改`gb2312`为`utf8`

```html
<meta http-equiv="Content-Type" content="text/html; charset=utf8" />
```

# scanCode 扫码

  ```ts
  Taro.scanCode({
    success: res => {
      console.log('scan success', res.result)
    },
    fail: res => {
      console.error('scan error', res)
    },
  })
  ```

# NFC 获取卡数据

  ```ts
  // 开启NFC
  const startNFC = () => {
    const nfcAdapter = Taro.getNFCAdapter() // 多次调用是同一个实例

    // 启动nfc
    nfcAdapter.startDiscovery({
      success: res => {
        setHasStarted(true)
        startSuccess?.(res)
      },
      fail: res => {
        setHasStarted(false)
        startFail?.(res)
      },
    })

    // 监听nfc
    nfcAdapter.onDiscovered(handler)
  }

  // 停止NFC
  const stopNFC = () => {
    const nfcAdapter = Taro.getNFCAdapter() // 多次调用是同一个实例
    if (hasStarted) {
      nfcAdapter.stopDiscovery({
        success: res => {
          stopSuccess?.(res)
        },
        fail: res => {
          stopFail?.(res)
        },
      })
    }
    nfcAdapter.offDiscovered(handler)
  }
  ```