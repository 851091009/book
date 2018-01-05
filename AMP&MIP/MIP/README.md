# MIP 学习笔记  


## 1. 什么是MIP

答：移动网页加速器，主要用于移动端网页加速
> MIP 能有优化网页JS和资源加载，达到加速打开网页的效果

MIP 主要有三部分组织成：
1. MIP HTML
2. MIP JS
3. MIP Cache

### 1. MIP HTML
> 提示：MIP HTML 基于 HTML 基础规范进行了扩展

MIP HTML 规范中有两类标签，一类是HTML 常规标签，另一类是 MIP 标签。MIP 标签也被称作 MIP HTML 组件，使用它们来代替 HTML 常规标签可以大幅度提高页面性能。

例如：**mip-img** 标签，他使得图片只在需要时才进行加载，减少页面渲染时间，节省了用户的流量

### 2.MIP JS

MIP JS 用于管理资源的加载，并支持上述 MIP 标签的使用，从而确保页面的快速渲染,提高页面各方面的性能  

MIP JS 最显著的优势是能够异步加载所有外部资源，整个页面渲染过程不会被页面中某些元素阻塞，从而实现页面渲染速度的提升。

此外，MIP JS 还涵盖了所有 iframe 的沙盒，与资源加载前提前计算页面元素布局，禁用缓慢 css 选择器等技术性能


## 2.<link rel="canonical" href="" /> 是什么意思？ 

**canonical 标签用于关联原页面和 MIP 页，保证 MIP 页继承原页面权重，在移动搜索时优先展示MIP页面。**
canonical 标签是 MIP 页连接外界的重要桥梁，不写或写错导致 MIP 页不能和原页面产生联系，导致权重丢失，MIP页不展现。

解决网站内容存在多个版本时,指定规范链接,解决重复收录问题！
canonical 告诉搜索引擎哪个页面是权威页面，总结一下，canonical 大致作用如下：  
    第一：使用 canonical 标签使网页标签标准化。
    第二：避免内容重复页面，搜索引擎收录更准确。
    第三：集中传递页面权重。

## 3.MIP 加速原理    

1.精心设计的 JavaScript  
2.所有静态资源需要标注尺寸  
3.不允许任何机制阻止页面渲染  
4.控制外部资源加载  
5.封装交互功能  
6.建议使用 inline 的 css  
7.只允许 GPU 加速的动画  
8.MIP 缓存  


### 3.1精心设计的 JavaScript  
为了去除臃肿的客户端脚本，MIP 文件不允许自定义 JavaScript;对一些强依赖 JavaScript 的功能 (如：广告、统计、交互)，MIP 提供与 MIP runtime 兼容的封装好的组件来实现。

JavaScript 引用原则：  
* 目前不允许用户自定义 JavaScript ,需要用 MIP 组件的形式引进来，从而确保安全性和性能保障
* 可以引入 mip-iframe 来引入部分富交互的功能，这样，开发时即使使用最影响性能的 **document.write**,也不会影响主页面渲染
* MIP 组件式开源的，允许开发者自定义组件

> 总结：MIP JavaScript 使用需要自定义组件的形式扩展，这样可以保证安全性和性能保障，不允许用户在页面中直接使用，如果需要 较多的 JavaScript 交互，使用 mip-iframe 来扩展，这不会影响到主页面的加载  
> tag: 去除臃肿的客户端脚本 、MIP runtime 组件、引用原则3、引入组件、富交互，mip-iframe、自定义组件

### 3.2 所有静态资源需要标注尺寸  
在页面开发的时候，资源尝尝不会被设定宽和高，特别是使用广告或者通过 **document.write();** 注入的时候。由于资源大小的不确定，页面经常要进行反复重新绘制。  
现在，MIP 要求所有的资源(广告、图片、音频和视频) 标明尺寸，当资源真正加载时，所有资源大小可以被立即推断出并迅速用户计算页面布局，加载中的资源将无缝呈现，不必因为页面频繁更新布局而影响到用户的阅读体验。

> 总计：MIP 要求所有的资源文件设定宽和高，这样能够快速的计算出页面的布局，从而提高用户的阅读体验。  
> tag: 资源尺寸、无缝呈现、阅读体验

### 3.3 不允许页面任何机制阻止页面渲染  
开发者任何自定义脚本，都需要通过 MIP 的 **tag** 反馈给 MIP，例如：mip-ad , mip-iframe 的，这些方式不会阻塞页面的 layout 和渲染。

> 总结：自定义脚本需要通过 MIP 的 tag 来实现

### 3.4 控制外部资源加载  
MIP runtime 会控制外部的资源加载来确保其高效性，从而使用户阅读的内容尽快出现在屏幕中。

> 总结：MIP 通过控制资源的加载，来确保数据的按需加载，尽快让用户阅读的内容出现在屏幕中

tag: MIP Runtime 控制外部资源、高效性、用户、屏幕中

### 3.5 封装交互功能
MIP 提倡网页给用户直接简单的体验，但这并不意味着MIP 限制了页面的生动和有趣，MIP runtime 提供了高度化的被封装的 JavaScript ,开发者无需投入过多精力去实现复杂的交互功能。

### 3.6 建议使用 inline 的 css
css 的加载，会阻止页面渲染，css 内联可以减少客户端开销

### 3.7 只允许 GPU 加速动画
MIP 只允许用 transforms 和 opacity 来完成动画效果，当动画在 GPU 上执行时，仅触发渲染层合并

### 3.8 MIP 缓存
MIP 另一个重要的意义在于能够帮助站长加速网页，百度将会把 MIP 网页缓存到报读 CDN 中。只要符合 MIP 标准，都可以使用 MIP 缓存


## 4.内容声明



## 5.MIP 的开发规范？

1.MIP HTML 规范
2.MIP 效验规则
3.MIP Cache 规范
4.组件开发规范
5.Canonical 使用规范

## 6.如果让百度 发现 网站的 MIP 页面？



## 7.MIP-Cache 生效流程？


## 8.MIP 禁用的标签有哪些？


提问题的页面：https://www.mipengine.org/doc/2-tech/1-mip-html.html

## 9.MIP 页面编码要求？


提问题的页面：https://www.mipengine.org/doc/2-tech/3-mip-cache.html

## 10.MIP Cache 更新机制

提问题的页面：https://www.mipengine.org/doc/2-tech/3-mip-cache.html

## 11.自定义样式使用规范

提问题的页面：https://www.mipengine.org/doc/2-tech/1-mip-html.html

## 12.MIP页面的开发流程


提问题页面：https://www.mipengine.org/doc/00-mip-101.html

## 13.MIP HTML 规范


## 14.MIP 校验规则

提问题的页面：https://www.mipengine.org/doc/2-tech/2-validate-mip.html

## 15.MIP Cache 规范


提问题的页面：https://www.mipengine.org/doc/2-tech/3-mip-cache.html

## 16.Canonical 使用规范

提问题的页面：https://www.mipengine.org/doc/2-tech/5-show-your-page.html


## 39.MIP是什么，有哪 3 块组成的？

## 40.组件布局支持的布局种类？

## 41.MIP 的内置组件有哪些？

> 以下是 官方博客的问答 http://www.cnblogs.com/mipengine/p/mip-faqs.html

## 17.开发 MIP 后，搜索流量是导流到 MIP，还是导流到原页面？

## 18.MIP 对什么样的内容有较好的效果？

## 19.加了 MIP 标签后对页面的展示有没有影响？

## 20.对于自适应站点，MIP 对页面的改造要怎么弄？

## 21.MIP 规范中写到不能用 link 标签对样式表进行加载，是不是我的样式都要写到 HTML 页面里，并且只能出现一次 style 标签？

## 22.如何提交 MIP 页？MIP 推送 URL 限制是多少？

## 23.MIP 化后对其他搜索引擎抓取收录以及 SEO 的影响如何？

## 24.针对拥有 PC、WAP、MIP 三套页面的网站，如何进行移动适配工具和 MIP 工具提交？

## 25.网站如何确认改造的 MIP 页面已经在线上生效？

## 26.MIP 页面提交并收录了为什么搜索结果没有 MIP 闪电图标。

## 27.MIP 页面提交给百度收录后，为什么有些会被转码？

## 28.MIP Cache 缓存更新时间是多长时间？

## 29.搜索结果打开是百度的域名，用户分享的是否也是是百度链接？使用百度域名是否不利于网站的品牌传播同时也会影响流量统计，该如何解决

## 30.搜索结果打开是百度的域名，是否会影响广告计费？

## 31.如果采取新建一套 MIP 页面的方式，假设新建 MIP 页面出现问题，譬如改造错误、失效或者其他不可预知问题，百度的处理机制是怎样的？


## 32.站长构建一个 MIP 的成本有多少？

## 33.加了 MIP 标签后对页面的展示有没有影响？

## 34.如何提交 MIP 页？MIP 推送 URL 限制是多少？

## 35.MIP 提交几个月时间了，生效量比较少，是什么原因

## 36.为什么在搜索生效后，百度统计看不到数据

## 37.为什么要使用 MIPCache?

## 38.如果提交的网址错了，怎么删除错误的网址，另外把页面都改成404对站点排名有没有影响？





CNZZ: 统计插件 https://github.com/mipengine/mip-extensions/tree/master/src/mip-stats-cnzz  
官方问题解决大全：https://github.com/mipengine/mip/wiki/MIP-%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88%E5%A4%A7%E5%85%A8  
