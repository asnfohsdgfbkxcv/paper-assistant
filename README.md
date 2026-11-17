<h1 align="center">📝文本排版</h1>

<p align="center">
  本仓库是 <a href="https://github.com/laorange/paper-assistant">laorange/paper-assistant</a> 的修改版。<br/>
  原项目名为「论文工具」，由 <a href="https://github.com/laorange">laorange</a> 开发并以 AGPL-3.0 协议开源，在此向原作者致谢。
</p>

<p align="center">
  本修改版的主要改动：开启「自动复制输出」后，会在输出更新时立即复制到剪贴板，以适配新版 Edge/Chrome 在页面失焦后无法写入剪贴板的限制；去掉了原站顶部的升级公测横幅；并将产品名称改为「文本排版」。
</p>

<p align="center">
  <a href="https://asnfohsdgfbkxcv.github.io/paper-assistant/" target="_blank">在线使用</a>
  |
  <a href="https://github.com/asnfohsdgfbkxcv/paper-assistant" target="_blank">本仓库源码</a>
</p>

<p align="center">
  原项目：
  <a href="https://github.com/laorange/paper-assistant" target="_blank">源码</a>
  |
  <a href="https://laorange.github.io/paper-assistant/" target="_blank">在线使用(GitHub)</a>
  |
  <a href="https://laorange.gitee.io/paper-assistant" target="_blank">在线使用(Gitee)</a>
  |
  <a href="https://github.com/swjqq/utools-paper-assistant" target="_blank">uTools插件</a><a href="https://github.com/swjqq" target="_blank">(@swjqq)</a>
</p>

<p align="center">
  <a href="/src/assets/UpdatedLogs.md" target="_blank">更新日志</a>
  |
  <a href="https://www.bilibili.com/video/BV1ZG4y1N7oM/" target="_blank">原作者视频介绍</a>
  |
  <a href="http://qny.quoi.top/img/sponsor-qr.jpg" target="_blank">鼓励原作者</a>
</p>

## 功能

#### (1) 文本复制工具

😣在复制PDF中的文字时，有时会出现莫名其妙的空格、换行、全角字符或乱码。

😫如果手动删改，工作量巨大；如果直接全文替换，可能会误删英文单词间的空格。

💡原项目「论文工具」正是为了解决这些问题而开发。

🚩如何使用？可直接[在线使用](https://asnfohsdgfbkxcv.github.io/paper-assistant/)后点击"使用说明"，或前往B站查看[原作者视频介绍](https://www.bilibili.com/video/BV1ZG4y1N7oM/)。

#### (2) 英文语法检查

~~✍如果您在电脑上在线使用，在输入英文时，将为您检查语法 (基于[Grammarly](https://developer.grammarly.com/))。~~

😭Grammarly的开发者接口已于2024年1月10日废弃，目前该功能已无法使用，详情可见其[官网公告](https://developer.grammarly.com/)。

#### (3) 其他辅助功能

- 删除引用角标，如: [1], [2, 3], [4-7], (1), (2, 3), (4-7)...
- 全角字符转半角字符
- 批量替换字符
- 汉字繁简转换
- 中英文标点转换
- ······

## Q&A

#### 为什么不直接查找替换？

出现的若是空格的话，直接替换会误删英文单词中的空格；因此需要用到正则表达式来替换，有较高门槛。

#### 仅粘贴文本，复制到文本文档/记事本？

仅粘贴文本、复制到记事本是**去除格式**，但并不能去除空行空格乱码这样的干扰内容，因为这些是"内容"而不是"格式"。

#### 复制到浏览器地址栏？

浏览器地址栏本质上是一个**单行无格式输入框**，确实可以去除换行符，但并不能处理空格、乱码和全角字符。

#### 为什么不推荐使用OCR？

有条件复制粘贴的话还是不太建议用OCR，识别不准确的话会产生新的麻烦。

#### 未来考虑加入翻译功能吗？

不考虑。若需PDF翻译功能，建议使用[知云文献](http://www.zhiyunwenxian.cn/)。

#### 空行空格乱码等痛点是在哪些软件出现的？

空行素材来自**Acrobat**，空格素材来自**Zotero**，乱码素材来源于**CAJViewer**，全角字符素材来自**知云文献**，重复换行素材来源于**Typora**。

## 致开发者

😉为方便看代码的朋友，原作者把文本处理的方法都单独整理成了[一个文件](https://github.com/asnfohsdgfbkxcv/paper-assistant/blob/master/src/assets/ts/article-copy-tool/handlers.ts)，并写了注释、规范了接口：

```typescript
// handlers.ts
export interface TextHandler {
    activate: boolean,  // 默认是否启用
    description: string,  // 对该功能的描述
    executor: (text: string) => string  // 功能函数
}
```

😄若您有好的想法，可以在以上内容的基础上修改。欢迎在本仓库发起 [`pull request`](https://github.com/asnfohsdgfbkxcv/paper-assistant/pulls)。若要贡献给原项目，请前往 [`laorange/paper-assistant`](https://github.com/laorange/paper-assistant)。
