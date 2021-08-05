
---
title: 开源静态文档网站生成工具
keywords: teedoc, markdown, jupyter notebook, html, 文档生成, 替代gitbook, 网站生成, 静态网站, 文档网站生成, 多文档, 静态博客
desc: teedoc，开源静态文档网站生成工具，将 markdown 或者 jupyter notbook 转换成 html 静态网页， 用于个人或企业建站，建博客，建资料库，wiki等
id: home_page
---

<div>
    <script src="/static/js/jquery.fullpage.min.js"></script>
    <link rel="stylesheet" href="/static/css/jquery.fullpage.min.css" type="text/css"/>
</div>

<div id="fullpage">
    <div class="section">
        <div>
            <h1><span>teedoc</span></h1>
            <h3>简而全的开源静态网站/文档/博客生成器</h3>
        </div>
        <div id="big_btn_wrapper">
            <div class="big_btn">
                <a href="/get_started/zh/">开始使用</a>
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
            dragAndMove: true,
            paddingBottom: nav_height + "px"
            // fixedElements: "#navbar"
        });
    });
</script>
</div>
