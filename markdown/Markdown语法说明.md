# Markdown语法说明
（[原文链接](https://www.appinn.com/markdown/index.html "markdown语法的详细说明")）

- 概述
  - 宗旨
  - 兼容HTML
  - 特殊字符自动转换
- 区块元素
  - 段落和换行
  - 标题
  - 区块引用
  - 分割线
- 区段元素
  - 连接
  - 强调
  - 代码
  - 图片
- 其他
  - 反斜杠
  - 自动链接
---

# 概述
## 宗旨
Markdown的目标是实现“易写易读”。
## 兼容HTML
可以使用HTML标记的一小部分，例如：
<h1>h1标题</h1>
而HTML的区块元素需要制约。

<html>
<table>
    <tr>
        <td>foo</td><td>fun</td>
    </tr>
<table>
</html>

## 特殊字符转换

# 区块元素
## 段落和换行
<br></br>
（注意不同的Markdown解释器会有所差别）
## 标题
这里只是用前面加#号的方式。
## 区块引用
> This is the first level of quoting.
>
> > This is nested blockquote.
>
> Back to the first level.

## 列表
无序列表使用星号、加号和减号作为列表标记：
* red
* green
* blue

等同于：
+ red
+ green
+ blue

也等同于
- red
- green
- blue

有序列表使用数字接一个英文句点：
1. red
2. green
3. blue

## 代码区块

```
#include <iostream>
using namespace std;

void main(){
    cout<< "hellp world!" << endl;
}
```

<pre><code>
#include <iostream>
using namespace std;

void main(){
    cout<< "hellp world!" << endl;
}
</code></pre>

## 分割线
你可以在一行中用三个以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西。你也可以在星号或是减号中间插入空格。下面每种写法都可以建立分隔线：

* * *

***

*****

- - -

---------------

# 区段元素
## 链接
Markdown 支持两种形式的链接语法：

行内式和参考式两种形式。

不管是哪一种，链接文字都是用 [方括号] 来标记。

要建立一个行内式的链接，只要在方块括号后面紧接着圆括号并插入网址链接即可，如果你还想要加上链接的 title 文字，只要在网址后面，用双引号把 title 文字包起来即可，例如：

This is [an example](http://example.com/ "Title") inline link.

[This link](http://example.net/) has no title attribute.




