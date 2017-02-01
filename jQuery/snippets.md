# 1.鼠标悬停
改变元素的hover属性，控制鼠标悬停的效果

```javascript
$("a").hover(
    function() {
        ... // mouse hover on
    },
    function() {
        ... // mouse leave
    }
);
```

# 2.阻止事件流默认行为
我们会使用 `<a>` `<button>` 标签来进行跳转或提交。但在单页应用中，我们常常不希望这些元素的默认行为来刷新页面，这时候就需要阻止这些行为。


```javascript
$("a").on("click", function(e) {
    e.preventDefault();
});
```

# 3.滚动
利用ScollTop来滚到到页面顶部，或内容的底部

```javascript
// scroll to top
$("a[href='#top']).click(function() {
    $("html, body").animate({ scrollTop: 0 }, "slow");
});

// scroll to bottom
$el = $("#container");
el.scrollTop(el.prop("scrollHeight");

```

# 4.Ajax调用

Ajax是jQuery最常用的部分，下面给出一个经典的调用模式。

```javascript
$.ajax({
    type: 'POST',
    url: 'api/foo',
    data: { ... }, // request body
    success: function(data) { ... }, // success callback
    error: function(err) {
        // error callback
        alter(err.status + ' ' + err.statusText);
    }
});
```

# 5.动画

动画可以帮助我们将页面活跃起来，jq的动画用起来也相当的直接。

```javascript
$("p").animate({
    left: '+=90px",
    top: '+=150px",
    opacity: 0.25
    }, 900, 'linear', function() {
        // animation complete callback
});
```

# 6.增删CSS类
动态的修改元素的class，就可以动态的改变样式。

```javascript
$("nav a").toggleClass("selected");
$("li").addClass("edit");
$("li").removeClass("edit");

$("#special").css("font-size", "20px");
```

# 7.元素可视性

通过元素的显示，隐藏，淡入淡出，来灵活的展现页面

```javascript
$("a.register").on("click", function(e) {
    $("#signup").fadeToggle(750, "linear");
});

$el.fadeIn();
$el.delay(500).fadeOut();

$el.show();
$el.hide("slide", { direction: "left" }, 1000);
```

# 8.加载外部内容

即使不通过ajax，我们也能够从外部文件获取新的内容到当前页面

```javascript
$("#content").load("somefile.html", function(response, status, xhr) {
    // error handling
    if (status === "error") {
        $("#content").html("An error occurred: " + xhr.status + " " + xhr.statusText);
    }
});
```

# 9.键盘事件

在用户交互时，键盘事件时不可缺少的一部分，可以通过键盘事件来达到快捷键或遮蔽某些功能。

```javascript
$("input").keydown(function(e) {
    
    if (e.which == 13) {
        e.preventDefault();
    }

});

$("input").keyup(function(e) {
    // run other event
});

$("#textbox").keypress(function(e) {
    // run other event
});
```

- [key code online](http://keycode.info/)
- [key code list](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/keyCode)

# 10.等列高度

有些时候CSS样式高度会有偏差，此时可以会用到，来动态调整，达到高度一致

```javascript
var maxheight = 0;
$("div.col").each(function() {
    if($(this).height() > maxheight) {
        maxheight = $(this).height();
    }
});

$("div.col").height(maxheight);
```

# 11.附加html

动态的添加，删除html，在操作DOM是一种常见手段

```javascript
var someText = "some html here";
$("p#text1").append(someText); // add after
$("p#text1").prepend(someText); // add before
```

# 12.读写元素属性

相当于 原生javascript中的 getAttribute("foo") setAttribute("foo", "bar")

```javascript
// get a tag href
var alink = $("a#user").attr("href");

// set a tag href
$("a#user").attr("href", "http://foo.com");

// set a tag multiple attributes
$("a#user").attr({
    href: "http://foo.com",
    alt: "foo description"
});

// get a custom data-id
$("button.edit").click(function() {
    var $li = $(this).closest('li');
    var id = $li.attr('data-id');
});

```

# 13.获取内容值

一般用来获取input中用户输入的值

```
$("p").click(function() {
    var htmlString = $(this).html();
    $(this).text(htmlString);
});

var value1 = $("input#username").val();
var value2 = $("input:checkbox:checked").val();
var value3 = $("input:radio[name=bar]:checked").val()
```

# 14.遍历DOM树

通过遍历DOM树，来找到想要的node节点

```javascript
// get the div previous the element
$("div#home").prev("div");

// get the div next the element
$("div#home").next("div");

// get the parent container element
$("div#home").parent();

// get only paragraphs found inside the element
$("div#home").children("p");
```

# 资源列表

- [jquery code snippet](http://blog.teamtreehouse.com/14-handy-jquery-code-snippets-for-developers)
- [key code online](http://keycode.info/)
- [key code list](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/keyCode)

# 结语

jQuery用一种非常快捷，简单的方式操作DOM元素，使工程师对页面视觉交互的控制能力大大提升。


