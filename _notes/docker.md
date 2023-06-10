---
title: "Docker"
---
from [[computer-basics]]

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
    add_posting("whatisdocker", "../assets/intro.png", "What is Docker?", "Introduction of Docker"); 
    add_posting("dockerarchitecture", "../assets/strategy.png", "Architecture of Docker", "How does docker work in various environment?");
    add_posting("dockercommand", "../assets/command.png", "Commands of Docker", "Let's execute docker container and image through command");
    add_posting("communicationofcontainer", "../assets/collab.png", "Configuration of container to communicate with other environment");
    add_posting("indexationofcontainer", "../assets/indexation.png", "Indexation of Container", "The chapter introduces network of container in docker");
    add_posting("handsoncontainer", "../assets/handson.png", "Hands-on Docker", "Docker technique for hands-on work");
    add_posting("dockercompose", "../assets/dockercompose.png", "docker-compose", "Docker Compose is used to define and manage multi-container Docker applications, simplifying their setup, configuration, and orchestration");
    add_posting("kubernetes", "../assets/kubernetes.png", "Kubernetes Introduction", "Kubernetes for multiple server manipulation and management");
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



