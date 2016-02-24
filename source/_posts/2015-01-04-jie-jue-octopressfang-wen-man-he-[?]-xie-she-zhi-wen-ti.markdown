---
layout: post
title: "解决Octopress访问慢和一些设置问题"
date: 2015-01-04 13:55:57 +0800
comments: true
thumbnail: /img/2015/06/6.jpg
categories: 
---
When我刚搭建好Octopress的时候，O(∩_∩)O~心情挺好的，感觉棒棒哒。<!--more-->And我点击Command+R刷新页面的时候，尼玛我脸都绿了。。。。由于GFW的原因，造成页面load很慢,从Console窗口可以看出主要就是Google的各种服务被墙了。。。<p>  

<b1>What the hell!</b1>

====================================================================
好吧，结素，开始正题！</p>
内容来自各大大的blog，由<a href="http://beyondvincent.com/blog/2013/07/27/107-hello-page-of-github/"><code>破船之家</code></a>，<a href="http://droidyue.com/blog/2014/06/22/fix-octopress-slow-loading-speed-issue-in-china-mainland/"><code>技术小黑屋</code></a></p>

<h2>1、初级问题</h2>
首先打开：<code>_config.yml</code></p>
我一狠心将Github，Twitter,Google +1, Google Plus, Pinboard, Delicious, Disqus, Google analytics, Facebook一律封杀了......艾玛，好残忍⇀ ⇀ </p>
全部前面加#注释了</p>

<h2>2、关键问题</h2>
Octopress很多依赖于Google的库和资源，So,google拜拜~~~~(>_<)~~~~ </p>

对于使用Google Analytics来说，加在ga.js这个文件简直是要命的慢，这里我使用自己存放在七牛CDN上的js.<a href="http://droidyue-tools.qiniudn.com/ga.js">http://droidyue-tools.qiniudn.com/ga.js</a> 已验证，完全可以正常收集数据。
参考如下，修改<code>source/_includes/google_analytics.html</code></p>

<figure class='code'><figcaption><span></span></figcaption><div class="highlight"><table><tr><td class="gutter"><pre class="line-numbers"><span class='line-number'>1</span>
<span class='line-number'>2</span>
<span class='line-number'>3</span>
<span class='line-number'>4</span>
<span class='line-number'>5</span>
<span class='line-number'>6</span>
<span class='line-number'>7</span>
<span class='line-number'>8</span>
</pre></td><td class='code'><pre><code class='html'><span class='line'> __gaq.push([&#39;_trackPageview&#39;]);
</span><span class='line'>
</span><span class='line'> (function() {
</span><span class='line'> var ga = document.createElement(&#39;script&#39;); ga.type = &#39;text/javascript&#39;; ga.async = true;
</span><span class='line'> ga.src=&#39;http://droidyue-tools.qiniudn.com/ga.js&#39;;
</span><span class='line'> var s = document.getElementsByTagName(&#39;script&#39;)[0]; s.parentNode.insertBefore(ga, s);
</span><span class='line'> })();
</span><span class='line'>   <span class="nt">&lt;/script&gt;</span>
</span></code></pre></td></tr></table></div></figure>


<h3>解决fonts.googleapis.com蜗牛慢</h3>

<p>这里我们使用数字公司提供的Google Fonts大陆解决方案，使用<code>fonts.useso.com</code>替换<code>fonts.googleapis.com</code>。<br/>
修改文件<code>/source/_includes/custom/head.html</code></p>

<figure class='code'><figcaption><span></span></figcaption><div class="highlight"><table><tr><td class="gutter"><pre class="line-numbers"><span class='line-number'>1</span>
<span class='line-number'>2</span>
<span class='line-number'>3</span>
</pre></td><td class='code'><pre><code class='html'><span class='line'> <span class="c">&lt;!--Fonts from Google&quot;s Web font directory at http://google.com/webfonts --&gt;</span>
</span><span class='line'><span class="nt">&lt;link</span> <span class="na">href=</span><span class="s">&quot;http://fonts.useso.com/css?family=PT+Serif:regular,italic,bold,bolditalic&quot;</span> <span class="na">rel=</span><span class="s">&quot;stylesheet&quot;</span> <span class="na">type=</span><span class="s">&quot;text/css&quot;</span><span class="nt">&gt;</span>
</span><span class='line'><span class="nt">&lt;link</span> <span class="na">href=</span><span class="s">&quot;http://fonts.useso.com/css?family=PT+Sans:regular,italic,bold,bolditalic&quot;</span> <span class="na">rel=</span><span class="s">&quot;stylesheet&quot;</span> <span class="na">type=</span><span class="s">&quot;text/css&quot;</span><span class="nt">&gt;</span>
</span></code></pre></td></tr></table></div></figure>


<h3>解决ajax.googleapis.com慢的问题</h3>

<p>修改<code>source/_includes/head.html</code></p>

<figure class='code'><figcaption><span></span></figcaption><div class="highlight"><table><tr><td class="gutter"><pre class="line-numbers"><span class='line-number'>1</span>
<span class='line-number'>2</span>
<span class='line-number'>3</span>
<span class='line-number'>4</span>
<span class='line-number'>5</span>
<span class='line-number'>6</span>
</pre></td><td class='code'><pre><code class='html'><span class='line'>   <span class="nt">&lt;link</span> <span class="na">href=</span><span class="s">&quot;/stylesheets/screen.css&quot;</span> <span class="na">media=</span><span class="s">&quot;screen, projection&quot;</span> <span class="na">rel=</span><span class="s">&quot;stylesheet&quot;</span> <span class="na">type=</span><span class="s">&quot;text/css&quot;</span><span class="nt">&gt;</span>
</span><span class='line'>   <span class="nt">&lt;link</span> <span class="na">href=</span><span class="s">&quot;/atom.xml&quot;</span> <span class="na">rel=</span><span class="s">&quot;alternate&quot;</span> <span class="na">title=</span><span class="s">&quot;技术小黑屋&quot;</span> <span class="na">type=</span><span class="s">&quot;application/atom+xml&quot;</span><span class="nt">&gt;</span>
</span><span class='line'>   <span class="nt">&lt;script </span><span class="na">src=</span><span class="s">&quot;/javascripts/modernizr-2.0.js&quot;</span><span class="nt">&gt;&lt;/script&gt;</span>
</span><span class='line'>   <span class="nt">&lt;script </span><span class="na">src=</span><span class="s">&quot;//ajax.useso.com/ajax/libs/jquery/1.9.1/jquery.min.js&quot;</span><span class="nt">&gt;&lt;/script&gt;</span>
</span><span class='line'>   <span class="nt">&lt;script&gt;</span><span class="o">!</span><span class="nb">window</span><span class="p">.</span><span class="nx">jQuery</span> <span class="o">&amp;&amp;</span> <span class="nb">document</span><span class="p">.</span><span class="nx">write</span><span class="p">(</span><span class="nx">unescape</span><span class="p">(</span><span class="s1">&#39;%3Cscript src=&quot;./javascripts/lib/jquery.min.js&quot;%3E%3C/script%3E&#39;</span><span class="p">))</span><span class="nt">&lt;/script&gt;</span>
</span><span class='line'>   <span class="nt">&lt;script </span><span class="na">src=</span><span class="s">&quot;/javascripts/octopress.js&quot;</span> <span class="na">type=</span><span class="s">&quot;text/javascript&quot;</span><span class="nt">&gt;&lt;/script&gt;</span>
</span></code></pre></td></tr></table></div></figure>

</p>

好了，到此的话就可以了，基本上访问慢得问题解决了，现在我们来添加评论功能，评论我们用< a href="http://duoshuo.com/"><code>多说</code></a></p>

<h2>添加评论</h2>
<h4>进入多说官网，注册账号</h4>
<h4>在_config.yml文件添加</h4>
<figure class='code'><div class="highlight"><table><tr><td class="gutter"><pre class="line-numbers"><span class='line-number'>1</span>
<span class='line-number'>2</span>
<span class='line-number'>3</span>
</pre></td><td class='code'><pre><code class=''><span class='line'># duoshuo comments
</span><span class='line'>duoshuo_comments: true
</span><span class='line'>duoshuo_short_name: yourname</span></code></pre></td></tr></table></div></figure>


<h4>在<code>source/_layouts/post.html</code>中添加多说评论模块</h4>

<figure class='code'><div class="highlight"><table><tr><td class="gutter"><pre class="line-numbers"><span class='line-number'>1</span>
<span class='line-number'>2</span>
<span class='line-number'>3</span>
<span class='line-number'>4</span>
<span class='line-number'>5</span>
<span class='line-number'>6</span>
</pre></td><td class='code'><pre><code class=''><span class='line'>｛% if site.duoshuo_short_name and site.duoshuo_comments == true and page.comments == true %｝
</span><span class='line'>  &lt;section&gt;
</span><span class='line'>    &lt;h1&gt;Comments&lt;/h1&gt;
</span><span class='line'>    &lt;div id="comments" aria-live="polite"&gt;｛% include post/duoshuo1.html %｝&lt;/div&gt;
</span><span class='line'>  &lt;/section&gt;
</span><span class='line'>｛% endif %｝</span></code></pre></td></tr></table></div></figure>


<h4>创建<code>source/_includes/post/duoshuo.html</code>，并填入如下内容</h4>

<figure class='code'><div class="highlight"><table><tr><td class="gutter"><pre class="line-numbers">
<span class='line-number'>1</span>
<span class='line-number'>2</span>
<span class='line-number'>3</span>
<span class='line-number'>4</span>
<span class='line-number'>5</span>
<span class='line-number'>6</span>
<span class='line-number'>7</span>
<span class='line-number'>8</span>
<span class='line-number'>9</span>
<span class='line-number'>10</span>
<span class='line-number'>11</span>
<span class='line-number'>12</span>
<span class='line-number'>13</span>
<span class='line-number'>14</span>
</pre></td><td class='code'><pre><code class=''>
<span class='line'>&lt;!-- Duoshuo Comment BEGIN --&gt;
</span><span class='line'>&lt;div class="ds-thread"&gt;&lt;/div&gt;
</span><span class='line'>&lt;script type="text/javascript"&gt;
</span><span class='line'>  var duoshuoQuery = {short_name:"beyondvincent"};
</span><span class='line'>  (function() {
</span><span class='line'>    var ds = document.createElement('script');
</span><span class='line'>    ds.type = 'text/javascript';ds.async = true;
</span><span class='line'>    ds.src = 'http://static.duoshuo.com/embed.js';
</span><span class='line'>    ds.charset = 'UTF-8';
</span><span class='line'>    (document.getElementsByTagName('head')[0] 
</span><span class='line'>    || document.getElementsByTagName('body')[0]).appendChild(ds);
</span><span class='line'>  })();
</span><span class='line'>&lt;/script&gt;
</span><span class='line'>&lt;!-- Duoshuo Comment END --&gt;</span></code></pre></td></tr></table></div></figure>


<h4>发布到站点</h4>

<figure class='code'><div class="highlight"><table><tr><td class="gutter"><pre class="line-numbers">
<span class='line-number'>1</span>
<span class='line-number'>2</span>
<span class='line-number'>3</span>
<span class='line-number'>4</span>
<span class='line-number'>5</span>
</pre></td><td class='code'><pre><code class=''>
<span class='line'>$ rake generate
</span><span class='line'>$ git add .
</span><span class='line'>$ git commit -am "添加多说评论" 
</span><span class='line'>$ git push origin source
</span><span class='line'>$ rake deploy</span></code></pre></td></tr></table></div></figure>

<h3>百度统计</h3>
从百度统计获取脚本，然后添加到文件source/_includes/after_footer.html文件
