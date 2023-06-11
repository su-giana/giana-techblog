---
title: Spring
---

- From [[backend]]

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

    add_posting('gppdofspring', 'http.png', 'How to react to HTTP request in Spring', 'Various annotation to specify HTTP method and URL that the method handle');
    add_posting('dbinspring', 'db.png', 'Database connection in Spring with H2');
    add_posting('springsetting', '../assets/server.png', 'Application Setting and Management', 'Setting and monitoring functions in Spring refer to the capabilities provided by the Spring Actuator');
    add_posting('springdata', '../assets/data.png', 'Spring Data', 'Spring data to reduce the amount of boilerplate code and repetitive task required when working with data access');
    add_postng('springmvc', '../assets/app.png', 'Spring MVC', 'Spring MVC to provide a structured approach to building web application');

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
