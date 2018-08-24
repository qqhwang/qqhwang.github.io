---
layout: post
title: '为网页添加带有百分比显示的返回顶部按钮'
tag: site
---
黄拼科使用H2O主题还没有几天，但是已经发现了一个小问题，那就是这个主题没有自带返回顶部按钮，这在2018年似乎有点说不过去。不过好在返回顶部按钮不是什么特别难的东西。下面为大家介绍一种带有阅读进度百分比的返回顶部按钮。

## 引入js文件

由于H2O主题内自带有相应的js文件，所以这一步就非常简单。不过，对于一般网页来说，你得先在网页中插入以下代码：

```javascript
// Author: Ray-Eldath
// refer to:
//  - https://github.com/theme-next/hexo-theme-next/blob/master/source/js/src/utils.js
class utils {
    static getContentVisibilityHeight() {
        var docHeight = $('.visible').height(),
            winHeight = $(window).height(),
            contentVisibilityHeight = (docHeight > winHeight) ? (docHeight - winHeight) : ($(document).height() -
                winHeight);
        return contentVisibilityHeight;
    }
    static isMobile() {
        return window.screen.width < 767;
    }
}
```
还有下面的代码
```javascript
< script type = "text/javascript" >
    $(document).ready(function () {
    if (utils.isMobile()) {
        $('.scroll').hide();
        return;
    }
    $(window).scroll(function () {
        let scrollValue = $(window).scrollTop();
 
        var scrollPercentRounded = Math.round((scrollValue / utils.getContentVisibilityHeight()) * 100);
        var scrollPercentMaxed = (scrollPercentRounded > 100) ? 100 : scrollPercentRounded;
 
        $('.scrollpercent').html(scrollPercentMaxed);
        scrollValue > 100 ? $('.scroll').fadeIn() : $('.scroll').fadeOut();
    });
 
    $('.scroll').click(function () {
        $('html, body').animate({
            scrollTop: 0
        }, 300);
    })
}) < /script>
```
呼~有点长，不过你可以把他们单独写到一个js文件里，这样就不会显得太长了。

## 插入按钮
完成了上面的工作后，就可以插入按钮了。下面的代码自带样式哦！（需要用到fontawesome）
```html
<style media="screen">
    .scroll {
        z-index: 10000;
        display: none;
        height: 24px;
        background: #222;
        color: #fff;
        line-height: 24px;
        text-align: center;
        position: fixed;
        right: 30px;
        bottom: 30px;
        cursor: pointer;
        font-size: 14px;
    }
</style>
<div class="scroll"> <i class="fa fa-arrow-up" style="margin-left: 4px;"></i>
 <span class="scrollpercent" style="margin-left: -2px;"></span>
 <span style="margin-right: 4px; margin-left: -4px;">%</span>
 
</div>
```
以上代码插入到任意位置都可以。这样，你的网页就有了一个能够显示阅读进度的返回顶部按钮了，是不是很棒呀
