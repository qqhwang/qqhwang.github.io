---
layout: post
title: '提示访问者升级浏览器'
tag: 2018
---
网站上明明有许多漂亮的设计，可是使用低版本浏览器的访问者却看不到，甚至还会看到错位的网站排版。这可怎么办？网上出现了不少的提供浏览器升级的网页，例如Wordpress旗下的browsehappy.com，还有热心网友为它制作了中国版：http://browsehappy.osfipin.com/，不过推荐的是一些国产浏览器。但是个人觉得，做的最好的还是[https://browser-update.org/zh/](https://browser-update.org/zh/)。

## 有什么用

Browser-update.org 工具可以礼貌而不张扬地通知访问者更新其浏览器以便使用您的网站。

## 特点

和其他工具不同，Browser-update.org的使用更为方便，自定义项目也很丰富，而且挺纯净的。你可以在它的网站上选择要提醒哪些版本的浏览器，并且可以自定义通知的样式。并且他们也为主流cms提供了插件。他们自己是这样说的：

*优势和功能*

- 礼貌而不张扬 在默认情况下，用户每天只会收到一次通知。通知区域很小，不会阻碍用户使用网站。 
 
- 少量维护及保持更新 我们致力于不断调整及改进检测代码，避免向用户发出错误通知。我们向用户呈现及时更新的可供其系统使用的浏览器列表。 
 
- 可自定义 您可以对消息样式、文本及其他选项进行自定义操作。 
 
- 本地化 通知消息可自动以用户所使用的语言显示。

怎么样，挺强大的吧。

## 示例代码

```javascript
<script> 
var $buoop = {required:{e:-4,f:-3,o:-3,s:-1,c:-3},insecure:true,api:2018.08 }; 
function $buo_f(){ 
 var e = document.createElement("script"); 
 e.src = "//browser-update.org/update.min.js"; 
 document.body.appendChild(e);
};
try {document.addEventListener("DOMContentLoaded", $buo_f,false)}
catch(e){window.attachEvent("onload", $buo_f)}
</script>
```
您只需要在网页源代码的任意位置包含以上代码即可。

## 其他

除了显示升级通知的小工具之外，他们还提供了十分有趣的统计，结果显示有相当大一部分使用IE/Edge的用户都转战Chrome了。统计页面：[https://browser-update.org/zh/stat.html](https://browser-update.org/zh/stat.html)
