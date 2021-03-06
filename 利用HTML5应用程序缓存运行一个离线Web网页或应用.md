# 利用HTML5应用程序缓存运行一个离线Web网页或应用
*2013-09-11 16:25:47*



HTML5的离线web应用允许我们在脱机时与网站进行交互。这在提高网站的访问速度和制作一款web离线应用上（如HTML5游戏）有一定的用处。

## HTML5应用程序缓存和浏览器缓存的区别。

（有些）浏览器会主动保存自己的缓存文件以加快网站加载速度。但是要实现浏览器缓存必须要满足一个前提，那就是网络必须要保持连接。如果网络没有连接，即使浏览器启用了对一个站点的缓存，依然无法打开这个站点。只会收到一条错误信息。而使用离线web应用，我们可以主动告诉浏览器应该从网站服务器中获取或缓存哪些文件，并且在网络离线状态下依然能够访问这个网站。

## 如何实现HTML5应用程序缓存。

实现HTML5应用程序缓存非常简单，只需三步，并且不需要任何API。只需要告诉浏览器需要离线缓存的文件，并对服务器和网页做一些简单的设置即可实现。

1.  创建一个 **cache.manifest** 文件，并确保文件具有正确的内

2.  在服务器上设置内容类

3.  所有的HTML文件都指向 **cache.manifest**

首先我们需要**建立一个名为 cache.manifest 的文件**，Windows平台下用记事本即可（也可用其他的IDE）。文件内容如下：


    CACHE MANIFEST
    #v1 - 2013-09-09
    CACHE:
    index.html
    favicon.ico
    css/main.css
    …
    
    NETWORK: *



其中 **CACHE:** 之后的部分为列出我们需要缓存的文件。 **NETWORK:** 之后可以指定在线白名单，即列出我们不希望离线存储的文件，因为通常它们的内容需要互联网访问才有意义。另外，在此部分我们可以使用快捷方式：通配符*。这将告诉浏览器，应用服务器中获取没有在显示部分中提到的任何文件或URL。需要特别指出的是，上面例子中的注释 **v1** 很有必要存在。只有当 cache.manifest 文件发生变化时，浏览器才会去更新应用缓存。如果你要更改缓存资源，你必须同时修改此文件中的内容，以便让浏览器知道它们需要更新缓存。你可以对清单文件做任何改动，但大家都认同的最佳实践则是修正版本号（即v*）。

接下来需要**在服务器上设置内容类型**：

假使你使用的事Apache服务器，在**.htaccess**文件中添加以下代码：

    AddType text/cache-manifest .manifest

最后，我们需要**将HTML页面指向清单文件**。通过设置每一个页面中HTML元素的manifest属性来完成这一步：

    <html manifest="/cache.manifest">

完成这一步后，就完成了web离线缓存的所有步骤。由于浏览的文件内容都没有更改且存储在本地，因此现在网页的打开速度会更快（即使是在线状态也如此）。

需要注意的问题：

*   网站的每一个html页面都必须设置html元素的manifest属性。一定要这样做

*   在你的整个网站应用中，只能有一个cache.manifest文件（建议放在网站根目录下）

*   部分浏览器（如IE8-）不支持HTML5离线缓存；

> 更多站外相关阅读：

> *   [使用应用缓存 - MDN](https://developer.mozilla.org/zh-CN/docs/HTML/Using_the_application_cache

> *   [使用 HTML5 开发离线应用 - IBM developerWorks](http://www.ibm.com/developerworks/cn/web/1011_guozb_html5off/)