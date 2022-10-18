> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_34496005/article/details/89398987)

## 一、什么是 wkhtmltopdf？
==================

wkhtmltopdf 是一个开源的，使用 Qt WebKit 渲染引擎，把 html 转换为 pdf 文件的命令行工具。wkhtmltopdf 还有一个双胞胎兄弟 wkhtmltoimage，顾名思义，它可以把 html 转换为 image 图片。

简单的讲，wkhtmltopdf 用于把网页转换成 pdf 文件。

详情可以参考：  
[官网](https://wkhtmltopdf.org/)  
[github 地址](https://github.com/wkhtmltopdf/wkhtmltopdf)

* * *

## 二、简单使用
======

### 1. 下载

下载对应操作系统的安装文件：[下载地址](https://wkhtmltopdf.org/downloads.html)

### 2. 使用

安装成功后就可以通过命令来直接使用了，它的命令格式为

```
wkhtmltopdf [GLOBAL OPTION]... [OBJECT]... <output file>

```

`[GLOBAL OPTION]` 是用来添加全局参数的区域，`[OBJECT]` 是用来添加 wkhtmltopdf 对象 (wkhtmltopdf 把对象分为 3 类，后文有详细内容) 的区域，如果有多个对象，则按顺序放入。`<output file>`则用以指定输出的 PDF 文件。

### 3. 示例

使用默认参数，把 csdn 网页转为 pdf 的示例如下：

```
➜  ~ wkhtmltopdf https://www.csdn.net /Users/kevin/Desktop/csdn.pdf
Loading pages (1/6)
QFont::setPixelSize: Pixel size <= 0 (0)                     ] 61%
Counting pages (2/6)
Resolving links (4/6)
Loading headers and footers (5/6)
Printing pages (6/6)
Done
➜  ~

```

* * *

## 三、对象和参数详解
=========

### 三类对象
----

命令 `wkhtmltopdf [GLOBAL OPTION]... [OBJECT]... <output file>` 中，OBJECT 表示一个对象。在 wkhtmltopdf 中，wkhtmltopdf 可以把多个对象输出到文件中，其中每个对象可以是页面对象，封面对象和大纲对象。对象按照命令行中的顺序，输出到 pdf 的结果文件里。

#### 1. 页面对象

页面对象使用以下方式将单个网页的内容放入输出文档中

```
(page)? <input url/file name> [PAGE OPTION]...

```

`(page)?` 表示 page 可以省略不写，即在不标明的时候，默认为页面对象。为页面对象所设置的参数可以被放置在全局参数域 ([GLOBAL OPTIONS]) 和页面参数域 ([PAGE OPTIONS]) 中，程序会根据实际情况在所有参数中找到合适的参数应用到页面、页眉和页脚。

#### 2. 封面对象

封面对象可以简单理解为是特殊的页面对象，能在页面对象上使用的参数，在封面对象上也都能使用。  
区别在于：

1.  封面对象输出的页面不会在 TOC 中出现
2.  封面对象输出的页面不会包含页眉和页脚

封面对象的使用方式为

```
cover <input url/file name> [PAGE OPTION]...

```

#### 3. 大纲对象

toc 是 table of content 的缩写，就是目录的意思，该对象会在输出文档中加入目录。其使用方式为

```
toc [TOC OPTION]...

```

所有能够在页面对象中使用的参数都可以用到大纲对象，并且还有许多的针对大纲对象的参数可以应用到大纲对象中。大纲是通过 XSLT 生成的，这意味着它可以被定义成任何你想看到的样子。可以通过命令行参数 --dump-default-toc-xsl 输出默认的 XSLT 文档，通过 --dump-outline 命令行参数也可指定以 XML 格式输出当前处理文档的目录到指定文件。后文会有详细的参数介绍

#### 五类参数
----

**注**：以下参数信息基于 wkhtmltopdf 2018 年 6 月 11 号发布的 0.12.5 版本，其他版本可以到官网查看具体信息

命令行选项有两种：  
选项可以基于每个对象或在全局中指定选项区域。  
“全局选项” 部分中的选项只能放置在全局选项区域  
而额外的命令行选项可以为每一个对象指定或在全局区域（GLOBAL OPTION）中进行全局指定

下列说明中的全局选项只能被放置在全局区域

##### 1. 全局参数

| 参数 | 说明 |
| :-- | :-- |
| `--`collate | 输出多个副本时进行校验 (默认设置) |
| `--`no-collate | 输出多个副本时不进行校验 |
| `--`cookie-jar `<path>` | 从提供的 cookie jar 文件中读取和写入 cookie |
| `--`copies `<number>` | 要打印到 PDF 文件中的副本数 (默认值为 1) |
| `-`d, `--`dpi `<dpi>` | 显式更改 dpi(dpi 即分辨率，但此参数对基于 x11 的系统没有影响，默认值为 96) |
| `-`H, `--`extended-help | 显示更详细的帮助，即更详细版本的 -h 命令 |
| `-`g, `--`grayscale | 以灰度的形式生成 PDF(彩色的更占空间，另外，有些特殊需求可能也只要灰度的 PDF) |
| `-`h, `--`help | 显示帮助文档 |
| `--`htmldoc | 以 html 的形式输出帮助文档 |
| `--`image-dpi `<integer>` | 当页面嵌入图像时，将它们缩小到指定的 dpi 尺寸 (默认值 600) |
| `--`image-quality `<integer>` | 当使用 jpeg 算法压缩图像时，使用指定的图片质量 (默认值 94) |
| `--`license | 输出授权信息并退出 |
| `--`log-level `<level>` | 在以下级别中指定日志级别：none, error, warn，info(默认) |
| `-`l, `--`lowquality | 生成低质量的 PDF/PS，对缩小结果文档大小，节约空间有较好帮助 |
| `--`manpage | 输出程序的手册页 |
| `-`B, `--`margin-bottom `<unitreal>` | 设置页面下边距 |
| `-`L, `--`margin-left `<unitreal>` | 设置页面左边距 (默认 10mm) |
| `-`R, `--`margin-right `<unitreal>` | 设置页面右边距 (默认 10mm) |
| `-`T, `--`margin-top `<unitreal>` | 设置页面上边距 |
| `-`O, `--`orientation `<orientation>` | 将方向设置为 “横向(Landscape)” 或“纵向 (Portrait)” 模式，默认是纵向(Portrait) |
| `--`page-height `<unitreal>` | 页面高度 |
| `-`s, `--`page-size `<Size>` | 设置页面大小为 A4(默认值)，Letter 等 |
| `--`page-width `<unitreal>` | 页面宽度 |
| `--`no-pdf-compression | 不对 PDF 使用无损压缩算法 (无损压缩不会对文件造成很大损失，此参数非必要不使用) |
| `-`q, `--`quiet | 减少冗长，保持向后兼容性；与使用–log level none 相同，即静态模式，不在标准输出中打印任何信息 |
| `--`read-args-from-stdin | 从 stdin[1](#fn1) 读取命令行参数 |
| `--`readme | 输出程序的 readme 文档 |
| `--`title `<text>` | 生成的 PDF 文件的标题（如果未指定，则使用第一个文档的标题） |
| `-`V, `--`version | 输出版本信息并退出 |

##### 2. 大纲参数

注：PDF 的大纲会根据 HTML 的标题和标签自动生成

| 参数 | 说明 |
| :-- | :-- |
| `--`dump-default-toc-xsl | 输出默认的 TOC xsl 样式的 "大纲" 到标准输出 (文件内容为 xml)[2](#fn2) |
| `--`dump-outline `<file>` | 输出 “大纲” 到指定的文件(文件内容也为 xml) |
| `--`outline | 在生成的 PDF 文档中输出 “大纲”(默认) |
| `--`no-outline | 不在 PDF 中输出 “大纲” |
| `--`outline-depth `<level>` | 设置生成大纲的深度 (默认值 4) |

##### 3. 页面参数

| 参数 | 说明 |
| :-- | :-- |
| `--`allow `<path>` | 允许加载指定文件夹中的一个或多个文件 (可重复 [3](#fn3)) |
| `--`background | 输出页面背景到 PDF 文档 (这是默认设置) |
| `--`no-background | 与`-background`相反 |
| `--`bypass-proxy-for `<value>` | 绕过主机代理 (可重复 [3](#fn3)) |
| `--`cache-dir `<path>` | 网页的缓存目录 |
| `--`checkbox-checked-svg `<path>` | 使用指定的 SVG 文件渲染选中的复选框 |
| `--`checkbox-svg `<path>` | 使用指定的 SVG 文件渲染未选中的筛选框 |
| `--`cookie `<name>` `<value>` | 设置访问网页时的 cookie(可重复 [3](#fn3))，值应该是 URL 编码的 |
| `--`custom-header `<name>` `<value>` | 设置访问网页时的 HTTP 头信息 (可重复 [3](#fn3)) |
| `--`custom-header-propagation | 为每个要加载的资源添加由 --custom-header 指定的 HTTP 头 |
| `--`no-custom-header-propagation | 与`--custom-header-propagation`相反 |
| `--`debug-javascript | 显示 javascript 调试输出的信息 |
| `--`no-debug-javascript | 与`--debug-javascript`相反 (这是默认设置) |
| `--`default-header | 添加一个默认的 “头”，在页面的左头显示页面的名字，在页面的右头显示页码 [4](#fn4) |
| `--`encoding `<encoding>` | 为输入的文本设置默认的编码方式 |
| `--`disable-external-links | 禁止页面中的外链生成超链接 |
| `--`enable-external-links | 与`--disable-external-links|`相反 (这是默认设置) |
| `--`disable-forms | 不转换 HTML 表单为 PDF 表单 (这是默认设置) |
| `--`enable-forms | 与`--disable-forms`相反 |
| `--`images | 加载图片并输出到 PDF 文档 (这是默认设置) |
| `--`no-images | 与`--images`相反 |
| `--`disable-internal-links | 禁止页面中的内链生成超链接 |
| `--`enable-internal-links | 与`--disable-internal-links|`相反 (这是默认设置) |
| `-`n, `--`disable-javascript | 禁止 WEB 页面执行 javascript |
| `--`enable-javascript | 允许 WEB 页面执行 javascript(这是默认设置) |
| `--`javascript-delay `<msec>` | 延迟一定的毫秒等待 javascript 执行完成 (默认值是 200) |
| `--`keep-relative-links | 将相对外部链接保留为相对外部链接 |
| `--`load-error-handling `<handler>` | 指定当页面加载失败后的动作，可以指定为：abort(中止，默认值)、ignore(忽略)、skip(跳过) |
| `--`load-media-error-handling `<handler>` | 指定当媒体文件加载失败后的动作，可以指定为：abort(中止)、ignore(忽略，默认值)、skip(跳过) |
| `--`disable-local-file-access | 不允许一个本地文件加载其他的本地文件，使用命令行参数 `--allow` 指定的目录除外。 |
| `--`enable-local-file-access | 与`--disable-local-file-access`相反 (这是默认设置) |
| `--`minimum-font-size `<int>` | 设置最小的字号，除非必要不推荐使用该参数 |
| `--`exclude-from-outline | 拒绝加载当前页面到 PDF 文档的目录和大纲中 |
| `--`include-in-outline | 与`--exclude-from-outline`相反 (这是默认设置) |
| `--`page-offset `<offset>` | 设置页码的起始值 (默认值为 0) |
| `--`password `<password>` | HTTP 身份认证的密码 |
| `--`disable-plugins | 禁止使用插件 (这是默认设置) |
| `--`enable-plugins | 允许使用插件，但插件可能并不工作 |
| `--`post `<name>` `<value>` | 添加一个 POST[^5] 字段 (可重复 [3](#fn3)) |
| `--`post-file `<name>` `<value>` | 添加一个 POST[^5] 文件 (可重复 [3](#fn3)) |
| `--`print-media-type | 用显示媒体类型代替屏幕 |
| `--`no-print-media-type | 与`--print-media-type`相反 (这是默认设置) |
| `-`p, `--`proxy `<proxy>` | 使用代理 |
| `--`proxy-hostname-lookup | 使用代理解析主机名 |
| `--`radiobutton-checked-svg `<path>` | 使用指定的 SVG 文件渲染选中的单选框 |
| `--`radiobutton-svg `<path>` | 使用指定的 SVG 文件渲染未选中的单选框 |
| `--`resolve-relative-links | 将相对外部链接解析为绝对链接 (默认) |
| `--`run-sript `<js>` | 页面加载完成后执行一个附加的 JS 文件 [^6]，可以重复使用此参数指定，多个要在页面加载完成后要执行的 JS 文件 (可重复 [3](#fn3)) |
| `--`disable-smart-shrinking | 禁用 WebKit 使用智能收缩策略，该策略使像素 / dpi(分辨率) 比率不为常量 |
| `--`enable-smart-shrinking | 与`--disable-smart-shrinking`相反 (这是默认设置) |
| `--`ssl-crt-path `<path>` | openssl-pem 格式的 ssl 客户机证书公钥的路径，可选后跟中间 CA 和受信任证书 |
| `--`ssl-key-password `<password>` | SSL 客户端证书私钥的密码 |
| `--`ssl-key-path `<path>` | openssl pem 格式的 ssl 客户机证书私钥路径 |
| `--`stop-slow-scripts | 停止运行缓慢的 javascript 代码 (这是默认设置) |
| `--`no-stop-slow-scripts | 与`--stop-slow-scripts`相反 |
| `--`disable-toc-back-links | 禁止从标题链接到目录 (toc)(这是默认设置) |
| `--`enable-toc-back-links | 与`--disable-toc-back-links`相反 |
| `--`user-style-sheet `<url>` | 设置一个在每个页面都加载的用户自定义样式表 |
| `--`username `<username>` | HTTP 身份验证的用户名 |
| `--`viewport-size <> | 如果有自定义滚动条或 CSS 属性溢出以模拟窗口大小时，需要设置窗口大小 |
| `--`window-status `<windowStatus>` | 在渲染呈现页面之前，等待 window.status 等于这个指定的字符串 |
| `--`zoom `<float>` | 设置转换成 PDF 时页面的缩放比例 (默认值是 1) |

##### 4. 页眉、页脚参数

**页眉参数**

| 参数 | 说明 |
| :-- | :-- |
| `--`header-center `<text>` | 在页眉的居中部分显示页眉文本 `<text>` |
| `--`header-font-name `<name>` | 设置页眉的字体 (默认为 Arial) |
| `--`header-font-size `<size>` | 设置页眉的字体大小 (默认为 12) |
| `--`header-html `<url>` | 添加一个 html 作为页眉 |
| `--`header-left `<text>` | 在页眉的居左部分显示页眉文本 `<text>` |
| `--`header-line | 在页眉下方显示一条直线分隔正文 |
| `--`no-header-line | 与`--header-line`相反 (这是默认设置) |
| `--`header-right `<text>` | 在页眉的居右部分显示页眉文本 `<text>` |
| `--`header-spacing `<real>` | 页眉与正文之间的距离 (默认为零) |

**页脚参数**

| 参数 | 说明 |
| :-- | :-- |
| `--`footer-center `<text>` | 在页脚的居中部分显示页脚文本 `<text>` |
| `--`footer-font-name `<name>` | 设置页脚的字体 (默认为 Arial) |
| `--`footer-font-size `<size>` | 设置页脚的字体大小 (默认为 12) |
| `--`footer-html `<url>` | 添加一个 html 作为页脚 |
| `--`footer-left `<text>` | 在页脚的居左部分显示页脚文本 `<text>` |
| `--`footer-line | 在页脚上方显示一条直线分隔正文 |
| `--`no-footer-line | 与`--footer-line`相反 (这是默认设置) |
| `--`footer-right `<text>` | 在页脚的居右部分显示页脚文本 `<text>` |
| `--`footer-spacing `<real>` | 页脚与正文之间的距离 (默认为零) |

**共同参数**

| 参数 | 说明 |
| :-- | :-- |
| `--`replace `<name>` `<value>` | 将 [name] 替换为页眉和页脚中的值(可重复 [3](#fn3)) |

##### 5. TOC 参数

toc 是 table of content 的缩写，可以理解为目录的意思

| 参数 | 说明 |
| :-- | :-- |
| `--`disable-dotted-lines | 在目录中不使用虚线 |
| `--`toc-header-text `<text>` | 设置目录的页眉文本 |
| `--`toc-level-indentation `<width>` | 对于 toc 中的每个标题级别按此长度缩进 (默认为 1em) |
| `--`disable-toc-links | 在目录中不生成指向内容锚点的超链接 |
| `--`toc-text-size-shrink `<real>` | 在目录中每级标题的缩放比例 (默认为 0.8) |
| `--`xsl-style-sheet `<file>` | 使用自定义的 XSL 样式表显示目录内容 |

* * *

1.  标准输入 [↩︎](#fnref1)
    
2.  尝试一下，你会在命令行中看到的 [↩︎](#fnref2)
    
3.  可重复的意思是可以多次设置该参数，用以指定多个参数值 [↩︎](#fnref3) [↩︎](#fnref3:1) [↩︎](#fnref3:2) [↩︎](#fnref3:3) [↩︎](#fnref3:4) [↩︎](#fnref3:5) [↩︎](#fnref3:6) [↩︎](#fnref3:7)
    
4.  这相当于进行了如下四个设置：–header-left=’[webpage]’ --header-right=’[page]/[toPage]’ --top 2cm --header-line [↩︎](#fnref4)