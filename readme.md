# Electron
## _The Last Markdown Editor, floweryan313


[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://www.electronjs.org/docs/latest/tutorial/quick-start )



electron 安装

- 正确安装node.js环境
- 用yarn or npm搭建脚手架
- 添加electron 依赖

## 注意事项

- 使用electron-builder打包 windows or macos 桌面应用
- electron-builder 打包报错需要外网
- electron-builder 下载文件需要自行下载，然后放在指定的AppData/Local/electron/Cache目录
- electron-xxx-win32-64 不用解压 放到AppData/Local/electron/Cache目录
- nsis / winCodeSign 解压放到AppData/Local/electron-builder/Cache目录
- electron-builder 打包过程报错 [详情请见] 这篇文章。


## 演示

要检查 Node.js 是否正确安装，请在您的终端输入以下命令：
```
node -v
v18.18.0
npm -v
9.8.1
```
创建应用
```
mkdir my-electron-app && cd my-electron-app

yarn init
```

你的 package.json 文件应该像这样：
```
{
  "name": "electron_demo",
  "version": "v1.0.0",
  "main": "index.js",
  "repository": "https://github.com/floweryan313/electron_demo.git",
  "author": "floweryan313 <floweryan313@gmail.com>",
  "license": "MIT"
}

```
将electron包安装到依赖中
```
yarn add --dev electron
```

>过程很漫长需要淘宝镜像

```
cnpm install electron@26.4.0
cnpm install electron-builder@24.6.4
```

```
npm config set registry https://registry.npm.taobao.org/
```

npm镜像

>翻墙时需要切换回来

```
npm config set registry https://registry.npmjs.org/
```

在package.json中要增加执行命令：
```
{
  "scripts": {
    "start": "electron ."
  }
}
```
```
yarn start
```
index.js 主进程

```
//main.js
// Modules to control application life and create native browser window
const { app, BrowserWindow } = require('electron')
const path = require('node:path')

const createWindow = () => {
  // Create the browser window.
  const mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      preload: path.join(__dirname, 'preload.js')
    }
  })

  // 加载 index.html
  mainWindow.loadFile('index.html')

  // 打开开发工具
  // mainWindow.webContents.openDevTools()
}

// 这段程序将会在 Electron 结束初始化
// 和创建浏览器窗口的时候调用
// 部分 API 在 ready 事件触发后才能使用。
app.whenReady().then(() => {
  createWindow()

  app.on('activate', () => {
    // 在 macOS 系统内, 如果没有已开启的应用窗口
    // 点击托盘图标时通常会重新创建一个新窗口
    if (BrowserWindow.getAllWindows().length === 0) createWindow()
  })
})

// 除了 macOS 外，当所有窗口都被关闭的时候退出程序。 因此, 通常
// 对应用程序和它们的菜单栏来说应该时刻保持激活状态, 
// 直到用户使用 Cmd + Q 明确退出
app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') app.quit()
})

// 在当前文件中你可以引入所有的主进程代码
// 也可以拆分成几个文件，然后用 require 导入。
```
preload.js 
```
// 所有的 Node.js API接口 都可以在 preload 进程中被调用.
// 它拥有与Chrome扩展一样的沙盒。
window.addEventListener('DOMContentLoaded', () => {
  const replaceText = (selector, text) => {
    const element = document.getElementById(selector)
    if (element) element.innerText = text
  }

  for (const dependency of ['chrome', 'node', 'electron']) {
    replaceText(`${dependency}-version`, process.versions[dependency])
  }
})
```
index.html

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <!-- https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP -->
    <meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self'">
    <title>你好!</title>
  </head>
  <body>
    <h1>你好!</h1>
    我们正在使用 Node.js <span id="node-version"></span>,
    Chromium <span id="chrome-version"></span>,
    和 Electron <span id="electron-version"></span>.

    <！-- 您也可以此进程中运行其他文件 -->
    <script src="./renderer.js"></script>
  </body>
</html>
```
打包并分发您的应用程序
```
cnpm install electron-builder@24.6.4  
```
打包命令
```
  "pack": "electron-builder --dir",
  "dist": "electron-builder"
```

```

PS D:\scratch\electron_demo> npm run dist

> electron_demo@v1.0.0 dist
> electron-builder

  • electron-builder  version=24.6.4 os=10.0.19045
  • description is missed in the package.json  appPackageFile=D:\scratch\electron_demo\package.json
  • writing effective config  file=dist\builder-effective-config.yaml
  • packaging       platform=win32 arch=x64 electron=26.4.0 appOutDir=dist\win-unpacked
  • default Electron icon is used  reason=application icon is not set
  • building        target=nsis file=dist\electron_demo Setup 1.0.0.exe archs=x64 oneClick=true perMachine=false
  • building block map  blockMapFile=dist\electron_demo Setup 1.0.0.exe.blockmap

```
打包成功：
```
 /dis/
```

## License

MIT

   [详情请见]: <https://blog.csdn.net/qq_59747594/article/details/132393855>
   [PlDb]: <https://github.com/joemccann/dillinger/tree/master/plugins/dropbox/README.md>
   [PlGh]: <https://github.com/joemccann/dillinger/tree/master/plugins/github/README.md>
   [PlGd]: <https://github.com/joemccann/dillinger/tree/master/plugins/googledrive/README.md>
   [PlOd]: <https://github.com/joemccann/dillinger/tree/master/plugins/onedrive/README.md>
   [PlMe]: <https://github.com/joemccann/dillinger/tree/master/plugins/medium/README.md>
   [PlGa]: <https://github.com/RahulHP/dillinger/blob/master/plugins/googleanalytics/README.md>
