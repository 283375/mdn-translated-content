---
title: EventSource.close()
slug: Web/API/EventSource/close
---
<div>{{APIRef('WebSockets API')}}</div>

<div> </div>

<p>{{domxref("EventSource")}} 的方法<code><strong>close()</strong></code>用于关闭当前的连接，如果调用了此方法，则会将{{domxref("EventSource.readyState")}}这个属性值设置为 2 (closed)</p>

<div class="note">
<p><strong>Note</strong>: 如果连接已经被关闭，此方法不会做任何事情</p>
</div>

<h2 id="语法"><strong>语法</strong></h2>

<pre class="syntaxbox">eventSource.close();</pre>

<h3 id="参数">参数</h3>

<p>None.</p>

<h3 id="返回值">返回值</h3>

<p>Void.</p>

<h2 id="例子">例子</h2>

<pre class="brush: js">var button = document.querySelector('button');
var evtSource = new EventSource('sse.php');

button.onclick = function() {
  console.log('Connection closed');
  evtSource.close();
}
</pre>

<div class="note">
<p><strong>Note</strong>: 你可以在 Github 上查看这整个例子： <a href="https://github.com/mdn/dom-examples/tree/master/server-sent-events">Simple SSE demo using PHP.</a></p>
</div>

<h2 id="规范">规范</h2>

{{Specifications}}

<h2 id="浏览器兼容性">浏览器兼容性</h2>

{{Compat("api.EventSource.close")}}

<h2 id="相关链接">相关链接</h2>

<ul>
 <li>{{domxref("EventSource")}}</li>
</ul>