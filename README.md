# csreport LaTeX 报告模板

基于 XeLaTeX + ctex 的中文学术报告模板，提供封面、目录、代码高亮、三线表、引用块等开箱即用的排版组件。

## 编译方式

必须使用 **XeLaTeX** 编译。推荐使用 `latexmk`：

```bash
latexmk -xelatex main.tex
```

手动编译（含参考文献）：

```bash
xelatex main.tex
bibtex main
xelatex main.tex
xelatex main.tex
```

### 字体依赖

模板使用以下字体，编译前请确保系统已安装：

| 用途 | 字体名称 |
|------|---------|
| 英文正文 | TeX Gyre Termes |
| 等宽字体 | Maple Mono CN |
| 中文宋体 | Noto Serif CJK SC |
| 中文黑体 | Noto Sans CJK SC |

如需替换字体，修改 `csreport.sty` 中 `\setmainfont`、`\setmonofont`、`\setCJKmainfont` 等命令即可。

## 目录结构

```
report-example/
├── main.tex            # 主文件
├── csreport.sty        # 样式文件（无需修改）
├── references.bib      # 参考文献
├── sec/                # 章节文件夹
│   └── 1_example.tex   # 示例章节（含所有模块演示）
└── images/             # 图片文件夹
```

## 报告元信息设置

在 `main.tex` 中通过以下命令设置报告信息：

```latex
\reporttitle{项目报告标题}        % 报告标题
\reportsubtitle{项目副标题}       % 副标题
\reportauthor{姓名}              % 作者
\studentid{12345678}             % 学号
\supervisor{指导教师}             % 指导教师
\department{计算机科学与工程系}    % 院系（有默认值，可省略）
\university{示例大学}             % 学校
\reportdate{May 2026}            % 日期
\reportheader{计算机项目报告}      % 页眉左侧文字
\reporttype{计算机高级程序设计项目报告}  % 封面底部报告类型
```

## 封面与前置页

```latex
\makecsreporttitle    % 生成封面
\csabstract{摘要内容}  % 生成摘要页（可选）
\makefrontmatter      % 生成目录并切换为阿拉伯页码
```

## 模块用法

### 1. 代码块

所有 `lstlisting` 代码块自动包裹在圆角灰色背景框中，无需额外操作。

**带标题的代码块：**

```latex
\listingcaption{代码标题}
\begin{lstlisting}[language=C]
int main() {
    return 0;
}
\end{lstlisting}
\label{lst:my-code}
```

**不带标题的代码块：**

```latex
\begin{lstlisting}[language=Python]
print("hello")
\end{lstlisting}
```

**行内代码：**

```latex
使用 \texttt{printf()} 函数输出
```

**支持的常用语言参数：** `C`, `Python`, `Java`, `bash`, `CMake`, `SQL` 等（完整列表见 `listings` 文档）。

### 2. 图片

**标准图片：**

```latex
\begin{figure}[H]
    \centering
    \includegraphics[width=0.85\textwidth]{images/example.png}
    \caption{图片说明}
    \label{fig:example}
\end{figure}
```

**占位图片（图片未就绪时使用）：**

```latex
\placeholderfigure{图片标题}{描述文字}
```

**并排子图：**

```latex
\begin{figure}[H]
    \centering
    \begin{subfigure}[b]{0.45\textwidth}
        \centering
        \includegraphics[width=\textwidth]{images/a.png}
        \caption{子图 A}
        \label{fig:sub-a}
    \end{subfigure}
    \hfill
    \begin{subfigure}[b]{0.45\textwidth}
        \centering
        \includegraphics[width=\textwidth]{images/b.png}
        \caption{子图 B}
        \label{fig:sub-b}
    \end{subfigure}
    \caption{总标题}
    \label{fig:both}
\end{figure}
```

### 3. 表格

**三线表（推荐）：**

```latex
\begin{table}[H]
    \centering
    \caption{表格标题}
    \label{tab:example}
    \begin{tabular}{lrrr}
        \toprule
        \textbf{列 A} & \textbf{列 B} & \textbf{列 C} & \textbf{列 D} \\
        \midrule
        行 1 & 1.0 & 2.0 & 3.0 \\
        行 2 & 4.0 & 5.0 & 6.0 \\
        \bottomrule
    \end{tabular}
\end{table}
```

**自适应宽度表格（`tabularx`）：**

```latex
\begin{tabularx}{\textwidth}{lY}
    \toprule
    \textbf{名称} & \textbf{说明} \\
    \midrule
    模块 A & 这一列会自动填充剩余宽度 \\
    \bottomrule
\end{tabularx}
```

**多行合并（`multirow`）：**

```latex
\multirow{2}{*}{合并文本}
```

**自定义列类型：**

| 列类型 | 说明 |
|--------|------|
| `Y` | 自适应宽度左对齐（用于 `tabularx`） |
| `C{3cm}` | 指定宽度居中 |
| `L{3cm}` | 指定宽度左对齐 |

### 4. 数学公式

**行内公式：**

```latex
时间复杂度为 $O(n^3)$，空间复杂度为 $O(n^2)$。
```

**行间公式：**

```latex
$$
C_{ij} = \sum_{k=1}^{n} A_{ik} \cdot B_{kj}
$$
```

**对齐多行公式：**

```latex
\begin{align}
    f(x)   &= ax^2 + bx + c \\
    f'(x)  &= 2ax + b
\end{align}
```

### 5. 列表

**有序列表：**

```latex
\begin{enumerate}
    \item 第一项
    \item 第二项
\end{enumerate}
```

**无序列表：**

```latex
\begin{itemize}
    \item \textbf{关键词}：说明文字
    \item 普通条目
\end{itemize}
```

**紧凑列表（适用于环境配置等信息）：**

```latex
\begingroup
\setstretch{1}
\begin{itemize}
    \setlength{\itemsep}{0pt}
    \setlength{\parsep}{0pt}
    \setlength{\parskip}{0pt}
    \setlength{\topsep}{0pt}
    \item \textbf{操作系统}：Ubuntu 24.04
    \item \textbf{编译器}：gcc 13.3.0
\end{itemize}
\endgroup
```

### 6. 引用块（Markdown 风格）

左侧带灰色竖线的引用块，适用于补充说明和注意事项：

```latex
\begin{mdquote}
引用内容。支持 \textbf{粗体} 和 \textit{斜体} 等格式。\\

使用 \texttt{\textbackslash\textbackslash} 换行。
\end{mdquote}
```

### 7. 算法伪代码

```latex
\begin{algorithm}[H]
    \caption{算法名称}
    \label{alg:example}
    \begin{algorithmic}[1]
        \REQUIRE 输入参数
        \ENSURE 输出结果
        \FOR{$i = 1$ \TO $n$}
            \STATE 执行操作
        \ENDFOR
        \RETURN 结果
    \end{algorithmic}
\end{algorithm}
```

### 8. 交叉引用

```latex
如代码~\ref{lst:my-code} 所示    % 引用代码清单
如图~\ref{fig:example} 所示      % 引用图片
如表~\ref{tab:example} 所示      % 引用表格
如算法~\ref{alg:example} 所示    % 引用算法
```

注意：使用 `~` 连接文字与 `\ref`，避免引用编号被换行分隔。

### 9. 超链接

```latex
\href{https://github.com/example}{显示文字}    % 带显示文字的链接
\url{https://example.com}                      % 直接显示 URL
```

### 10. 中文字体命令

```latex
{\songti 宋体文本}
{\heiti 黑体文本}
{\kaishu 楷书文本}
{\fangsong 仿宋文本}
```

### 11. 参考文献

在 `references.bib` 中添加条目：

```bibtex
@book{key,
  title     = {书名},
  author    = {作者},
  year      = {2024},
  publisher = {出版社}
}
```

在正文中引用：

```latex
如文献~\cite{key} 所述
```

使用 `\nocite{*}` 可将 `.bib` 中所有条目都列入参考文献列表。

### 12. 章节文件组织

推荐将各章节放在 `sec/` 目录下，在 `main.tex` 中用 `\input` 引入：

```latex
\input{sec/1_intro.tex}
\input{sec/2_design.tex}
\input{sec/3_implementation.tex}
```

### 13. 附录

```latex
\appendix

\chapter{附录}
\section{补充材料}
附录内容...
```

## 颜色定义

模板预定义了以下颜色，可在正文中通过 `\color{name}` 或 `\textcolor{name}{text}` 使用：

| 名称 | RGB | 用途 |
|------|-----|------|
| `mainblue` | (20, 60, 120) | 章节标题、超链接 |
| `lightblue` | (235, 242, 252) | 浅蓝背景 |
| `codegray` | (245, 245, 245) | 代码块背景 |
| `commentgreen` | (0, 120, 60) | 代码注释 |
| `keywordblue` | (0, 70, 180) | 代码关键字 |
| `stringred` | (170, 30, 30) | 代码字符串 |
