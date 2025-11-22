# Number Adder for Obsidian

**Number Adder** 是一个为 Obsidian 笔记提供自动编号功能的插件。它能够根据文档结构，自动为**标题 (Headings)** 和 **数学公式 (Math Equations)** 添加有序编号。

> 本项目基于 **[Number Headings Obsidian](https://github.com/onlyafly/number-headings-obsidian)** 开发。
> 核心逻辑归功于原作者 **onlyafly**。本项目在此基础上进行了定制化修改（公式编号、中文支持、智能排除），旨在满足特定的文档排版需求。
>
> This project is a fork/enhanced version based on **[Number Headings Obsidian](https://github.com/onlyafly/number-headings-obsidian)**. All credits for the core logic go to **onlyafly**. This version includes customizations such as Chinese numbering support, advanced formula depth control, and robust code-block exclusion.

## ✨ 主要功能 (Features)

1.  **智能标题编号**：

      * 支持自动为 H1-H6 标题添加层级编号（如 `1.1`, `1.1.1`）。
      * **多样式支持**：支持阿拉伯数字 (`1`)、字母 (`A`, `a`)、**中文数字 (`一`)** 以及 **带圈数字 (`①`)**。

2.  **公式自动编号 (增强版)**：

      * 自动扫描 MathJax 块 (`$$...$$`) 并添加 `\tag{}`。
      * **连续模式**：全文公式统一计数（如 `(1)`, `(2)`）。
      * **章节模式**：基于最近标题的层级编号（如 `(1.1-1)`, `(1.1-2)`）。
      * **[新增] 层级深度限制**：在章节模式下，可设定索引的最大深度（例如限制为 4）。若公式位于 H5/H6 下，将强制基于其所属的 H4 父标题进行编号，避免编号过长。

3.  **稳健性与智能排除 [新增]**：

      * **代码块保护**：自动识别并跳过 Markdown 代码块（` ``` `）中的内容，防止误伤 LaTeX 教学示例或脚本代码。
      * **表格保护**：自动跳过表格行（以 `|` 开头），防止破坏表格结构。

4.  **非侵入式自动更新**：

      * 采用 `blur`（失焦）触发机制：当您暂停写作切换窗口或点击外部时，插件才会更新编号，避免打断写作思路或造成光标跳动。
      * 支持自定义**毫秒级**刷新延迟。

5.  **高度可配置**：

      * 通过命令面板或设置页通过可视化界面调整配置。
      * 配置自动保存到文档的 FrontMatter (YAML) 中，确保每篇文档的独立性。

## 📥 安装 (Installation)

由于本项目是手动构建版本，请按照以下步骤安装：

1.  进入您的 Obsidian 仓库目录：`.obsidian/plugins/`。
2.  新建文件夹 `number-adder-obsidian`。
3.  将 `main.js`, `manifest.json`, `styles.css` 放入该文件夹。
4.  重启 Obsidian，在“第三方插件”设置中启用 **Number Adder**。

## 🚀 使用指南 (Usage)

### 1\. 基础使用

按下 `Cmd/Ctrl + P` 打开命令面板，输入 **"Numbering Control"** 并回车，即可打开**编号控制面板**。

在面板中，您可以：

  * **手动编号**：点击按钮立即为当前文档的标题或公式生成编号。
  * **移除编号**：一键清除所有由插件生成的编号。
  * **自动编号设置**：开启/关闭自动编号，并调整样式。

### 2\. 自动编号设置 (Settings)

您可以在插件设置页或控制面板中调整以下参数：

  * **编号范围**：设置起始层级（如从 H2 开始）和最大层级。
  * **样式自定义**：
      * `1` : 阿拉伯数字 (1, 2, 3...)
      * `A` / `a` : 英文字母 (A, B, C...)
      * `一` : 中文数字 (一, 二, 三...) *[本版特色]*
      * `①` : 带圈数字 (①, ②, ③...) *[本版特色]*
  * **公式编号模式**：
      * `Continuous`: 连续编号。
      * `Heading-based`: 基于章节。
      * **Heading-based 最大索引层级**：*(仅在 heading-based 模式显示)* 设置公式引用的最大标题深度（1-6）。例如设为 `4`，则即使在 H5 章节下，公式也会显示为 `(H4编号-序号)`。

### 3\. FrontMatter 配置

插件会将配置以紧凑格式保存在文档顶部的 YAML 区域。新增的公式深度设置也会同步在此处：

```yaml
---
number headings: auto, 1-6, 111111, ....., 111111
number formulas: auto, heading-based(4)
---
```

*(注意：`heading-based(4)` 中的数字代表最大索引深度)*

## ⚙️ 开发者说明 (Development)

本插件使用 TypeScript 编写（源码编译为 JS）。

  * **核心逻辑**：基于栈（Stack）的层级追踪算法。
  * **稳健性设计**：利用 Obsidian `CachedMetadata` API 获取代码块区域，在编号扫描时通过“排除区域”检查，确保代码块和表格内的 `$$` 或 `#` 不会被错误处理。
  * **性能优化**：使用了 `replaceRangeEconomically` 和 `editor.transaction`，确保只在内容发生实际变化时写入，并合并撤销（Undo）历史。
  * **中文支持**：在 `main.js` 中内置了中文数字映射表 (`chineseNumbers`)。
