---
title: open source static doc site generator
keywords: teedoc, markdown, jupyter notebook, html, document generation, alternative gitbook, website generation, static website, document website generation, multiple documents
desc: teedoc, open source static doc site generator, convert markdown or jupyter notbook into html static webpage
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
            <h3>Concise and comprehensive website/document generator</h3>
        </div>
        <div id="big_btn_wrapper">
            <div class="big_btn">
                <a href="/get_started/en/">Get started</a>
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
