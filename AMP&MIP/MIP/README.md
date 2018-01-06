# MIP 学习笔记  


## 1. 什么是MIP

答：移动网页加速器，主要用于移动端网页加速
> MIP 能有优化网页JS和资源加载，达到加速打开网页的效果

MIP 主要有三部分组织成：
1. MIP HTML
2. MIP JS
3. MIP Cache

* MIP HTML 基于 HTML 中的基础标签制定全新的规范，通过对一些基础标签的使用限制和功能扩展，使 HTML 能够展现更加丰富的内容。
* MIP JS 可以保证，MIP HTML 的页面快速渲染。
* MIP Cache 用于实现页面的快速缓存，从而进一步提高页面性能。


### 1. MIP HTML 进行什么扩展？
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
从搜索页点出的 MIP 页面，其页面上的任何内容(包括但不限于广告、在线咨询、统计等组件) 均视为原站点的投放和使用


## 5.MIP 的开发规范？

1.MIP HTML 规范
2.MIP 效验规则
3.MIP Cache 规范
4.组件开发规范
5.Canonical 使用规范

## 6.如果让百度 发现 网站的 MIP 页面？
在某些情况下，站点对同一个 html 页面，可能存在两种，一个是 mip 页，一个是原页面，搜索引擎会抓取这两个页面，并利用 canonical 标签讲她们链接起来

**关联标签：**  
必须在 MIP 添加 <link rel="canonical"> 指向原始页面，以保证 MIP 更好的继承原始页面的权重  

使用规则：  
* <link rel="miphtml"> 在 H5 页使用，指向对应内容的 MIP 页，方便搜索引擎发现对应的MIP 页
* <link rel="canonical"> 在 MIP 页中使用，指向内容对应的 H5 页面。
* 若没有 H5 页面，则指向内容对应的 PC 页。
* 若直接在原连接上修改 MIP，则 caconical 指向当前 URL 

**注：在 <head></head> 中添加**

在原页面添加关联标签，是MIP 页面继承原页面的权重，在 MIP 添加指向原页面的链接

## 7.MIP-Cache 生效流程？

在 MIP 页被爬虫抓取后，会自动对静态资源进行缓存，并且替换页面中的静态资源引用地址为缓存地址。搜索结果页会优先跳转到 MIP-Cache url,在 MIP-Cache 缓存到期进行一次回源，访问原页面 URL 并重新缓存。

总结：MIP-CACHE 对页面的静态资源缓存，MIP-CACHE 会定时进行回源，并重新缓存

## 8.MIP 禁用的标签有哪些？

1. img
2. video
3. iframe
4. audio
5. script
6. frame
7. object

提问题的页面：https://www.mipengine.org/doc/2-tech/1-mip-html.html

## 9.MIP 页面编码要求？

必须是 UTF-8

提问题的页面：https://www.mipengine.org/doc/2-tech/3-mip-cache.html

## 10.MIP Cache 更新机制

有三种更新的机制：  
1.常规更新机制
2.快速更新机制
3.页面删除

### 10.1 常规更新机制
* 页面的缓存时间为 52 分钟-5天 (由该页面用户点击量和站点本身稳定性决定)
* 图片缓存时间为 10 天
* MIP-JS 组件文件的缓存时间为 10 分钟

在当前文件过期后，MIP-Cache 会重新抓取资源。如果是HTML 页面，MIP-Cache 还会对页面文件进行 MIP 规范检验。如果此时页面内容不再符合 MIP 规范，MIP-Cache 就不再缓存这个页面了，这样，所有符合 MIP-Cache 中的页面都是最新的，并且符合 MIP 规范

### 10.2 快速更新机制
当线上页面出现 BUG 紧急修复，发现网页有黄反需要紧急更新和删除的内容时， MIP-Cache 也开放了单独的清理接口，阅读 MIP-Cache 了解更过信息，生效时间大概 5 分钟。

### 10.3 MIP-Cache 页面删除
如果有一些废弃页面需要删除：  

* 站长首先删除本站原页面
* 调用 MIP-Cache 快速更新机制删除 Cache
* 删除后，请给 MIP-Cache 非 200 (404或者其他) 状态码，防止 cache 中缓存错误页


提问题的页面：https://www.mipengine.org/doc/2-tech/3-mip-cache.html

## 11.自定义样式使用规范

处于性能考虑，html 中不允许使用内联 style ,所有样式只能放到 head 的 style 标签里


提问题的页面：https://www.mipengine.org/doc/2-tech/1-mip-html.html

## 12.MIP页面的开发流程

### 1.创建 HTML 页面
 需要注意的是：  
 * 在 <html> 标签中增加 mip 标识，例：<html mip>
 * 在 <hade> 编码为 utf-8
 * 添加 meta-vierport,用于移动端展现

````html
<!DOCTYPE html>
<html mip>
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width,minimum-scale=1,initial-scale=1">
        <title>Hello World</title>
    </head>
    <body>
        <h1>Hello World!</h1>
    </body>
</html>
````
### 2.添加 MIP 运行环境

在 HTML 代码中，添加 MIP 依赖的 mip.js 和 mip.css 

````html
<!DOCTYPE html>
<html mip>
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width,minimum-scale=1,initial-scale=1">
        <link rel="stylesheet" type="text/css" href="https://c.mipcdn.com/static/v1/mip.css">
        <title>Hello World</title>
    </head>
    <body>
        <h1>Hello World!</h1>
        <script src="https://c.mipcdn.com/static/v1/mip.js"></script>
    </body>
</html>
````

### 3. 添加 MIP 关联标签
<link rel="miphtml"> 和 <link rel="canonical"> 主要用于告诉搜索引擎页面间的关系。添加关联标签后，MIP 页会继承 原页面(移动端)的点击权重，同时 MIP 将作为搜索引擎首选的导流页面

提问题页面：https://www.mipengine.org/doc/00-mip-101.html

## 13.MIP HTML 规范
1.头部使用规范  
2.页面元素规范  
3.HTML属性  
4.自定义样式使用规范  
5.验证规范  

### 13.1 头部使用规范
1. 起始标签使用 <doctype html>  
2. html 标签必须加上 mip  
3. 必须包含head 和 body 标签
4. 必须在 head 中必须包含字符集声明， <meta charset="utf-8"> ,字符集统一为 **utf-8**
5. 必须在 head 标签中包含 viewport 设置标签：<meta name="viewprot" content="width=device-width,initial-scale=1"> , 推荐包含 minimum-scale=1
6. 必须在 head 标签中包含 < link rel="stylesheet" type="text/css" href="https://c.mipcdn.com/static/v1/mip.css" >
7. 必须在 head 标签中包含 <link rel="canonical" href="http(s)://xxx" >
8. 需要在 body 标签中包含 <script src="https://c.mipcdn.com/static/v1/mip.js" ></script> ，如果在 head 标签中 则需加async 属性；

### 13.2 页面元素规范

### 13.3 HTML属性
* MIP HTML 中所有 on 开发的属性都不允许使用，如：onclick, onmouseover、
* MIP HTML 中允许使用 on 属性
* MIP HTML 中不允许使用 style 属性

### 13.4 自定义样式使用规范  

处于性能考虑，html 中不允许使用内链 style，所有样式只能放到 head 的 style 标签里

### 13.5 验证规范

所写的代码需经过 MIP 验证规则验证



## 14.MIP 校验规则

主要验证属性和标签错误  

1. 缺少强制标签  
2. 禁用标签
3. 无效属性值
4. 属性值无效值
5. 缺少强制属性
6. 直接父标签错误
7. 非法父级标签
8. 强制父级标签
9. 唯一标签重复



提问题的页面：https://www.mipengine.org/doc/2-tech/2-validate-mip.html

## 15.MIP Cache 规范

在开发页面时，无需对 MIP-Cache 进行额外关注，只要保证 MIP 页面，图片等资源是允许 MIP cache 的 UA (baidumip,baidumib) 抓取即可



提问题的页面：https://www.mipengine.org/doc/2-tech/3-mip-cache.html

## 16.Canonical 使用规范

在某些情况下，站点对同一个 html 页面，可能存在两种，一个是 MIP 页，一个是 原页面。搜索殷勤会抓取这两个页面，并利用 canonical 标签联系起来




提问题的页面：https://www.mipengine.org/doc/2-tech/5-show-your-page.html


## 39.MIP是什么，有哪 3 块组成的？

MIP 移动网页加速器，分别包含 MIP-HTML，MIP-JS， MIP-CAHCE

## 40.组件布局支持的布局种类？

有 6 种：
|类型|备注|
|------------|------|
|responsive  |能够根据width、height的值，算出元素对应的比例，在不同屏幕宽度上做自适应，非常适合图片、视频等需要大小自适应的组件|
|fixed-height|

提问题的页面：https://www.mipengine.org/doc/3-widget/11-widget-layout.html
## 41.MIP 的内置组件有哪些？

> 以下是 官方博客的问答 http://www.cnblogs.com/mipengine/p/mip-faqs.html

## 17.开发 MIP 后，搜索流量是导流到 MIP，还是导流到原页面？

应该导流到 MIP 页，需要在 MIP 页面中做好原页面对应关系


## 18.MIP 对什么样的内容有较好的效果？

答：对内容浏览类的站点支持比较全面


## 19.加了 MIP 标签后对页面的展示有没有影响？

没有影响。MIP 标签会自带一些样式，但这些样式是没有被覆盖的。所以加了 MIP 标签后网页还是可以保持原来的样式。


## 20.对于自适应站点，MIP 对页面的改造要怎么弄？

MIP 改造主要有三步：  
    1.引用MIP提供的 JS 和 CSS   
    2.替换部分标签为 mip-html 标签  
    3.去除原来的 JS 逻辑，改引用 mip-js 组件  

是否需要特殊改造，由网站对 “响应式” 的实现方式决定：  
    * 如果您的自适应站点是用 CSS 的 Media Query 来实现的，那么不需要做特殊的改造，MIP 对 CSS 没有限制。
    * 如果您的自适应是通过 JS 计算实现的，则需要进行相应的改造（见改造第 3 步）。 如果您需要的功能不在我们的组件列表里面，



## 21.MIP 规范中写到不能用 link 标签对样式表进行加载，是不是我的样式都要写到 HTML 页面里，并且只能出现一次 style 标签？

MIP 规范不建议外链样式表，是为了节省网络加载时间，加速 MIP 页面。 开发时，样式表可以作为单独的文件维护，上线前通过一次编译（fis3，grunt，gulp）实现文件内联，将 css 作为<style>标签插入<html>, 达到 MIP 要求。


## 22.如何提交 MIP 页？MIP 推送 URL 限制是多少？

博客《百度站长平台 MIP 引入工具》中有提交 MIP 页的详细教程。提交上限根据站点而不同，最少的上限是每天 10000 条 URL，上不封顶。

## 23.MIP 化后对其他搜索引擎抓取收录以及 SEO 的影响如何？

在原页面 MIP 化，不会影响其它搜索引擎的抓取收录，也不会影响页面权重。新增 MIP 页可通过 robots.txt 文件禁用其它搜索引擎的抓取，从而保证原页面的权重。MIP相关的内容可以这么写(假设您的目录是/mip/):

    User-agent: Baiduspider
    （这里不用写关于mip的内容）

    User-agent: Googlebot
    Disallow: /mip/



## 24.针对拥有 PC、WAP、MIP 三套页面的网站，如何进行移动适配工具和 MIP 工具提交？

MIP 页面可单独通过百度站长平台的 MIP引入提交，不会影响 PC 和 WAP

## 25.网站如何确认改造的 MIP 页面已经在线上生效？

MIP 页面在百度搜索结果页异步打开，只需要在百度搜索结果中搜索链接。并且打开后是 百度的链接

## 26.MIP 页面提交并收录了为什么搜索结果没有 MIP 闪电图标。

这种情况大多数是因为 MIP 页面乱码或者页面源码不能通过 校验 导致。

## 27.MIP 页面提交给百度收录后，为什么有些会被转码？

由于广告不符合规范。MIP 页面和普通页面性质相同。都是可以访问的 html 页面。百度搜索会将质量较低的页面转码，无论是否是 MIP 页面都会被转码。如果页面被转码 请参考《百度移动搜索落地页体验白皮书——广告篇》 修改页面内广告的投放。

## 28.MIP Cache 缓存更新时间是多长时间？

目前在 50 分钟


## 29.搜索结果打开是百度的域名，用户分享的是否也是是百度链接？使用百度域名是否不利于网站的品牌传播同时也会影响流量统计，该如何解决

如果用户通过分享组件，则分享的标题，图片和内容都是原页面内容，具体分享内容可以在使用组件时定义，这种做法不会影响品牌传播，也不会影响流量统计。如果用户直接从浏览器复制链接分享，那么分享的是当前页面 URL，URL 中是能够反解出原页面 URL 的。统计代码在页面加载完成后都会生效，不会受到分享的影响

## 30.搜索结果打开是百度的域名，是否会影响广告计费？

从搜索结果也点出的 MIP 页面，其页面上的任何内容均视为原站点上投放和使用

## 31.如果采取新建一套 MIP 页面的方式，假设新建 MIP 页面出现问题，譬如改造错误、失效或者其他不可预知问题，百度的处理机制是怎样的？

MIP 室友回退机制的，MIP 访问出现问题后，会直接回到原来的 H5 页面，不会影响权重， MIP 会被更认可和优先

## 32.站长构建一个 MIP 的成本有多少？

MIP HTML 是基于 HTML 进行优化。对新建的站点，按照 MIP 规范开发即可，没有额外的成本。如果是已有的页面，需要具体看页面生成方式，模板形式改造一次性成本


## 33.加了 MIP 标签后对页面的展示有没有影响？

没有影响。MIP 标签会自带一些样式，但这些样式都是可以被覆盖的，所以加了 MIP 标签后页面还可以保持原来的样子

## 34.如何提交 MIP 页？MIP 推送 URL 限制是多少？

通过百度的站长平台，上限根据站点的不同会不一样，最少的上限是 10000 条，上不封顶

## 35.MIP 提交几个月时间了，生效量比较少，是什么原因

提交 MIP 页面后，经过收录、校验、和生效三个步骤，才能在结果页看到闪电标

收录较少的原因也会根据站点的不同，请先确认页面是否被收录并且能通多 MIP 校验

## 36.为什么在搜索生效后，百度统计看不到数据


## 37.为什么要使用 MIPCache?



## 38.如果提交的网址错了，怎么删除错误的网址，另外把页面都改成404对站点排名有没有影响？

可以使用 MIP-CACHE 的更新接口，删除错误网址，如果还有对应的H5网页的话，对排名没有影响


CNZZ: 统计插件 https://github.com/mipengine/mip-extensions/tree/master/src/mip-stats-cnzz  
官方问题解决大全：https://github.com/mipengine/mip/wiki/MIP-%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88%E5%A4%A7%E5%85%A8  
