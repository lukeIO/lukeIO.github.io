# Markdown

**Markdown**基本格式

# **标题**
```
# 一级标题
## 二级标题
### 三级标题
```
# 一级标题
## 二级标题
### 三级标题

# **列表项**
```
* 第一项
- 第二项
* 第三项
  * abcdef
    * opqrst
    * uvwxyz
  * abcdef
    * opqrst
    * uvwxyz
- 第四项
  - abcdef
    - opqrst
    - uvwxyz
  - abcdef
    - opqrst
    - uvwxyz
1. 第一项
2. 第二项
3. 第三项
```
* 第一项
- 第二项
* 第三项
  * abcdef
    * opqrst
    * uvwxyz
  * abcdef
    * opqrst
    * uvwxyz
- 第四项
  - abcdef
    - opqrst
    - uvwxyz
  - abcdef
    - opqrst
    - uvwxyz
1. 第一项
2. 第二项
3. 第三项

# **任务列表**
```
- [ ] 项目一
- [x] 项目二
```
- [ ] 项目一
- [x] 项目二

# **段落**
  段落之间用空行分割

  换行  一行结束时输入两个空格

# **内容对齐**
```
&lt;p align=&#34;left&#34;&gt;左对齐&lt;/p&gt;
&lt;p align=&#34;center&#34;&gt;居中&lt;/p&gt;
&lt;p align=&#34;right&#34;&gt;右对齐&lt;/p&gt;
```
&lt;p align=&#34;left&#34;&gt;左对齐&lt;/p&gt;
&lt;p align=&#34;center&#34;&gt;居中&lt;/p&gt;
&lt;p align=&#34;right&#34;&gt;右对齐&lt;/p&gt;

# **分隔线**

```
---
___
```
---
___
# **脚注**
```
这里有脚注[^1]
这里有脚注[^note]
这里有脚注[^注释]
[^1]:脚注1的内容
[^note]:脚注2的内容
[^注释]:脚注3的内容
```
这里有脚注[^1]
这里有脚注[^note]
这里有脚注[^注释]
[^1]:脚注1的内容
[^note]:脚注2的内容
[^注释]:脚注3的内容

# **文本格式**
```
*斜体*
_斜体_
**粗体**
__粗体__
***粗斜体***
___粗斜体___
~~删除线~~
==高亮==
&lt;font color=&#34;blue&#34;&gt;字体颜色&lt;/font&gt;
```
*斜体*

_斜体_

**粗体**

__粗体__

***粗斜体***

___粗斜体___

~~删除线~~

==高亮==

&lt;font color=&#34;blue&#34;&gt;字体颜色&lt;/font&gt;

# **引用**
```
&gt; Markdown 是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，它可以使普通文本内容具有一定的格式。
&gt;&gt; 由于Markdown的轻量化、易读易写特性，并且对于图片，图表、数学式都有支持，许多网站都广泛使用Markdown来撰写帮助文档或是用于论坛上发表消息。
```
&gt; Markdown 是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，它可以使普通文本内容具有一定的格式。
&gt;&gt; 由于Markdown的轻量化、易读易写特性，并且对于图片，图表、数学式都有支持，许多网站都广泛使用Markdown来撰写帮助文档或是用于论坛上发表消息。

## **定位标记**
```
[转到列表项](#列表项)
[转到三级标题](#三级标题)
```
[转到列表项](#列表项)

[转到三级标题](#三级标题)

# **插入链接**
```
[Google](https://www.google.com)
```
[Google](https://www.google.com)

# **插入图片**
```
![图片名称](图片链接或路径)
&lt;img src=&#34;图片链接或路径&#34; width=&#34;300&#34; alt=&#34;图片名称&#34;&gt;&lt;/img&gt;
```
![图片名称](图片链接或路径)
&lt;img src=&#34;图片链接或路径&#34; width=&#34;300&#34; alt=&#34;图片名称&#34;&gt;&lt;/img&gt;

# **插入代码**
```
`int  a b c`
```
`int  a b c`

````
```
int  a  b  c
a=b&#43;c
```
````
```
int  a  b  c
a=b&#43;c
```

# **插入数学式**
```
$x^2&#43;y^3=z$
```
$x^2&#43;y^3=z$

```
$$
y=x^2&#43;2a&#43;b/2
$$
```
$$
y=x^2&#43;2a&#43;b/2
$$
# **插入表格**
```
| HEADER | HEADER | HEADER |
|:----:|:----:|:----:|
|      |      |      |
|      |      |      |
|      |      |      |
```
| HEADER | HEADER | HEADER |
|:----:|:----:|:----:|
|      |      |      |
|      |      |      |
|      |      |      |




---

> 作者: LukeIO  
> URL: https://lukeio.github.io/posts/markdown/  

