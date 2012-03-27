About repository
================

This is the Jekyll blog with [Octopress](http://octopress.org) *classic* theme.

Feature
-------

- Only based Jekyll except plugins, so can deploy on GitHub pages directly.
- With beautiful theme of [Octopress](http://octopress.org) *classic* theme.
- A basic *Tag Cloud* sidebar.
- Archives and Tags only depends `Liquid` template instead of plugin.
- google-code-prettify for code hilight.

Test
----

```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```
