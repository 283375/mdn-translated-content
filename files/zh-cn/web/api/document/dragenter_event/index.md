---
title: dragenter
slug: Web/API/Document/dragenter_event
---
<div>当拖动的元素或被选择的文本进入有效的放置目标时， <code>dragenter</code> 事件被触发。</div>

<h2 id="基本信息">基本信息</h2>

<table class="properties">
 <tbody>
  <tr>
   <td>是否冒泡</td>
   <td>是</td>
  </tr>
  <tr>
   <td>是否可取消</td>
   <td>是</td>
  </tr>
  <tr>
   <td>目标对象</td>
   <td>用户指定的元素或者 body 元素</td>
  </tr>
  <tr>
   <td>接口</td>
   <td>{{domxref("DragEvent")}}</td>
  </tr>
  <tr>
   <td>默认动作</td>
   <td>取消拖动</td>
  </tr>
 </tbody>
</table>

<h2 id="属性">属性</h2>

<table>
 <thead>
  <tr>
   <th scope="col">属性</th>
   <th scope="col">类型</th>
   <th scope="col">描述</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td><code>target</code> {{readonlyInline}}</td>
   <td><a href="/en-US/docs/Web/API/EventTarget"><code>EventTarget</code></a></td>
   <td>被拖动的元素下面的元素。</td>
  </tr>
  <tr>
   <td><code>type</code> {{readonlyInline}}</td>
   <td><a href="/en-US/docs/Web/API/DOMString"><code>DOMString</code></a></td>
   <td>事件类型</td>
  </tr>
  <tr>
   <td><code>bubbles</code> {{readonlyInline}}</td>
   <td><a href="/en-US/docs/Web/API/Boolean"><code>Boolean</code></a></td>
   <td>事件是否正常冒泡</td>
  </tr>
  <tr>
   <td><code>cancelable</code> {{readonlyInline}}</td>
   <td><a href="/en-US/docs/Web/API/Boolean"><code>Boolean</code></a></td>
   <td>事件是否已被取消？</td>
  </tr>
  <tr>
   <td><code>view</code> {{readonlyInline}}</td>
   <td><a href="/en-US/docs/Web/API/WindowProxy"><code>WindowProxy</code></a></td>
   <td><a href="/en-US/docs/Web/API/Document/defaultView"><code>document.defaultView</code></a> (<code>window</code> of the document)</td>
  </tr>
  <tr>
   <td><code>detail</code> {{readonlyInline}}</td>
   <td><code>long</code> (<code>float</code>)</td>
   <td>0.</td>
  </tr>
  <tr>
   <td><code>dataTransfer</code></td>
   <td>DataTransfer</td>
   <td>The data that underlies a drag-and-drop operation, known as the <a href="/en-US/docs/Web/API/DataTransfer">drag data store</a>. Protected mode.</td>
  </tr>
  <tr>
   <td><code>currentTarget</code> {{readonlyInline}}</td>
   <td>EventTarget</td>
   <td>触发事件的元素</td>
  </tr>
  <tr>
   <td><code>relatedTarget</code> {{readonlyInline}}</td>
   <td>EventTarget</td>
   <td>For <code>mouseover</code>, <code>mouseout</code>, <code>mouseenter</code> and <code>mouseleave</code> events: the target of the complementary event (the <code>mouseleave</code> target in the case of a <code>mouseenter</code> event). <code>null</code> otherwise.</td>
  </tr>
  <tr>
   <td><code>screenX</code> {{readonlyInline}}</td>
   <td>long</td>
   <td>全局（屏幕）坐标中鼠标指针的 X 坐标。</td>
  </tr>
  <tr>
   <td><code>screenY</code> {{readonlyInline}}</td>
   <td>long</td>
   <td>全局（屏幕）坐标中鼠标指针的 Y 坐标。</td>
  </tr>
  <tr>
   <td><code>clientX</code> {{readonlyInline}}</td>
   <td>long</td>
   <td>本地（DOM 内容）坐标中鼠标指针的 X 坐标。</td>
  </tr>
  <tr>
   <td><code>clientY</code> {{readonlyInline}}</td>
   <td>long</td>
   <td>本地（DOM 内容）坐标中鼠标指针的 Y 坐标。</td>
  </tr>
  <tr>
   <td><code>button</code> {{readonlyInline}}</td>
   <td>unsigned short</td>
   <td>鼠标事件触发时按下的按钮号：左键= 0，中键= 1（如果存在），右键= 2。 对于配置为左手使用的鼠标，其中按钮操作反转，则值从右向左读取。</td>
  </tr>
  <tr>
   <td><code>buttons</code> {{readonlyInline}}</td>
   <td>unsigned short</td>
   <td>The buttons being pressed when the mouse event was fired: Left button=1, Right button=2, Middle (wheel) button=4, 4th button (typically, "Browser Back" button)=8, 5th button (typically, "Browser Forward" button)=16. If two or more buttons are pressed, returns the logical sum of the values. E.g., if Left button and Right button are pressed, returns 3 (=1 | 2). <a href="/en-US/docs/Web/API/MouseEvent">More info</a>.</td>
  </tr>
  <tr>
   <td><code>mozPressure</code> {{readonlyInline}}</td>
   <td>float</td>
   <td>触发事件时屏幕的压力值，介于 0.0 到 1.0 之间</td>
  </tr>
  <tr>
   <td><code>ctrlKey</code> {{readonlyInline}}</td>
   <td>boolean</td>
   <td>事件触发时 ctrl 键是否被按下</td>
  </tr>
  <tr>
   <td><code>shiftKey</code> {{readonlyInline}}</td>
   <td>boolean</td>
   <td>事件触发时 shift 键是否被按下</td>
  </tr>
  <tr>
   <td><code>altKey</code> {{readonlyInline}}</td>
   <td>boolean</td>
   <td>事件触发时 alt 键是否被按下</td>
  </tr>
  <tr>
   <td><code>metaKey</code> {{readonlyInline}}</td>
   <td>boolean</td>
   <td>事件触发时 meta 键是否被按下</td>
  </tr>
 </tbody>
</table>

<h2 id="示例：dropzone">示例：dropzone</h2>

<p>{{page('/zh-CN/docs/Web/Events/dragstart', '示例：dropzone')}}</p>

<h2 id="规范">规范</h2>

{{Specifications}}

<h2 id="浏览器支持">浏览器支持</h2>

{{Compat("api.Document.dragenter_event")}}

<h2 id="相关">相关</h2>

<ul>
 <li>{{event("drag")}}</li>
 <li>{{event("dragstart")}}</li>
 <li>{{event("dragend")}}</li>
 <li>{{event("dragover")}}</li>
 <li>{{event("dragenter")}}</li>
 <li>{{event("dragleave")}}</li>
 <li>{{event("dragexit")}}</li>
 <li>{{event("drop")}}</li>
</ul>