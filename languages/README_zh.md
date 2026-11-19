# VKT ReadPos — 读位

[English](../README.md) | 中文 | [日本語](README_ja.md) | [Deutsch](README_de.md) | [Español](README_es.md) | [Français](README_fr.md)

Chrome、Edge 与 Firefox 扩展：记住每个页面读到哪儿，并把「整页现场」保存下来随时恢复。

**阅读续读**服务「长文读到一半」；**现场快照**服务「操作页面做到一半」。
所有计算和存储都在本机完成，无账号、无同步服务、无埋点；唯一的联网请求是授权校验。

## 免费功能

- **按页记录阅读位置**：停止滚动 600ms 后写入一次，切走标签页/关闭页面时再写一次。不做逐像素写入。
- **续读浮标**：重新打开同一页面时，右下角出现「继续上次位置 · 62%」，点一下平滑滚回去。**绝不自动滚动**。
- **安静**：浮标数秒后自动淡出；鼠标移到右下角可以让它重新出现。除此之外不在页面上画任何东西。
- **短页面不打扰**：低于触发距离（默认 600px）的位置不提示。
- **当前站点暂停 / 清除本页记录**：Popup 里各一键。
- **最近 20 页历史**：Popup 列出位置和百分比，点一行即可重新打开。
- **SPA 路由分别记录**：`/app/inbox` 与 `/app/settings` 是两个页面；`#/route` 这类哈希路由也算。
- **懒加载兜底**：保存的位置超出当前文档高度时，恢复会重试两次，仍够不到就静默放弃——不跟页面较劲。
- **数据可携带**：JSON 导出 / 导入，免费。
- **恢复前先确认**：恢复现场前会列出将被覆盖的内容。默认开启，且免费——确认步骤不是付费点。

## Pro（9.99 美元一次性买断）

| | 免费 | Pro |
|---|---|---|
| 阅读位置记录 | ✅ | ✅ |
| 历史条数 | 20 页 | 500 页 |
| 续读浮标 | 固定时序 | 出现延迟 / 淡出时间 / 触发距离可调 |
| 忽略规则 | 域名 | 域名 **+** 正则表达式 |
| 页面现场（保存 + 恢复） | — | ✅ |
| 文本框 / 多行文本 / 下拉框 / 复选框 / 单选框 / 展开面板 | — | ✅ |
| 同一页面多个命名现场 | — | ✅ |
| 每页现场上限 | — | 可配（1–20） |
| 包含只读字段 | — | 可选 |
| 快捷键保存现场 | — | 可选（默认关闭） |
| JSON 导出 / 导入 | ✅ 位置 | ✅ 位置 + 现场 |

一个授权可在两台浏览器上激活；移除授权会释放名额。校验每两小时最多一次，离线可用缓存授权，校验失败也不会把你锁在外面。

## 现场快照覆盖哪些字段

| 元素 | 恢复内容 |
|---|---|
| `input` 文本类（text / email / url / tel / number / search / date…） | 值 |
| `textarea` | 值 |
| `select` | 选中项（找不到值就按选项文字匹配） |
| `checkbox` / `radio` | 选中状态 |
| `details` | 展开 / 折叠 |
| 滚动位置 | 与浮标相同的重试逻辑 |

写入走原生 setter 并派发 `input` / `change`，React / Vue 能感知。字段定位顺序是「结构路径 → id → name」，页面轻微重排也能恢复。

### 明确不做的

- **密码、文件、隐藏域、一次性验证码、卡号、CVV、IBAN、SSN**：在采集阶段就排除，不是恢复时才过滤，也没有开关可以打开。
- **只读 / 禁用字段**：默认跳过（Pro 可打开只读）；计算出来的字段放进现场只是噪音。
- **`role="tab"` 面板**：原规划里有，已删除。Tab 状态由框架驱动，写回 `aria-selected` 要么无效，要么触发跳转——看着能用实际不可靠的功能比没有更糟。
- **`contenteditable` 富文本编辑器**：没有既可靠又不破坏框架的写回方式，宁可留着不做。
- **Shadow DOM 内与跨域 iframe 内的字段**：跨这些边界建不出稳定路径，直接跳过而不是猜。

## 工作原理

1. 滚动任意页面，停止约 600ms 后位置按归一化 URL 写入扩展本地存储（`chrome.storage.local`，Firefox 上是 `browser.storage.local`）。
2. 再次回到该页面，若保存位置超过触发距离、且当前仍在顶部附近，浮标在 600ms 后出现。
3. 点它，平滑滚回原处；页面变高（懒加载图片）时会重试，仍不可达就静默。
4. Pro 可在 Popup 里「保存现场」（或按快捷键），恢复时先弹确认面板列出要覆盖的内容，再整体写回。

## 隐私

- 位置、现场、设置都在扩展本地存储（`chrome.storage.local`，Firefox 上是 `browser.storage.local`），不上传。
- 无统计、无埋点、无账号。
- 采集到的字段值不离开浏览器：恢复只是本地读 + 本地写。
- 唯一的网络请求是授权激活/校验，只在你输入授权码之后发生。

## 权限

| 权限 | 原因 |
|---|---|
| `storage` | 位置、现场、设置、授权状态，全部本地 |
| `http://*/*`、`https://*/*` | 内容脚本必须在**你滚动之前**就存在于每个页面，否则没有东西可记。与书签类工具不同，这个工具没法等到你逐站点授权——这也是它申请广泛页面访问权限的原因，商店说明里已明确写出。 |

没有 `tabs`、`scripting`、`debugger`、`activeTab`。Chrome 的 `chrome://`、Edge 的 `edge://`、Firefox 的 `about:`、`file://` 和其他扩展页面不运行任何脚本。

## 支持的浏览器

| 浏览器 | 构建在哪 | 最低版本 |
|---|---|---|
| Chrome | 本仓库 | Chrome 116 —— Manifest V3，`background.service_worker` |
| Edge | 本仓库，与 Chrome 同一个包 | Edge 116 |
| Firefox | `codeup/VKT-ReadPos` | Firefox 140 —— Manifest V3，`background.scripts` |

两个构建是同一个扩展：`common.js`、`content/content.js`、`popup/`、`_locales/`、图标与本文档都共用，一边导出的 JSON 另一边可直接导入。差异只有三处：清单（后台是脚本列表加载的事件页，而不是 service worker；另有 gecko id 与 AMO 要求的数据收集声明）、API 命名空间（Firefox 用 Promise 版的 `browser.*`，不是回调版 `chrome.*`），以及打包脚本（Firefox 版额外产出审核用源码 zip）。

用户能看到的唯一差别：可选快捷键要在 Firefox 的附加组件管理器里设置（about:addons → 齿轮图标 → 管理扩展快捷键），而不是 `chrome://extensions/shortcuts`。

## 开发

```bash
npm install         # 仅开发依赖：terser、puppeteer-core（截图）
npm test            # node --test，99 条断言
npm run build       # 压缩打包 → publish/readpos-v{version}.zip
npm run locales     # 由 tools/make-locales.js 重新生成六语言文件
npm run icons       # 重新生成图标
npm run shoot       # 生成商店截图（需要 Chrome）
```

## 链接

- 官网：https://annmax1983.com
- 授权与价格：https://annmax1983.com/checkout?plugin=readpos
