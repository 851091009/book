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


CNZZ: 统计插件 https://github.com/mipengine/mip-extensions/tree/master/src/mip-stats-cnzz  
官方问题解决大全：https://github.com/mipengine/mip/wiki/MIP-%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88%E5%A4%A7%E5%85%A8  
