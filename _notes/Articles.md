---
title: "Articles"
---

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

    add_posting('Why-I-decided-to-start-my-career-early' , 'interview.jpg', 'Why I decided to start my career early', 
    'This post contains content about why I chose to plan my career right after I graduate university. In process, I made my own standard in hiring process.');
    add_posting('nightmare-of-human-resource-management-developers', "manpower.jpg", "Nightmare of human resource managements - developers",
    "I thoughfully lookback my old friend's saying, why you developer guys keep quiting own job, even company providing various merits to you out of their ass. What developer ultimately wants? I disolved my thought in this post.")
    add_posting('Why-I-chose-to-be-a-backend-developer', "select.jpeg", "Why I chose to be backend developer",
    "Before I jump to job market, I organized my story of my journey to decide to be a backend developer.")
    add_posting('How-end-users-dealing-with-fintech', "customer.jpg", "How end users dealing with fintech",
    "Based on customer's sight, I tore apart how fintech leading market in my words.")
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

