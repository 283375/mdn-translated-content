---
title: HTMLMediaElement.currentTime
slug: Web/API/HTMLMediaElement/currentTime
---
<div>{{APIRef("HTML DOM")}}</div>

<div> </div>

<div><strong><code>HTMLMediaElement.currentTime</code></strong> 属性会以秒为单位返回当前媒体元素的播放时间。设置这个属性会改变媒体元素当前播放位置。</div>

<div> </div>

<div class="note">
<p>该处原文：The <strong><code>HTMLMediaElement.currentTime</code></strong> property gives the current playback time in seconds. Setting this value seeks the media to the new time.</p>
</div>

<h2 id="Syntax">语法</h2>

<pre class="brush: js">var <em>cTime</em> = <em>video</em>.currentTime;
<em>video</em>.currentTime = 35;</pre>

<p> </p>

<h3 id="返回值">返回值</h3>

<p>一个 <code>double</code> 双精度浮点数。</p>

<h2 id="Examples">示例</h2>

<pre class="brush: js">var obj = document.createElement('video');
console.log(obj.currentTime);
</pre>

<h2 id="规范">规范</h2>

{{Specifications}}

<h2 id="浏览器兼容性">浏览器兼容性</h2>

{{Compat("api.HTMLMediaElement.currentTime")}}

<h2 id="See_Also">相关链接</h2>

<ul>
 <li>The interface defining it, {{domxref("HTMLMediaElement")}}.</li>
 <li><code><a href="/en-US/docs/Web/API/HTMLMediaElement/fastSeek">HTMLMediaElement.fastSeek()</a></code> for another method of setting the time</li>
</ul>