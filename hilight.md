---
layout: page
title: 测试 github 的语法高亮功能
---

以下是 Ruby 代码

```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```

以下是 python 代码
```python
import os

def main ():
    print 'Current direcotry', os.getcwd ()
```
