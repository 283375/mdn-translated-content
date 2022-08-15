---
title: KeyboardEvent.keyCode
slug: Web/API/KeyboardEvent/keyCode
---
<p>{{APIRef("DOM Events")}}{{deprecated_header()}}</p>

<p>这个只读的属性 <strong><code>KeyboardEvent.keyCode</code></strong> 代表着一个唯一标识的所按下的键的未修改值，它依据于一个系统和实现相关的数字代码。这通常是与密钥对应的二进制的 ASCII ({{RFC(20)}}) 或 Windows 1252 码。如果这个键不能被标志，这个值为 0。</p>

<p>你应该尽量避免使用它；它已经被弃用了一段时间。相反的，如果它在你的浏览器中被实现了的话，你应该使用{{domxref("KeyboardEvent.code")}}。 不幸的是，有一些浏览器还是没有实现它，所以你在使用之前必须要小心，确认你所使用的那个被所有目标浏览器所支持。</p>

<div class="note">
<p>在处理 keydown 和 keyup 事件时，Web 开发人员不应使用可打印字符的 keycode 属性。如上所述，keycode 属性对可打印字符不有用，尤其是那些按下 shift 或 alt 键的输入。在实现快捷键处理程序时，事件（“keypress”）事件通常更好（至少当 gecko 是正在使用的运行时）。详情请参见 Gecko 按键事件。</p>
</div>

<h2 id="Example">Example</h2>

<pre class="brush: js">window.addEventListener("keydown", function (event) {
  if (event.defaultPrevented) {
    return; // 如果已取消默认操作，则不应执行任何操作
  }

  var handled = false;
  if (event.key !== undefined) {
    // 使用 KeyboardEvent.key 处理事件，并将 handled 设置为 true。
  } else if (event.keyCode !== undefined) {
    //使用 KeyboardEvent.keyCode 处理事件并将 handled 设置为 true。
  }

  if (handled) {
    // 如果事件已处理，则禁止“双重操作”
    event.preventDefault();
  }
}, true);
</pre>

<h2 id="规范">规范</h2>

{{Specifications}}

<h2 id="浏览器兼容性">浏览器兼容性</h2>

{{Compat("api.KeyboardEvent.keyCode")}}

<h2 id="键码值">键码值</h2>

<h3 id="标准位置的可打印键">标准位置的可打印键</h3>

<p>在标准位置按下或释放可打印键导致的按键事件值在浏览器之间不兼容。<br>
 IE 只将本机虚拟密钥代码值公开为 keyboardvent.keycode。<br>
 Google Chrome、Chromium 和 Safari 必须根据输入字符确定值。如果输入字符可以用 US 键盘布局输入，则使用 US 键盘布局上的 keycode 值。<br>
 从 gecko 15 geckore lease（“15.0”）开始，gecko 从一个可由键输入的 ASCII 字符（即使具有移位修饰符或支持 ASCII 的键盘布局）决定键码值。有关详细信息，请参见以下规则：</p>

<ol>
 <li>如果系统是 Windows，并且按下键的本机键代码指示键是 A-Z 或 0-9，请使用 keycode。</li>
 <li>如果系统是 Mac，并且按下键的本机键码指示键为 0-9，则使用 keycode。</li>
 <li>如果按下键输入一个 ASCII 字母或数字，没有修改键，请使用 keycode。</li>
 <li>如果按下键输入带 SHIFT 键的 ASCII 字母或数字，请使用 keycode。</li>
 <li>如果按下键输入另一个没有修改键的 ASCII 字符，请使用 keycode。</li>
 <li>如果按下键输入另一个带 SHIFT 键的 ASCII 字符，请使用 keycode。</li>
 <li>否则，即按下键输入一个 Unicode 字符：</li>
</ol>

<ul>
 <li>如果键盘布局是支持 ASCII 的键盘布局（即，可以输入 ASCII 字母），则使用 0 或者根据下面的附加规则计算。</li>
 <li>否则，即键盘布局不支持 ASCII，使用环境中安装的具有最高优先级的支持 ASCII 的键盘布局：
  <ul>
   <li>如果按备用键盘布局上的键输入一个 ASCII 字母或数字，请使用 keycode。</li>
   <li>否则，使用 0 或者根据下面的附加规则计算。</li>
  </ul>
 </li>
</ul>

<p>从 Firefox 60 {{geckoRelease("60.0")}}开始，Gecko 会尽可能的根据以下规则额设置标点符号的 <code>keyCode</code> 值（当满足上述 7.1 或者 7.2 的时候）:</p>

<div class="warning">
<p>这些附加规则的目的是为了使键盘布局映射 unicode 字符映射到美国键盘标点符号的用户可以使用只支持 ASCII 的键盘或者美国键盘布局的 Firefox 的 web 应用。否则，新映射的 <code>keyCode</code> 值可能会和其他按键冲突。例如，如果当前键盘布局是俄语，<code>"Period"</code> 键 和 <code>"Slash"</code> 键的 <code>keyCode</code> 都会是 <code>190</code> （<code>KeyEvent.DOM_VK_PERIOD</code>）。如果你需要区分这些按键但是你自己又不想支持时间上所有的键盘布局，你可能应该使用 {{domxref("KeyboardEvent.code")}}。</p>
</div>

<ol>
 <li>如果运行 macOS 或者 Linux:
  <ol>
   <li>如果你当前的键盘布局不支持 ASCII 并且候选支持 ASCII 键盘布局可用。
    <ol>
     <li>如果候选支持 ASCII 的键盘布局仅通过未修改的键产生 ASCII 字符，请对该字符使用<code>keyCode。</code></li>
     <li>如果候选支持 ASCII 的键盘布局产生带有 Shift 键修饰符的 ASCII 字符，请对该字符使用<code>keyCode</code>。</li>
     <li>否则，在美国键盘布局激活时，使用使用<code>keyCode</code>表示由按键产生的 ASCII 字符。</li>
    </ol>
   </li>
   <li>否则，在美国键盘布局激活时，使用使用<code>keyCode</code>表示由按键产生的 ASCII 字符。</li>
  </ol>
 </li>
 <li>如果运行 Windows：
  <ol>
   <li>当美国键盘布局激活时，使用映射到 Windows 的相同虚拟键代码的按键产生的 ASCII 字符的<code>keyCode</code>值。</li>
  </ol>
 </li>
</ol>

<p>由标准位置的可打印键引起的每个浏览器的 keydown 事件的 keycode 值</p>

<table>
 <thead>
  <tr>
   <th colspan="1" rowspan="3" scope="row">{{domxref("KeyboardEvent.code")}}</th>
   <th colspan="3" rowspan="1" scope="col">Internet Explorer 11</th>
   <th colspan="6" rowspan="1" scope="col">Google Chrome 34</th>
   <th colspan="3" rowspan="1" scope="col">Chromium 34</th>
   <th colspan="3" rowspan="1" scope="col">Safari 7</th>
   <th colspan="9" rowspan="1" scope="col">Gecko 29</th>
  </tr>
  <tr>
   <th colspan="3" rowspan="1" scope="col">Windows</th>
   <th colspan="3" rowspan="1" scope="col">Windows</th>
   <th colspan="3" rowspan="1" scope="col">Mac (10.9)</th>
   <th colspan="3" rowspan="1" scope="col">Linux (Ubuntu 14.04)</th>
   <th colspan="3" rowspan="1" scope="col">Mac (10.9)</th>
   <th colspan="3" rowspan="1" scope="col">Windows</th>
   <th colspan="3" rowspan="1" scope="col">Mac (10.9)</th>
   <th colspan="3" rowspan="1" scope="col">Linux (Ubuntu 14.04)</th>
  </tr>
  <tr>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th colspan="1" scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
  </tr>
 </thead>
 <tfoot>
  <tr>
   <th colspan="1" rowspan="3" scope="row">{{domxref("KeyboardEvent.code")}}</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th colspan="1" scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
  </tr>
  <tr>
   <th colspan="3" rowspan="1" scope="col">Windows</th>
   <th colspan="3" rowspan="1" scope="col">Windows</th>
   <th colspan="3" rowspan="1" scope="col">Mac (10.9)</th>
   <th colspan="3" rowspan="1" scope="col">Linux (Ubuntu 14.04)</th>
   <th colspan="3" rowspan="1" scope="col">Mac (10.9)</th>
   <th colspan="3" rowspan="1" scope="col">Windows</th>
   <th colspan="3" rowspan="1" scope="col">Mac (10.9)</th>
   <th colspan="3" rowspan="1" scope="col">Linux (Ubuntu 14.04)</th>
  </tr>
  <tr>
   <th colspan="3" rowspan="1" scope="col">Internet Explorer 11</th>
   <th colspan="6" rowspan="1" scope="col">Google Chrome 34</th>
   <th colspan="3" rowspan="1" scope="col">Chromium 34</th>
   <th colspan="3" rowspan="1" scope="col">Safari 7</th>
   <th colspan="9" rowspan="1" scope="col">Gecko 29</th>
  </tr>
 </tfoot>
 <tbody>
  <tr>
   <th scope="row"><code>"Digit1"</code></th>
   <td colspan="3" rowspan="1"><code>0x31 (49)</code></td>
   <td colspan="3" rowspan="1"><code>0x31 (49)</code></td>
   <td colspan="3" rowspan="1"><code>0x31 (49)</code></td>
   <td colspan="3" rowspan="1"><code>0x31 (49)</code></td>
   <td colspan="3" rowspan="1"><code>0x31 (49)</code></td>
   <td colspan="3" rowspan="1"><code>0x31 (49)</code></td>
   <td colspan="3" rowspan="1"><code>0x31 (49)</code></td>
   <td colspan="3" rowspan="1"><code>0x31 (49)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Digit2"</code></th>
   <td colspan="3" rowspan="1"><code>0x32 (50)</code></td>
   <td colspan="3" rowspan="1"><code>0x32 (50)</code></td>
   <td colspan="3" rowspan="1"><code>0x32 (50)</code></td>
   <td colspan="3" rowspan="1"><code>0x32 (50)</code></td>
   <td colspan="3" rowspan="1"><code>0x32 (50)</code></td>
   <td colspan="3" rowspan="1"><code>0x32 (50)</code></td>
   <td colspan="3" rowspan="1"><code>0x32 (50)</code></td>
   <td colspan="3" rowspan="1"><code>0x32 (50)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Digit3"</code></th>
   <td colspan="3" rowspan="1"><code>0x33 (51)</code></td>
   <td colspan="3" rowspan="1"><code>0x33 (51)</code></td>
   <td colspan="3" rowspan="1"><code>0x33 (51)</code></td>
   <td colspan="3" rowspan="1"><code>0x33 (51)</code></td>
   <td colspan="3" rowspan="1"><code>0x33 (51)</code></td>
   <td colspan="3" rowspan="1"><code>0x33 (51)</code></td>
   <td colspan="3" rowspan="1"><code>0x33 (51)</code></td>
   <td colspan="3" rowspan="1"><code>0x33 (51)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Digit4"</code></th>
   <td colspan="3" rowspan="1"><code>0x34 (52)</code></td>
   <td colspan="3" rowspan="1"><code>0x34 (52)</code></td>
   <td colspan="3" rowspan="1"><code>0x34 (52)</code></td>
   <td colspan="3" rowspan="1"><code>0x34 (52)</code></td>
   <td colspan="3" rowspan="1"><code>0x34 (52)</code></td>
   <td colspan="3" rowspan="1"><code>0x34 (52)</code></td>
   <td colspan="3" rowspan="1"><code>0x34 (52)</code></td>
   <td colspan="3" rowspan="1"><code>0x34 (52)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Digit5"</code></th>
   <td colspan="3" rowspan="1"><code>0x35 (53)</code></td>
   <td colspan="3" rowspan="1"><code>0x35 (53)</code></td>
   <td colspan="3" rowspan="1"><code>0x35 (53)</code></td>
   <td colspan="3" rowspan="1"><code>0x35 (53)</code></td>
   <td colspan="3" rowspan="1"><code>0x35 (53)</code></td>
   <td colspan="3" rowspan="1"><code>0x35 (53)</code></td>
   <td colspan="3" rowspan="1"><code>0x35 (53)</code></td>
   <td colspan="3" rowspan="1"><code>0x35 (53)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Digit6"</code></th>
   <td colspan="3" rowspan="1"><code>0x36 (54)</code></td>
   <td colspan="3" rowspan="1"><code>0x36 (54)</code></td>
   <td colspan="3" rowspan="1"><code>0x36 (54)</code></td>
   <td colspan="3" rowspan="1"><code>0x36 (54)</code></td>
   <td colspan="3" rowspan="1"><code>0x36 (54)</code></td>
   <td colspan="3" rowspan="1"><code>0x36 (54)</code></td>
   <td colspan="3" rowspan="1"><code>0x36 (54)</code></td>
   <td colspan="3" rowspan="1"><code>0x36 (54)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Digit7"</code></th>
   <td colspan="3" rowspan="1"><code>0x37 (55)</code></td>
   <td colspan="3" rowspan="1"><code>0x37 (55)</code></td>
   <td colspan="3" rowspan="1"><code>0x37 (55)</code></td>
   <td colspan="3" rowspan="1"><code>0x37 (55)</code></td>
   <td colspan="3" rowspan="1"><code>0x37 (55)</code></td>
   <td colspan="3" rowspan="1"><code>0x37 (55)</code></td>
   <td colspan="3" rowspan="1"><code>0x37 (55)</code></td>
   <td colspan="3" rowspan="1"><code>0x37 (55)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Digit8"</code></th>
   <td colspan="3" rowspan="1"><code>0x38 (56)</code></td>
   <td colspan="3" rowspan="1"><code>0x38 (56)</code></td>
   <td colspan="3" rowspan="1"><code>0x38 (56)</code></td>
   <td colspan="3" rowspan="1"><code>0x38 (56)</code></td>
   <td colspan="3" rowspan="1"><code>0x38 (56)</code></td>
   <td colspan="3" rowspan="1"><code>0x38 (56)</code></td>
   <td colspan="3" rowspan="1"><code>0x38 (56)</code></td>
   <td colspan="3" rowspan="1"><code>0x38 (56)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Digit9"</code></th>
   <td colspan="3" rowspan="1"><code>0x39 (57)</code></td>
   <td colspan="3" rowspan="1"><code>0x39 (57)</code></td>
   <td colspan="3" rowspan="1"><code>0x39 (57)</code></td>
   <td colspan="3" rowspan="1"><code>0x39 (57)</code></td>
   <td colspan="3" rowspan="1"><code>0x39 (57)</code></td>
   <td colspan="3" rowspan="1"><code>0x39 (57)</code></td>
   <td colspan="3" rowspan="1"><code>0x39 (57)</code></td>
   <td colspan="3" rowspan="1"><code>0x39 (57)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Digit0"</code></th>
   <td colspan="3" rowspan="1"><code>0x30 (48)</code></td>
   <td colspan="3" rowspan="1"><code>0x30 (48)</code></td>
   <td colspan="3" rowspan="1"><code>0x30 (48)</code></td>
   <td colspan="3" rowspan="1"><code>0x30 (48)</code></td>
   <td colspan="3" rowspan="1"><code>0x30 (48)</code></td>
   <td colspan="3" rowspan="1"><code>0x30 (48)</code></td>
   <td colspan="3" rowspan="1"><code>0x30 (48)</code></td>
   <td colspan="3" rowspan="1"><code>0x30 (48)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyA"</code></th>
   <td colspan="3" rowspan="1"><code>0x41 (65)</code></td>
   <td colspan="3" rowspan="1"><code>0x41 (65)</code></td>
   <td colspan="3" rowspan="1"><code>0x41 (65)</code></td>
   <td colspan="3" rowspan="1"><code>0x41 (65)</code></td>
   <td colspan="3" rowspan="1"><code>0x41 (65)</code></td>
   <td colspan="3" rowspan="1"><code>0x41 (65)</code></td>
   <td colspan="3" rowspan="1"><code>0x41 (65)</code></td>
   <td colspan="3" rowspan="1"><code>0x41 (65)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyB"</code></th>
   <td colspan="3" rowspan="1"><code>0x42 (66)</code></td>
   <td colspan="3" rowspan="1"><code>0x42 (66)</code></td>
   <td colspan="3" rowspan="1"><code>0x42 (66)</code></td>
   <td colspan="3" rowspan="1"><code>0x42 (66)</code></td>
   <td colspan="3" rowspan="1"><code>0x42 (66)</code></td>
   <td colspan="3" rowspan="1"><code>0x42 (66)</code></td>
   <td colspan="3" rowspan="1"><code>0x42 (66)</code></td>
   <td colspan="3" rowspan="1"><code>0x42 (66)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyC"</code></th>
   <td colspan="3" rowspan="1"><code>0x43 (67)</code></td>
   <td colspan="3" rowspan="1"><code>0x43 (67)</code></td>
   <td colspan="3" rowspan="1"><code>0x43 (67)</code></td>
   <td colspan="3" rowspan="1"><code>0x43 (67)</code></td>
   <td colspan="3" rowspan="1"><code>0x43 (67)</code></td>
   <td colspan="3" rowspan="1"><code>0x43 (67)</code></td>
   <td colspan="3" rowspan="1"><code>0x43 (67)</code></td>
   <td colspan="3" rowspan="1"><code>0x43 (67)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyD"</code></th>
   <td colspan="3" rowspan="1"><code>0x44 (68)</code></td>
   <td colspan="3" rowspan="1"><code>0x44 (68)</code></td>
   <td colspan="3" rowspan="1"><code>0x44 (68) </code></td>
   <td colspan="3" rowspan="1"><code>0x44 (68)</code></td>
   <td colspan="3" rowspan="1"><code>0x44 (68)</code></td>
   <td colspan="3" rowspan="1"><code>0x44 (68)</code></td>
   <td colspan="3" rowspan="1"><code>0x44 (68)</code></td>
   <td colspan="3" rowspan="1"><code>0x44 (68)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyE"</code></th>
   <td colspan="3" rowspan="1"><code>0x45 (69)</code></td>
   <td colspan="3" rowspan="1"><code>0x45 (69)</code></td>
   <td colspan="3" rowspan="1"><code>0x45 (69)</code></td>
   <td colspan="3" rowspan="1"><code>0x45 (69)</code></td>
   <td colspan="3" rowspan="1"><code>0x45 (69)</code></td>
   <td colspan="3" rowspan="1"><code>0x45 (69)</code></td>
   <td colspan="3" rowspan="1"><code>0x45 (69)</code></td>
   <td colspan="3" rowspan="1"><code>0x45 (69)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyF"</code></th>
   <td colspan="3" rowspan="1"><code>0x46 (70)</code></td>
   <td colspan="3" rowspan="1"><code>0x46 (70)</code></td>
   <td colspan="3" rowspan="1"><code>0x46 (70)</code></td>
   <td colspan="3" rowspan="1"><code>0x46 (70)</code></td>
   <td colspan="3" rowspan="1"><code>0x46 (70)</code></td>
   <td colspan="3" rowspan="1"><code>0x46 (70)</code></td>
   <td colspan="3" rowspan="1"><code>0x46 (70)</code></td>
   <td colspan="3" rowspan="1"><code>0x46 (70)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyG"</code></th>
   <td colspan="3" rowspan="1"><code>0x47 (71)</code></td>
   <td colspan="3" rowspan="1"><code>0x47 (71)</code></td>
   <td colspan="3" rowspan="1"><code>0x47 (71)</code></td>
   <td colspan="3" rowspan="1"><code>0x47 (71)</code></td>
   <td colspan="3" rowspan="1"><code>0x47 (71)</code></td>
   <td colspan="3" rowspan="1"><code>0x47 (71)</code></td>
   <td colspan="3" rowspan="1"><code>0x47 (71)</code></td>
   <td colspan="3" rowspan="1"><code>0x47 (71)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyH"</code></th>
   <td colspan="3" rowspan="1"><code>0x48 (72)</code></td>
   <td colspan="3" rowspan="1"><code>0x48 (72)</code></td>
   <td colspan="3" rowspan="1"><code>0x48 (72)</code></td>
   <td colspan="3" rowspan="1"><code>0x48 (72)</code></td>
   <td colspan="3" rowspan="1"><code>0x48 (72)</code></td>
   <td colspan="3" rowspan="1"><code>0x48 (72)</code></td>
   <td colspan="3" rowspan="1"><code>0x48 (72)</code></td>
   <td colspan="3" rowspan="1"><code>0x48 (72)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyI"</code></th>
   <td colspan="3" rowspan="1"><code>0x49 (73)</code></td>
   <td colspan="3" rowspan="1"><code>0x49 (73)</code></td>
   <td colspan="3" rowspan="1"><code>0x49 (73)</code></td>
   <td colspan="3" rowspan="1"><code>0x49 (73)</code></td>
   <td colspan="3" rowspan="1"><code>0x49 (73)</code></td>
   <td colspan="3" rowspan="1"><code>0x49 (73)</code></td>
   <td colspan="3" rowspan="1"><code>0x49 (73)</code></td>
   <td colspan="3" rowspan="1"><code>0x49 (73)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyJ"</code></th>
   <td colspan="3" rowspan="1"><code>0x4A (74)</code></td>
   <td colspan="3" rowspan="1"><code>0x4A (74)</code></td>
   <td colspan="3" rowspan="1"><code>0x4A (74)</code></td>
   <td colspan="3" rowspan="1"><code>0x4A (74)</code></td>
   <td colspan="3" rowspan="1"><code>0x4A (74)</code></td>
   <td colspan="3" rowspan="1"><code>0x4A (74)</code></td>
   <td colspan="3" rowspan="1"><code>0x4A (74)</code></td>
   <td colspan="3" rowspan="1"><code>0x4A (74)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyK"</code></th>
   <td colspan="3" rowspan="1"><code>0x4B (75)</code></td>
   <td colspan="3" rowspan="1"><code>0x4B (75)</code></td>
   <td colspan="3" rowspan="1"><code>0x4B (75)</code></td>
   <td colspan="3" rowspan="1"><code>0x4B (75)</code></td>
   <td colspan="3" rowspan="1"><code>0x4B (75)</code></td>
   <td colspan="3" rowspan="1"><code>0x4B (75)</code></td>
   <td colspan="3" rowspan="1"><code>0x4B (75)</code></td>
   <td colspan="3" rowspan="1"><code>0x4B (75)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyL"</code></th>
   <td colspan="3" rowspan="1"><code>0x4C (76)</code></td>
   <td colspan="3" rowspan="1"><code>0x4C (76)</code></td>
   <td colspan="3" rowspan="1"><code>0x4C (76)</code></td>
   <td colspan="3" rowspan="1"><code>0x4C (76)</code></td>
   <td colspan="3" rowspan="1"><code>0x4C (76)</code></td>
   <td colspan="3" rowspan="1"><code>0x4C (76)</code></td>
   <td colspan="3" rowspan="1"><code>0x4C (76)</code></td>
   <td colspan="3" rowspan="1"><code>0x4C (76)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyM"</code></th>
   <td colspan="3" rowspan="1"><code>0x4D (77)</code></td>
   <td colspan="3" rowspan="1"><code>0x4D (77)</code></td>
   <td colspan="3" rowspan="1"><code>0x4D (77)</code></td>
   <td colspan="3" rowspan="1"><code>0x4D (77)</code></td>
   <td colspan="3" rowspan="1"><code>0x4D (77)</code></td>
   <td colspan="3" rowspan="1"><code>0x4D (77)</code></td>
   <td colspan="3" rowspan="1"><code>0x4D (77)</code></td>
   <td colspan="3" rowspan="1"><code>0x4D (77)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyN"</code></th>
   <td colspan="3" rowspan="1"><code>0x4E (78)</code></td>
   <td colspan="3" rowspan="1"><code>0x4E (78)</code></td>
   <td colspan="3" rowspan="1"><code>0x4E (78)</code></td>
   <td colspan="3" rowspan="1"><code>0x4E (78)</code></td>
   <td colspan="3" rowspan="1"><code>0x4E (78)</code></td>
   <td colspan="3" rowspan="1"><code>0x4E (78)</code></td>
   <td colspan="3" rowspan="1"><code>0x4E (78)</code></td>
   <td colspan="3" rowspan="1"><code>0x4E (78)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyO"</code></th>
   <td colspan="3" rowspan="1"><code>0x4F (79)</code></td>
   <td colspan="3" rowspan="1"><code>0x4F (79)</code></td>
   <td colspan="3" rowspan="1"><code>0x4F (79)</code></td>
   <td colspan="3" rowspan="1"><code>0x4F (79)</code></td>
   <td colspan="3" rowspan="1"><code>0x4F (79)</code></td>
   <td colspan="3" rowspan="1"><code>0x4F (79)</code></td>
   <td colspan="3" rowspan="1"><code>0x4F (79)</code></td>
   <td colspan="3" rowspan="1"><code>0x4F (79)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyP"</code></th>
   <td colspan="3" rowspan="1"><code>0x50 (80)</code></td>
   <td colspan="3" rowspan="1"><code>0x50 (80)</code></td>
   <td colspan="3" rowspan="1"><code>0x50 (80)</code></td>
   <td colspan="3" rowspan="1"><code>0x50 (80)</code></td>
   <td colspan="3" rowspan="1"><code>0x50 (80)</code></td>
   <td colspan="3" rowspan="1"><code>0x50 (80)</code></td>
   <td colspan="3" rowspan="1"><code>0x50 (80)</code></td>
   <td colspan="3" rowspan="1"><code>0x50 (80)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyQ"</code></th>
   <td colspan="3" rowspan="1"><code>0x51 (81)</code></td>
   <td colspan="3" rowspan="1"><code>0x51 (81)</code></td>
   <td rowspan="1"><code>0x51 (81)</code></td>
   <td rowspan="1"><code>0x51 (81)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0xBA (186)</code></td>
   <td><code>0x51 (81)</code></td>
   <td><code>0x51 (81)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0xBA (186)</code></td>
   <td rowspan="1"><code>0x51 (81)</code></td>
   <td rowspan="1"><code>0x51 (81)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0xBA (186)</code></td>
   <td colspan="3" rowspan="1"><code>0x51 (81)</code></td>
   <td><code>0x51 (81)</code></td>
   <td><code>0x51 (81)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0xBA (186)</code></td>
   <td colspan="3" rowspan="1"><code>0x51 (81)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyR"</code></th>
   <td colspan="3" rowspan="1"><code>0x52 (82)</code></td>
   <td colspan="3" rowspan="1"><code>0x52 (82)</code></td>
   <td colspan="3" rowspan="1"><code>0x52 (82)</code></td>
   <td colspan="3" rowspan="1"><code>0x52 (82)</code></td>
   <td colspan="3" rowspan="1"><code>0x52 (82)</code></td>
   <td colspan="3" rowspan="1"><code>0x52 (82)</code></td>
   <td colspan="3" rowspan="1"><code>0x52 (82)</code></td>
   <td colspan="3" rowspan="1"><code>0x52 (82)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyS"</code></th>
   <td colspan="3" rowspan="1"><code>0x53 (83)</code></td>
   <td colspan="3" rowspan="1"><code>0x53 (83)</code></td>
   <td colspan="3" rowspan="1"><code>0x53 (83)</code></td>
   <td colspan="3" rowspan="1"><code>0x53 (83)</code></td>
   <td colspan="3" rowspan="1"><code>0x53 (83)</code></td>
   <td colspan="3" rowspan="1"><code>0x53 (83)</code></td>
   <td colspan="3" rowspan="1"><code>0x53 (83)</code></td>
   <td colspan="3" rowspan="1"><code>0x53 (83)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyT"</code></th>
   <td colspan="3" rowspan="1"><code>0x54 (84)</code></td>
   <td colspan="3" rowspan="1"><code>0x54 (84)</code></td>
   <td colspan="3" rowspan="1"><code>0x54 (84)</code></td>
   <td colspan="3" rowspan="1"><code>0x54 (84)</code></td>
   <td colspan="3" rowspan="1"><code>0x54 (84)</code></td>
   <td colspan="3" rowspan="1"><code>0x54 (84)</code></td>
   <td colspan="3" rowspan="1"><code>0x54 (84)</code></td>
   <td colspan="3" rowspan="1"><code>0x54 (84)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyU"</code></th>
   <td colspan="3" rowspan="1"><code>0x55 (85)</code></td>
   <td colspan="3" rowspan="1"><code>0x55 (85)</code></td>
   <td colspan="3" rowspan="1"><code>0x55 (85)</code></td>
   <td colspan="3" rowspan="1"><code>0x55 (85)</code></td>
   <td colspan="3" rowspan="1"><code>0x55 (85)</code></td>
   <td colspan="3" rowspan="1"><code>0x55 (85)</code></td>
   <td colspan="3" rowspan="1"><code>0x55 (85)</code></td>
   <td colspan="3" rowspan="1"><code>0x55 (85)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyV"</code></th>
   <td colspan="3" rowspan="1"><code>0x56 (86)</code></td>
   <td colspan="3" rowspan="1"><code>0x56 (86)</code></td>
   <td colspan="3" rowspan="1"><code>0x56 (86)</code></td>
   <td colspan="3" rowspan="1"><code>0x56 (86)</code></td>
   <td colspan="3" rowspan="1"><code>0x56 (86)</code></td>
   <td colspan="3" rowspan="1"><code>0x56 (86)</code></td>
   <td colspan="3" rowspan="1"><code>0x56 (86)</code></td>
   <td colspan="3" rowspan="1"><code>0x56 (86)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyW"</code></th>
   <td colspan="3" rowspan="1"><code>0x57 (87)</code></td>
   <td colspan="3" rowspan="1"><code>0x57 (87)</code></td>
   <td colspan="3" rowspan="1"><code>0x57 (87)</code></td>
   <td colspan="3" rowspan="1"><code>0x57 (87)</code></td>
   <td colspan="3" rowspan="1"><code>0x57 (87)</code></td>
   <td colspan="3" rowspan="1"><code>0x57 (87)</code></td>
   <td colspan="3" rowspan="1"><code>0x57 (87)</code></td>
   <td colspan="3" rowspan="1"><code>0x57 (87)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyX"</code></th>
   <td colspan="3" rowspan="1"><code>0x58 (88)</code></td>
   <td colspan="3" rowspan="1"><code>0x58 (88)</code></td>
   <td colspan="3" rowspan="1"><code>0x58 (88)</code></td>
   <td colspan="3" rowspan="1"><code>0x58 (88)</code></td>
   <td colspan="3" rowspan="1"><code>0x58 (88)</code></td>
   <td colspan="3" rowspan="1"><code>0x58 (88)</code></td>
   <td colspan="3" rowspan="1"><code>0x58 (88)</code></td>
   <td colspan="3" rowspan="1"><code>0x58 (88)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyY"</code></th>
   <td colspan="3" rowspan="1"><code>0x59 (89)</code></td>
   <td colspan="3" rowspan="1"><code>0x59 (89)</code></td>
   <td colspan="3" rowspan="1"><code>0x59 (89)</code></td>
   <td colspan="3" rowspan="1"><code>0x59 (89)</code></td>
   <td colspan="3" rowspan="1"><code>0x59 (89)</code></td>
   <td colspan="3" rowspan="1"><code>0x59 (89)</code></td>
   <td colspan="3" rowspan="1"><code>0x59 (89)</code></td>
   <td colspan="3" rowspan="1"><code>0x59 (89)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"KeyZ"</code></th>
   <td colspan="3" rowspan="1"><code>0x5A (90)</code></td>
   <td colspan="3" rowspan="1"><code>0x5A (90)</code></td>
   <td colspan="3" rowspan="1"><code>0x5A (90)</code></td>
   <td colspan="3" rowspan="1"><code>0x5A (90)</code></td>
   <td colspan="3" rowspan="1"><code>0x5A (90)</code></td>
   <td colspan="3" rowspan="1"><code>0x5A (90)</code></td>
   <td colspan="3" rowspan="1"><code>0x5A (90)</code></td>
   <td colspan="3" rowspan="1"><code>0x5A (90)</code></td>
  </tr>
 </tbody>
</table>

<p>由标准位置的可打印键（US 布局中的标点符号）引起的每个浏览器的 keydown 事件的 keycode 值：</p>

<table>
 <thead>
  <tr>
   <th colspan="1" rowspan="3" scope="row">{{domxref("KeyboardEvent.code")}}</th>
   <th colspan="3" rowspan="1" scope="col">Internet Explorer 11</th>
   <th colspan="6" rowspan="1" scope="col">Google Chrome 34</th>
   <th colspan="3" rowspan="1" scope="col">Chromium 34</th>
   <th colspan="3" rowspan="1" scope="col">Safari 7</th>
   <th colspan="9" rowspan="1" scope="col">Gecko 29</th>
  </tr>
  <tr>
   <th colspan="3" rowspan="1" scope="col">Windows</th>
   <th colspan="3" rowspan="1" scope="col">Windows</th>
   <th colspan="3" rowspan="1" scope="col">Mac (10.9)</th>
   <th colspan="3" rowspan="1" scope="col">Linux (Ubuntu 14.04)</th>
   <th colspan="3" rowspan="1" scope="col">Mac (10.9)</th>
   <th colspan="3" rowspan="1" scope="col">Windows (10.9)</th>
   <th colspan="3" rowspan="1" scope="col">Mac (10.9)</th>
   <th colspan="3" rowspan="1" scope="col">Linux (Ubuntu 14.04)</th>
  </tr>
  <tr>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th colspan="1" scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
  </tr>
 </thead>
 <tfoot>
  <tr>
   <th colspan="1" rowspan="3" scope="row">{{domxref("KeyboardEvent.code")}}</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
   <th colspan="1" scope="col">US</th>
   <th scope="col">Japanese</th>
   <th scope="col">Greek</th>
  </tr>
  <tr>
   <th colspan="3" rowspan="1" scope="col">Windows</th>
   <th colspan="3" rowspan="1" scope="col">Windows</th>
   <th colspan="3" rowspan="1" scope="col">Mac (10.9)</th>
   <th colspan="3" rowspan="1" scope="col">Linux (Ubuntu 14.04)</th>
   <th colspan="3" rowspan="1" scope="col">Mac (10.9)</th>
   <th colspan="3" rowspan="1" scope="col">Windows</th>
   <th colspan="3" rowspan="1" scope="col">Mac (10.9)</th>
   <th colspan="3" rowspan="1" scope="col">Linux (Ubuntu 14.04)</th>
  </tr>
  <tr>
   <th colspan="3" rowspan="1" scope="col">Internet Explorer 11</th>
   <th colspan="6" rowspan="1" scope="col">Google Chrome 34</th>
   <th colspan="3" rowspan="1" scope="col">Chromium 34</th>
   <th colspan="3" rowspan="1" scope="col">Safari 7</th>
   <th colspan="9" rowspan="1" scope="col">Gecko 29</th>
  </tr>
 </tfoot>
 <tbody>
  <tr>
   <th scope="row"><code>"Comma"</code></th>
   <td colspan="3" rowspan="2"><code>0xBC (188)</code></td>
   <td colspan="3" rowspan="2"><code>0xBC (188)</code></td>
   <td colspan="3" rowspan="2"><code>0xBC (188)</code></td>
   <td colspan="3" rowspan="2"><code>0xBC (188)</code></td>
   <td colspan="3" rowspan="2"><code>0xBC (188)</code></td>
   <td colspan="3" rowspan="2"><code>0xBC (188)</code></td>
   <td colspan="3" rowspan="2"><code>0xBC (188)</code></td>
   <td colspan="3" rowspan="2"><code>0xBC (188)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Comma"</code> with <kbd>Shift</kbd></th>
  </tr>
  <tr>
   <th scope="row"><code>"Period"</code></th>
   <td colspan="3" rowspan="2"><code>0xBE (190)</code></td>
   <td colspan="3" rowspan="2"><code>0xBE (190)</code></td>
   <td colspan="3" rowspan="2"><code>0xBE (190)</code></td>
   <td colspan="3" rowspan="2"><code>0xBE (190)</code></td>
   <td colspan="3" rowspan="2"><code>0xBE (190)</code></td>
   <td colspan="3" rowspan="2"><code>0xBE (190)</code></td>
   <td colspan="3" rowspan="2"><code>0xBE (190)</code></td>
   <td colspan="3" rowspan="2"><code>0xBE (190)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Period"</code> with <kbd>Shift</kbd></th>
  </tr>
  <tr>
   <th scope="row"><code>"Semicolon"</code></th>
   <td colspan="1" rowspan="2"><code>0xBA (186)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xBB (187)</code></td>
   <td colspan="1" rowspan="2"><code>0xBA (186)</code></td>
   <td colspan="1" rowspan="2"><code>0xBA (186)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xBB (187)</code></td>
   <td colspan="1" rowspan="2"><code>0xBA (186)</code></td>
   <td colspan="1" rowspan="2"><code>0xBA (186)</code></td>
   <td><code>0xBA (186)</code> *1</td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xE5 (229) *2</code></td>
   <td colspan="1" rowspan="2"><code>0xBA (186)</code></td>
   <td><code>0xBA (186)</code></td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xE5 (229) *3</code></td>
   <td colspan="1" rowspan="2"><code>0xBA (186)</code></td>
   <td><code>0xBA (186)</code> *1</td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xE5 (229) *2</code></td>
   <td colspan="1" rowspan="2"><code>0x3B (59)</code></td>
   <td colspan="1" rowspan="2"><code>0x3B (59)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code></td>
   <td colspan="1" rowspan="2"><code>0x3B (59)</code></td>
   <td colspan="1" rowspan="2"><code>0x3B (59)</code> *1</td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code></td>
   <td colspan="1" rowspan="2"><code>0x3B (59)</code></td>
   <td colspan="1" rowspan="2"><code>0x3B (59)</code></td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Semicolon"</code> with <kbd>Shift</kbd></th>
   <td style="background-color: rgb(255, 204, 255);"><code>0xBB (187) </code>*1</td>
   <td style="background-color: rgb(255, 204, 255);"><code>0xBB (187)</code></td>
   <td style="background-color: rgb(255, 204, 255);"><code>0xBB (187)</code> *1</td>
  </tr>
  <tr>
   <th scope="row"><code>"Quote"</code></th>
   <td colspan="1" rowspan="2"><code>0xDE (222)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xBA (186)</code></td>
   <td colspan="1" rowspan="2"><code>0xDE (222)</code></td>
   <td colspan="1" rowspan="2"><code>0xDE (222)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xBA (186)</code></td>
   <td colspan="1" rowspan="2"><code>0xDE (222)</code></td>
   <td colspan="1" rowspan="2"><code>0xDE (222)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0xBA (186) *1</code></td>
   <td colspan="1" rowspan="2"><code>0xDE (222)</code></td>
   <td colspan="1" rowspan="2"><code>0xDE (222)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0xBA (186)</code></td>
   <td colspan="1" rowspan="2"><code>0xDE (222)</code></td>
   <td colspan="1" rowspan="2"><code>0xDE (222)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0xBA (186)</code>  *1</td>
   <td colspan="1" rowspan="2"><code>0xDE (222)</code></td>
   <td colspan="1" rowspan="2"><code>0xDE (222)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0x3A (58)</code></td>
   <td colspan="1" rowspan="2"><code>0xDE (222)</code></td>
   <td colspan="1" rowspan="2"><code>0xDE (222)</code></td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0x3A (58)</code> *1</td>
   <td colspan="1" rowspan="2"><code>0xDE (222)</code></td>
   <td colspan="1" rowspan="2"><code>0xDE (222)</code></td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0x3A (58)</code></td>
   <td colspan="1" rowspan="2"><code>0xDE (222)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Quote"</code> with Shift</th>
   <td style="background-color: rgb(255, 204, 255);"><code>0xDE (222)</code> *1</td>
   <td style="background-color: rgb(255, 204, 255);"><code>0x38 (56)</code></td>
   <td style="background-color: rgb(255, 204, 255);"><code>0xDE (222)</code> *1</td>
  </tr>
  <tr>
   <th scope="row"><code>"BracketLeft"</code></th>
   <td colspan="1" rowspan="2"><code>0xDB (219)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xC0(192)</code></td>
   <td colspan="1" rowspan="2"><code>0xDB (219)</code></td>
   <td colspan="1" rowspan="2"><code>0xDB (219)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xC0(192)</code></td>
   <td colspan="1" rowspan="2"><code>0xDB (219)</code></td>
   <td colspan="1" rowspan="2"><code>0xDB (219)</code></td>
   <td><code>0xDB (219)</code> *1</td>
   <td colspan="1" rowspan="2"><code>0xDB (219)</code></td>
   <td colspan="1" rowspan="2"><code>0xDB (219)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x32 (50)</code></td>
   <td colspan="1" rowspan="2"><code>0xDB (219)</code></td>
   <td colspan="1" rowspan="2"><code>0xDB (219)</code></td>
   <td><code>0xDB (219) *1 </code></td>
   <td colspan="1" rowspan="2"><code>0xDB (219)</code></td>
   <td colspan="1" rowspan="2"><code>0xDB (219)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0x40 (64)</code></td>
   <td colspan="1" rowspan="2"><code>0xDB (219)</code></td>
   <td colspan="1" rowspan="2"><code>0xDB (219)</code></td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0x40 (64)</code> *1</td>
   <td colspan="1" rowspan="2"><code>0xDB (219)</code></td>
   <td colspan="1" rowspan="2"><code>0xDB (219)</code></td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0x40 (64)</code></td>
   <td colspan="1" rowspan="2"><code>0xDB (219)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"BracketLeft"</code> with <kbd>Shift</kbd></th>
   <td style="background-color: rgb(255, 204, 255);"><code>0xC0 (192) *1</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0xC0 (192)</code></td>
   <td style="background-color: rgb(255, 204, 255);"><code>0xC0 (192) *1</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"BracketRight"</code></th>
   <td colspan="1" rowspan="2"><code>0xDD (221)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDB (219)</code></td>
   <td colspan="1" rowspan="2"><code>0xDD (221)</code></td>
   <td colspan="1" rowspan="2"><code>0xDD (221)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDB (219)</code></td>
   <td colspan="1" rowspan="2"><code>0xDD (221)</code></td>
   <td colspan="1" rowspan="2"><code>0xDD (221)</code></td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDB (219) *1</code></td>
   <td colspan="1" rowspan="2"><code>0xDD (221)</code></td>
   <td colspan="1" rowspan="2"><code>0xDD (221)</code></td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDB (219)</code></td>
   <td colspan="1" rowspan="2"><code>0xDD (221)</code></td>
   <td colspan="1" rowspan="2"><code>0xDD (221)</code></td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDB (219) *1</code></td>
   <td colspan="1" rowspan="2"><code>0xDD (221)</code></td>
   <td colspan="1" rowspan="2"><code>0xDD (221)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDB (219)</code></td>
   <td colspan="1" rowspan="2"><code>0xDD (221)</code></td>
   <td colspan="1" rowspan="2"><code>0xDD (221)</code></td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDB (219)</code> *1</td>
   <td colspan="1" rowspan="2"><code>0xDD (221)</code></td>
   <td colspan="1" rowspan="2"><code>0xDD (221)</code></td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDB (219)</code></td>
   <td colspan="1" rowspan="2"><code>0xDD (221)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"BracketRight"</code> with <kbd>Shift</kbd></th>
  </tr>
  <tr>
   <th scope="row"><code>"Backquote"</code></th>
   <td colspan="1" rowspan="2"><code>0xC0 (192)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(153, 153, 153);"><code>N/A</code></td>
   <td colspan="1" rowspan="2"><code>0xC0 (192)</code></td>
   <td colspan="1" rowspan="2"><code>0xC0 (192)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(153, 153, 153);"><code>N/A</code></td>
   <td colspan="1" rowspan="2"><code>0xC0 (192)</code></td>
   <td colspan="3" rowspan="2"><code>0xC0 (192)</code></td>
   <td colspan="1" rowspan="2"><code>0xC0 (192)</code></td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xF4 (244)</code></td>
   <td colspan="1" rowspan="2"><code>0xC0 (192)</code></td>
   <td colspan="3" rowspan="2"><code>0xC0 (192)</code></td>
   <td colspan="1" rowspan="2"><code>0xC0 (192)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(153, 153, 153);"><code>N/A</code></td>
   <td colspan="1" rowspan="2"><code>0xC0 (192)</code></td>
   <td colspan="3" rowspan="2"><code>0xC0 (192)</code></td>
   <td colspan="1" rowspan="2"><code>0xC0 (192)</code></td>
   <td colspan="1" rowspan="2"><code>0x00 (0)</code></td>
   <td colspan="1" rowspan="2"><code>0xC0 (192)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Backquote"</code> with <kbd>Shift</kbd></th>
  </tr>
  <tr>
   <th scope="row"><code>"Backslash"</code></th>
   <td colspan="1" rowspan="2"><code>0xDC (220)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDD (221)</code></td>
   <td colspan="1" rowspan="2"><code>0xDC (220)</code></td>
   <td colspan="1" rowspan="2"><code>0xDC (220)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDD (221)</code></td>
   <td colspan="1" rowspan="2"><code>0xDC (220)</code></td>
   <td colspan="3" rowspan="2"><code>0xDC (220)</code></td>
   <td colspan="1" rowspan="2"><code>0xDC (220)</code></td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDD (221)</code></td>
   <td colspan="1" rowspan="2"><code>0xDC (220)</code></td>
   <td colspan="3" rowspan="2"><code>0xDC (220)</code></td>
   <td colspan="1" rowspan="2"><code>0xDC (220)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDD (221)</code></td>
   <td colspan="1" rowspan="2"><code>0xDC (220)</code></td>
   <td colspan="3" rowspan="2"><code>0xDC (220)</code></td>
   <td colspan="1" rowspan="2"><code>0xDC (220)</code></td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDD (221)</code></td>
   <td colspan="1" rowspan="2"><code>0xDC (220)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Backslash"</code> with <kbd>Shift</kbd></th>
  </tr>
  <tr>
   <th scope="row"><code>"Minus"</code></th>
   <td colspan="3" rowspan="2"><code>0xBD (189)</code></td>
   <td colspan="3" rowspan="2"><code>0xBD (189)</code></td>
   <td colspan="1" rowspan="2"><code>0xBD (189)</code></td>
   <td><code>0xBD (189)</code> *1</td>
   <td colspan="1" rowspan="2"><code>0xBD (189)</code></td>
   <td colspan="1" rowspan="2"><code>0xBD (189)</code></td>
   <td><code>0xBD (189)</code></td>
   <td colspan="1" rowspan="2"><code>0xBD (189)</code></td>
   <td rowspan="1"><code>0xBD (189)</code></td>
   <td rowspan="1"><code>0xBD (189) *1</code></td>
   <td rowspan="1"><code>0xBD (189)</code></td>
   <td colspan="3" rowspan="2"><code>0xAD (173)</code></td>
   <td rowspan="2"><code>0xAD (173)</code></td>
   <td rowspan="2"><code>0xAD (173) *1</code></td>
   <td rowspan="2"><code>0xAD (173)</code></td>
   <td colspan="3" rowspan="2"><code>0xAD (173)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Minus"</code> with <kbd>Shift</kbd></th>
   <td style="background-color: rgb(255, 204, 255);"><code>0xBB (187)</code> *1</td>
   <td style="background-color: rgb(255, 204, 255);"><code>0xBB (187)</code></td>
   <td><code>0xBD (189)</code></td>
   <td style="background-color: rgb(255, 204, 255);"><code>0xBB (187) *1</code></td>
   <td><code>0xBD (189)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Equal"</code></th>
   <td colspan="1" rowspan="2"><code>0xBB (187)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDE (222)</code></td>
   <td colspan="1" rowspan="2"><code>0xBB (187)</code></td>
   <td colspan="1" rowspan="2"><code>0xBB (187)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDE (222)</code></td>
   <td colspan="1" rowspan="2"><code>0xBB (187)</code></td>
   <td colspan="1" rowspan="2"><code>0xBB (187)</code></td>
   <td><code>0xBB (187) *1</code></td>
   <td colspan="1" rowspan="2"><code>0xBB (187)</code></td>
   <td colspan="1" rowspan="2"><code>0xBB (187)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x36 (54)</code></td>
   <td colspan="1" rowspan="2"><code>0xBB (187)</code></td>
   <td rowspan="1"><code>0xBB (187)</code></td>
   <td rowspan="1"><code>0xBB (187) *1</code></td>
   <td rowspan="1"><code>0xBB (187)</code></td>
   <td colspan="1" rowspan="2"><code>0x3D (61)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xA0 (160)</code></td>
   <td colspan="1" rowspan="2"><code>0x3D (61)</code></td>
   <td colspan="1" rowspan="2"><code>0x3D (61)</code></td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xA0 (160)</code> *1</td>
   <td colspan="1" rowspan="2"><code>0x3D (61)</code></td>
   <td rowspan="2"><code>0x3D (61)</code></td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xA0 (160)</code></td>
   <td colspan="1" rowspan="2"><code>0x3D (61)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Equal"</code> with <kbd>Shift</kbd></th>
   <td style="background-color: rgb(255, 204, 255);"><code>0xC0 (192) *1</code></td>
   <td style="background-color: rgb(255, 204, 255);"><code>0xC0 (192)</code></td>
   <td><code>0xBB (187)</code></td>
   <td style="background-color: rgb(255, 204, 255);"><code>0xC0 (192) *1</code></td>
   <td><code>0xBB (187)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"IntlRo"</code></th>
   <td colspan="1" rowspan="2"><code>0xC1 (193)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xE2 (226)</code></td>
   <td colspan="1" rowspan="2"><code>0xC1 (193)</code></td>
   <td colspan="1" rowspan="2"><code>0xC1 (193)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xE2 (226)</code></td>
   <td colspan="1" rowspan="2"><code>0xC1 (193)</code></td>
   <td colspan="1" rowspan="2"><code>0xBD (189)</code></td>
   <td colspan="1" rowspan="2"><code>0xBD (189)</code></td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code></td>
   <td colspan="1" rowspan="2">*4</td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDC (220)</code><br>
     </td>
   <td colspan="1" rowspan="2">*4</td>
   <td rowspan="2"><code>0xBD (189)</code></td>
   <td rowspan="2"><code>0xBD (189)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xE5 </code>(229) *5</td>
   <td colspan="1" rowspan="2"><code>0x00 (0)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDC (220)</code></td>
   <td colspan="1" rowspan="2"><code>0x00 (0)</code></td>
   <td colspan="1" rowspan="2"><code>0xA7 (167)</code></td>
   <td colspan="1" rowspan="2"><code>0xA7 (167)</code></td>
   <td colspan="1" rowspan="2"><code>0x00 (0)</code></td>
   <td colspan="1" rowspan="2"><code>0x00 (0)</code></td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDC (220)</code></td>
   <td colspan="1" rowspan="2"><code>0x00 (0)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"IntlRo"</code> with <kbd>Shift</kbd></th>
  </tr>
  <tr>
   <th scope="row"><code>"IntlYen"</code></th>
   <td colspan="1" rowspan="2"><code>0xFF (255)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDC (220)</code></td>
   <td colspan="1" rowspan="2"><code>0xFF (255)</code></td>
   <td colspan="1" rowspan="2"><code>0xFF (255)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDC (220)</code></td>
   <td colspan="1" rowspan="2"><code>0xFF (255)</code></td>
   <td><code>0x00 (0)</code></td>
   <td><code>0x00 (0)</code></td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code></td>
   <td colspan="1" rowspan="2">*4</td>
   <td style="background-color: rgb(255, 255, 204);"><code>0xDC (220)</code></td>
   <td colspan="1" rowspan="2">*4</td>
   <td rowspan="1"><code>0x00 (0)</code></td>
   <td rowspan="1"><code>0x00 (0)</code></td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xE5 </code>(229) *5</td>
   <td colspan="1" rowspan="2"><code>0x00 (0)</code></td>
   <td colspan="1" rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDC </code>(220)</td>
   <td colspan="1" rowspan="2"><code>0x00 (0)</code></td>
   <td colspan="1" rowspan="2"><code>0xDC </code>(220)</td>
   <td colspan="1" rowspan="2"><code>0xDC </code>(220)</td>
   <td colspan="1" rowspan="2"><code>0x00 (0)</code></td>
   <td colspan="1" rowspan="2"><code>0x00 (0)</code></td>
   <td rowspan="2" style="background-color: rgb(255, 255, 204);"><code>0xDC (220)</code></td>
   <td colspan="1" rowspan="2"><code>0x00 (0)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"IntlYen"</code> with <kbd>Shift</kbd></th>
   <td><code>0xDC (220)</code></td>
   <td><code>0xDC (220)</code></td>
   <td style="background-color: rgb(255, 204, 255);"><code>0xBD (189)</code></td>
   <td><code>0xDC (220)</code></td>
   <td><code>0xDC (220)</code></td>
  </tr>
 </tbody>
</table>

<p>*1 该值是从 JIS 键盘输入的。使用 ANSI 键盘时，键代码值和输入字符是从美国键盘布局中选择的。<br>
 *2 按键是一个死键。keyup 事件的值是 0xba（186）。<br>
 *3 按键是一个死键。keyup 事件的值为 0x10（16）。<br>
 *4 没有触发任何按键事件。<br>
 *5 该键在希腊键盘布局中不可用（不输入任何字符）。keyup 事件的值为 0x00（0）。</p>

<h3 id="不可打印键（功能键）">不可打印键（功能键）</h3>

<table>
 <caption>由修改键引起的每个浏览器的 keydown 事件的 keycode 值：</caption>
 <thead>
  <tr>
   <th colspan="1" rowspan="2" scope="row">{{domxref("KeyboardEvent.code")}}</th>
   <th scope="col">Internet Explorer 11</th>
   <th colspan="2" rowspan="1" scope="col">Google Chrome 34</th>
   <th scope="col">Chromium 34</th>
   <th scope="col">Safari 7</th>
   <th colspan="3" rowspan="1" scope="col">Gecko 29</th>
  </tr>
  <tr>
   <th scope="col">Windows</th>
   <th scope="col">Windows</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Linux (Ubuntu 14.04)</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Windows</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Linux (Ubuntu 14.04)</th>
  </tr>
 </thead>
 <tfoot>
  <tr>
   <th colspan="1" rowspan="2" scope="row">{{domxref("KeyboardEvent.code")}}</th>
   <th scope="col">Windows</th>
   <th scope="col">Windows</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Linux (Ubuntu 14.04)</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Windows</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Linux (Ubuntu 14.04)</th>
  </tr>
  <tr>
   <th scope="col">Internet Explorer 11</th>
   <th colspan="2" rowspan="1" scope="col">Google Chrome 34</th>
   <th scope="col">Chromium 34</th>
   <th scope="col">Safari 7</th>
   <th colspan="3" rowspan="1" scope="col">Gecko 29</th>
  </tr>
 </tfoot>
 <tbody>
  <tr>
   <th scope="row"><code>"AltLeft"</code></th>
   <td><code>0x12 (18)</code></td>
   <td><code>0x12 (18)</code></td>
   <td><code>0x12 (18)</code></td>
   <td><code>0x12 (18)</code></td>
   <td><code>0x12 (18)</code></td>
   <td><code>0x12 (18)</code></td>
   <td><code>0x12 (18)</code></td>
   <td><code>0x12 (18)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"AltRight"</code></th>
   <td><code>0x12 (18)</code></td>
   <td><code>0x12 (18)</code></td>
   <td><code>0x12 (18)</code></td>
   <td><code>0x12 (18)</code></td>
   <td><code>0x12 (18)</code></td>
   <td><code>0x12 (18)</code></td>
   <td><code>0x12 (18)</code></td>
   <td><code>0x12 (18)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"AltRight"</code> when it's <code>"AltGraph"</code> key</th>
   <td>*1</td>
   <td>*1</td>
   <td style="background-color: rgb(153, 153, 153);">N/A</td>
   <td style="background-color: rgb(255, 255, 204);"><code>0xE1 (225)</code></td>
   <td style="background-color: rgb(153, 153, 153);">N/A</td>
   <td>*1</td>
   <td style="background-color: rgb(153, 153, 153);">N/A</td>
   <td style="background-color: rgb(255, 255, 204);"><code>0xE1 (225)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"CapsLock"</code></th>
   <td><code>0x14 (20)</code> *2</td>
   <td><code>0x14 (20)</code> *2</td>
   <td><code>0x14 (20)</code></td>
   <td><code>0x14 (20)</code></td>
   <td><code>0x14 (20)</code></td>
   <td><code>0x14 (20)</code> *2</td>
   <td><code>0x14 (20)</code></td>
   <td><code>0x14 (20)</code> *3</td>
  </tr>
  <tr>
   <th scope="row"><code>"ControlLeft"</code></th>
   <td><code>0x11 (17)</code></td>
   <td><code>0x11 (17)</code></td>
   <td><code>0x11 (17)</code></td>
   <td><code>0x11 (17)</code></td>
   <td><code>0x11 (17)</code></td>
   <td><code>0x11 (17)</code></td>
   <td><code>0x11 (17)</code></td>
   <td><code>0x11 (17)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"ControlRight"</code></th>
   <td><code>0x11 (17)</code></td>
   <td><code>0x11 (17)</code></td>
   <td><code>0x11 (17)</code></td>
   <td><code>0x11 (17)</code></td>
   <td><code>0x11 (17)</code></td>
   <td><code>0x11 (17)</code></td>
   <td><code>0x11 (17)</code></td>
   <td><code>0x11 (17)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"OSLeft"</code></th>
   <td><code>0x5B (91)</code></td>
   <td><code>0x5B (91)</code></td>
   <td><code>0x5B (91)</code></td>
   <td><code>0x5B (91)</code></td>
   <td><code>0x5B (91)</code></td>
   <td><code>0x5B (91)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0xE0 (224)</code></td>
   <td><code>0x5B (91)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"OSRight"</code></th>
   <td><code>0x5C (92)</code></td>
   <td><code>0x5C (92)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x5D (93)</code></td>
   <td><code>0x5C (92)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x5D (93)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x5B (91)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0xE0 (224)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x5B (91)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"ShiftLeft"</code></th>
   <td><code>0x10 (16)</code></td>
   <td><code>0x10 (16)</code></td>
   <td><code>0x10 (16)</code></td>
   <td><code>0x10 (16)</code></td>
   <td><code>0x10 (16)</code></td>
   <td><code>0x10 (16)</code></td>
   <td><code>0x10 (16)</code></td>
   <td><code>0x10 (16)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"ShiftRight"</code></th>
   <td><code>0x10 (16)</code></td>
   <td><code>0x10 (16)</code></td>
   <td><code>0x10 (16)</code></td>
   <td><code>0x10 (16)</code></td>
   <td><code>0x10 (16)</code></td>
   <td><code>0x10 (16)</code></td>
   <td><code>0x10 (16)</code></td>
   <td><code>0x10 (16)</code></td>
  </tr>
 </tbody>
</table>

<p>*1 在 Windows 上，“altgraph”键导致“controlLeft”键事件和“altRight”键事件。<br>
 *2 当日语键盘布局处于活动状态时，“capslock”键没有 <kbd>Shift</kbd> 会导致 0xf0（240）。该键作为“<code>Alphanumeric</code>”键工作，其标签为“英数”。<br>
 *3 当日语键盘布局处于活动状态时，“capslock”键没有 <kbd>Shift</kbd> 会导致 0x00（0）。该键作为“<code>Alphanumeric</code>”键工作，其标签为“英数”。</p>

<p>由不可打印的键引起的每个浏览器的 keydown 事件的 keycode 值：</p>

<table>
 <thead>
  <tr>
   <th colspan="1" rowspan="2" scope="row">{{domxref("KeyboardEvent.code")}}</th>
   <th scope="col">Internet Explorer 11</th>
   <th colspan="2" rowspan="1" scope="col">Google Chrome 34</th>
   <th scope="col">Chromium 34</th>
   <th scope="col">Safari 7</th>
   <th colspan="3" rowspan="1" scope="col">Gecko 29</th>
  </tr>
  <tr>
   <th scope="col">Windows</th>
   <th scope="col">Windows</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Linux (Ubuntu 14.04)</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Windows</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Linux (Ubuntu 14.04)</th>
  </tr>
 </thead>
 <tfoot>
  <tr>
   <th colspan="1" rowspan="2" scope="row">{{domxref("KeyboardEvent.code")}}</th>
   <th scope="col">Windows</th>
   <th scope="col">Windows</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Linux (Ubuntu 14.04)</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Windows</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Linux (Ubuntu 14.04)</th>
  </tr>
  <tr>
   <th scope="col">Internet Explorer 11</th>
   <th colspan="2" rowspan="1" scope="col">Google Chrome 34</th>
   <th scope="col">Chromium 34</th>
   <th scope="col">Safari 7</th>
   <th colspan="3" rowspan="1" scope="col">Gecko 29</th>
  </tr>
 </tfoot>
 <tbody>
  <tr>
   <th scope="row"><code>"ContextMenu"</code></th>
   <td><code>0x5D (93)</code></td>
   <td><code>0x5D (93)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code> *1</td>
   <td><code>0x5D (93)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code> *1</td>
   <td><code>0x5D (93)</code></td>
   <td><code>0x5D (93)</code></td>
   <td><code>0x5D (93)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Enter"</code></th>
   <td><code>0x0D (13)</code></td>
   <td><code>0x0D (13)</code></td>
   <td><code>0x0D (13)</code></td>
   <td><code>0x0D (13)</code></td>
   <td><code>0x0D (13)</code></td>
   <td><code>0x0D (13)</code></td>
   <td><code>0x0D (13)</code></td>
   <td><code>0x0D (13)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Space"</code></th>
   <td><code>0x20 (32)</code></td>
   <td><code>0x20 (32)</code></td>
   <td><code>0x20 (32)</code></td>
   <td><code>0x20 (32)</code></td>
   <td><code>0x20 (32)</code></td>
   <td><code>0x20 (32)</code></td>
   <td><code>0x20 (32)</code></td>
   <td><code>0x20 (32)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Tab"</code></th>
   <td><code>0x09 (9)</code></td>
   <td><code>0x09 (9)</code></td>
   <td><code>0x09 (9)</code></td>
   <td><code>0x09 (9)</code></td>
   <td><code>0x09 (9)</code></td>
   <td><code>0x09 (9)</code></td>
   <td><code>0x09 (9)</code></td>
   <td><code>0x09 (9)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Delete"</code></th>
   <td><code>0x2E (46)</code></td>
   <td><code>0x2E (46)</code></td>
   <td><code>0x2E (46)</code></td>
   <td><code>0x2E (46)</code></td>
   <td><code>0x2E (46)</code></td>
   <td><code>0x2E (46)</code></td>
   <td><code>0x2E (46)</code></td>
   <td><code>0x2E (46)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"End"</code></th>
   <td><code>0x23 (35)</code></td>
   <td><code>0x23 (35)</code></td>
   <td><code>0x23 (35)</code></td>
   <td><code>0x23 (35)</code></td>
   <td><code>0x23 (35)</code></td>
   <td><code>0x23 (35)</code></td>
   <td><code>0x23 (35)</code></td>
   <td><code>0x23 (35)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Help"</code></th>
   <td style="background-color: rgb(153, 153, 153);">N/A</td>
   <td style="background-color: rgb(153, 153, 153);">N/A</td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x2D (45)</code> *2</td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x2F (47)</code> *3</td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x2D (45)</code> *2</td>
   <td style="background-color: rgb(153, 153, 153);">N/A</td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x2D (45)</code> *2</td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x06 (6)</code> *3</td>
  </tr>
  <tr>
   <th scope="row"><code>"Home"</code></th>
   <td><code>0x24 (36)</code></td>
   <td><code>0x24 (36)</code></td>
   <td><code>0x24 (36)</code></td>
   <td><code>0x24 (36)</code></td>
   <td><code>0x24 (36)</code></td>
   <td><code>0x24 (36)</code></td>
   <td><code>0x24 (36)</code></td>
   <td><code>0x24 (36)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Insert"</code></th>
   <td><code>0x2D (45)</code></td>
   <td><code>0x2D (45)</code></td>
   <td><code>0x2D (45)</code></td>
   <td><code>0x2D (45)</code></td>
   <td><code>0x2D (45)</code></td>
   <td><code>0x2D (45)</code></td>
   <td><code>0x2D (45)</code></td>
   <td><code>0x2D (45)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"PageDown"</code></th>
   <td><code>0x22 (34)</code></td>
   <td><code>0x22 (34)</code></td>
   <td><code>0x22 (34)</code></td>
   <td><code>0x22 (34)</code></td>
   <td><code>0x22 (34)</code></td>
   <td><code>0x22 (34)</code></td>
   <td><code>0x22 (34)</code></td>
   <td><code>0x22 (34)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"PageUp"</code></th>
   <td><code>0x21 (33)</code></td>
   <td><code>0x21 (33)</code></td>
   <td><code>0x21 (33)</code></td>
   <td><code>0x21 (33)</code></td>
   <td><code>0x21 (33)</code></td>
   <td><code>0x21 (33)</code></td>
   <td><code>0x21 (33)</code></td>
   <td><code>0x21 (33)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"ArrowDown"</code></th>
   <td><code>0x28 (40)</code></td>
   <td><code>0x28 (40)</code></td>
   <td><code>0x28 (40)</code></td>
   <td><code>0x28 (40)</code></td>
   <td><code>0x28 (40)</code></td>
   <td><code>0x28 (40)</code></td>
   <td><code>0x28 (40)</code></td>
   <td><code>0x28 (40)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"ArrowLeft"</code></th>
   <td><code>0x25 (37)</code></td>
   <td><code>0x25 (37)</code></td>
   <td><code>0x25 (37)</code></td>
   <td><code>0x25 (37)</code></td>
   <td><code>0x25 (37)</code></td>
   <td><code>0x25 (37)</code></td>
   <td><code>0x25 (37)</code></td>
   <td><code>0x25 (37)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"ArrowRight"</code></th>
   <td><code>0x27 (39)</code></td>
   <td><code>0x27 (39)</code></td>
   <td><code>0x27 (39)</code></td>
   <td><code>0x27 (39)</code></td>
   <td><code>0x27 (39)</code></td>
   <td><code>0x27 (39)</code></td>
   <td><code>0x27 (39)</code></td>
   <td><code>0x27 (39)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"ArrowUp"</code></th>
   <td><code>0x26 (38)</code></td>
   <td><code>0x26 (38)</code></td>
   <td><code>0x26 (38)</code></td>
   <td><code>0x26 (38)</code></td>
   <td><code>0x26 (38)</code></td>
   <td><code>0x26 (38)</code></td>
   <td><code>0x26 (38)</code></td>
   <td><code>0x26 (38)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Escape"</code></th>
   <td><code>0x1B (27)</code></td>
   <td><code>0x1B (27)</code></td>
   <td><code>0x1B (27)</code></td>
   <td><code>0x1B (27)</code></td>
   <td><code>0x1B (27)</code></td>
   <td><code>0x1B (27)</code></td>
   <td><code>0x1B (27)</code></td>
   <td><code>0x1B (27)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"PrintScreen"</code></th>
   <td><code>0x2C (44)</code> *4</td>
   <td><code>0x2C (44)</code> *4</td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x7C (124)</code>*5</td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x2A (42)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x7C (124)</code>*5</td>
   <td><code>0x2C (44)</code> *4</td>
   <td><code>0x2C (44)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x2A (42)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"ScrollLock"</code></th>
   <td><code>0x91 (145)</code></td>
   <td><code>0x91 (145)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x7D (125)</code>*5</td>
   <td><code>0x91 (145)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x7D (125)</code>*5</td>
   <td><code>0x91 (145)</code></td>
   <td><code>0x91 (145)</code></td>
   <td><code>0x91 (145)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Pause"</code></th>
   <td><code>0x13 (19)</code> *6</td>
   <td><code>0x13 (19)</code> *6</td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x7E (126)</code>*5</td>
   <td><code>0x13 (19)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x7E (126)</code>*5</td>
   <td><code>0x13 (19)</code> *6</td>
   <td><code>0x13 (19)</code></td>
   <td><code>0x13 (19)</code></td>
  </tr>
 </tbody>
</table>

<p>*1 keypress 事件被激发，其 keypcode 和 charcode 为 0x10（16），但文本未输入编辑器。<br>
 *2 在 Mac 电脑上，“<code>Help</code>”键映射到电脑键盘的“<code>Insert</code>”键。这些<code>keyCode</code> 值与“<code>Insert</code>”键的<code>keyCode</code>值相同。<br>
 *3 在 Fedora 20 上测试。<br>
 *4 仅激发 keyup 事件。<br>
 *5 PC 的“<code>PrintScreen</code>”、“<code>ScrollLock</code>”和“<code>Pause</code>”映射到 Mac 的“F13”、“F14”和“F15”。Chrome 和 Safari 用 Mac 的键映射相同的<code>keyCode</code>值。<br>
 *6“<code>Pause</code>”键加上 <kbd>Control</kbd> 导致 0x03（3）。</p>

<p>由功能键引起的每个浏览器的 keydown 事件的 keycode 值：</p>

<table>
 <thead>
  <tr>
   <th colspan="1" rowspan="2" scope="row">{{domxref("KeyboardEvent.code")}}</th>
   <th scope="col">Internet Explorer 11</th>
   <th colspan="2" rowspan="1" scope="col">Google Chrome 34</th>
   <th scope="col">Chromium 34</th>
   <th scope="col">Safari 7</th>
   <th colspan="3" rowspan="1" scope="col">Gecko 29</th>
  </tr>
  <tr>
   <th scope="col">Windows</th>
   <th scope="col">Windows</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Linux (Ubuntu 14.04)</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Windows</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Linux (Ubuntu 14.04)</th>
  </tr>
 </thead>
 <tfoot>
  <tr>
   <th colspan="1" rowspan="2" scope="row">{{domxref("KeyboardEvent.code")}}</th>
   <th scope="col">Windows</th>
   <th scope="col">Windows</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Linux (Ubuntu 14.04)</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Windows</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Linux (Ubuntu 14.04)</th>
  </tr>
  <tr>
   <th scope="col">Internet Explorer 11</th>
   <th colspan="2" rowspan="1" scope="col">Google Chrome 34</th>
   <th scope="col">Chromium 34</th>
   <th scope="col">Safari 7</th>
   <th colspan="3" rowspan="1" scope="col">Gecko 29</th>
  </tr>
 </tfoot>
 <tbody>
  <tr>
   <th scope="row"><code>"F1"</code></th>
   <td><code>0x70 (112)</code></td>
   <td><code>0x70 (112)</code></td>
   <td><code>0x70 (112)</code></td>
   <td><code>0x70 (112)</code></td>
   <td><code>0x70 (112)</code></td>
   <td><code>0x70 (112)</code></td>
   <td><code>0x70 (112)</code></td>
   <td><code>0x70 (112)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"F2"</code></th>
   <td><code>0x71 (113)</code></td>
   <td><code>0x71 (113)</code></td>
   <td><code>0x71 (113)</code></td>
   <td><code>0x71 (113)</code></td>
   <td><code>0x71 (113)</code></td>
   <td><code>0x71 (113)</code></td>
   <td><code>0x71 (113)</code></td>
   <td><code>0x71 (113)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"F3"</code></th>
   <td><code>0x72 (114)</code></td>
   <td><code>0x72 (114)</code></td>
   <td><code>0x72 (114)</code></td>
   <td><code>0x72 (114)</code></td>
   <td><code>0x72 (114)</code></td>
   <td><code>0x72 (114)</code></td>
   <td><code>0x72 (114)</code></td>
   <td><code>0x72 (114)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"F4"</code></th>
   <td><code>0x73 (115)</code></td>
   <td><code>0x73 (115)</code></td>
   <td><code>0x73 (115)</code></td>
   <td><code>0x73 (115)</code></td>
   <td><code>0x73 (115)</code></td>
   <td><code>0x73 (115)</code></td>
   <td><code>0x73 (115)</code></td>
   <td><code>0x73 (115)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"F5"</code></th>
   <td><code>0x74 (116)</code></td>
   <td><code>0x74 (116)</code></td>
   <td><code>0x74 (116)</code></td>
   <td><code>0x74 (116)</code></td>
   <td><code>0x74 (116)</code></td>
   <td><code>0x74 (116)</code></td>
   <td><code>0x74 (116)</code></td>
   <td><code>0x74 (116)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"F6"</code></th>
   <td><code>0x75 (117)</code></td>
   <td><code>0x75 (117)</code></td>
   <td><code>0x75 (117)</code></td>
   <td><code>0x75 (117)</code></td>
   <td><code>0x75 (117)</code></td>
   <td><code>0x75 (117)</code></td>
   <td><code>0x75 (117)</code></td>
   <td><code>0x75 (117)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"F7"</code></th>
   <td><code>0x76 (118)</code></td>
   <td><code>0x76 (118)</code></td>
   <td><code>0x76 (118)</code></td>
   <td><code>0x76 (118)</code></td>
   <td><code>0x76 (118)</code></td>
   <td><code>0x76 (118)</code></td>
   <td><code>0x76 (118)</code></td>
   <td><code>0x76 (118)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"F8"</code></th>
   <td><code>0x77 (119)</code></td>
   <td><code>0x77 (119)</code></td>
   <td><code>0x77 (119)</code></td>
   <td><code>0x77 (119)</code></td>
   <td><code>0x77 (119)</code></td>
   <td><code>0x77 (119)</code></td>
   <td><code>0x77 (119)</code></td>
   <td><code>0x77 (119)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"F9"</code></th>
   <td><code>0x78 (120)</code></td>
   <td><code>0x78 (120)</code></td>
   <td><code>0x78 (120)</code></td>
   <td><code>0x78 (120)</code></td>
   <td><code>0x78 (120)</code></td>
   <td><code>0x78 (120)</code></td>
   <td><code>0x78 (120)</code></td>
   <td><code>0x78 (120)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"F10"</code></th>
   <td><code>0x79 (121)</code></td>
   <td><code>0x79 (121)</code></td>
   <td><code>0x79 (121)</code></td>
   <td><code>0x79 (121)</code></td>
   <td><code>0x79 (121)</code></td>
   <td><code>0x79 (121)</code></td>
   <td><code>0x79 (121)</code></td>
   <td><code>0x79 (121)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"F11"</code></th>
   <td><code>0x7A (122)</code></td>
   <td><code>0x7A (122)</code></td>
   <td><code>0x7A (122)</code></td>
   <td><code>0x7A (122)</code></td>
   <td><code>0x7A (122)</code></td>
   <td><code>0x7A (122)</code></td>
   <td><code>0x7A (122)</code></td>
   <td><code>0x7A (122)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"F12"</code></th>
   <td><code>0x7B (123)</code></td>
   <td><code>0x7B (123)</code></td>
   <td><code>0x7B (123)</code></td>
   <td><code>0x7B (123)</code></td>
   <td><code>0x7B (123)</code></td>
   <td><code>0x7B (123)</code></td>
   <td><code>0x7B (123)</code></td>
   <td><code>0x7B (123)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"F13"</code></th>
   <td><code>0x7C (124)</code></td>
   <td><code>0x7C (124)</code></td>
   <td><code>0x7C (124)</code></td>
   <td><code>0x7C (124)</code> *1</td>
   <td><code>0x7C (124)</code></td>
   <td><code>0x7C (124)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x2C (44)</code> *2</td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code> *3</td>
  </tr>
  <tr>
   <th scope="row"><code>"F14"</code></th>
   <td><code>0x7D (125)</code></td>
   <td><code>0x7D (125)</code></td>
   <td><code>0x7D (125)</code></td>
   <td><code>0x7D (125)</code> *1</td>
   <td><code>0x7D (125)</code></td>
   <td><code>0x7D (125)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x91 (145)</code> *2</td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code> *3</td>
  </tr>
  <tr>
   <th scope="row"><code>"F15"</code></th>
   <td><code>0x7E (126)</code></td>
   <td><code>0x7E (126)</code></td>
   <td><code>0x7E (126)</code></td>
   <td><code>0x7E (126)</code> *1</td>
   <td><code>0x7E (126)</code></td>
   <td><code>0x7E (126)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x13 (19)</code> *2</td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code> *3</td>
  </tr>
  <tr>
   <th scope="row"><code>"F16"</code></th>
   <td><code>0x7F (127)</code></td>
   <td><code>0x7F (127)</code></td>
   <td><code>0x7F (127)</code></td>
   <td><code>0x7F (127)</code> *1</td>
   <td><code>0x7F (127)</code></td>
   <td><code>0x7F (127)</code></td>
   <td><code>0x7F (127)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code> *3</td>
  </tr>
  <tr>
   <th scope="row"><code>"F17"</code></th>
   <td><code>0x80 (128)</code></td>
   <td><code>0x80 (128)</code></td>
   <td><code>0x80 (128)</code></td>
   <td><code>0x80 (128)</code> *1</td>
   <td><code>0x80 (128)</code></td>
   <td><code>0x80 (128)</code></td>
   <td><code>0x80 (128)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code> *3</td>
  </tr>
  <tr>
   <th scope="row"><code>"F18"</code></th>
   <td><code>0x81 (129)</code></td>
   <td><code>0x81 (129)</code></td>
   <td><code>0x81 (129)</code></td>
   <td><code>0x81 (129)</code> *1</td>
   <td><code>0x81 (129)</code></td>
   <td><code>0x81 (129)</code></td>
   <td><code>0x81 (129)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code> *3</td>
  </tr>
  <tr>
   <th scope="row"><code>"F19"</code></th>
   <td><code>0x82 (130)</code></td>
   <td><code>0x82 (130)</code></td>
   <td><code>0x82 (130)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>N/A</code> *4</td>
   <td><code>0x82 (130)</code></td>
   <td><code>0x82 (130)</code></td>
   <td><code>0x82 (130)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code> *3</td>
  </tr>
  <tr>
   <th scope="row"><code>"F20"</code></th>
   <td><code>0x83 (131)</code></td>
   <td><code>0x83 (131)</code></td>
   <td><code>0x83 (131)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>N/A</code> *4</td>
   <td style="background-color: rgb(255, 255, 204);"><code>0xE5 (229)</code> *5</td>
   <td><code>0x83 (131)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>N/A</code> *6</td>
  </tr>
  <tr>
   <th scope="row"><code>"F21"</code></th>
   <td><code>0x84 (132)</code></td>
   <td><code>0x84 (132)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code> *7</td>
   <td style="background-color: rgb(255, 255, 204);"><code>N/A</code> *4</td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code> *7</td>
   <td><code>0x84 (132)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>N/A</code> *8</td>
   <td style="background-color: rgb(255, 255, 204);"><code>N/A</code> *6</td>
  </tr>
  <tr>
   <th scope="row"><code>"F22"</code></th>
   <td><code>0x85 (133)</code></td>
   <td><code>0x85 (133)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code> *7</td>
   <td style="background-color: rgb(255, 255, 204);"><code>N/A</code> *4</td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code> *7</td>
   <td><code>0x85 (133)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>N/A</code> *8</td>
   <td style="background-color: rgb(255, 255, 204);"><code>N/A</code> *6</td>
  </tr>
  <tr>
   <th scope="row"><code>"F23"</code></th>
   <td><code>0x86 (134)</code></td>
   <td><code>0x86 (134)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code> *7</td>
   <td style="background-color: rgb(255, 255, 204);"><code>N/A</code> *4</td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code> *7</td>
   <td><code>0x86 (134)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>N/A</code> *8</td>
   <td style="background-color: rgb(255, 255, 204);"><code>N/A</code> *6</td>
  </tr>
  <tr>
   <th scope="row"><code>"F24"</code></th>
   <td><code>0x87 (135)</code></td>
   <td><code>0x87 (135)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code> *7</td>
   <td style="background-color: rgb(255, 255, 204);"><code>N/A</code> *4</td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code> *7</td>
   <td><code>0x87 (135)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>N/A</code> *8</td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x00 (0)</code> *3</td>
  </tr>
 </tbody>
</table>

<p>*1 在 Fedora 20 上测试。<br>
 *2 PC 的“<code>PrintScreen</code>”、“<code>ScrollLock</code>”和“<code>Pause</code>”映射到 Mac 的“F13”、“F14”和“F15”。火狐映射到和 PC 相同的<code>keyCode</code>。<br>
 *3 在 Fedora 20 上测试。这些键不会导致<code>GDK_Fxx</code> 按键符号。如果键产生正确的按键符号，这些值必须与 IE 相同。<br>
 *4 在 Fedora 20 上测试。这些键不会在 chromium 上引起 dom 键事件。<br>
 *5 keyup 事件的 keycode 值为 0x83（131）。<br>
 *6 在 Fedora 20 上测试。这些密钥不会在 Firefox 上引起 DOM 密钥事件。<br>
 *7 仅激发 keydown 事件。<br>
 *8 在 Firefox 上不会触发任何 DOM 密钥事件。</p>

<h3 id="小键盘键">小键盘键</h3>

<p>由 numPad 中的键在 numLock 状态下导致的每个浏览器的 keyDown 事件的 keycode 值</p>

<table>
 <thead>
  <tr>
   <th colspan="1" rowspan="2" scope="row">{{domxref("KeyboardEvent.code")}}</th>
   <th scope="col">Internet Explorer 11</th>
   <th colspan="2" rowspan="1" scope="col">Google Chrome 34</th>
   <th scope="col">Chromium 34</th>
   <th scope="col">Safari 7</th>
   <th colspan="3" rowspan="1" scope="col">Gecko 29</th>
  </tr>
  <tr>
   <th scope="col">Windows</th>
   <th scope="col">Windows</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Linux (Ubuntu 14.04)</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Windows</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Linux (Ubuntu 14.04)</th>
  </tr>
 </thead>
 <tfoot>
  <tr>
   <th colspan="1" rowspan="2" scope="row">{{domxref("KeyboardEvent.code")}}</th>
   <th scope="col">Windows</th>
   <th scope="col">Windows</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Linux (Ubuntu 14.04)</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Windows</th>
   <th scope="col">Mac (10.9)</th>
   <th scope="col">Linux (Ubuntu 14.04)</th>
  </tr>
  <tr>
   <th scope="col">Internet Explorer 11</th>
   <th colspan="2" rowspan="1" scope="col">Google Chrome 34</th>
   <th scope="col">Chromium 34</th>
   <th scope="col">Safari 7</th>
   <th colspan="3" rowspan="1" scope="col">Gecko 29</th>
  </tr>
 </tfoot>
 <tbody>
  <tr>
   <th scope="row"><code>"NumLock"</code></th>
   <td><code>0x90 (144)</code></td>
   <td><code>0x90 (144)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x0C (12)</code> *1</td>
   <td><code>0x90 (144)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x0C (12)</code> *1</td>
   <td><code>0x90 (144)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x0C (12)</code> *1</td>
   <td><code>0x90 (144)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Numpad0"</code></th>
   <td><code>0x60 (96)</code></td>
   <td><code>0x60 (96)</code></td>
   <td><code>0x60 (96)</code></td>
   <td><code>0x60 (96)</code></td>
   <td><code>0x60 (96)</code></td>
   <td><code>0x60 (96)</code></td>
   <td><code>0x60 (96)</code></td>
   <td><code>0x60 (96)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Numpad1"</code></th>
   <td><code>0x61 (97)</code></td>
   <td><code>0x61 (97)</code></td>
   <td><code>0x61 (97)</code></td>
   <td><code>0x61 (97)</code></td>
   <td><code>0x61 (97)</code></td>
   <td><code>0x61 (97)</code></td>
   <td><code>0x61 (97)</code></td>
   <td><code>0x61 (97)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Numpad2"</code></th>
   <td><code>0x62 (98)</code></td>
   <td><code>0x62 (98)</code></td>
   <td><code>0x62 (98)</code></td>
   <td><code>0x62 (98)</code></td>
   <td><code>0x62 (98)</code></td>
   <td><code>0x62 (98)</code></td>
   <td><code>0x62 (98)</code></td>
   <td><code>0x62 (98)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Numpad3"</code></th>
   <td><code>0x63 (99)</code></td>
   <td><code>0x63 (99)</code></td>
   <td><code>0x63 (99)</code></td>
   <td><code>0x63 (99)</code></td>
   <td><code>0x63 (99)</code></td>
   <td><code>0x63 (99)</code></td>
   <td><code>0x63 (99)</code></td>
   <td><code>0x63 (99)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Numpad4"</code></th>
   <td><code>0x64 (100)</code></td>
   <td><code>0x64 (100)</code></td>
   <td><code>0x64 (100)</code></td>
   <td><code>0x64 (100)</code></td>
   <td><code>0x64 (100)</code></td>
   <td><code>0x64 (100)</code></td>
   <td><code>0x64 (100)</code></td>
   <td><code>0x64 (100)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Numpad5"</code></th>
   <td><code>0x65 (101)</code></td>
   <td><code>0x65 (101)</code></td>
   <td><code>0x65 (101)</code></td>
   <td><code>0x65 (101)</code></td>
   <td><code>0x65 (101)</code></td>
   <td><code>0x65 (101)</code></td>
   <td><code>0x65 (101)</code></td>
   <td><code>0x65 (101)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Numpad6"</code></th>
   <td><code>0x66 (102)</code></td>
   <td><code>0x66 (102)</code></td>
   <td><code>0x66 (102)</code></td>
   <td><code>0x66 (102)</code></td>
   <td><code>0x66 (102)</code></td>
   <td><code>0x66 (102)</code></td>
   <td><code>0x66 (102)</code></td>
   <td><code>0x66 (102)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Numpad7"</code></th>
   <td><code>0x67 (103)</code></td>
   <td><code>0x67 (103)</code></td>
   <td><code>0x67 (103)</code></td>
   <td><code>0x67 (103)</code></td>
   <td><code>0x67 (103)</code></td>
   <td><code>0x67 (103)</code></td>
   <td><code>0x67 (103)</code></td>
   <td><code>0x67 (103)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Numpad8"</code></th>
   <td><code>0x68 (104)</code></td>
   <td><code>0x68 (104)</code></td>
   <td><code>0x68 (104)</code></td>
   <td><code>0x68 (104)</code></td>
   <td><code>0x68 (104)</code></td>
   <td><code>0x68 (104)</code></td>
   <td><code>0x68 (104)</code></td>
   <td><code>0x68 (104)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Numpad9"</code></th>
   <td><code>0x69 (105)</code></td>
   <td><code>0x69 (105)</code></td>
   <td><code>0x69 (105)</code></td>
   <td><code>0x69 (105)</code></td>
   <td><code>0x69 (105)</code></td>
   <td><code>0x69 (105)</code></td>
   <td><code>0x69 (105)</code></td>
   <td><code>0x69 (105)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"NumpadAdd"</code></th>
   <td><code>0x6B (107)</code></td>
   <td><code>0x6B (107)</code></td>
   <td><code>0x6B (107)</code></td>
   <td><code>0x6B (107)</code></td>
   <td><code>0x6B (107)</code></td>
   <td><code>0x6B (107)</code></td>
   <td><code>0x6B (107)</code></td>
   <td><code>0x6B (107)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"NumpadComma"</code> inputting <code>","</code></th>
   <td><code>0xC2 (194)</code></td>
   <td><code>0xC2 (194)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0xBC (188)</code></td>
   <td style="background-color: rgb(153, 153, 153);"><code>Always inputs </code>"."</td>
   <td style="background-color: rgb(255, 255, 204);"><code>0xBC (188)</code></td>
   <td><code>0xC2 (194)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x6C (108)</code></td>
   <td style="background-color: rgb(153, 153, 153);"><code>Always inputs </code>"."</td>
  </tr>
  <tr>
   <th scope="row"><code>"NumpadComma"</code> inputting <code>"."</code> or empty string</th>
   <td><code>0xC2 (194)</code></td>
   <td><code>0xC2 (194)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0xBE (190)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x6E (110)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0xBE (190)</code></td>
   <td><code>0xC2 (194)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x6C (108)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x6E (110)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"NumpadDecimal"</code> inputting <code>"."</code></th>
   <td><code>0x6E (110)</code></td>
   <td><code>0x6E (110)</code></td>
   <td><code>0x6E (110)</code></td>
   <td><code>0x6E (110)</code></td>
   <td><code>0x6E (110)</code></td>
   <td><code>0x6E (110)</code></td>
   <td><code>0x6E (110)</code></td>
   <td><code>0x6E (110)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"NumpadDecimal"</code> inputting <code>","</code></th>
   <td><code>0x6E (110)</code></td>
   <td><code>0x6E (110)</code></td>
   <td><code>0x6E (110)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x6C (108)</code></td>
   <td><code>0x6E (110)</code></td>
   <td><code>0x6E (110)</code></td>
   <td><code>0x6E (110)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x6C (108)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"NumpadDivide"</code></th>
   <td><code>0x6F (111)</code></td>
   <td><code>0x6F (111)</code></td>
   <td><code>0x6F (111)</code></td>
   <td><code>0x6F (111)</code></td>
   <td><code>0x6F (111)</code></td>
   <td><code>0x6F (111)</code></td>
   <td><code>0x6F (111)</code></td>
   <td><code>0x6F (111)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"NumpadEnter"</code></th>
   <td><code>0x0D (13)</code></td>
   <td><code>0x0D (13)</code></td>
   <td><code>0x0D (13)</code></td>
   <td><code>0x0D (13)</code></td>
   <td><code>0x0D (13)</code></td>
   <td><code>0x0D (13)</code></td>
   <td><code>0x0D (13)</code></td>
   <td><code>0x0D (13)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"NumpadEqual"</code></th>
   <td><code>0x0C (12)</code></td>
   <td><code>0x0C (12)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0xBB (187)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0xBB (187)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0xBB (187)</code></td>
   <td><code>0x0C (12)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x3D (61)</code></td>
   <td style="background-color: rgb(255, 255, 204);"><code>0x3D (61)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"NumpadMultiply"</code></th>
   <td><code>0x6A (106)</code></td>
   <td><code>0x6A (106)</code></td>
   <td><code>0x6A (106)</code></td>
   <td><code>0x6A (106)</code></td>
   <td><code>0x6A (106)</code></td>
   <td><code>0x6A (106)</code></td>
   <td><code>0x6A (106)</code></td>
   <td><code>0x6A (106)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"NumpadSubtract"</code></th>
   <td><code>0x6D (109)</code></td>
   <td><code>0x6D (109)</code></td>
   <td><code>0x6D (109)</code></td>
   <td><code>0x6D (109)</code></td>
   <td><code>0x6D (109)</code></td>
   <td><code>0x6D (109)</code></td>
   <td><code>0x6D (109)</code></td>
   <td><code>0x6D (109)</code></td>
  </tr>
 </tbody>
</table>

<p>*1“numlock”键在 Mac 上作为“clear”键工作。</p>

<p>由处于无 numlock 状态的 numpad 中的键引起的每个浏览器的 keydown 事件的 keycode 值</p>

<table>
 <thead>
  <tr>
   <th colspan="1" rowspan="2" scope="row">{{domxref("KeyboardEvent.code")}}</th>
   <th scope="col">Internet Explorer 11</th>
   <th colspan="1" rowspan="1" scope="col">Google Chrome 34</th>
   <th scope="col">Chromium 34</th>
   <th colspan="2" rowspan="1" scope="col">Gecko 29</th>
  </tr>
  <tr>
   <th scope="col">Windows</th>
   <th scope="col">Windows</th>
   <th scope="col">Linux (Ubuntu 14.04)</th>
   <th scope="col">Windows</th>
   <th scope="col">Linux (Ubuntu 14.04)</th>
  </tr>
 </thead>
 <tfoot>
  <tr>
   <th colspan="1" rowspan="2" scope="row">{{domxref("KeyboardEvent.code")}}</th>
   <th scope="col">Windows</th>
   <th scope="col">Windows</th>
   <th scope="col">Linux (Ubuntu 14.04)</th>
   <th scope="col">Windows</th>
   <th scope="col">Linux (Ubuntu 14.04)</th>
  </tr>
  <tr>
   <th scope="col">Internet Explorer 11</th>
   <th colspan="1" rowspan="1" scope="col">Google Chrome 34</th>
   <th scope="col">Chromium 34</th>
   <th colspan="2" rowspan="1" scope="col">Gecko 29</th>
  </tr>
 </tfoot>
 <tbody>
  <tr>
   <th scope="row"><code>"Numpad0"</code> (<code>"Insert"</code>)</th>
   <td><code>0x2D (45)</code></td>
   <td><code>0x2D (45)</code></td>
   <td><code>0x2D (45)</code></td>
   <td><code>0x2D (45)</code></td>
   <td><code>0x2D (45)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Numpad1"</code> (<code>"End"</code>)</th>
   <td><code>0x23 (35)</code></td>
   <td><code>0x23 (35)</code></td>
   <td><code>0x23 (35)</code></td>
   <td><code>0x23 (35)</code></td>
   <td><code>0x23 (35)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Numpad2"</code> (<code>"ArrowDown"</code>)</th>
   <td><code>0x28 (40)</code></td>
   <td><code>0x28 (40)</code></td>
   <td><code>0x28 (40)</code></td>
   <td><code>0x28 (40)</code></td>
   <td><code>0x28 (40)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Numpad3"</code> (<code>"PageDown"</code>)</th>
   <td><code>0x22 (34)</code></td>
   <td><code>0x22 (34)</code></td>
   <td><code>0x22 (34)</code></td>
   <td><code>0x22 (34)</code></td>
   <td><code>0x22 (34)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Numpad4"</code> (<code>"ArrowLeft"</code>)</th>
   <td><code>0x25 (37)</code></td>
   <td><code>0x25 (37)</code></td>
   <td><code>0x25 (37)</code></td>
   <td><code>0x25 (37)</code></td>
   <td><code>0x25 (37)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Numpad5"</code></th>
   <td><code>0x0C (12)</code></td>
   <td><code>0x0C (12)</code></td>
   <td><code>0x0C (12)</code></td>
   <td><code>0x0C (12)</code></td>
   <td><code>0x0C (12)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Numpad6"</code> (<code>"ArrowRight"</code>)</th>
   <td><code>0x27 (39)</code></td>
   <td><code>0x27 (39)</code></td>
   <td><code>0x27 (39)</code></td>
   <td><code>0x27 (39)</code></td>
   <td><code>0x27 (39)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Numpad7"</code> (<code>"Home"</code>)</th>
   <td><code>0x24 (36)</code></td>
   <td><code>0x24 (36)</code></td>
   <td><code>0x24 (36)</code></td>
   <td><code>0x24 (36)</code></td>
   <td><code>0x24 (36)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Numpad8"</code> (<code>"ArrowUp"</code>)</th>
   <td><code>0x26 (38)</code></td>
   <td><code>0x26 (38)</code></td>
   <td><code>0x26 (38)</code></td>
   <td><code>0x26 (38)</code></td>
   <td><code>0x26 (38)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"Numpad9"</code> (<code>"PageUp"</code>)</th>
   <td><code>0x21 (33)</code></td>
   <td><code>0x21 (33)</code></td>
   <td><code>0x21 (33)</code></td>
   <td><code>0x21 (33)</code></td>
   <td><code>0x21 (33)</code></td>
  </tr>
  <tr>
   <th scope="row"><code>"NumpadDecimal"</code> (<code>"Delete"</code>)</th>
   <td><code>0x2E (46)</code></td>
   <td><code>0x2E (46)</code></td>
   <td><code>0x2E (46)</code></td>
   <td><code>0x2E (46)</code></td>
   <td><code>0x2E (46)</code></td>
  </tr>
 </tbody>
</table>

<p>*最近的 Mac 没有“numlock”键和状态。因此，未锁定状态不可用。</p>

<h2 id="常数值的键码">常数值的键码</h2>

<p>gecko 在 keyboardvent 中定义了许多 keycode 值，用于显式地生成映射表。这些值对 Firefox 的附加开发人员很有用，但在公共网页中却没有那么有用。</p>

<table style="width: auto;">
 <tbody>
  <tr>
   <td class="header">常数</td>
   <td class="header">值</td>
   <td class="header">描述</td>
  </tr>
  <tr>
   <td><code>DOM_VK_CANCEL</code></td>
   <td style="white-space: nowrap;">0x03 (3)</td>
   <td>Cancel key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_HELP</code></td>
   <td style="white-space: nowrap;">0x06 (6)</td>
   <td>Help key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_BACK_SPACE</code></td>
   <td style="white-space: nowrap;">0x08 (8)</td>
   <td>Backspace key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_TAB</code></td>
   <td style="white-space: nowrap;">0x09 (9)</td>
   <td>Tab key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_CLEAR</code></td>
   <td style="white-space: nowrap;">0x0C (12)</td>
   <td>"5" key on Numpad when NumLock is unlocked. Or on Mac, clear key which is positioned at NumLock key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_RETURN</code></td>
   <td style="white-space: nowrap;">0x0D (13)</td>
   <td>Return/enter key on the main keyboard.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_ENTER</code></td>
   <td style="white-space: nowrap;">0x0E (14)</td>
   <td>Reserved, but not used. {{Deprecated_Inline}} (Dropped, see {{bug(969247)}}.)</td>
  </tr>
  <tr>
   <td><code>DOM_VK_SHIFT</code></td>
   <td style="white-space: nowrap;">0x10 (16)</td>
   <td>Shift key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_CONTROL</code></td>
   <td style="white-space: nowrap;">0x11 (17)</td>
   <td>Control key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_ALT</code></td>
   <td style="white-space: nowrap;">0x12 (18)</td>
   <td>Alt (Option on Mac) key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_PAUSE</code></td>
   <td style="white-space: nowrap;">0x13 (19)</td>
   <td>Pause key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_CAPS_LOCK</code></td>
   <td style="white-space: nowrap;">0x14 (20)</td>
   <td>Caps lock.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_KANA</code></td>
   <td style="white-space: nowrap;">0x15 (21)</td>
   <td>Linux support for this keycode was added in Gecko 4.0.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_HANGUL</code></td>
   <td style="white-space: nowrap;">0x15 (21)</td>
   <td>Linux support for this keycode was added in Gecko 4.0.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_EISU</code></td>
   <td style="white-space: nowrap;">0x 16 (22)</td>
   <td>"英数" key on Japanese Mac keyboard.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_JUNJA</code></td>
   <td style="white-space: nowrap;">0x17 (23)</td>
   <td>Linux support for this keycode was added in Gecko 4.0.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_FINAL</code></td>
   <td style="white-space: nowrap;">0x18 (24)</td>
   <td>Linux support for this keycode was added in Gecko 4.0.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_HANJA</code></td>
   <td style="white-space: nowrap;">0x19 (25)</td>
   <td>Linux support for this keycode was added in Gecko 4.0.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_KANJI</code></td>
   <td style="white-space: nowrap;">0x19 (25)</td>
   <td>Linux support for this keycode was added in Gecko 4.0.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_ESCAPE</code></td>
   <td style="white-space: nowrap;">0x1B (27)</td>
   <td>Escape key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_CONVERT</code></td>
   <td style="white-space: nowrap;">0x1C (28)</td>
   <td>Linux support for this keycode was added in Gecko 4.0.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_NONCONVERT</code></td>
   <td style="white-space: nowrap;">0x1D (29)</td>
   <td>Linux support for this keycode was added in Gecko 4.0.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_ACCEPT</code></td>
   <td style="white-space: nowrap;">0x1E (30)</td>
   <td>Linux support for this keycode was added in Gecko 4.0.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_MODECHANGE</code></td>
   <td style="white-space: nowrap;">0x1F (31)</td>
   <td>Linux support for this keycode was added in Gecko 4.0.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_SPACE</code></td>
   <td style="white-space: nowrap;">0x20 (32)</td>
   <td>Space bar.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_PAGE_UP</code></td>
   <td style="white-space: nowrap;">0x21 (33)</td>
   <td>Page Up key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_PAGE_DOWN</code></td>
   <td style="white-space: nowrap;">0x22 (34)</td>
   <td>Page Down key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_END</code></td>
   <td style="white-space: nowrap;">0x23 (35)</td>
   <td>End key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_HOME</code></td>
   <td style="white-space: nowrap;">0x24 (36)</td>
   <td>Home key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_LEFT</code></td>
   <td style="white-space: nowrap;">0x25 (37)</td>
   <td>Left arrow.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_UP</code></td>
   <td style="white-space: nowrap;">0x26 (38)</td>
   <td>Up arrow.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_RIGHT</code></td>
   <td style="white-space: nowrap;">0x27 (39)</td>
   <td>Right arrow.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_DOWN</code></td>
   <td style="white-space: nowrap;">0x28 (40)</td>
   <td>Down arrow.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_SELECT</code></td>
   <td style="white-space: nowrap;">0x29 (41)</td>
   <td>Linux support for this keycode was added in Gecko 4.0.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_PRINT</code></td>
   <td style="white-space: nowrap;">0x2A (42)</td>
   <td>Linux support for this keycode was added in Gecko 4.0.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_EXECUTE</code></td>
   <td style="white-space: nowrap;">0x2B (43)</td>
   <td>Linux support for this keycode was added in Gecko 4.0.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_PRINTSCREEN</code></td>
   <td style="white-space: nowrap;">0x2C (44)</td>
   <td>Print Screen key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_INSERT</code></td>
   <td style="white-space: nowrap;">0x2D (45)</td>
   <td>Ins(ert) key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_DELETE</code></td>
   <td style="white-space: nowrap;">0x2E (46)</td>
   <td>Del(ete) key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_0</code></td>
   <td style="white-space: nowrap;">0x30 (48)</td>
   <td>"0" key in standard key location.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_1</code></td>
   <td style="white-space: nowrap;">0x31 (49)</td>
   <td>"1" key in standard key location.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_2</code></td>
   <td style="white-space: nowrap;">0x32 (50)</td>
   <td>"2" key in standard key location.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_3</code></td>
   <td style="white-space: nowrap;">0x33 (51)</td>
   <td>"3" key in standard key location.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_4</code></td>
   <td style="white-space: nowrap;">0x34 (52)</td>
   <td>"4" key in standard key location.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_5</code></td>
   <td style="white-space: nowrap;">0x35 (53)</td>
   <td>"5" key in standard key location.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_6</code></td>
   <td style="white-space: nowrap;">0x36 (54)</td>
   <td>"6" key in standard key location.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_7</code></td>
   <td style="white-space: nowrap;">0x37 (55)</td>
   <td>"7" key in standard key location.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_8</code></td>
   <td style="white-space: nowrap;">0x38 (56)</td>
   <td>"8" key in standard key location.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_9</code></td>
   <td style="white-space: nowrap;">0x39 (57)</td>
   <td>"9" key in standard key location.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_COLON</code></td>
   <td style="white-space: nowrap;">0x3A (58)</td>
   <td>Colon (":") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_SEMICOLON</code></td>
   <td style="white-space: nowrap;">0x3B (59)</td>
   <td>Semicolon (";") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_LESS_THAN</code></td>
   <td style="white-space: nowrap;">0x3C (60)</td>
   <td>Less-than ("&lt;") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_EQUALS</code></td>
   <td style="white-space: nowrap;">0x3D (61)</td>
   <td>Equals ("=") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_GREATER_THAN</code></td>
   <td style="white-space: nowrap;">0x3E (62)</td>
   <td>Greater-than ("&gt;") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_QUESTION_MARK</code></td>
   <td style="white-space: nowrap;">0x3F (63)</td>
   <td>Question mark ("?") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_AT</code></td>
   <td style="white-space: nowrap;">0x40 (64)</td>
   <td>Atmark ("@") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_A</code></td>
   <td style="white-space: nowrap;">0x41 (65)</td>
   <td>"A" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_B</code></td>
   <td style="white-space: nowrap;">0x42 (66)</td>
   <td>"B" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_C</code></td>
   <td style="white-space: nowrap;">0x43 (67)</td>
   <td>"C" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_D</code></td>
   <td style="white-space: nowrap;">0x44 (68)</td>
   <td>"D" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_E</code></td>
   <td style="white-space: nowrap;">0x45 (69)</td>
   <td>"E" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F</code></td>
   <td style="white-space: nowrap;">0x46 (70)</td>
   <td>"F" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_G</code></td>
   <td style="white-space: nowrap;">0x47 (71)</td>
   <td>"G" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_H</code></td>
   <td style="white-space: nowrap;">0x48 (72)</td>
   <td>"H" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_I</code></td>
   <td style="white-space: nowrap;">0x49 (73)</td>
   <td>"I" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_J</code></td>
   <td style="white-space: nowrap;">0x4A (74)</td>
   <td>"J" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_K</code></td>
   <td style="white-space: nowrap;">0x4B (75)</td>
   <td>"K" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_L</code></td>
   <td style="white-space: nowrap;">0x4C (76)</td>
   <td>"L" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_M</code></td>
   <td style="white-space: nowrap;">0x4D (77)</td>
   <td>"M" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_N</code></td>
   <td style="white-space: nowrap;">0x4E (78)</td>
   <td>"N" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_O</code></td>
   <td style="white-space: nowrap;">0x4F (79)</td>
   <td>"O" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_P</code></td>
   <td style="white-space: nowrap;">0x50 (80)</td>
   <td>"P" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_Q</code></td>
   <td style="white-space: nowrap;">0x51 (81)</td>
   <td>"Q" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_R</code></td>
   <td style="white-space: nowrap;">0x52 (82)</td>
   <td>"R" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_S</code></td>
   <td style="white-space: nowrap;">0x53 (83)</td>
   <td>"S" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_T</code></td>
   <td style="white-space: nowrap;">0x54 (84)</td>
   <td>"T" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_U</code></td>
   <td style="white-space: nowrap;">0x55 (85)</td>
   <td>"U" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_V</code></td>
   <td style="white-space: nowrap;">0x56 (86)</td>
   <td>"V" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_W</code></td>
   <td style="white-space: nowrap;">0x57 (87)</td>
   <td>"W" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_X</code></td>
   <td style="white-space: nowrap;">0x58 (88)</td>
   <td>"X" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_Y</code></td>
   <td style="white-space: nowrap;">0x59 (89)</td>
   <td>"Y" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_Z</code></td>
   <td style="white-space: nowrap;">0x5A (90)</td>
   <td>"Z" key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN</code></td>
   <td style="white-space: nowrap;">0x5B (91)</td>
   <td>Windows logo key on Windows. Or Super or Hyper key on Linux.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_CONTEXT_MENU</code></td>
   <td style="white-space: nowrap;">0x5D (93)</td>
   <td>Opening context menu key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_SLEEP</code></td>
   <td style="white-space: nowrap;">0x5F (95)</td>
   <td>Linux support for this keycode was added in Gecko 4.0.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_NUMPAD0</code></td>
   <td style="white-space: nowrap;">0x60 (96)</td>
   <td>"0" on the numeric keypad.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_NUMPAD1</code></td>
   <td style="white-space: nowrap;">0x61 (97)</td>
   <td>"1" on the numeric keypad.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_NUMPAD2</code></td>
   <td style="white-space: nowrap;">0x62 (98)</td>
   <td>"2" on the numeric keypad.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_NUMPAD3</code></td>
   <td style="white-space: nowrap;">0x63 (99)</td>
   <td>"3" on the numeric keypad.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_NUMPAD4</code></td>
   <td style="white-space: nowrap;">0x64 (100)</td>
   <td>"4" on the numeric keypad.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_NUMPAD5</code></td>
   <td style="white-space: nowrap;">0x65 (101)</td>
   <td>"5" on the numeric keypad.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_NUMPAD6</code></td>
   <td style="white-space: nowrap;">0x66 (102)</td>
   <td>"6" on the numeric keypad.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_NUMPAD7</code></td>
   <td style="white-space: nowrap;">0x67 (103)</td>
   <td>"7" on the numeric keypad.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_NUMPAD8</code></td>
   <td style="white-space: nowrap;">0x68 (104)</td>
   <td>"8" on the numeric keypad.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_NUMPAD9</code></td>
   <td style="white-space: nowrap;">0x69 (105)</td>
   <td>"9" on the numeric keypad.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_MULTIPLY</code></td>
   <td style="white-space: nowrap;">0x6A (106)</td>
   <td>"*" on the numeric keypad.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_ADD</code></td>
   <td style="white-space: nowrap;">0x6B (107)</td>
   <td>"+" on the numeric keypad.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_SEPARATOR</code></td>
   <td style="white-space: nowrap;">0x6C (108)</td>
   <td></td>
  </tr>
  <tr>
   <td><code>DOM_VK_SUBTRACT</code></td>
   <td style="white-space: nowrap;">0x6D (109)</td>
   <td>"-" on the numeric keypad.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_DECIMAL</code></td>
   <td style="white-space: nowrap;">0x6E (110)</td>
   <td>Decimal point on the numeric keypad.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_DIVIDE</code></td>
   <td style="white-space: nowrap;">0x6F (111)</td>
   <td>"/" on the numeric keypad.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F1</code></td>
   <td style="white-space: nowrap;">0x70 (112)</td>
   <td>F1 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F2</code></td>
   <td style="white-space: nowrap;">0x71 (113)</td>
   <td>F2 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F3</code></td>
   <td style="white-space: nowrap;">0x72 (114)</td>
   <td>F3 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F4</code></td>
   <td style="white-space: nowrap;">0x73 (115)</td>
   <td>F4 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F5</code></td>
   <td style="white-space: nowrap;">0x74 (116)</td>
   <td>F5 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F6</code></td>
   <td style="white-space: nowrap;">0x75 (117)</td>
   <td>F6 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F7</code></td>
   <td style="white-space: nowrap;">0x76 (118)</td>
   <td>F7 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F8</code></td>
   <td style="white-space: nowrap;">0x77 (119)</td>
   <td>F8 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F9</code></td>
   <td style="white-space: nowrap;">0x78 (120)</td>
   <td>F9 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F10</code></td>
   <td style="white-space: nowrap;">0x79 (121)</td>
   <td>F10 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F11</code></td>
   <td style="white-space: nowrap;">0x7A (122)</td>
   <td>F11 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F12</code></td>
   <td style="white-space: nowrap;">0x7B (123)</td>
   <td>F12 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F13</code></td>
   <td style="white-space: nowrap;">0x7C (124)</td>
   <td>F13 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F14</code></td>
   <td style="white-space: nowrap;">0x7D (125)</td>
   <td>F14 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F15</code></td>
   <td style="white-space: nowrap;">0x7E (126)</td>
   <td>F15 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F16</code></td>
   <td style="white-space: nowrap;">0x7F (127)</td>
   <td>F16 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F17</code></td>
   <td style="white-space: nowrap;">0x80 (128)</td>
   <td>F17 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F18</code></td>
   <td style="white-space: nowrap;">0x81 (129)</td>
   <td>F18 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F19</code></td>
   <td style="white-space: nowrap;">0x82 (130)</td>
   <td>F19 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F20</code></td>
   <td style="white-space: nowrap;">0x83 (131)</td>
   <td>F20 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F21</code></td>
   <td style="white-space: nowrap;">0x84 (132)</td>
   <td>F21 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F22</code></td>
   <td style="white-space: nowrap;">0x85 (133)</td>
   <td>F22 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F23</code></td>
   <td style="white-space: nowrap;">0x86 (134)</td>
   <td>F23 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_F24</code></td>
   <td style="white-space: nowrap;">0x87 (135)</td>
   <td>F24 key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_NUM_LOCK</code></td>
   <td style="white-space: nowrap;">0x90 (144)</td>
   <td>Num Lock key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_SCROLL_LOCK</code></td>
   <td style="white-space: nowrap;">0x91 (145)</td>
   <td>Scroll Lock key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_OEM_FJ_JISHO</code></td>
   <td style="white-space: nowrap;">0x92 (146)</td>
   <td>An <a href="#OEM_specific_keys_on_Windows">OEM specific key on Windows</a>. This was used for "Dictionary" key on Fujitsu OASYS.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_OEM_FJ_MASSHOU</code></td>
   <td style="white-space: nowrap;">0x93 (147)</td>
   <td>An <a href="#OEM_specific_keys_on_Windows">OEM specific key on Windows</a>. This was used for "Unregister word" key on Fujitsu OASYS.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_OEM_FJ_TOUROKU</code></td>
   <td style="white-space: nowrap;">0x94 (148)</td>
   <td>An <a href="#OEM_specific_keys_on_Windows">OEM specific key on Windows</a>. This was used for "Register word" key on Fujitsu OASYS.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_OEM_FJ_LOYA</code></td>
   <td style="white-space: nowrap;">0x95 (149)</td>
   <td>An <a href="#OEM_specific_keys_on_Windows">OEM specific key on Windows</a>. This was used for "Left OYAYUBI" key on Fujitsu OASYS.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_OEM_FJ_ROYA</code></td>
   <td style="white-space: nowrap;">0x96 (150)</td>
   <td>An <a href="#OEM_specific_keys_on_Windows">OEM specific key on Windows</a>. This was used for "Right OYAYUBI" key on Fujitsu OASYS.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_CIRCUMFLEX</code></td>
   <td style="white-space: nowrap;">0xA0 (160)</td>
   <td>Circumflex ("^") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_EXCLAMATION</code></td>
   <td style="white-space: nowrap;">0xA1 (161)</td>
   <td>Exclamation ("!") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_DOUBLE_QUOTE</code></td>
   <td style="white-space: nowrap;">0xA3 (162)</td>
   <td>Double quote (""") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_HASH</code></td>
   <td style="white-space: nowrap;">0xA3 (163)</td>
   <td>Hash ("#") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_DOLLAR</code></td>
   <td style="white-space: nowrap;">0xA4 (164)</td>
   <td>Dollar sign ("$") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_PERCENT</code></td>
   <td style="white-space: nowrap;">0xA5 (165)</td>
   <td>Percent ("%") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_AMPERSAND</code></td>
   <td style="white-space: nowrap;">0xA6 (166)</td>
   <td>Ampersand ("&amp;") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_UNDERSCORE</code></td>
   <td style="white-space: nowrap;">0xA7 (167)</td>
   <td>Underscore ("_") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_OPEN_PAREN</code></td>
   <td style="white-space: nowrap;">0xA8 (168)</td>
   <td>Open parenthesis ("(") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_CLOSE_PAREN</code></td>
   <td style="white-space: nowrap;">0xA9 (169)</td>
   <td>Close parenthesis (")") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_ASTERISK</code></td>
   <td style="white-space: nowrap;">0xAA (170)</td>
   <td>Asterisk ("*") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_PLUS</code></td>
   <td style="white-space: nowrap;">0xAB (171)</td>
   <td>Plus ("+") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_PIPE</code></td>
   <td style="white-space: nowrap;">0xAC (172)</td>
   <td>Pipe ("|") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_HYPHEN_MINUS</code></td>
   <td style="white-space: nowrap;">0xAD (173)</td>
   <td>Hyphen-US/docs/Minus ("-") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_OPEN_CURLY_BRACKET</code></td>
   <td style="white-space: nowrap;">0xAE (174)</td>
   <td>Open curly bracket ("{") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_CLOSE_CURLY_BRACKET</code></td>
   <td style="white-space: nowrap;">0xAF (175)</td>
   <td>Close curly bracket ("}") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_TILDE</code></td>
   <td style="white-space: nowrap;">0xB0 (176)</td>
   <td>Tilde ("~") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_VOLUME_MUTE</code></td>
   <td style="white-space: nowrap;">0xB5 (181)</td>
   <td>Audio mute key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_VOLUME_DOWN</code></td>
   <td style="white-space: nowrap;">0xB6 (182)</td>
   <td>Audio volume down key</td>
  </tr>
  <tr>
   <td><code>DOM_VK_VOLUME_UP</code></td>
   <td style="white-space: nowrap;">0xB7 (183)</td>
   <td>Audio volume up key</td>
  </tr>
  <tr>
   <td><code>DOM_VK_COMMA</code></td>
   <td style="white-space: nowrap;">0xBC (188)</td>
   <td>Comma (",") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_PERIOD</code></td>
   <td style="white-space: nowrap;">0xBE (190)</td>
   <td>Period (".") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_SLASH</code></td>
   <td style="white-space: nowrap;">0xBF (191)</td>
   <td>Slash ("/") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_BACK_QUOTE</code></td>
   <td style="white-space: nowrap;">0xC0 (192)</td>
   <td>Back tick ("`") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_OPEN_BRACKET</code></td>
   <td style="white-space: nowrap;">0xDB (219)</td>
   <td>Open square bracket ("[") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_BACK_SLASH</code></td>
   <td style="white-space: nowrap;">0xDC (220)</td>
   <td>Back slash ("\") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_CLOSE_BRACKET</code></td>
   <td style="white-space: nowrap;">0xDD (221)</td>
   <td>Close square bracket ("]") key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_QUOTE</code></td>
   <td style="white-space: nowrap;">0xDE (222)</td>
   <td>Quote (''') key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_META</code></td>
   <td style="white-space: nowrap;">0xE0 (224)</td>
   <td>Meta key on Linux, Command key on Mac.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_ALTGR</code></td>
   <td style="white-space: nowrap;">0xE1 (225)</td>
   <td>AltGr key (Level 3 Shift key or Level 5 Shift key) on Linux.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_ICO_HELP</code></td>
   <td style="white-space: nowrap;">0xE3 (227)</td>
   <td>An <a href="#OEM_specific_keys_on_Windows">OEM specific key on Windows</a>. This is (was?) used for Olivetti ICO keyboard.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_ICO_00</code></td>
   <td style="white-space: nowrap;">0xE4 (228)</td>
   <td>An <a href="#OEM_specific_keys_on_Windows">OEM specific key on Windows</a>. This is (was?) used for Olivetti ICO keyboard.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_ICO_CLEAR</code></td>
   <td style="white-space: nowrap;">0xE6 (230)</td>
   <td>An <a href="#OEM_specific_keys_on_Windows">OEM specific key on Windows</a>. This is (was?) used for Olivetti ICO keyboard.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_OEM_RESET</code></td>
   <td style="white-space: nowrap;">0xE9 (233)</td>
   <td>An <a href="#OEM_specific_keys_on_Windows">OEM specific key on Windows</a>. This was used for Nokia/Ericsson's device.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_OEM_JUMP</code></td>
   <td style="white-space: nowrap;">0xEA (234)</td>
   <td>An <a href="#OEM_specific_keys_on_Windows">OEM specific key on Windows</a>. This was used for Nokia/Ericsson's device.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_OEM_PA1</code></td>
   <td style="white-space: nowrap;">0xEB (235)</td>
   <td>An <a href="#OEM_specific_keys_on_Windows">OEM specific key on Windows</a>. This was used for Nokia/Ericsson's device.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_OEM_PA2</code></td>
   <td style="white-space: nowrap;">0xEC (236)</td>
   <td>An <a href="#OEM_specific_keys_on_Windows">OEM specific key on Windows</a>. This was used for Nokia/Ericsson's device.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_OEM_PA3</code></td>
   <td style="white-space: nowrap;">0xED (237)</td>
   <td>An <a href="#OEM_specific_keys_on_Windows">OEM specific key on Windows</a>. This was used for Nokia/Ericsson's device.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_OEM_WSCTRL</code></td>
   <td style="white-space: nowrap;">0xEE (238)</td>
   <td>An <a href="#OEM_specific_keys_on_Windows">OEM specific key on Windows</a>. This was used for Nokia/Ericsson's device.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_OEM_CUSEL</code></td>
   <td style="white-space: nowrap;">0xEF (239)</td>
   <td>An <a href="#OEM_specific_keys_on_Windows">OEM specific key on Windows</a>. This was used for Nokia/Ericsson's device.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_OEM_ATTN</code></td>
   <td style="white-space: nowrap;">0xF0 (240)</td>
   <td>An <a href="#OEM_specific_keys_on_Windows">OEM specific key on Windows</a>. This was used for Nokia/Ericsson's device.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_OEM_FINISH</code></td>
   <td style="white-space: nowrap;">0xF1 (241)</td>
   <td>An <a href="#OEM_specific_keys_on_Windows">OEM specific key on Windows</a>. This was used for Nokia/Ericsson's device.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_OEM_COPY</code></td>
   <td style="white-space: nowrap;">0xF2 (242)</td>
   <td>An <a href="#OEM_specific_keys_on_Windows">OEM specific key on Windows</a>. This was used for Nokia/Ericsson's device.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_OEM_AUTO</code></td>
   <td style="white-space: nowrap;">0xF3 (243)</td>
   <td>An <a href="#OEM_specific_keys_on_Windows">OEM specific key on Windows</a>. This was used for Nokia/Ericsson's device.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_OEM_ENLW</code></td>
   <td style="white-space: nowrap;">0xF4 (244)</td>
   <td>An <a href="#OEM_specific_keys_on_Windows">OEM specific key on Windows</a>. This was used for Nokia/Ericsson's device.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_OEM_BACKTAB</code></td>
   <td style="white-space: nowrap;">0xF5 (245)</td>
   <td>An <a href="#OEM_specific_keys_on_Windows">OEM specific key on Windows</a>. This was used for Nokia/Ericsson's device.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_ATTN</code></td>
   <td style="white-space: nowrap;">0xF6 (246)</td>
   <td>Attn (Attention) key of IBM midrange computers, e.g., AS/400.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_CRSEL</code></td>
   <td style="white-space: nowrap;">0xF7 (247)</td>
   <td>CrSel (Cursor Selection) key of IBM 3270 keyboard layout.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_EXSEL</code></td>
   <td style="white-space: nowrap;">0xF8 (248)</td>
   <td>ExSel (Extend Selection) key of IBM 3270 keyboard layout.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_EREOF</code></td>
   <td style="white-space: nowrap;">0xF9 (249)</td>
   <td>Erase EOF key of IBM 3270 keyboard layout.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_PLAY</code></td>
   <td style="white-space: nowrap;">0xFA (250)</td>
   <td>Play key of IBM 3270 keyboard layout.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_ZOOM</code></td>
   <td style="white-space: nowrap;">0xFB (251)</td>
   <td>Zoom key.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_PA1</code></td>
   <td style="white-space: nowrap;">0xFD (253)</td>
   <td>PA1 key of IBM 3270 keyboard layout.</td>
  </tr>
  <tr>
   <td><code>DOM_VK_WIN_OEM_CLEAR</code></td>
   <td style="white-space: nowrap;">0xFE (254)</td>
   <td>Clear key, but we're not sure the meaning difference from <code>DOM_VK_CLEAR</code>.</td>
  </tr>
 </tbody>
</table>

<h3 id="Windows上的OEM特定密钥">Windows 上的 OEM 特定密钥</h3>

<p>在 Windows 上，虚拟密钥代码的某些值是为特定于 OEM 的密钥定义（保留）的。它们可用于非标准键盘上的特殊键。换句话说，一些值被两个或多个供应商（或硬件）用于不同的含义。</p>

<p>从 gecko 21（并且早于 15）开始，仅在 Windows 上的 keycode 属性上提供 OEM 特定的键值。因此，它们对于通常的 Web 应用程序不有用。它们仅对内部网应用程序或类似情况有用。</p>

<p>查看 MSDN 上的"<a href="http://msdn.microsoft.com/en-us/library/aa452679.aspx">Manufacturer-specific Virtual-Key Codes (Windows CE 5.0)</a>"了解更多</p>