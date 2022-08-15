---
title: Element.requestFullscreen()
slug: Web/API/Element/requestFullScreen
---
<div>{{APIRef("Fullscreen API")}}</div>

<p><code><strong>Element.requestFullscreen()</strong></code> 方法用于发出异步请求使元素进入全屏模式。</p>

<p>调用此 API 并不能保证元素一定能够进入全屏模式。如果元素被允许进入全屏幕模式，返回的{{JSxRef("Promise")}}会 resolve，并且该元素会收到一个{{event("fullscreenchange")}}事件，通知它已经进入全屏模式。如果全屏请求被拒绝，返回的 promise 会变成 rejected 并且该元素会收到一个{{Event('fullscreenerror')}}事件。如果该元素已经从原来的文档中分离，那么该文档将会收到这些事件。</p>

<p>早期的 Fullscreen API 实现总是会把这些事件发送给 document，而不是调用的元素，所以你自己可能需要处理这样的情况。参考 {{SectionOnPage("/zh-CN/docs/Web/Events/fullscreen", "Browser compatibility")}} 来得知哪些浏览器做了这个改动。</p>

<div class="note">
<p>注意：这个方法只能在用户交互或者设备方向改变的时候调用，否则将会失败。</p>
</div>

<h2 id="语法">语法</h2>

<pre class="syntaxbox"><em>var Promise = Element</em>.requestFullscreen(options);
</pre>

<h3 id="参数">参数</h3>

<p><code>options</code> {{optional_inline}}</p>

<p>一个{{domxref("FullscreenOptions")}}对象提供切换到全屏模式的控制选项。目前，唯一的选项是{{domxref("FullscreenOptions.navigationUI", "navigationUI")}}，这控制了是否在元素处于全屏模式时显示导航条 UI。默认值是<code>"auto"</code>，表明这将由浏览器来决定是否显示导航条。</p>

<h3 id="返回值">返回值</h3>

<p>在完成切换全屏模式后，返回一个已经用值 <code>undefined</code> resolved 的{{JSxRef("Promise")}}</p>

<h3 id="异常">异常</h3>

<p><em><code>requestFullscreen()</code> </em><em>通过拒绝返回的 <code>Promise</code>来生成错误条件，而不是抛出一个传统的异常。拒绝控制器接收以下的某一个值：</em></p>

<dl>
 <dt>{{jsxref("TypeError")}}</dt>
 <dd>在以下几种情况下，会抛出<code>TypeError</code>：</dd>
</dl>

<ul>
 <li>文档中包含的元素未完全激活，也就是说不是当前活动的元素。</li>
 <li>元素不在文档之内。</li>
 <li>因为功能策略限制配置或其他访问控制，元素不被允许使用<code>"fullscreen"</code>功能。</li>
 <li>元素和它的文档是同一个节点。 </li>
</ul>

<dl>
</dl>

<h2 id="示例">示例</h2>

<p>在调用<code>requestFullScreen() 之前，可以对</code>{{event("fullscreenchange")}} 和 {{event("fullscreenerror")}}事件进行监听，这样在请求进入全屏模式成功或者失败时都能收到事件通知。</p>

<h2 id="规范">规范</h2>

{{Specifications}}

<h2 id="浏览器兼容性">浏览器兼容性</h2>

{{Compat("api.Element.requestFullscreen")}}

<h2 id="参阅">参阅</h2>

<ul>
 <li><a href="/en-US/docs/Web/API/Fullscreen_API">Full-screen API</a></li>
 <li>{{ domxref("Element.requestFullscreen()") }}</li>
 <li>{{ domxref("Document.exitFullscreen()") }}</li>
 <li>{{ domxref("Document.fullscreen") }}</li>
 <li>{{ domxref("Document.fullscreenElement") }}</li>
 <li>{{ cssxref(":fullscreen") }}</li>
 <li>{{ HTMLAttrXRef("allowfullscreen", "iframe") }}</li>
</ul>