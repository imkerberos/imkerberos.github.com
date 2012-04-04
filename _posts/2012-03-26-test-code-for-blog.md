---
layout: post
title: 测试 Blog 代码高亮
tags: [Jekyll]
desc: 测试 google-code-prettify 的代码着色效果.
category: Test
comments: true
---

以下是 Python 代码:

    class AdView (object):
        def __init__ (self, name = None):
            self.name = name

        def test (self):
            if self.name == 'admin':
                return False
            else
                return True

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

> 测试 github 能否高亮
{:title="The blockquote title"}
{: #myid}

{:.c}
    int main() {
        printf ("Hello World!\n");
    }

以下是 $\exp(-\frac{x^2}{2})$ 代码:

{:title="Code block title"}
{: .latex}
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

