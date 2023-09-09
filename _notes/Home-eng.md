---
title: Home
---

<h1>Hi! I'm a developer studied in Korea, Giana 🍀</h1>
<div  width="1500vw" height="200vh" style="background: #E8F3D6">
<p style="padding: 2vw 1vh; border-radius: 4px;">
  💰 Plus, <span style="font-weight: bold">Junior Developer</span> interested in fintech.
  <br><br>
  👩‍🦯 Usability for all user, focus on end-user not only for technology
  <br><br>
  🗣 Communicate effectively with co-worker, estabilsh sustainable dev. environment with automated platform 
</p>
</div>

<div style="padding:1vw 1vh; display:flex; justify-content:flex-start;">
<img src="../assets/image.jpg" height="250em" width="250em" style="border-radius:4px; margin: 0em 0em; padding-right:2em;">
<div style="padding:1vw 0vh;">
<span style="font-size: larger; padding: 2vw 0vh; color:#FAAB78;">Contact 📟. <br><strong style="font-size: x-larger; color: black;">giananews@gmail.com</strong></span>
<br><br>
<span style="font-size: larger; padding: 2vw 0vh; color: #FAAB78;">Github 🫙.<br> <strong style="font-size: x-larger; color: black;">
<a href="https://github.com/califonia-ahri/">Github Link</a>
</strong></span>
</div>
</div>

<hr>

### Tech. stack
- Computer basics : ```Computer Structure```, ```Algorithm```, ```Ubuntu Linux```, ```Docker```

<hr>


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
    add_posting("whameleon", "cameleon.png" ,"Whameleon Project", "Payment System for Everyone, Traveling the world");
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
        margin: 4.5vw 5vw;
    }
    .preimg
    {
        display: inline-block;
        width: 16vw;
        height: 16vw;
        border-radius: 10px;
        margin: 0em 0em;
        margin-right: 3vw;
        vertical-align: middle;
    }
    span
    {
        display: block;
        font-size: 1.5vw;
    }
    h1
    {
        font-size: 2.5vw;
        margin-top:0em;
    }
</style>