---
title: 'OfflineAudioContext: complete event'
slug: Web/API/OfflineAudioContext/complete_event
---
<p>{{DefaultAPISidebar("Web Audio API")}}</p>

<p><code>complete</code>当离线音频上下文的呈现完成时，将触发{{domxref("OfflineAudioContext")}}接口的事件。</p>

<table class="properties">
 <tbody>
  <tr>
   <th scope="row">泡泡</th>
   <td>没有</td>
  </tr>
  <tr>
   <th scope="row">取消</th>
   <td>没有</td>
  </tr>
  <tr>
   <th scope="row">默认操作</th>
   <td>没有</td>
  </tr>
  <tr>
   <th scope="row">接口</th>
   <td>{{domxref( "OfflineAudioCompletionEvent")}}</td>
  </tr>
  <tr>
   <th scope="row">事件处理程序属性</th>
   <td>{{domxref( "OfflineAudioContext.oncomplete")}}</td>
  </tr>
 </tbody>
</table>

<h2 id="例子">例子</h2>

<p>处理完成后，您可能希望使用<code>oncomplete</code>处理程序提示用户现在可以播放音频，并启用播放按钮：</p>

<pre class="brush: js">offlineAudioCtx.addEventListener('complete',()=&gt; {
  console.log('Offline audio processing now complete');
  showModalDialog('Song processed and ready to play');
  playBtn.disabled = false;
})</pre>

<p>You can also set up the event handler using the {{domxref("OfflineAudioContext.oncomplete")}} property:</p>

<pre class="brush: js">offlineAudioCtx.oncomplete = function() {
  console.log('Offline audio processing now complete');
  showModalDialog('Song processed and ready to play');
  playBtn.disabled = false;
}</pre>

<h2 id="Specifications">Specifications</h2>

{{Specifications}}

<h2 id="Browser_compatibility">Browser compatibility</h2>



<p>{{Compat("api.OfflineAudioContext.complete_event")}}</p>

<h2 id="See_also">See also</h2>

<ul>
 <li>{{domxref( "离线音频上下文.oncomplete")}}</li>
 <li><a href="/en-US/docs/Web_Audio_API">Web Audio API</a></li>
</ul>