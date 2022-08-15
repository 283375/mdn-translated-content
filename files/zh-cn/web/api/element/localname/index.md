---
title: Element.localName
slug: Web/API/Element/localName
---
<div>{{APIRef("DOM")}}</div>

<p><code><strong>Element.localName</strong></code> 只读属性，返回本地名称的。</p>

<div class="note">
<p>在 DOM4 之前这个 API 定义在{{domxref("Node")}}接口。</p>
</div>

<h2 id="语法">语法</h2>

<pre class="syntaxbox"><var>name</var> = <var>element</var>.localName
</pre>

<h3 id="返回值">返回值</h3>

<p> {{domxref("DOMString")}} 返回元素限定名的本地部分。</p>

<h2 id="示例">示例</h2>

<p>(必须配合 XML 文档类型，如 <code>text/xml</code> 或 <code>application/xhtml+xml</code>.)</p>

<pre class="brush:xml">&lt;html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:svg="http://www.w3.org/2000/svg"&gt;
&lt;head&gt;
  &lt;script type="application/javascript"&gt;&lt;![CDATA[
  function test() {
    var text = document.getElementById('text');
    var circle = document.getElementById('circle');

    text.value = "&lt;svg:circle&gt; has:\n" +
                 "localName = '" + circle.localName + "'\n" +
                 "namespaceURI = '" + circle.namespaceURI + "'";
  }
  ]]&gt;&lt;/script&gt;
&lt;/head&gt;
&lt;body onload="test()"&gt;
  &lt;svg:svg version="1.1"
    width="100px" height="100px"
    viewBox="0 0 100 100"&gt;
    &lt;svg:circle cx="50" cy="50" r="30" style="fill:#aaa" id="circle"/&gt;
  &lt;/svg:svg&gt;
  &lt;textarea id="text" rows="4" cols="55"/&gt;
&lt;/body&gt;
&lt;/html&gt;
</pre>

<h2 id="说明">说明</h2>

<p>节点的本地名称是节点限定名的一部分出现在冒号之后。限定名通常当作特定 XML 文档命名空间的一部分。例如在限定名 <code>ecomm:partners 中 partners 是本地名，ecomm 是前缀。</code></p>

<pre class="brush:xml">&lt;ecomm:business id="soda_shop" type="brick_n_mortar" xmlns:ecomm="http://example.com/ecomm"&gt;
  &lt;ecomm:partners&gt;
    &lt;ecomm:partner id="1001"&gt;Tony's Syrup Warehouse
    &lt;/ecomm:partner&gt;
  &lt;/ecomm:partner&gt;
&lt;/ecomm:business&gt;
</pre>

<div class="note">
<p><strong>提示：</strong> 在 {{Gecko("1.9.2")}} 之前，此属性返回 HTML DOM 的 HTML 元素本地名称的大写版本 (而不是 XML DOM 的 HTML 元素). 在最后一个版本，符合 HTML5 规范下，当 HTML DOM 的 HTML 或 XML DOMs 的 XHTML 的小写元素时此属性返回内部 DOM storage。{{domxref("element.tagName","tagName")}} 属性仍然返回 HTML DOM 的 HTML 元素本地名称的大写版本。</p>
</div>

<h2 id="规范">规范</h2>

{{Specifications}}

<h2 id="浏览器兼容性">浏览器兼容性</h2>

{{Compat("api.Element.localName")}}

<h2 id="相关链接">相关链接</h2>

<ul>
 <li>{{domxref("Element.namespaceURI")}}</li>
 <li>{{domxref("Element.prefix")}}</li>
 <li>{{domxref("Attr.localName")}}</li>
 <li>{{domxref("Node.localName")}}</li>
</ul>