---
title: "Design Pattern"
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
    add_posting("singletone", "../assets/singleton.png", "Singletone Pattern", "Singletone pattern for database connection module"); 
    add_posting("factorypattern", "../assets/factorypattern.png", "Factory Pattern", "Let's suppose we run a coffee factory");
    add_posting("strategypattern", "../assets/strategy.png", "Strategy Pattern", "Same context, different strategy");
    add_posting("observerpattern", "../assets/observer.png", "Observer Pattern", "What if you write notification algorithm of twitter?");
    add_posting("proxypattern", "../assets/server.png", "Proxy Pattern", "Role of proxy server");
    add_posting("iteratorpattern", "../assets/iterator.png", "Iterator Pattern", "Iterator to traversal collection objects")
    add_posting("mvcpattern", "../assets/collab.png", "MVC Pattern", "MVC, MVP, MVVC patterns");
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



