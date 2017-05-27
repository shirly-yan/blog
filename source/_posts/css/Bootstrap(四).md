---
title: Bootstrap(四)
date: 2017-05-26 16:53:15
categories: [css]
tags: [bootstrap]
---
Bootstrap表单
<!-- more -->

<h1>表单</h1>

<h2>垂直表单(默认)</h2>
<img src="/images/55.png" width="300" height="186" />
```html
<form role="form">
  <div class="form-group">
    <label for="exampleInputEmail1">邮箱：</label>
    <input type="email" class="form-control" id="exampleInputEmail1" placeholder="请输入您的邮箱地址">
  </div>
  <div class="form-group">
    <label for="exampleInputPassword1">密码</label>
    <input type="password" class="form-control" id="exampleInputPassword1" placeholder="请输入您的邮箱密码">
  </div>
  <div class="checkbox">
    <label>
      <input type="checkbox"> 记住密码
    </label>
  </div>
  <button type="submit" class="btn btn-default">进入邮箱</button>
</form>
```
<h2>水平表单</h2>
<img src="/images/56.png" width="300" height="138" />
```html
<form class="form-horizontal" role="form">
  <div class="form-group">
    <label for="inputEmail3" class="col-sm-2 control-label">邮箱</label>
    <div class="col-sm-10">
      <input type="email" class="form-control" id="inputEmail3" placeholder="请输入您的邮箱地址">
    </div>
  </div>
  <div class="form-group">
    <label for="inputPassword3" class="col-sm-2 control-label">密码</label>
    <div class="col-sm-10">
      <input type="password" class="form-control" id="inputPassword3" placeholder="请输入您的邮箱密码">
    </div>
  </div>
  <div class="form-group">
    <div class="col-sm-offset-2 col-sm-10">
      <div class="checkbox">
        <label>
          <input type="checkbox"> 记住密码
        </label>
      </div>
    </div>
  </div>
  <div class="form-group">
    <div class="col-sm-offset-2 col-sm-10">
      <button type="submit" class="btn btn-default">进入邮箱</button>
    </div>
  </div>
</form>
```
<h2>内联表单</h2>
<img src="/images/6.png" width="300" height="50" />
在label标签运用了一个类名“sr-only”，标签没显示就是这个样式将标签隐藏了。
```html
<form class="form-inline" role="form">
<div class="form-group">
  <label class="sr-only" for="exampleInputEmail2">邮箱</label>
  <input type="email" class="form-control" id="exampleInputEmail2" placeholder="请输入你的邮箱地址">
</div>
<div class="form-group">
  <label class="sr-only" for="exampleInputPassword2">密码</label>
  <input type="password" class="form-control" id="exampleInputPassword2" placeholder="请输入你的邮箱密码">
</div>
<div class="checkbox">
<label>
   <input type="checkbox">记住密码
</label>
</div>
<button type="submit" class="btnbtn-default">进入邮箱</button>
</form>
```
<h2>输入框input</h2>
```html
<form role="form">
<div class="form-group">
<input type="email" class="form-control" placeholder="Enter email">
</div>
</form>
```

<h2>下拉选择框select</h2>
```html
<form role="form">
<div class="form-group">
  <select class="form-control">
    <option>1</option>
    <option>2</option>
    <option>3</option>
    <option>4</option>
    <option>5</option>
  </select>
  </div>
  <div class="form-group">
  <select multiple class="form-control">
    <option>1</option>
    <option>2</option>
    <option>3</option>
    <option>4</option>
    <option>5</option>
  </select>
</div>
</form>
```
<h2>文本域textarea</h2>
```html
<form role="form">
  <div class="form-group">
    <textarea class="form-control" rows="3"></textarea>
  </div>
</form>
```

<h2>(复选框checkbox和单选择按钮radio</h2>
```html
<form role="form">
  <h3>案例1</h3>
  <div class="checkbox">
    <label>
      <input type="checkbox" value="">
      记住密码
    </label>
  </div>
  <div class="radio">
    <label>
      <input type="radio" name="optionsRadios" id="optionsRadios1" value="love" checked>
      喜欢
    </label>
  </div>
    <div class="radio">
    <label>
      <input type="radio" name="optionsRadios" id="optionsRadios2" value="hate">
      不喜欢
    </label>
  </div>
</form>     
```

<h2>复选框和单选按钮水平排列</h2>
```html
<form role="form">
  <div class="form-group">
    <label class="checkbox-inline">
      <input type="checkbox"  value="option1">游戏
    </label>
    <label class="checkbox-inline">
      <input type="checkbox"  value="option2">摄影
    </label>
    <label class="checkbox-inline">
    <input type="checkbox"  value="option3">旅游
    </label>
  </div>
  <div class="form-group">
    <label class="radio-inline">
      <input type="radio"  value="option1" name="sex">男性
    </label>
    <label class="radio-inline">
      <input type="radio"  value="option2" name="sex">女性
    </label>
    <label class="radio-inline">
      <input type="radio"  value="option3" name="sex">中性
    </label>
  </div>
</form>
```

<h2>表单控件大小</h2>
```html
<input class="form-control input-lg" type="text" placeholder="添加.input-lg，控件变大">
<input class="form-control" type="text" placeholder="正常大小">
<input class="form-control input-sm" type="text" placeholder="添加.input-sm，控件变小">
```

<h2>表单控件状态</h2>
<h3>禁用状态</h3>
在相应的表单控件上添加属性“disabled”
在Bootstrap框架中，如果fieldset设置了disabled属性，整个域都将处于被禁用状态。
```html
<input class="form-control" type="text" placeholder="表单已禁用，不能输入" disabled>
```
<h3>验证状态</h3>
在Bootstrap框架中同样提供这几种效果。
+ 1、.has-warning: 警告状态（黄色）
+ 2、.has-error：错误状态（红色）
+ 3、.has-success：成功状态（绿色）
使用的时候只需要在form-group容器上对应添加状态类名。

如果你想让表单在对应的状态下显示 icon 出来，只需要在对应的状态下添加类名“has-feedback”。请注意，此类名要与“has-error”、“has-warning”和“has-success”在一起：
而且必须在表单中添加了一个 span 元素：
```html
<span class="glyphiconglyphicon-warning-sign form-control-feedback"></span>
```
<img src="/images/57.png" width="800" height="734" />

```html
<h3>示例1</h3>
<form role="form">
  <div class="form-group has-success">
    <label class="control-label" for="inputSuccess1">成功状态</label>
    <input type="text" class="form-control" id="inputSuccess1" placeholder="成功状态" >
  </div>
  <div class="form-group has-warning">
    <label class="control-label" for="inputWarning1">警告状态</label>
    <input type="text" class="form-control" id="inputWarning1" placeholder="警告状态">
  </div>
  <div class="form-group has-error">
    <label class="control-label" for="inputError1">错误状态</label>
    <input type="text" class="form-control" id="inputError1" placeholder="错误状态">
  </div>
</form>  
<br>
<br>
<br>
<h3>示例2</h3>   
<form role="form">
  <div class="form-group has-success has-feedback">
    <label class="control-label" for="inputSuccess1">成功状态</label>
    <input type="text" class="form-control" id="inputSuccess1" placeholder="成功状态" >
    <span class="glyphicon glyphicon-ok form-control-feedback"></span>
  </div>
  <div class="form-group has-warning has-feedback">
    <label class="control-label" for="inputWarning1">警告状态</label>
    <input type="text" class="form-control" id="inputWarning1" placeholder="警告状态">
    <span class="glyphicon glyphicon-warning-sign form-control-feedback"></span>
  </div>
  <div class="form-group has-error has-feedback">
    <label class="control-label" for="inputError1">错误状态</label>
    <input type="text" class="form-control" id="inputError1" placeholder="错误状态">
    <span class="glyphicon glyphicon-remove form-control-feedback"></span>  
  </div>

</form>
```
<h3>提示信息</h3>
在Bootstrap框架中也提供了不同的提示信息的效果。使用了一个"help-block"样式，将提示信息以块状显示，并且显示在控件底部。
<img src="/images/58.png" width="300" height="160" />
```html
<form role="form">
<div class="form-group has-success has-feedback">
  <label class="control-label" for="inputSuccess1">成功状态</label>
  <input type="text" class="form-control" id="inputSuccess1" placeholder="成功状态" >
  <span class="help-block">你输入的信息是正确的</span>
  <span class="glyphiconglyphicon-ok form-control-feedback"></span>
</div>
</form>
```
<h2>按钮</h2>
<h3>制作按钮</h3>
虽然在Bootstrap框架中使用任何标签元素都可以实现按钮风格，但个人并不建议这样使用，为了避免浏览器兼容性问题，个人强烈建议使用button或a标签来制作按钮。
```html
<button class="btn btn-default" type="button">button标签按钮</button>
<input type="submit" class="btn btn-default" value="input标签按钮"/>
<a href="##" class="btn btn-default">a标签按钮</a>
<span class="btn btn-default">span标签按钮</span>
<div class="btn btn-default">div标签按钮</div>
```
<img src="/images/61.png" width="250" height="69" />

<h3>按钮风格</h3>
```html
<button class="btn" type="button">基础按钮.btn</button>
<button class="btn btn-default" type="button">默认按钮.btn-default</button>
<button class="btn btn-primary" type="button">主要按钮.btn-primary</button>
<button class="btn btn-success" type="button">成功按钮.btn-success</button>
<button class="btn btn-info" type="button">信息按钮.btn-info</button>
<button class="btn btn-warning" type="button">警告按钮.btn-warning</button>
<button class="btn btn-danger" type="button">危险按钮.btn-danger</button>
<button class="btn btn-link" type="button">链接按钮.btn-link</button>
```
<img src="/images/62.png" width="800" height="331" />
<img src="/images/63.png" width="800" height="808" />

<h3>按钮大小</h3>
```html
<button class="btn btn-primary btn-lg" type="button">大型按钮.btn-lg</button> 
<button class="btn btn-primary" type="button">正常按钮</button>
<button class="btn btn-primary btn-sm" type="button">小型按钮.btn-sm</button>
<button class="btn btn-primary btn-xs" type="button">超小型按钮.btn-xs</button>
```
<img src="/images/64.png" width="800" height="708" />

<h3>按钮状态</h3>
<h4>活动状态</h4>
Bootstrap按钮的活动状态主要包括按钮的
+ 悬浮状态(:hover)
+ 点击状态(:active)
+ 焦点状态(:focus)

<h4>禁用状态</h4>
在Bootstrap框架中，要禁用按钮有两种实现方式：
+ 方法1：在标签中添加disabled属性
+ 方法2：在元素标签中添加类名“disabled”

<img src="/images/65.png" width="300" height="119" />

```html
<button class="btnbtn-primary btn-lgbtn-block" type="button" disabled="disabled">通过disabled属性禁用按钮</button>
<button class="btnbtn-primary btn-block disabled" type="button">通过添加类名disabled禁用按钮</button>
<button class="btnbtn-primary btn-smbtn-block" type="button">未禁用的按钮</button>
```
<h3>基本按钮</h3>
```html
<button class="btn" type="button">基本按钮</button>
```
<img src="/images/59.png" width="300" height="90" />

<h3>默认按钮</h3>
```html
<button class="btn btn-default" type="button">默认按钮</button>
```
<img src="/images/60.png" width="200" height="114" />

<h1>图像</h1>

在Bootstrap框架中对于图像的样式风格提供以下几种风格：
+ 1、img-responsive：响应式图片，主要针对于响应式设计
+ 2、img-rounded：圆角图片
+ 3、img-circle：圆形图片
+ 4、img-thumbnail：缩略图片

```html
<img  alt="140x140" src="http://placehold.it/140x140">
<img  class="img-rounded" alt="140x140" src="http://placehold.it/140x140">
<img  class="img-circle" alt="140x140" src="http://placehold.it/140x140">
<img  class="img-thumbnail" alt="140x140" src="http://placehold.it/140x140">
<img  class="img-responsive" alt="140x140" src="http://placehold.it/140x140">
```
<img src="/images/66.png" width="300" height="140" />

<h1>图标</h1>

所有图标名称
[http://getbootstrap.com/components/#glyphicons](http://getbootstrap.com/components/#glyphicons)

```html
<span class="glyphicon glyphicon-search"></span>
<span class="glyphicon glyphicon-asterisk"></span>
<span class="glyphicon glyphicon-plus"></span>
<span class="glyphicon glyphicon-cloud"></span>
```




<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
