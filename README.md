# <img src="https://user-images.githubusercontent.com/381895/226091100-f5567a28-7736-4d37-8f84-e08f297b7e1a.png" alt="logo" height="60" valign="middle" /> wavesurfer.js

[![npm](https://img.shields.io/npm/v/wavesurfer.js)](https://www.npmjs.com/package/wavesurfer.js) [![sponsor](https://img.shields.io/badge/sponsor_us-🤍-%23B14586)](https://github.com/sponsors/katspaugh)

**Wavesurfer.js** 是一个交互式波形渲染与音频播放库，非常适合用于 Web 应用。它借助现代 Web 技术，提供稳健且视觉体验出色的音频播放功能。

<img width="626" alt="waveform screenshot" src="https://github.com/katspaugh/wavesurfer.js/assets/381895/05f03bed-800e-4fa1-b09a-82a39a1c62ce">

---
### 黄金赞助商 💖 [Closed Caption Creator](https://www.closedcaptioncreator.com) —— 专业字幕编辑器
---

# 目录

1. [快速开始](#快速开始)
2. [API 参考](#api-参考)
3. [插件](#插件)
4. [CSS 样式](#css-样式)
5. [常见问题](#常见问题)
6. [开发](#开发)
7. [测试](#测试)
8. [反馈](#反馈)

## 快速开始

安装并导入该包：

```bash
npm install --save wavesurfer.js
```
```js
import WaveSurfer from 'wavesurfer.js'
```

或者，插入一个 UMD 脚本标签，它会将库导出为全局变量 `WaveSurfer`：
```html
<script src="https://unpkg.com/wavesurfer.js@7"></script>
```

创建一个 wavesurfer 实例并传入各种[配置项](https://wavesurfer.xyz/docs/api/options)：
```js
const wavesurfer = WaveSurfer.create({
  container: '#waveform',
  waveColor: '#4F4A85',
  progressColor: '#383351',
  url: '/audio.mp3',
})
```

要导入某个插件，例如 [Regions 插件](https://wavesurfer.xyz/examples/?regions.js)：
```js
import Regions from 'wavesurfer.js/dist/plugins/regions.esm.js'
```

或者以脚本标签的形式引入，它会导出 `WaveSurfer.Regions`：
```html
<script src="https://unpkg.com/wavesurfer.js@7/dist/plugins/regions.min.js"></script>
```

TypeScript 类型已包含在包内，因此无需安装 `@types/wavesurfer.js`。

查看更多[示例](https://wavesurfer.xyz/examples)。

## 文档

请参阅我们网站上的 [wavesurfer.js 指南](https://wavesurfer.xyz/docs/)，它提供了面向初学者的实用文档，涵盖常见使用场景、插件、框架集成以及故障排查。

## API 参考

完整的自动生成 API 参考位于 [wavesurfer.xyz/docs/api](https://wavesurfer.xyz/docs/api/)：

 * [方法](https://wavesurfer.xyz/docs/api/methods)
 * [配置项](https://wavesurfer.xyz/docs/api/options)
 * [事件](https://wavesurfer.xyz/docs/api/events)

## 插件

我们维护了许多官方插件，用于添加各种额外功能：

 * [Regions](https://wavesurfer.xyz/examples/?regions.js) —— 用于音频区域的视觉叠加层与标记
 * [Timeline](https://wavesurfer.xyz/examples/?timeline.js) —— 在波形下方显示刻度与时间标签
 * [Minimap](https://wavesurfer.xyz/examples/?minimap.js) —— 一个小型波形，作为主波形的滚动条
 * [Envelope](https://wavesurfer.xyz/examples/?envelope.js) —— 用于添加淡入淡出效果并控制音量的图形界面
 * [Record](https://wavesurfer.xyz/examples/?record.js) —— 从麦克风录制音频并渲染波形
 * [Spectrogram](https://wavesurfer.xyz/examples/?spectrogram.js) —— 音频频谱可视化（由 @akreal 编写）
 * [Hover](https://wavesurfer.xyz/examples/?hover.js) —— 在鼠标悬停波形时显示一条竖线与时间戳

## CSS 样式

wavesurfer.js v7 被渲染到 Shadow DOM 树中。这使它内部的 CSS 与网页其余部分相互隔离。
不过，仍然可以通过 `::part()` 伪元素选择器，使用 CSS 对 wavesurfer.js 的各种元素进行样式设置。
例如：

```css
#waveform ::part(cursor):before {
  content: '🏄';
}
#waveform ::part(region) {
  font-family: fantasy;
}
```

你可以在 DOM 检查器中查看哪些元素可以设置样式 —— 它们会带有一个 `part` 属性。
请参阅[这个示例](https://wavesurfer.xyz/examples/?styling.js) 来体验样式设置。

## 常见问题

关于在你的网站上集成 wavesurfer.js 有任何疑问？欢迎在我们的 [讨论论坛](https://github.com/wavesurfer-js/wavesurfer.js/discussions/categories/q-a) 中提问。

不过请记住，该论坛专用于 wavesurfer 相关的问题。如果你是 JavaScript 新手，需要了解导入 NPM 模块等基础内容，请先考虑向 ChatGPT 或 StackOverflow 求助。

### 常见问题解答

<details>
  <summary>我遇到了 CORS 问题</summary>
  Wavesurfer 会从你指定的 URL 获取音频以便解码。请确保该 URL 允许从你的域名获取数据。在浏览器端的 JavaScript 中，你只能从<b>同源域名</b>获取数据，或者从另一个域名获取数据——但前提是该域名启用了 <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS">CORS</a>。因此，如果你的音频文件位于外部域名，请务必确保该域名返回了正确的 Access-Control-Allow-Origin 响应头。在请求方（也就是你的 JS 代码）这边你对此无能为力。
</details>

<details>
  <summary>wavesurfer 支持大文件吗？</summary>
  由于 wavesurfer 完全在浏览器端使用 Web Audio 解码音频，较大的音频片段可能因内存限制而解码失败。我们建议对大文件使用预解码的峰值数据（参见<a href="https://wavesurfer.xyz/examples/?predecoded.js">此示例</a>）。你可以使用 <a href="https://codeberg.org/chrisn/audiowaveform">audiowaveform</a> 这样的工具来生成峰值数据。
</details>

<details>
  <summary>那流式音频呢？</summary>
  流式音频仅在使用<a href="https://wavesurfer.xyz/examples/?predecoded.js">预解码的峰值数据与时长</a>时才受支持。
</details>

<details>
  <summary>我的音频与波形之间出现了偏差，该如何修复？</summary>
  如果你使用的是 VBR（可变比特率）音频文件，音频与波形之间可能会出现偏差。可以通过将文件转换为 CBR（恒定比特率）来解决。
  <p>或者，你也可以使用更为准确的 <a href="https://wavesurfer.xyz/examples/?webaudio-shim.js">Web Audio 垫片（shim）</a>。</p>
</details>

<details>
  <summary>如何将 wavesurfer.js 连接到 Web Audio 音效？</summary>
  一般来说，wavesurfer.js 并不打算成为 Web Audio 一切功能的封装器。它只是一个带波形可视化的播放器。它确实允许通过导出其音频元素，将自己连接到 Web Audio 音频图（参见<a href="https://wavesurfer.xyz/examples/?4436ec40a2ab943243755e659ae32196">此示例</a>），但仅此而已。请不要指望 wavesurfer 能够剪辑、添加音效或以任何方式处理你的音频。
</details>

## 开发

要进行开发，请按照以下步骤操作：

 1. 安装开发依赖：

```
yarn
```

 2. 以监听模式启动 TypeScript 编译器并启动一个 HTTP 服务器：

```
yarn start
```

该命令会在你的浏览器中打开 http://localhost:9090，并支持热重载，让你在开发过程中实时看到改动效果。

## 测试

测试使用 Cypress 框架编写。它们混合了端到端测试与视觉回归测试。

要在本地运行测试套件，首先构建项目：
```
yarn build
```

然后启动测试：
```
yarn cypress
```

## 反馈

我们很重视你的反馈与贡献！

如果你遇到任何问题或有改进建议，欢迎在[论坛](https://github.com/wavesurfer-js/wavesurfer.js/discussions/categories/q-a)中发帖。

希望你喜欢使用 wavesurfer.js，也期待听到你使用该库的体验！
