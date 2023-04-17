---
title: "Django"
---
from [[technology]]

<h1>Section.</h1>

<div id="postings"></div>

<script>
    function add_posting(url, image, title, des)
    {
        let main = document.getElementById('postings');

        let obj = document.createElement('a');
        obj.setAttribute('class', 'posting');
        let url_ = "https://giana-blog.netlify.app/" + url + "/";
        obj.setAttribute('href', url_);

        let div = document.createElement('div');
        let preimage = document.createElement('img');
        preimage.setAttribute('class', 'preimg');
        preimage.setAttribute('src', "https://giana-blog.netlify.app/assets/"+image);
        obj.appendChild(preimage);

        div.setAttribute('class', 'post-body');
        let h1 = document.createElement('h1');
        h1.setAttribute('class', "post-title");
        h1.innerText = title;
        div.appendChild(h1);

        let span = document.createElement('span');
        span.innerText = des;
        div.appendChild(span);
        obj.appendChild(div);
        main.appendChild(obj);
    }
    add_posting("startdjango", "intro.png", "Start Django in Macbook M1 Pro (Nginx + gunicorn + django)", "Configureation setting and process to start project in Django"); 
    add_posting("djdesignpattern", "designpattern.png", "Django Design Pattern", "MTV pattern and security coding in django");
    add_posting("djangotemplate", "template.png", "Template of View, Template and Model", "How to write code of MVT pattern?");
    add_posting("djangodbconnect", "db.png", "Add databases to Django", "Redis and Postgresql for Django");
    add_posting("djangorest", "crud.png", "CRUD programming with django's RestFramework API", "Construction of CRUD(posting system) with Django RestFrameworkAPI");
    add_posting("djangovue", "vue.png" "vue.js for frontend", "Let's make frontend with vue.js!");
    add_posting("djangovue", "create.png", "Vue foundation and Create operation")
</script>

<style>
    .post-body
    {
        display:grid;
        place-items: center normal;
        padding: 2vw 0vw;
    }
    .posting
    {
        display: flex;
        justify-content: flex-start;
        margin: 3vw 3vw;
    }
    .preimg
    {
        display: inline-block;
        width: 10vw;
        height: 10vw;
        border-radius: 10px;
        margin: 0em 0em;
        margin-right: 3vw;
        vertical-align: middle;
    }
    span
    {
        display: block;
        font-size: 1vw;
    }
    h1
    {
        font-size: 2vw;
        margin-top:0em;
    }
</style>


