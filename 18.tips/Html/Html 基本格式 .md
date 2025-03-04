# Html 基本格式

```jsx
<h1>最大的标题</h1>
<h2> . . . </h2>
<h3> . . . </h3>
<h4> . . . </h4>
<h5> . . . </h5>
<h6>最小的标题</h6>
```

```jsx
<p>这是一个段落。</p>
<br> （换行）
<hr> （水平线）
<!-- 这是注释 -->
```

```jsx
<b>粗体文本</b>
<code>计算机代码</code>
<em>强调文本</em>
<i>斜体文本</i>
<kbd>键盘输入</kbd>
<pre>预格式化文本</pre>
<small>更小的文本</small>
<strong>重要的文本</strong>
```

```jsx
<abbr> （缩写）
<address> （联系信息）
<bdo> （文字方向）
<blockquote> （从另一个源引用的部分）
<cite> （工作的名称）
<del> （删除的文本）
<ins> （插入的文本）
<sub> （下标文本）
<sup> （上标文本）
```

```jsx
普通的链接：<a href="[http://www.example.com/](http://www.example.com/)">链接文本</a>
图像链接： <a href="[http://www.example.com/](http://www.example.com/)"><img src="URL" alt="替换文本"></a>
邮件链接： <a [href="mailto:webmaster@example.com](mailto:href=%22mailto:webmaster@example.com)">发送e-mail</a>
书签：
<a id="tips">提示部分</a>
<a href="#tips">跳到提示部分</a>
```

```jsx
//图片
<img src="URL" alt="替换文本" height="42" width="42">
```

```jsx
<style type="text/css">
h1 {color:red;}
p {color:blue;}
</style>
<div>文档中的块级元素</div>
<span>文档中的内联元素</span>
```

```jsx
<ul>
    <li>项目</li>
    <li>项目</li>
</ul>
<ol>
    <li>第一项</li>
    <li>第二项</li>
</ol>
<table border="1">
  <tr>
    <th>表格标题</th>
    <th>表格标题</th>
  </tr>
  <tr>
    <td>表格数据</td>
    <td>表格数据</td>
  </tr>
</table>
<iframe src="demo_iframe.htm"></iframe>

<form action="demo_form.php" method="post/get">
<input type="text" name="email" size="40" maxlength="50">
<input type="password">
<input type="checkbox" checked="checked">
<input type="radio" checked="checked">
<input type="submit" value="Send">
<input type="reset">
<input type="hidden">
<select>
<option>苹果</option>
<option selected="selected">香蕉</option>
<option>樱桃</option>
</select>
<textarea name="comment" rows="60" cols="20"></textarea>
 
</form>

&lt; 等同于 <
&gt; 等同于 >
&#169; 等同于 ©
```