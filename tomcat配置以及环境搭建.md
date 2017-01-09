
<!DOCTYPE HTML><html><head><link type="text/css" rel="stylesheet" id="wiz_code_highlight_link" href="tomcat配置以及环境搭建_files/wiz_code_highlight.css"><style id="wiz_custom_css">html, body {
    font-size: 15px;
}

body {
    font-family: Helvetica, "Hiragino Sans GB", "微软雅黑", "Microsoft YaHei UI", SimSun, SimHei, arial, sans-serif;
    line-height: 1.6;
    margin: 0;
    padding: 20px 15px;
    padding: 1.33rem 1rem;
}

h1, h2, h3, h4, h5, h6 {
    margin: 20px 0 10px;
    margin: 1.33rem 0 0.667rem;
    padding: 0;
    font-weight: bold;
}

h1 {
    font-size: 21px;
    font-size: 1.4rem;
}

h2 {
    font-size: 20px;
    font-size: 1.33rem;
}

h3 {
    font-size: 18px;
    font-size: 1.2rem;
}

h4 {
    font-size: 17px;
    font-size: 1.13rem;
}

h5 {
    font-size: 15px;
    font-size: 1rem;
}

h6 {
    font-size: 15px;
    font-size: 1rem;
    color: #777777;
    margin: 1rem 0;
}

div, p, ul, ol, dl, li {
    margin: 0;
}

blockquote, table, pre, code {
    margin: 8px 0;
}

ul, ol {
    padding-left: 32px;
    padding-left: 2.13rem;
}

blockquote {
    padding: 0 12px;
    padding: 0 0.8rem;
}

blockquote > :first-child {
    margin-top: 0;
}

blockquote > :last-child {
    margin-bottom: 0;
}

img {
    border: 0;
    max-width: 100%;
    height: auto !important;
    margin: 2px 0;
}

table {
    border-collapse: collapse;
    border: 1px solid #bbbbbb;
}

td, th {
    padding: 4px 8px;
    border-collapse: collapse;
    border: 1px solid #bbbbbb;
    height: 28px;
    word-break: break-all;
    box-sizing: border-box;
}

@media only screen and (-webkit-max-device-width: 1024px), only screen and (-o-max-device-width: 1024px), only screen and (max-device-width: 1024px), only screen and (-webkit-min-device-pixel-ratio: 3), only screen and (-o-min-device-pixel-ratio: 3), only screen and (min-device-pixel-ratio: 3) {
    html, body {
        font-size: 17px;
    }

    body {
        line-height: 1.7;
        padding: 0.75rem 0.9375rem;
        color: #353c47;
    }

    h1 {
        font-size: 2.125rem;
    }

    h2 {
        font-size: 1.875rem;
    }

    h3 {
        font-size: 1.625rem;
    }

    h4 {
        font-size: 1.375rem;
    }

    h5 {
        font-size: 1.125rem;
    }

    h6 {
        color: inherit;
    }

    ul, ol {
        padding-left: 2.5rem;
    }

    blockquote {
        padding: 0 0.9375rem;
    }
}

html, body {
    font-family:Helvetica Neue;
    font-size:15px;
    background-color:#FFFFFF;
}
</style><link rel="stylesheet" charset="utf-8" name="wiz_tmp_editor_style" href="/Applications/WizNote.app/Contents/Resources/files/wizeditor/dependency/fonts.css"></head><body ><h5><span class="s1">网络的架构（面试题）</span></h5><p class="p1"><span style="line-height: 1.6;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;*C/S</span><span class="Apple-tab-span" style="line-height: 1.6;">	</span>&nbsp;<span style="line-height: 1.6;">client/server</span><span class="Apple-tab-span" style="line-height: 1.6;">	</span><span style="line-height: 1.6;">客户端/服务器端</span><span class="Apple-tab-span" style="line-height: 1.6;">	</span><span class="Apple-tab-span" style="line-height: 1.6;">	</span><span style="line-height: 1.6;">例子：QQ</span><span class="Apple-tab-span" style="line-height: 1.6;">	</span><span style="line-height: 1.6;">快播</span><span class="Apple-tab-span" style="line-height: 1.6;">	</span><span style="line-height: 1.6;">暴风影音</span></p><p class="p1"><span class="s1">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; * 优点：交互性好，服务器压力小。</span></p><p class="p1"><span class="s1"><span class="Apple-tab-span"></span><span class="Apple-tab-span"></span><span class="Apple-tab-span"></span><span class="Apple-tab-span"></span><span class="Apple-tab-span"></span>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; * 缺点：客户端更新了，下载。</span></p><p class="p2"><span class="s1"><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span></span></p><p class="p1"><span class="s1"><span class="Apple-tab-span"></span><span class="Apple-tab-span"></span><span class="Apple-tab-span"></span><span class="Apple-tab-span"></span>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;* B/S<span class="Apple-tab-span">	</span>browser/server<span class="Apple-tab-span">	</span>浏览器/服务器端<span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span>例子：购物网站<span class="Apple-tab-span">	</span>12306<span class="Apple-tab-span">	</span></span></p><p class="p1"><span class="s1"><span class="Apple-tab-span"></span><span class="Apple-tab-span"></span><span class="Apple-tab-span"></span><span class="Apple-tab-span"></span><span class="Apple-tab-span"></span>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; * 缺点：服务器压力大。</span></p><p></p><p class="p1"><span class="s1"><span class="Apple-tab-span"></span><span class="Apple-tab-span"></span><span class="Apple-tab-span"></span><span class="Apple-tab-span"></span><span class="Apple-tab-span"></span>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; * 优点：服务器更新就ok。</span></p><p class="p1"><span class="s1"></span></p><h5><span class="s1">服务器的介绍</span></h5><p class="p1"><span class="s1">原理：网络编程</span></p><p class="p1"><span class="s1">概念：</span></p><p class="p1"><span class="s1">  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp;* 硬件：就是一台主机。</span></p><p class="p1"><span class="s1"><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; * 软件：安装了服务器的软件（tomcat）。<span class="Apple-tab-span">	</span></span></p><p class="p1"><span class="s1"><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; * 安装软件后，称为WEB服务器。</span></p><p class="p1"><span class="s1"><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; * 启动服务器，访问资源。</span></p><p class="p1"><span class="s1"><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; * 访问：http://+ip+端口号<span class="Apple-tab-span">	</span>找到主机。如果资源的文件，就可以访问了。</span></p><p class="p1"><span class="s1"><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; * 访问百度：http://www.baidu.com/</span></p><p class="p1"><span class="s1"><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; * HTTP协议默认端口号是80，可以不写。</span></p><p class="p1"><span class="s1"><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; * ping www.baidu.com<span class="Apple-tab-span">	</span>61.135.169.121</span></p><p class="p2"><span class="s1"><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span></span></p><p class="p1"><span class="s1"><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; * 如果想访问本机的服务器（扩展）</span></p><p class="p1"><span class="s1"><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; * http://localhost:80</span></p><p class="p1"><span class="s1"></span></p><p class="p1"><span class="s1"><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; * <a href="http://127.0.0.1:80">http://127.0.0.1:80</a> </span></p><p class="p1"><br></p><h5>常见的服务器</h5><div><ul><li><p class="p1"><span class="s1">WebLogic<span class="Apple-tab-span">	</span>BEA公司开发的（被Oracle收购了）<span class="Apple-tab-span">	</span>收费的<span class="Apple-tab-span">	</span>支持JAVAEE所有的规范（EJB servlet/jsp规范）</span></p><p class="p1"><span class="s1"><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span>* （JAVA<span class="Apple-tab-span">	</span>MySql(Oracle)<span class="Apple-tab-span">	</span>WebLogic）</span></p><p class="p2"><span class="s1"><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span></span></p></li><li><p class="p1"><span class="s1"></span></p><p class="p1"><span class="s1">WebSphere<span class="Apple-tab-span">	</span>IBM公司开发的<span class="Apple-tab-span">	</span>收费的<span class="Apple-tab-span">	</span>支持JAVAEE所有的规范（EJB servlet/jsp规范）</span></p></li><li><p class="p1"><span class="s1"></span></p><p class="p1"><span class="s1">Tomcat <span class="Apple-tab-span">	</span>apache（开源的组织，非常的伟大）只Servlet/JSP规范。免费的</span></p></li></ul><div><br></div></div><h5>tomcat修改端口</h5><div><p class="p1"></p><div><pre class="prettyprint linenums prettyprinted"><ol class="linenums"><li class="L0"><code class="language-c"><span class="pln">tomcat</span><span class="pun">/</span><span class="pln">conf</span><span class="pun">/</span><span class="pln">server</span><span class="pun">.</span><span class="pln">xml</span><span class="pun">配置文件</span></code></li><li class="L1"><code class="language-c"><span class="pun">&lt;</span><span class="typ">Connector</span><span class="pln"> port</span><span class="pun">=</span><span class="str">"80"</span><span class="pln"> protocol</span><span class="pun">=</span><span class="str">"HTTP/1.1"</span><span class="pln"> </span></code></li><li class="L2"><code class="language-c"><span class="pln">  connectionTimeout</span><span class="pun">=</span><span class="str">"20000"</span><span class="pln"> </span></code></li><li class="L3"><code class="language-c"><span class="pln">  redirectPort</span><span class="pun">=</span><span class="str">"8443"</span><span class="pln"> </span><span class="pun">/&gt;</span></code></li></ol></pre></div><h5><br>Tomcat目录结构</h5></div><div><div class="wiz-table-container" style="position: relative; padding: 15px 0px 5px;"><div class="wiz-table-body"><table style="width: 240px;"><tbody><tr><td align="left" valign="middle" style="width:120px" class="">bin</td><td align="left" valign="middle" style="width:120px" class=""><p class="p1"><span class="s1">启动项，关闭项</span></p><title></title></td></tr><tr><td class="" style="width: 120px;">conf</td><td style="width: 120px;" class="">配置文件<br></td></tr><tr><td class="" style="width: 120px;">lib</td><td style="width: 120px;" class=""><p class="p1"><span class="s1">服务器运行使用的jar包</span></p><title></title></td></tr><tr><td class="" style="width: 120px;">logs</td><td style="width: 120px;" class=""><p class="p1"><span class="s1">日志文件，运行时产生的日志</span></p><title></title></td></tr><tr><td class="" style="width: 120px;"><p class="p1"><span class="s1">webapps</span></p><title></title></td><td style="width: 120px;" class="">web应用<br></td></tr><tr><td class="" style="width: 120px;"><p class="p1"><span class="s1">temp</span></p><title></title></td><td style="width: 120px;" class=""><p class="p1"><span class="s1">运行时临时文件</span></p><title></title></td></tr><tr><td class="" style="width: 120px;">work</td><td style="width: 120px;" class="">JSP翻译成

<p class="p1"><span class="s1">Servlet程序</span></p><title></title></td></tr></tbody></table></div></div><h5><span class="s1">开发动态的WEB资源程序，目录结构如下（必须记住）</span></h5><p class="p1"></p><p class="p1"><span class="s1">Servlet/JSP只要包含，就称为动态的WEB资源</span></p><p class="p1"></p><div><pre class="prettyprint linenums prettyprinted"><ol class="linenums"><li class="L0"><code class="language-c"><span class="pln">website</span></code></li><li class="L1"><code class="language-c"><span class="pln">	</span><span class="pun">|---存放</span><span class="pln">	HTML CSS JAVASCRIPT JSP </span><span class="pun">图片</span></code></li><li class="L2"><code class="language-c"><span class="pln">	WEB</span><span class="pun">-</span><span class="pln">INF</span></code></li><li class="L3"><code class="language-c"><span class="pln">		      </span><span class="pun">|</span></code></li><li class="L4"><code class="language-c"><span class="pln">		      web</span><span class="pun">.</span><span class="pln">xml	</span><span class="pun">程序的入口。配置文件（必须有的）</span></code></li><li class="L5"><code class="language-c"><span class="pln">		      classes		</span><span class="pun">文件夹，名称固定的</span><span class="pln">  </span><span class="pun">可选的</span></code></li><li class="L6"><code class="language-c"><span class="pln">		      lib			</span><span class="pun">文件夹，名称固定</span><span class="pln">	</span><span class="pun">可选的</span></code></li></ol></pre></div><p></p><h5><span class="s1">Tomcat管理员的配置</span></h5><div><div><pre class="prettyprint linenums prettyprinted"><ol class="linenums"><li class="L0"><code class="language-c"><span class="pun">在</span><span class="pln">tomcat</span><span class="pun">/</span><span class="pln">conf</span><span class="pun">/</span><span class="pln">tomcat</span><span class="pun">-</span><span class="pln">user</span><span class="pun">.</span><span class="pln">xml</span></code></li><li class="L1"><code class="language-c"><span class="pun">&lt;</span><span class="pln">role rolename</span><span class="pun">=</span><span class="str">"manager"</span><span class="pun">/&gt;</span></code></li><li class="L2"><code class="language-c"><span class="pun">&lt;</span><span class="pln">user username</span><span class="pun">=</span><span class="str">"admin"</span><span class="pln"> password</span><span class="pun">=</span><span class="str">"admin"</span><span class="pln"> roles</span><span class="pun">=</span><span class="str">"manager"</span><span class="pun">/&gt;</span></code></li></ol></pre></div><div><br></div><h5>如何部署WEB程序</h5><div><div><pre class="prettyprint linenums prettyprinted"><ol class="linenums"><li class="L0"><code class="language-html"><span class="pln">* 项目复制到webapps目录下。</span></code></li><li class="L1"><code class="language-html"><span class="pln">					</span></code></li><li class="L2"><code class="language-html"><span class="pln">* 通过配置虚拟路径的方式。</span></code></li><li class="L3"><code class="language-html"><span class="pln">   * 直接修改配置文件</span></code></li><li class="L4"><code class="language-html"><span class="pln">   * 写到tomcat/conf/server.xml</span></code></li><li class="L5"><code class="language-html"><span class="pln">	 * 找到</span><span class="tag">&lt;Host&gt;</span><span class="pln">标签，配置到Host标签的中间</span></code></li><li class="L6"><code class="language-html"><span class="pln">	 * 目的：通过配置，配置访问路径，准确找到c:\\bb的文件</span></code></li><li class="L7"><code class="language-html"><span class="pln">		* </span><span class="tag">&lt;Context</span><span class="pln"> </span><span class="atn">docBase</span><span class="pun">=</span><span class="atv">"文件夹的真实目录"</span><span class="pln"> </span><span class="atn">path</span><span class="pun">=</span><span class="atv">"虚拟路径（访问路径）"</span><span class="pln"> </span><span class="tag">&gt;&lt;Context&gt;</span></code></li><li class="L8"><code class="language-html"><span class="pln">		* </span><span class="tag">&lt;Context</span><span class="pln"> </span><span class="atn">docBase</span><span class="pun">=</span><span class="atv">"C:\\bb"</span><span class="pln"> </span><span class="atn">path</span><span class="pun">=</span><span class="atv">"/itcast"</span><span class="pln"> </span><span class="tag">&gt;&lt;/Context&gt;</span></code></li><li class="L9"><code class="language-html"><span class="pln">		* 访问：http://localhost:80/itcast</span></code></li><li class="L0"><code class="language-html"><span class="pln">							</span></code></li><li class="L1"><code class="language-html"><span class="pln">    * 自己编写一个配置文件（格式）（推荐使用）</span></code></li><li class="L2"><code class="language-html"><span class="pln">		* 自定义xxx.xml结尾文件，在$CATALINA_HOME/conf/[enginename]/[hostname]/ directory.目录下。</span></code></li><li class="L3"><code class="language-html"><span class="pln">		* 把xxx当成虚拟（访问）路径。</span></code></li><li class="L4"><code class="language-html"><span class="pln">						</span></code></li><li class="L5"><code class="language-html"><span class="pln">		* 在xml的文件中编写。</span></code></li><li class="L6"><code class="language-html"><span class="pln">			* 在哪个目录下：</span></code></li><li class="L7"><code class="language-html"><span class="pln">			* $CATALINA_HOME/conf/[enginename]/[hostname]/ directory.</span></code></li><li class="L8"><code class="language-html"><span class="pln">			* 如果找引擎的名称和主机的名称，在server.xml中找。</span></code></li><li class="L9"><code class="language-html"><span class="pln">			* tomcat/conf/Catalina/localhost/ccc.xml</span></code></li><li class="L0"><code class="language-html"><span class="pln">								</span></code></li><li class="L1"><code class="language-html"><span class="pln">				* ccc.xml的文件编写什么内容？</span></code></li><li class="L2"><code class="language-html"><span class="pln">					* </span><span class="tag">&lt;Context</span><span class="pln"> </span><span class="atn">docBase</span><span class="pun">=</span><span class="atv">"C:\\cc"</span><span class="tag">&gt;&lt;/Context&gt;</span></code></li><li class="L3"><code class="language-html"><span class="pln">				* 访问：http://localhost:80/ccc</span></code></li><li class="L4"><code class="language-html"><span class="pln">							</span></code></li><li class="L5"><code class="language-html"><span class="pln">			</span></code></li><li class="L6"><code class="language-html"><span class="pln">			* 配置虚拟主机（了解）	</span></code></li></ol></pre></div><div><br></div><h5>HTTP的版本</h5><div><ul><li>HTTP/1.0&nbsp;
<p class="p1"><span class="s1">链接后，只能获取一个web资源&nbsp;</span><span style="line-height: 1.6;">链接后，发送请求，服务器做出响应，链接立即断开。</span></p></li><li><p class="p1"><span class="s1">HTTP/1.1&nbsp;</span><span style="line-height: 1.6;">链接后，可以获取多个web资源&nbsp;</span><span style="line-height: 1.6;">链接后，发送请求，服务器做出响应，链接不会立即断开。</span></p><p class="p1"><span class="s1"><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span><span class="Apple-tab-span">	</span>再次发送请求，直接有一段时间没操作，自动断开。</span></p></li></ul></div></div><div><img border="0" class="" src="tomcat配置以及环境搭建_files/015b777b-1def-4b48-9c95-35c64a78f7f3.png"></div><br></div><div><div><img border="0" src="tomcat配置以及环境搭建_files/67e532bf-2261-45ab-9c16-20efa1cb4d91.png"></div><br></div><p class="p1"></p><h5><br></h5><p></p><div></div></div></body></html>
