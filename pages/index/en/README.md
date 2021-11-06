---
title: open source static doc site generator
keywords: teedoc, markdown, jupyter notebook, html, document generation, alternative gitbook, website generation, static website, document website generation, multiple documents
desc: teedoc, open source static doc site generator, convert markdown or jupyter notbook into html static webpage
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
            <h3>Concise and comprehensive website/document generator</h3>
        </div>
        <div class="big_btn_wrapper">
            <div class="big_btn">
                <a href="#" id="learn_more">Learn More</a>
            </div>
            <div class="big_btn">
                <a href="/get_started/en/">Get started</a>
            </div>
        </div>
    </div>
    <div class="section dsc_wrapper_left">
        <div>
            <div class="dsc_left">
                <h2>Write easily</h2>
                <h2>Automatically generate</h2>
            </div>
            <div class="dsc_right">
                Write with <img class="logo" src="/static/image/markdown.svg"> Markdown Or <img class="logo" src="/static/image/jupyter.svg">jupyter notebook ,</br>
                automatically generate HTML pages
            </div>
        </div>
    </div>
    <div class="section dsc_wrapper_right">
        <div>
            <div class="dsc_left">
                <img class="logo" src="/static/image/windows.svg"> <img class="logo" src="/static/image/linux.svg"> <img class="logo" src="/static/image/mac.svg"><br/>
                Code by python, cross platform, install just by pip,</br>
                <img class="logo" src="/static/image/plugin.svg"> support plugins
            </div>
            <div class="dsc_right">
                <h2>Cross platform</h2>
                <h2>Plugins support</h2>
            </div>
        </div>
    </div>
    <div class="section dsc_wrapper_left">
        <div>
            <div class="dsc_left">
                <h2>Multiple docs</h2>
                <h2>Blog</h2>
            </div>
            <div class="dsc_right">
                <img class="logo" src="/static/image/books.svg"> Supoort multiple docs/version, each has its content(sidebar). Say bye bye to large number of domains for your docs</br>
                <img class="logo" src="/static/image/blog.svg"> Support lite blog system
            </div>
        </div>
    </div>
    <div class="section dsc_wrapper_right">
        <div>
            <div class="dsc_left">
                <img class="logo" src="/static/image/theme.svg"> Default theme for efficient read, and you can install theme plugin</br>
                At the same time, it supports custom css and js, and can precisely control the style of any page element by specifying the id on the page
            </div>
            <div class="dsc_right">
                <h2>Theme can be customized</h2>
            </div>
        </div>
    </div>
        <div class="section dsc_wrapper_left">
        <div>
            <div class="dsc_left">
                <h2>Faster generate</h2>
                <h2>Real-time preview</h2>
            </div>
            <div class="dsc_right">
                <img class="logo" src="/static/image/speed.svg"> Build in parallel, make full use of processor performance, and document rendering in an instant</br>
                <img class="logo" src="/static/image/browser.svg"> Support browser real-time preview and modification
            </div>
        </div>
    </div>
    <div class="section dsc_wrapper_right">
        <div>
            <div class="dsc_left">
                <img class="logo" src="/static/image/server.svg"> All generated static pages, copy to the server to complete the deployment</br>
                <img class="logo" src="/static/image/seo.svg"> SEO friendly, such as page keywords customization, sitemap automatic generation, etc.
            </div>
            <div class="dsc_right">
                <h2>Simple to deploy</h2>
                <h2>SEO friendly</h2>
            </div>
        </div>
    </div>
    <div class="section" style="height: 100vh;">
        <div class="big_btn_wrapper">
            <div class="big_btn">
                <a href="/get_started/zh/">More Detail</a>
            </div>
            <div class="big_btn">
                <a href="/get_started/zh/usage/quick_start.html">Quick Start</a>
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

