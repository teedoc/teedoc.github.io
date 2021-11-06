
---
title: 开源静态文档网站生成工具
keywords: teedoc, markdown, jupyter notebook, html, 文档生成, 替代gitbook, 网站生成, 静态网站, 文档网站生成, 多文档, 静态博客
desc: teedoc，开源静态文档网站生成工具，将 markdown 或者 jupyter notbook 转换成 html 静态网页， 用于个人或企业建站，建博客，建资料库，wiki等
id: home_page
---

<div>
    <!-- <script src="/static/js/scrolloverflow.min.js"></script> -->
    <script src="/static/js/jquery.fullpage.min.js"></script>
    <link rel="stylesheet" href="/static/css/jquery.fullpage.min.css" type="text/css"/>
</div>

<div id="fullpage">
    <div class="section" style="height: 100vh;">
        <div>
            <h1><span>teedoc</span></h1>
            <h3>简而全的开源静态网站/文档/博客生成器</h3>
        </div>
        <div class="big_btn_wrapper">
            <div class="big_btn">
                <a href="#" id="learn_more">了解更多</a>
            </div>
            <div class="big_btn">
                <a href="/get_started/zh/">开始使用</a>
            </div>
        </div>
    </div>
    <div class="section dsc_wrapper_left">
        <div>
            <div class="dsc_left">
                <h2>简单书写</h2>
                <h2>自动生成</h2>
            </div>
            <div class="dsc_right">
                使用 <img class="logo" src="/static/image/markdown.svg"> Markdown 或者 <img class="logo" src="/static/image/jupyter.svg">jupyter notebook 书写，</br>
                自动生成 HTML 页面
            </div>
        </div>
    </div>
    <div class="section dsc_wrapper_right">
        <div>
            <div class="dsc_left">
                <img class="logo" src="/static/image/windows.svg"> <img class="logo" src="/static/image/linux.svg"> <img class="logo" src="/static/image/mac.svg"><br/>
                使用 python 编写， 跨平台， 使用 pip 即可安装，</br>
                <img class="logo" src="/static/image/plugin.svg"> 支持插件安装及自定义插件
            </div>
            <div class="dsc_right">
                <h2>跨平台</h2>
                <h2>插件支持</h2>
            </div>
        </div>
    </div>
    <div class="section dsc_wrapper_left">
        <div>
            <div class="dsc_left">
                <h2>多文档</h2>
                <h2>博客</h2>
            </div>
            <div class="dsc_right">
                <img class="logo" src="/static/image/books.svg"> 支持多文档，每篇文档有单独的目录，也可以多版本。你的大量文档再也不用散落在各个域名下了！</br>
                <img class="logo" src="/static/image/blog.svg"> 同时支持轻量博客系统
            </div>
        </div>
    </div>
    <div class="section dsc_wrapper_right">
        <div>
            <div class="dsc_left">
                <img class="logo" src="/static/image/theme.svg"> 默认主题为高效阅读而设计，提供主题插件支持</br>
                同时支持自定义 css、js，能通过页面指定 id 精确控制任何一个页面元素的样式
            </div>
            <div class="dsc_right">
                <h2>主题可定制</h2>
            </div>
        </div>
    </div>
        <div class="section dsc_wrapper_left">
        <div>
            <div class="dsc_left">
                <h2>快速构建</h2>
                <h2>实时预览</h2>
            </div>
            <div class="dsc_right">
                <img class="logo" src="/static/image/speed.svg"> 并行构建，充分利用处理器性能，文档渲染就在一瞬间</br>
                <img class="logo" src="/static/image/browser.svg"> 支持浏览器实时预览修改
            </div>
        </div>
    </div>
    <div class="section dsc_wrapper_right">
        <div>
            <div class="dsc_left">
                <img class="logo" src="/static/image/server.svg"> 生成的都是静态页面，拷贝到服务器即可完成部署</br>
                <img class="logo" src="/static/image/seo.svg"> SEO 友好，比如 页关键词自定义、sitemap自动生成等
            </div>
            <div class="dsc_right">
                <h2>部署简单</h2>
                <h2>SEO 友好</h2>
            </div>
        </div>
    </div>
    <div class="section" style="height: 100vh;">
        <div class="big_btn_wrapper">
            <div class="big_btn">
                <a href="/get_started/zh/">查看详细介绍</a>
            </div>
            <div class="big_btn">
                <a href="/get_started/zh/usage/quick_start.html">快速上手</a>
            </div>
        </div>
    </div>
</div>

<div>
<script type='text/javascript'>
    $(document).ready(function () {
        var html = $("#page_footer").html();
        $("#page_footer").remove();
        $("#fullpage").append('<div id="page_footer" class="section fp-auto-height">' + html + "</div>");
        var nav_height = $("#navbar").height();
        $('#fullpage').fullpage({
            menu: '#navbar',
            navigation: true,
            css3: true,
            // dragAndMove: true,
            paddingBottom: nav_height + "px"
            // scrollOverflow: true,
	        // scrollOverflowReset: true,
            // fixedElements: "#navbar"
        });
        $("#learn_more").on("click", function(){
            $.fn.fullpage.moveTo(2);
        });
        $("#to_top").on("click", function(){
            $.fn.fullpage.moveTo(1);
        });
    });
</script>
</div>

<canvas id="backgroundCanvas"  style="top:0; bottom:0; left:0; right:0; position:fixed; z-index: -99;">
</canvas>
<script>
    var isDark = false;
    function createCanvas(dark = null){
        var c=document.getElementById("backgroundCanvas");
        c.height = document.body.clientHeight;
        c.width = document.body.clientWidth;
        var ctx=c.getContext("2d");
        if(dark == null){
            if(getTheme() == "dark"){
                dark = true;
            }else{
                dark = false;
            }
        }
        if(dark){
            ctx.fillStyle="#171717";
            isDark = true;
        }else{
            ctx.fillStyle="#f6f6f6";
            isDark = false;
        }
        var rect = [
    [0.05, 0.3, 0.05, 0.03],
    [0.1, 0.6, 0.05, 0.03],
    [0.12, 0.4, 0.05, 0.13],
    [0.22, 0.35, 0.13, 0.12],
    [0.05, 0.8, 0.1, 0.1],
    [0.18, 0.7, 0.16, 0.14],
    [0.95, 0.2, 0.05, 0.03],
    [0.9, 0.6, 0.05, 0.03],
    [0.7, 0.5, 0.05, 0.13],
    [0.78, 0.35, 0.13, 0.12],
    [0.8, 0.8, 0.16, 0.14],
    [0.6, 0.7, 0.1, 0.24],
    ];
        rect.forEach(function(v, index, array) {
            ctx.fillRect(v[0] * c.width, v[1] * c.height, v[2] * c.width, v[3] * c.height);
        });
    }
    $(window).resize(function() {
        createCanvas();
    });
    $("#themes").on("click", function(){
        createCanvas(!isDark);
    });
    $().ready(function(){
            createCanvas();
        });
</script>
