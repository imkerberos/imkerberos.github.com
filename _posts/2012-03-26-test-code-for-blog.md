---
layout: post
title: 测试 Blog 代码高亮
tags: [Jekyll]
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

