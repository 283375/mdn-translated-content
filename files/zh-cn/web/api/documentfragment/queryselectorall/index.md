---
title: DocumentFragment.querySelectorAll()
slug: Web/API/DocumentFragment/querySelectorAll
---
<div>{{ApiRef("DOM")}}</div>

<p>DocumentFragment.queryselectorall() 方法返回{{domxref("NodeList")}}中的元素{{domxref("DocumentFragment")}}(使用文档节点的深度优先顺序遍历) 匹配指定的选择器组。</p>

<p>如果参数中指定的选择器无效，则会引发一个带 SYNTAX_ERR 值的{{domxref("DOMException")}}。</p>

<div class="note">
<p><strong>注意：这个 API 的定义被移动到{{domxref("ParentNode")}}接口。</strong></p>
</div>

<h2 id="语法">语法</h2>

<pre class="syntaxbox"><em>elementList</em> = <em>documentframgment</em>.querySelectorAll(<em>selectors</em>);</pre>

<h3 id="参数">参数</h3>

<dl>
 <dt><em>selectors</em></dt>
 <dd>是一个{{domxref("DOMString")}}包含一个或多个用逗号分隔的 CSS 选择器。</dd>
</dl>

<h2 id="示例">示例</h2>

<p>此示例返回 DocumentFragment 中所有 div 元素的列表，其中包含一个类“note”或“alert”:</p>

<pre class="brush: js">var matches = documentfrag.querySelectorAll("div.note, div.alert");
</pre>

<h2 id="规范">规范</h2>

{{Specifications}}

<h2 id="浏览器兼容性">浏览器兼容性</h2>



<p>{{Compat("api.DocumentFragment.querySelectorAll")}}</p>

<h2 id="参阅">参阅</h2>

<ul>
 <li>它所属的{{domxref("DocumentFragment")}}接口。</li>
</ul>