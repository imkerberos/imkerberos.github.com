---
layout: post
title: 测试 kramdown 的语法以及本站的功能
tags: [Jekyll]
desc: 修改 jekyll 的缺省 markdown 引擎为 kramdown, kramdown 对 markdown 做了很多扩展. 这样在 github 上可以实现一些额外的功能.
category: Test
comments: true
---

## 表格 ##

maruku 和 rdiscount 不支持表格. 表格语法是 PHP Markdown Extension 的扩展, 现在 kramdown 引擎实现了这个扩展. 本站使用的是 kramdown 引擎. 

表格的代码, 下面的表格代码是缺省样式:

    表头      | 列1        | 列 2       |  列3        | 列4
    ----------|------------|:-----------|------------:|:--------------:
    行1       | 内容       | 内容左对齐 | 内容右对齐  | 内容中间对齐
    行2       | 内容       | 内容左对齐 | 内容右对齐  | 内容中间对齐
    行3       | 内容       | 内容左对齐 | 内容右对齐  | 内容中间对齐
    行4       | 内容       | 内容左对齐 | 内容右对齐  | 内容中间对齐

最后的表格效果:

表头      | 列1        | 列 2       |  列3        | 列4
----------|------------|:-----------|------------:|:--------------:
行1       | 内容       | 内容左对齐 | 内容右对齐  | 内容中间对齐
行2       | 内容       | 内容左对齐 | 内容右对齐  | 内容中间对齐
行3       | 内容       | 内容左对齐 | 内容右对齐  | 内容中间对齐
行4       | 内容       | 内容左对齐 | 内容右对齐  | 内容中间对齐

当然, kramdown 比 rdiscount 好处在于, 在表格内部也可以使用 markdown 语法, 所以 kramdown 叫 multimarkdown. 下面的例子
在表格内部使用了 markdown 语法以及 kramdown 的特定语法, 比如 Latex 公式.

    表头      | 列1        | 列 2               |  列3        | 列4
    ----------|------------|:-------------------|------------:|:--------------:
    行1       | *强调*     | 公式语法 $\frac{x}{y^2}$ | 链接语法 [Google](http://www.google.com.cn)  | 内容中间对齐
    行2       | _强调_     | 内容左对齐         | 内容右对齐  | 内容中间对齐
    行3       | **强调**   | 内容左对齐长       | 内容右对齐  | 内容中间对齐
    行4       | __强调__   | 内容左对齐         | 内容右对齐  | 内容中间对齐

效果如下

表头      | 列1        | 列 2               |  列3        | 列4
----------|------------|:-------------------|------------:|:--------------:
行1       | *强调*     | 公式 $\frac{x}{y^2}$ | 链接语法 [Google](http://www.google.com.cn)  | 内容中间对齐
行2       | _强调_     | 内容左对齐长一点才有效果   | 内容右对齐  | 内容中间对齐
行3       | **强调**   | 内容左对齐         | 内容右对齐长一点才有效果  | 内容中间对齐
行4       | __强调__   | 内容左对齐带下标[^foot]         | 内容右对齐  | 内容中间对齐 -- 长一点才有效果

## 源代码块

由于 GitHub 的 Jekyll 没有语法高亮, 所以本站添加了 google-code-prettify 进行 Javascript 层的语法高亮处理, 以下是 Python 代码:

    class AdView (object):
        def __init__ (self, name = None):
            self.name = name

        def test (self):
            if self.name == 'admin':
                return False
            else
                return True

当然也可以使用 kramdown 的语法方式, 注意 title 和 lang-py 之间必须有空行, google-code-prettify 默认可以监测到 python 语法, 但是对于 lua 和 latex 就无法监测到, 所以可以使用 `{:.lang-lua}` 和 `{:.lang-tex}` 指定语法高亮类型. kramdown 的好处就在于避免 HTML 语法出现在文件中.

    {: title="测试Python代码"}

    {:.lang-py}
        class AdView (object):
            def __init__ (self, name = None):
                self.name = name

            def test (self):
                if self.name == 'admin':
                    return False
                else
                    return True

下面是效果

{: title="测试Python代码"}

{:.lang-py}
    class AdView (object):
        def __init__ (self, name = None):
            self.name = name

        def test (self):
            if self.name == 'admin':
                return False
            else
                return True

对于 Latex 语法的高亮, rdiscount 配合 google-code-prettify 需要使用  `<pre  class="lang-tex">` 才能高亮. 如果不添加 css 属性,
google-code-prettify 无法正确处理, 效果如下:

    \begin{align*}
      & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
      = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
      & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
          \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
          \vdots & \ddots & \vdots \\
          \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
        \end{array} \right)
      \left( \begin{array}{c}
          y_1 \\
          \vdots \\
          y_n
        \end{array} \right)
    \end{align*}

添加 `<pre class="lang-tex">` 以后的效果:

<pre class="lang-tex">
\begin{align*}
  & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
  = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
  & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
      \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
      \vdots & \ddots & \vdots \\
      \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
    \end{array} \right)
  \left( \begin{array}{c}
      y_1 \\
      \vdots \\
      y_n
    \end{array} \right)
\end{align*}
</pre>

使用 kramdown 特定语法效果,源代码:

<pre class="lang-tex">
{:title="测试 Tex 代码"}

{:.lang-tex}
    $$
    \begin{align*}
      & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
      = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
      & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
          \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
          \vdots & \ddots & \vdots \\
          \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
        \end{array} \right)
      \left( \begin{array}{c}
          y_1 \\
          \vdots \\
          y_n
        \end{array} \right)
    \end{align*}
    $$
</pre>

效果:

{:title="测试 Tex 代码"}

{:.lang-tex}
    $$
    \begin{align*}
      & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
      = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
      & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
          \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
          \vdots & \ddots & \vdots \\
          \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
        \end{array} \right)
      \left( \begin{array}{c}
          y_1 \\
          \vdots \\
          y_n
        \end{array} \right)
    \end{align*}
    $$

以下是 C 代码:

    #include <stdio.h>
    int main (void)
    {
        print ("Hello World!");
        return 0;
    }

以下是 Objective-C 代码:

    #import <Foundation>
    @interface AdView : NSObject <UIApplication> {
        UIWindow * window;
    }
    @property (nonatomic, retain) IBOutlet UIWindow *window;
    @end

    int main ()
    {
        w = [[AdView alloc] init];
        [w release];
        print ("Hello World!");
        return 0;
    }

以下是 HTML 代码

{:title="测试 HTML 代码"}

{:.lang-html}
    <html>
        <head>
            <title> 测试 </title>
            <script type = "text/javascript">
                var foo = new Object ();
                for ( a in foo ) {
                    foo = 2;
                }
            </srcript>
        </head>
        <body>
            <a href="http://www.ios-dreamer.com"> 这是我的主页 </a>
        </body>
    </html>

以下是 Javascript 代码

    var foo = new Object ();
    for ( a in foo ) {
        foo = 2;
    }

{: #para-one}

## LaTex 公式

以下是 LaTex 公式代码:

{:title="测试 Tex 代码"}

{:.lang-tex}

    $$
    \begin{align*}
      & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
      = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
      & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
          \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
          \vdots & \ddots & \vdots \\
          \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
        \end{array} \right)
      \left( \begin{array}{c}
          y_1 \\
          \vdots \\
          y_n
        \end{array} \right)
    \end{align*}
    $$

和效果

$$
\begin{align*}
  & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
  = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
  & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
      \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
      \vdots & \ddots & \vdots \\
      \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
    \end{array} \right)
  \left( \begin{array}{c}
      y_1 \\
      \vdots \\
      y_n
    \end{array} \right)
\end{align*}
$$

[^foot]: 这是下标测试
