# QQScreenshotSwitch - 切换 QQ 新版/旧版截图工具

QQ 最近推出了新版截图工具，对比旧版主要有几个方面的变化：

- 旧的截图工具是一个独立进程，QQ 和截图工具之间使用 Chromium 的 Mojo 进行 IPC
  通信（已经有人逆向并做了 [独立版](https://github.com/EEEEhex/QQImpl)，百度直接搜《逆向调用 QQ 截图 NT》即可）；新版截图工具就在
  QQ 进程内，基本使用 JS+Electron 实现
- 新版截图工具多了一些功能：圆角、阴影功能，以及适配了 QQ 的暗色主题

但新版截图工具目前没有开放给所有人使用，看到群里一部分群友想要新版截图工具但没有收到，或是相同电脑上某几个账号收到了而另外几个账号没收到；另一些群友觉得新截图工具不如旧版的习惯想换回旧版的；于是写了这个工具，可以随意切换
QQ 的旧版/新版截图工具。

## 安装

新版 QQ 使用的是 Electron 框架，QQScreenshotSwitch 的本体是一个 60K 左右大小的 js 文件，让 QQ 加载这个文件即可。

加载 JS 文件的方法有很多，最简单的方法是一个叫 LiteLoaderQQNT 的插件框架，官网： <https://liteloaderqqnt.github.io>
GitHub： <https://github.com/LiteLoaderQQNT/LiteLoaderQQNT>

LiteLoaderQQNT 的官网上提供了详细的安装方法，安装好之后就可以安装插件了。最简单的方法，直接点击页面最上面绿色的 Download
按钮下载即可。下载好之后解压扔进 LiteLoaderQQNT 的插件（plugins）文件夹里，再启动 QQ 就可以了。

## 使用

安装好之后启动 QQ，照常登录，QQ 不会有任何变化。这是因为 QQScreenshotSwitch 默认是不启用功能的，需要进行一下快速的配置。

打开任意一个地址栏（Win+R 可以打开，或者打开「我的电脑」然后点最上方的地址栏也行）然后输入 `"%USERPROFILE%/.qqsss"`
回车，可以打开 QQScreenshotSwitch 的配置文件夹。

如果你使用的不是 Windows 的话，macOS 在桌面按 `⌘⇧G` 然后输入 `~/Library/Containers/com.tencent.qq/Data/.qqsss`
回车，Linux 在桌面按`Alt+F2` 输入 `~/.qqsss` 回车，也会打开这个文件夹。

![1](https://github.com/bakyrd/QQScreenshotSwitch/assets/20179549/fe16a2f9-66ba-475e-a245-b6f614245a03)

文件夹里有一个 `config.txt`，双击用记事本打开，在这里可以修改 QQScreenshotSwitch 的工作模式。

有四种模式：

- `new`：使用新版截图
- `old`：使用旧版截图
- `default`：永远使用当前版本默认的截图（目前所有版本 QQ 默认的都是旧版截图
- `noop`：保持 QQ 配置，不进行修改

文件里默认写着的就是 `noop`。删掉 `noop` 改成你想要的工作模式，保存后重启 QQ 就可以生效了。

如果你不想使用文件配置，或者需要临时修改单个 QQ 进程的工作模式的话，可以用环境变量
`QQSSS`（控制面板添加系统环境变量 QQSSS，值就是上面的模式），或者直接用命令行开关
`--qqsss`（命令行在 QQNT 目录打开然后用 `QQ --qqsss=new` 启动，new 改成你需要的模式即可）

## 效果

旧版截图跟以前的 PCQQ 完全一样：

![2](https://github.com/bakyrd/QQScreenshotSwitch/assets/20179549/2d6e55d9-cd66-42d0-bad2-538b8d51f945)

新版截图支持圆角和阴影，按钮更大，同时适配了 QQ 的暗色主题，看起来有点帅：

![3](https://github.com/bakyrd/QQScreenshotSwitch/assets/20179549/fdc9ee99-eccf-42bd-8dd7-e4555040aa5f)

但如果使用长截图功能的话新版截图会退出然后调用旧版截图，感觉应该是技术限制。新版截图实际上的工作逻辑就是截一张全屏图然后开一个全屏浏览器窗口贴上去，长截图大概是比较难实现。

## 原理

原理挺简单的，QQ 在启动时会读取一系列配置，从中找到截图的配置，然后改 `true` 或改 `false`
就能切换新旧截图工具了。配置项内容里直接有 `"useNewTools": true` 字样，一下就能看出来。

因为原理比较简单+本人写的代码比较烂所以不开源了，感兴趣的小伙伴可以自己动手试一试实现。
