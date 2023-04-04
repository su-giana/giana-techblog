---
title: "Algorithm"
---
from [[computer-basics]]

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

    add_posting('oops', 'overview.jpeg', 'Overview of Problem Solving', 'This chapter explained effective approach to problem solving technique');
    add_posting('acd', 'debug.png', 'About Coding and Debugging', 'This section described how to code better and common mistake');
    add_posting('ivind', 'mathproof.png', 'Mathematical induction and loop in variant', 'Mathemacial proofs for problem solving');
    add_posting('bf', 'bf.png', 'Brute Force', 'The first step of algorithm, brute force and greedy! This section represented source code examples of brute force and essential condition when it comes to try brute force');
    add_posting('divandconq', 'conq.png', 'Divide and Conquer', "Let's divide problem and conquer it throught solving small size problems. The post explained various algorithm using divide and conquer and tips for it");
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



