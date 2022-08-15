---
title: CanvasRenderingContext2D.transform()
slug: Web/API/CanvasRenderingContext2D/transform
---
<div>{{APIRef}}</div>

<p><code><strong>CanvasRenderingContext2D</strong></code><strong><code>.transform()</code></strong> 是 Canvas 2D API 使用矩阵多次叠加当前变换的方法，矩阵由方法的参数进行描述。你可以缩放、旋转、移动和倾斜上下文。</p>

<p>参见 {{domxref("CanvasRenderingContext2D.setTransform()", "setTransform()")}} 方法，这个方法使用单位矩阵重新设置当前的变换并且会调用 <code>transform()。</code></p>

<h2 id="语法">语法</h2>

<pre class="syntaxbox">void <var><em>ctx</em>.transform(a, b, c, d, e, f);</var>
</pre>

<p>变换矩阵的描述： <math><semantics><mrow><mo>[</mo><mtable columnalign="center center center" rowspacing="0.5ex"><mtr><mtd><mi>a</mi></mtd><mtd><mi>c</mi></mtd><mtd><mi>e</mi></mtd></mtr><mtr><mtd><mi>b</mi></mtd><mtd><mi>d</mi></mtd><mtd><mi>f</mi></mtd></mtr><mtr><mtd><mn>0</mn></mtd><mtd><mn>0</mn></mtd><mtd><mn>1</mn></mtd></mtr></mtable><mo>]</mo></mrow><annotation encoding="TeX">\left[ \begin{array}{ccc} a &amp; c &amp; e \\ b &amp; d &amp; f \\ 0 &amp; 0 &amp; 1 \end{array} \right]</annotation></semantics></math></p>

<h3 id="参数">参数</h3>

<dl>
 <dt><code>a (m11)</code></dt>
 <dd>水平缩放。</dd>
 <dt><em><code>b (m12)</code></em></dt>
 <dd>垂直倾斜。</dd>
 <dt><code>c (m21)</code></dt>
 <dd>水平倾斜。</dd>
 <dt><code>d (m22)</code></dt>
 <dd>垂直缩放。</dd>
 <dt><code>e (dx)</code></dt>
 <dd>水平移动。</dd>
 <dt><code>f (dy)</code></dt>
 <dd>垂直移动。</dd>
</dl>

<h2 id="示例">示例</h2>

<h3 id="使用_transform_方法">使用 <code>transform</code> 方法</h3>

<p>这是一段使用 <code>transform</code> 方法的简单的代码片段。</p>

<h4 id="HTML">HTML</h4>

<pre class="brush: html">&lt;canvas id="canvas"&gt;&lt;/canvas&gt;
</pre>

<h4 id="JavaScript">JavaScript</h4>

<pre class="brush: js; highlight:[4]">var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");

ctx.transform(1,1,0,1,0,0);
ctx.fillRect(0,0,100,100);
</pre>

<p>修改下面的代码并在线查看 canvas 的变化：</p>

<div class="hidden">
<h6 id="Playable_code">Playable code</h6>

<pre class="brush: html">&lt;canvas id="canvas" width="400" height="200" class="playable-canvas"&gt;&lt;/canvas&gt;
&lt;div class="playable-buttons"&gt;
  &lt;input id="edit" type="button" value="Edit" /&gt;
  &lt;input id="reset" type="button" value="Reset" /&gt;
&lt;/div&gt;
&lt;textarea id="code" class="playable-code"&gt;
ctx.transform(1,1,0,1,0,0);
ctx.fillRect(0,0,100,100);
ctx.setTransform();
&lt;/textarea&gt;
</pre>

<pre class="brush: js">var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");
var textarea = document.getElementById("code");
var reset = document.getElementById("reset");
var edit = document.getElementById("edit");
var code = textarea.value;

function drawCanvas() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  eval(textarea.value);
}

reset.addEventListener("click", function() {
  textarea.value = code;
  ctx.setTransform(1, 0, 0, 1, 0, 0);
  drawCanvas();
});

edit.addEventListener("click", function() {
  textarea.focus();
})

textarea.addEventListener("input", drawCanvas);
window.addEventListener("load", drawCanvas);
</pre>
</div>

<p>{{ EmbedLiveSample('Playable_code', 700, 360) }}</p>

<h2 id="规范描述">规范描述</h2>

{{Specifications}}

<h2 id="浏览器兼容性">浏览器兼容性</h2>

{{Compat("api.CanvasRenderingContext2D.transform")}}

<h2 id="参见">参见</h2>

<ul>
 <li>接口定义， {{domxref("CanvasRenderingContext2D")}}</li>
 <li>{{domxref("CanvasRenderingContext2D.setTransform()")}}</li>
</ul>