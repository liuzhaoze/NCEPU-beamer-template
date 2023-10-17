# NCEPU-beamer-template

> 华北电力大学Beamer/LaTeX汇报模板

如需插入 SVG 矢量图，请先下载 [Inkscape](https://inkscape.org/zh-hans/) ；否则注释掉 main.tex 中与 svg 相关的包和配置（第 6 、15 、17 行）

可下载 main.pdf 预览效果

## Troubleshooting

如果你使用了 `mint` 包，并且出现了编译错误，请在编译选项中添加 `-shell-escape` 选项

Error message:

```text
Package minted Error: You must invoke LaTeX with the -shell-escape flag.
```

如果你是用的是 VSCode 的 LaTeX Workshop 插件，可以在 `settings.json` 中添加如下配置：

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
