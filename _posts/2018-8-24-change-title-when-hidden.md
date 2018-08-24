---
layout: post
title: '添加网页标题崩溃欺骗搞怪特效'
tag: site
---
有些网站的网页标题是会卖萌的！没有点它时就会显示(●—●)喔哟，崩溃啦！点回去后标题又恢复正常。用Javascript就可以实现这个效果了。

## 实现代码

```javascript
<script type="text/javascript">
<!--崩溃欺骗-->
 var OriginTitle = document.title;
 var titleTime;
 document.addEventListener('visibilitychange', function () {
     if (document.hidden) {
         $('[rel="icon"]').attr('href', "/img/TEP.ico");
         document.title = '(●—●)喔哟，崩溃啦！';
         clearTimeout(titleTime);
     }
     else {
         $('[rel="icon"]').attr('href', "/favicon.ico");
         document.title = '(/≧▽≦/)咦！又好了！' + OriginTitle;
         titleTime = setTimeout(function () {
             document.title = OriginTitle;
         }, 2000);
     }
 });
 </script>
 ```
 
 将此代码插入到网页里就可以了。
 
 来源：[https://asdfv1929.github.io/2018/01/25/crash-cheat/](https://asdfv1929.github.io/2018/01/25/crash-cheat/)
