---
layout: post
title: 会念诗的api
tag: site
---

# 念诗？没有问题

很多博主喜欢在博客底部添加[一言api](/2018/08/22/add-hitokoto-to-your-site.html)，从而使博客的底部每次都会显示一句不同的话。不过，除了普通的句子之外，你的博客现在还可以念诗了。“[今日诗词](https://www.jinrishici.com/)”提供了免费的api接口，让你能够快速让自己的博客学会念诗。

# 不只是会念诗

除了会念诗，这个api最大的特点是能够根据不同地点、时间、节日、季节、天气、景观、城市进行智能推荐。比如在晚上，它可能会推荐“灯火已收正月半，山南山北花撩乱”；在成都，它可能会推荐“ 凭寄狂夫书一纸，信在成都万里桥”。相比一般的一言api，它已经优秀很多了。值得一提的是，在多种平台上，你都可以进行调用。

# 通用简单安装代码

```HTML
<span id="jinrishici-sentence">正在加载今日诗词....</span>
<script src="https://sdk.jinrishici.com/v2/browser/jinrishici.js" charset="utf-8"></script>
```
SDK 会自动寻找 id 或者 class 为 jinrishici-sentence 的标签，将里面的内容替换为诗词。如果需要在多个地方显示诗词，添加多个 class=" jinrishici-sentence " 的 span 即可，像这样：

```HTML
<span id="jinrishici-sentence"></span>
```

更多信息请移步[https://www.jinrishici.com/doc/](https://www.jinrishici.com/doc/)

# 展示

![今日诗词](https://v2.jinrishici.com/one.svg)
刷新会变哟！

_最后更新于2019年2月11日_
