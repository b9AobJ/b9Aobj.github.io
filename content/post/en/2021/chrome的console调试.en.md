---
title: "Chrome的console调试" # Title of the blog post.
date: 2021-05-10T18:07:16+08:00 # Date of post creation.
description: "Article description." # Description used for search engine.
featured: true # Sets if post is a featured post, making it appear on the sidebar. A featured post won't be listed on the sidebar if it's the current page
draft: false # Sets whether to render this page. Draft of true will not be rendered.
toc: false # Controls if a table of contents should be generated for first-level links automatically.
# menu: main
#featureImage: "/images/path/file.jpg" # Sets featured image on blog post.
#thumbnail: "/images/path/thumbnail.png" # Sets thumbnail image appearing inside card on homepage.
shareImage: "/images/path/share.png" # Designate a separate image for social media sharing.
codeMaxLines: 10 # Override global value for how many lines within a code block before auto-collapsing.
codeLineNumbers: false # Override global value for showing of line numbers within code block.
figurePositionShow: true # Override global value for showing the figure label.
categories:
  - Chrome
tags:
  - Console
  - Chrome
---

1、$_

`$_`上次结果的引用

2、$i

需要Chrome插件:(Console Importer)[https://chrome.google.com/webstore/detail/console-importer/hgajpakhafplebkdljleajgbpdmplhie/related]，可以快速的在console中引入`npm`库。  
例如： `$i('lodash')`或者`$i('moment')`,就可以使用lodash或者moment库了。

3、copy(...)

`copy`任何资源，如`copy($0)`或`copy($_)`

4、学会使用console.table来显示数组或者对象

5、