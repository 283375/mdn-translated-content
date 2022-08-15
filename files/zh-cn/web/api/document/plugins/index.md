---
title: Document.plugins
slug: Web/API/Document/plugins
---
<div>{{APIRef("DOM")}}</div>

<div>{{domxref("Document")}}接口的<strong>插件</strong>只读属性返回一个{{domxref("HTMLCollection")}} 对象，该对象包含一个或多个{{domxref("HTMLEmbedElement")}}s 表示当前文档中的{{HTMLElement("embed")}} 元素。</div>

<div> </div>

<div class="note">对于已安装的插件列表，请使用 <a href="/en-US/docs/Web/API/NavigatorPlugins/plugins">NavigatorPlugins.plugins</a> 插件。</div>

<h2 id="Syntax">语法</h2>

<pre class="syntaxbox"><var>embedArrayObj</var> = document.plugins
</pre>

<h3 id="值">值</h3>

<p>一个 {{domxref("HTMLCollection")}}, 如果文档中没有嵌入则为 null。</p>

<h2 id="Specifications">规范</h2>

{{Specifications}}

<h2 id="浏览器兼容性">浏览器兼容性</h2>

<p>{{Compat("api.Document.plugins")}}</p>

<h2 id="See_also">另请参见</h2>

<ul>
 <li><a href="https://docs.microsoft.com/en-us/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms537477(v=vs.85)">MSDN documentation</a></li>
</ul>