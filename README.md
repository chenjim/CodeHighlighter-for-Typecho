# CodeHighlighter-for-Typecho

基于 PrismJS 的代码语法高亮插件 for Typecho，可显示语言类型、行号，并支持一键复制代码到剪贴板。

- 本仓库（fork，维护中）：<https://github.com/chenjim/CodeHighlighter-for-Typecho>
- 上游原仓库（原作者 Copterfly，已停更）：<https://github.com/Copterfly/CodeHighlighter-for-Typecho>

> 本仓库为 fork。在原作者版本的基础上，把内置的 PrismJS 从 **1.14.0 升级到 1.30.0**，扩充支持语言并重建主题样式。重建方式见文末「资产重建说明」。

## 起始

本插件基于 [PrismJS](https://prismjs.com/) 为 `Typecho` 提供代码语法高亮。

可显示语言类型、行号，有复制功能。（请勿与其它同类插件同时启用，以免互相影响）

**兼容性**：已在 Typecho 1.3.0 + PHP 8.2 实测通过；Typecho 1.1 亦可使用，其它版本请自行尝试。

## 使用方法

第 1 步：下载本插件，解压，放到 `usr/plugins/` 目录中；

第 2 步：文件夹名改为 `CodeHighlighter`；

第 3 步：登录管理后台，激活插件；

第 4 步：设置：选择主题风格、是否显示行号等。

**代码写法**（Typecho 编辑器需以 `<!--markdown-->` 开头启用 Markdown）：

````
```javascript
// 语言类型写在围栏后，必填
// codes go here
```
````

## 重要说明

### 可设置项

**1. 选择高亮主题风格**（8 种）

- `coy.css`
- `dark.css`
- `default.css`
- `funky.css`
- `okaikia.css`（默认选中；对应 Prism 的 okaidia 主题，文件名沿用原插件拼写）
- `solarized-light.css`
- `tomorrow-night.css`
- `twilight.css`

**2. 是否在代码左侧显示行号**（默认开启）

### 支持的语言与自定义

本仓库内置 **51 种语言**（原版为 21 种）：

`markup` `css` `clike` `javascript` `apacheconf` `bash` `c` `cpp` `java` `csharp` `aspnet` `coffeescript` `markup-templating` `php` `smarty` `git` `less` `markdown` `nginx` `sql` `python` `kotlin` `groovy` `gradle` `json` `json5` `yaml` `toml` `ini` `diff` `docker` `cmake` `makefile` `sass` `scss` `nasm` `go` `rust` `ruby` `swift` `powershell` `http` `graphql` `protobuf` `objectivec` `wasm` `regex` `uri` `typescript` `jsx` `tsx`

另做了两个别名映射：`jsonc → json`、`asm → nasm`。

内置的 4 个 Prism 插件：`line-numbers`、`toolbar`、`show-language`、`copy-to-clipboard`。

如需自行定制，可用 Prism 官方下载器勾选主题/语言/插件后，替换 `static/prism.js` 与 `static/styles/<风格名>.css`。下载器配置链接见文末。

## 资产重建说明

本仓库的 `static/prism.js` 与 `static/styles/*.css` 由 **PrismJS 1.30.0**（npm 包 `prismjs@1.30.0`）重新构建，替换了原先打包的 1.14.0 版本。步骤：

1. 取源码：`npm install prismjs@1.30.0`
2. `static/prism.js` = `components/prism-core.min.js` + 各语言组件 `components/prism-<lang>.min.js`（按依赖顺序拼接）+ 4 个插件
   - 语言顺序（被依赖者在前）：core → `markup` `css` `clike` `javascript` → 其余语言 → `typescript` `jsx` `tsx`（`jsx` 依赖 `markup`+`javascript`，`tsx` 依赖 `jsx`+`typescript`）
   - 插件：`plugins/line-numbers`、`plugins/toolbar`、`plugins/show-language`、`plugins/copy-to-clipboard`（`toolbar` 必须先于后两者，它们要向 toolbar 注册按钮）
   - 文件末尾追加别名：`Prism.languages.jsonc = Prism.languages.json;`、`Prism.languages.asm = Prism.languages.nasm;`
3. 每个 `static/styles/<风格名>.css` = 对应主题 CSS + `plugins/line-numbers/prism-line-numbers.min.css` + `plugins/toolbar/prism-toolbar.min.css`

主题文件名对应关系（保留原插件命名，含 `okaikia` 拼写）：

| 本仓库文件 | PrismJS 主题 |
|---|---|
| `coy.css` | `prism-coy` |
| `dark.css` | `prism-dark` |
| `default.css` | `prism`（默认主题） |
| `funky.css` | `prism-funky` |
| `okaikia.css` | `prism-okaidia` |
| `solarized-light.css` | `prism-solarizedlight` |
| `tomorrow-night.css` | `prism-tomorrow` |
| `twilight.css` | `prism-twilight` |

## 更新记录

- **2026-10-09**：PrismJS 1.14.0 → 1.30.0；支持语言 21 → 51 种；新增 `jsonc`/`asm` 别名；8 套主题样式按 1.30.0 重建；修正失效链接并补充本说明。
- **1.0.0**：原作者 Copterfly 初始版本。

## 联系与授权

原作者：Copterfly。本 fork 的维护与问题反馈请走本仓库 Issues：<https://github.com/chenjim/CodeHighlighter-for-Typecho/issues>

> 注：原作者网站 `copterfly.cn` 已无法访问，原 README 中的博客链接与示例插图已一并移除。

Prism 下载器当前配置（对应本仓库内置的主题/语言/插件）：

<https://prismjs.com/download.html#themes=prism-okaidia&languages=markup+css+clike+javascript+apacheconf+bash+c+cpp+java+csharp+aspnet+coffeescript+markup-templating+php+smarty+git+less+markdown+nginx+sql+python+kotlin+groovy+gradle+json+json5+yaml+toml+ini+diff+docker+cmake+makefile+sass+scss+nasm+go+rust+ruby+swift+powershell+http+graphql+protobuf+objectivec+wasm+regex+uri+typescript+jsx+tsx&plugins=line-numbers+toolbar+show-language+copy-to-clipboard>
