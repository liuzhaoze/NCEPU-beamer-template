# NCEPU-beamer-template

> 华北电力大学 Beamer/LaTeX 汇报模板

如需插入 SVG 矢量图，请先下载 [Inkscape](https://inkscape.org/zh-hans/) ；否则注释掉 main.tex 中与 svg 设置部分的代码。

可下载 main.pdf 预览效果

推荐使用 VSCode 的 [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop) 插件进行编辑，使用 [tex-fmt](https://github.com/WGUNDERWOOD/tex-fmt?tab=readme-ov-file#cargo) 进行代码格式化。在 `settings.json` 中启用 `tex-fmt`：

```json
"latex-workshop.formatting.latex": "tex-fmt"
```

## minted 包编译错误修复

如果你使用了 `minted` 包，并且出现了如下编译错误，请在编译选项中添加 `-shell-escape` 选项

Error message:

```text
Package minted Error: You must invoke LaTeX with the -shell-escape flag.
```

如果你是用的是 VSCode 的 [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop) 插件，可以在 `settings.json` 中添加如下配置：

```json
"latex-workshop.latex.tools": [
    {
        "name": "latexmk",
        "command": "latexmk",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "-pdf",
            "-shell-escape", // <-- add this line
            "-outdir=%OUTDIR%",
            "%DOC%"
        ],
        "env": {}
    }
]
```

## 设置字体

> 注意：在 Windows 系统下，安装字体时需要选择**为所有用户安装**，否则可能会出现字体找不到的问题。

设置字体所需要的包 `fontspec` 不支持 pdfLaTeX，只支持 XeLaTeX 和 LuaLaTeX 。因此我们需要将 [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop) 插件的默认编译器设置为 XeLaTeX 或 LuaLaTeX 。这里以 XeLaTeX 为例，`settings.json` 配置如下：

```json
// 设置默认编译器为 XeLaTeX
"latex-workshop.latex.recipe.default": "latexmk (xelatex)",
// 给 XeLaTeX 添加 -shell-escape 选项
"latex-workshop.latex.tools": [
    // pdfLaTeX 配置项
    {
        "name": "latexmk",
        "command": "latexmk",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "-pdf",
            "-shell-escape", // <-- add this line
            "-outdir=%OUTDIR%",
            "%DOC%"
        ],
        "env": {}
    },
    // XeLaTeX 配置项
    {
        "name": "xelatexmk",
        "command": "latexmk",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "-xelatex",
            "-shell-escape", // <-- add this line
            "-outdir=%OUTDIR%",
            "%DOC%"
        ],
        "env": {}
    }
]
```

模板中使用的字体的获取方法：

- HarmonyOS Sans : <https://developer.huawei.com/consumer/cn/design/resource-V1/>
- CodeNewRoman Nerd Font : <https://www.nerdfonts.com/font-downloads>

## 使用 pympress 显示演讲备注

如果希望在放映时显示类似 PowerPoint 的演讲者视图，可以使用 [pympress](https://github.com/Cimbali/pympress) 放映 Beamer 生成的 PDF。

在导言区加入：

```latex
\usepackage{pgfpages}
\setbeameroption{show notes on second screen=right}
```

使用 `pympress main.pdf` 命令放映，快捷键 `s` 可以切换屏幕顺序。

### 普通备注

```latex
\begin{frame}{研究背景}
  \begin{itemize}
    \item Serverless computing simplifies application deployment.
    \item Resource management remains challenging.
  \end{itemize}

  \note{
    这一页先介绍 serverless computing 的基本优势，
    然后引出资源管理中的性能与成本权衡问题。
  }
\end{frame}
```

### 以项目符号显示备注

```latex
\begin{frame}{研究动机}
  \begin{itemize}
    \item 工作流函数之间存在数据依赖。
    \item 资源分配会影响执行时间和运行成本。
  \end{itemize}

  \note[item]{先说明 serverless workflow 的执行特点。}
  \note[item]{再说明资源分配决策的重要性。}
  \note[item]{最后引出本文的优化目标。}
\end{frame}
```

### 逐步显示内容的备注

```latex
\begin{frame}{算法流程}
  \begin{itemize}
    \item<1-> 状态观测
    \item<2-> 动作选择
    \item<3-> 奖励反馈
  \end{itemize}

  \note<1>{这一阶段说明智能体如何观测当前系统状态。}
  \note<2>{这一阶段说明智能体如何选择资源分配动作。}
  \note<3>{这一阶段说明奖励如何反映性能和成本。}
\end{frame}
```

如果只是生成提交或分享用的干净版 PDF，可以将备注隐藏：

```latex
\setbeameroption{hide notes}
```
