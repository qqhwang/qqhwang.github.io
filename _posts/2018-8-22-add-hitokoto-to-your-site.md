---
layout: post
title: '使用一言为你的网站底部添加一句随机的话'
tag: 2018
---

## 一言简介

一言，正如其名，就是一句话。但是这样简单的一句话有时也会让你心生感触。而网络上出现的一言api，可以让你在网页上添加随机的一句话，有时看起来也是挺...赏心悦目的。下面来看看如何在网页上添加这样的一句话吧。

## 在哪里插入代码

一般来说，大多数博客都将一言放在底部。对于Wordpress，这应该是在footer.php文件里，对于其他大多数博客程序，网页底部代码都应该出现在类似于footer这样的名称的文件里。例如对于本站使用的jekyll，就应该在_includes/footer.html里面添加代码。

## 使用Hitokoto.cn的api接口

Hitokoto.cn的简介：

> 一言网(Hitokoto.cn)创立于2016年，隶属于萌创Team，目前网站主要提供一句话服务。 

> 动漫也好、小说也好、网络也好，不论在哪里，我们总会看到有那么一两个句子能穿透你的心。我们把这些句子汇聚起来，形成一言网络，以传递更多的感动。如果可以，我们希望我们没有停止服务的那一天。

> 简单来说，一言指的就是一句话，可以是动漫中的台词，也可以是网络上的各种小段子。

> 或是感动，或是开心，有或是单纯的回忆。来到这里，留下你所喜欢的那一句句话，与大家分享，这就是一言存在的目的。

Hitokoto.cn提供了简单易用的api接口，用起来也是十分方便。只需要在需要出现一言的地方添加以下代码就可以了：

```HTML
<p id="hitokoto">:D 获取中...</p>
<!-- 兼容低版本浏览器 (包括 IE)，可移除 -->
<script src="https://cdn.jsdelivr.net/npm/bluebird@3/js/browser/bluebird.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/whatwg-fetch@2.0.3/fetch.min.js"></script>
<!--End-->
<script>
  fetch('https://v1.hitokoto.cn')
    .then(function (res){
      return res.json();
    })
    .then(function (data) {
      var hitokoto = document.getElementById('hitokoto');
      hitokoto.innerText = data.hitokoto; 
    })
    .catch(function (err) {
      console.error(err);
    })
</script>
```

或者可以使用下面这个更简洁的版本，不过对老式的浏览器可能兼容性更差：

```HTML
<p id="hitokoto">:D 获取中...</p>
<script src="https://v1.hitokoto.cn/?encode=js&select=%23hitokoto" defer></script>
```

具体效果可以看本站底部~

## 使用lwl12.com提供的api

除了Hitokoto.cn，你还可以选择lwl12提供的api（句子有所不同）。
lwl12的介绍：

> 相信大家都有或曾经有过自己的摘抄本。「一言」就好似一个公开的摘抄本，我们在此记录那些让人感怀的，让人振奋的，让人感动的，让人一眼就有所感触的短句，并通过公共 API 的形式使你能够在自己的项目中调用它们。我们希望通过一言，链接大家对各方文字的美好记忆，伴你踩碎迷茫，走过时光。

下面是lwl12的api：

```HTML
<script type="text/javascript" src="https://api.lwl12.com/hitokoto/main/get?encode=js&charset=utf-8"></script><div id="lwlhitokoto"><script>lwlhitokoto()</script></div>
```

另外lwl12还有Android app：[一言](https://www.coolapk.com/apk/com.hitokoto)

具体要使用谁的api就看你的喜好了。

## 更高级的用法

请看他们的文档说明：
- [https://hitokoto.cn/api](https://hitokoto.cn/api)
- [https://blog.lwl12.com/read/hitokoto-api.html](https://blog.lwl12.com/read/hitokoto-api.html)

## 构建自己的api

如果你不想使用他们的api，你也可以用张戈博客提供的代码在自己的网站上构建api：

```php
<?php
//获取句子文件的绝对路径
//如果你介意别人可能会拖走这个文本，可以把文件名自定义一下，或者通过Nginx禁止拉取也行。
$path = dirname(__FILE__);
$file = file($path."/hitokoto.txt");
 
//随机读取一行
$arr  = mt_rand( 0, count( $file ) - 1 );
$content  = trim($file[$arr]);
 
//编码判断，用于输出相应的响应头部编码
if (isset($_GET['charset']) && !empty($_GET['charset'])) {
    $charset = $_GET['charset'];
    if (strcasecmp($charset,"gbk") == 0 ) {
        $content = mb_convert_encoding($content,'gbk', 'utf-8');
    }
} else {
    $charset = 'utf-8';
}
header("Content-Type: text/html; charset=$charset");
 
//格式化判断，输出js或纯文本
if ($_GET['format'] === 'js') {
    echo "function hitokoto(){document.write('" . $content ."');}";
} else {
    echo $content;
}
```

以上代码保存为 index.php，然后上传到网站根目录下的 hitokoto 文件夹（这个自己随意定义）最后，创建 hitokoto.txt 文本文件（建议一行一句话）上传至相同目录。现在，浏览器访问 http://你的域名/hitokoto/ 就可以看到输出内容了。

调用方法:

```HTML
<script type="text/javascript" src="https://你的域名/hitokoto/?format=js&charset=utf-8"></script>
<div id="hitokoto"><script>hitokoto()</script></div>
```

来源：
- [https://zhangge.net/5127.html](https://zhangge.net/5127.html)
- [https://xiaolin.in/read/hitokoto-api-xiaolin-edition.html](https://xiaolin.in/read/hitokoto-api-xiaolin-edition.html)

最后更新于2018-8-23
