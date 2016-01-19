title: webpack-ppt
speaker: 陈钦辉
transition: slide3
files: /js/webpack.js,/css/webpack.css,/js/zoom.js
theme: white
usemathjax: yes

[slide class="first-slide" style="background-image:url('/img/webpack.png')"]
# webpack 介绍
## 如有理解偏差，拿砖轻拍


[slide ]
# 传统页面，我们希望资源是这样加载的
<div >
 <img class="normal-web" src="/normal.png" height="450">
</div>


[slide]
# 单页面应用，我们希望资源是这样加载的
----

<div >
 <img class="normal-web" src="/spa.png" height="700">
</div>

[slide]
# 云课堂，是那种类型？
----
    * 混合型 = 传统页面 + 单页面应用
    * 通过后端路由，不同URL对应不同.ftl模板 （传统页面 `40个独立入口` UMI统计）
    * 一个ftl模板页面，里面注册多个模块 （单页面应用，`10个独立主入口` 通过`hash实现`）

<pre>
    <code class="javascript">
    g.dispatcher._$regist(eu.umi('commonutil'),'commonutil.html');
    g.dispatcher._$regist(eu.umi('myCloudClass'),'myCloudClass/myCloudClass.html');
    g.dispatcher._$regist(eu.umi('myCloudCourse'),'myCloudClass/course/myCloudCourse.html');
    g.dispatcher._$regist(eu.umi('myCloudStoreCourse'),'myCloudClass/course/myCloudStoreCourse.html');
    // 每个eu.umi()注册对应hash模块
    // xxx.html 是一个模块，包括 html,js(动态懒加载)
    // 而css我们直接在ftl中引用所有的模块样式，但开发scss仍然模块化
    </code>
</pre>

[slide]

# 云课堂 NEJ如何打包的？
- **js**
     * `线上：` 一个文件在2个以上（包含2个）文件中出现就会合并到core文件中，仅该页面或者模块的js直接内嵌，当大于50k外链引入
     * `也支持配置:`**手动添加**合并列表，手动管理依赖也不现实，打包成一个core，其余每个文件依赖单独打包成一个文件`本地尝试失败 --`
- **css**<br>
     - 同上



[slide]
##40传统入口 + 10单页面入口 + 这样打包  = 发送了什么？

<div >
 <img class="normal-web" src="/compare.png" height="800">
</div>


[slide]
## 更糟糕？
----
### 因为我们云课堂有40个传统页面入口
###仅仅少数的是单页面应用（多个页面共享一个core.js）。
<br>
 - 页面加载速度是影响用户体验的第一因素，也影响到SEO的抓取与排名
 - `大部分用户点击大部分页面`(传统页面)，每次都要重新`解析5万行`JS，耗时5秒，
 - 并且core.js 是不缓存的，用户每次更换页面还要`请求1M`，耗时0.3秒 `会阻塞，导致延迟`
<div >
 <img class="normal-web" src="/nocache.png" >
</div>

 - 为什么不缓存？ `因为只要有两个文件使用同一个js就被打包入，那么常常更改的模块也被打包进core.js，不能给浏览器缓存机会,利用版本更新`


[slide]

# webpack 来了
- ###特点
<div style="text-align:center; color: black;font-weight:bolder">
    code split<br>
    require anything
</div>

<div >
 <img class="normal-web" src="/webpack.png" width="800">
</div>
[官网webpack](https://webpack.github.io/docs/what-is-webpack.html)

[slide]
# show me the code
<iframe data-src="http://www.baidu.com" src="about:blank;"></iframe>


