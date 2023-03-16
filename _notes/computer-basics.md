---
title: "Computer Basics"
---

<div id="sections"></div>

<script>
    function add_section(url, image, title)
    {
        let main = document.getElementById('sections');

        let obj = document.createElement('a');
        obj.setAttribute('class', 'section');
        let url_ = "https://giana-blog.netlify.app/" + url + "/";
        obj.setAttribute('href', url_);

        let preimage = document.createElement('img');
        preimage.setAttribute('class', 'secimg');
        preimage.setAttribute('src', "https://giana-blog.netlify.app/assets/"+image);
        obj.appendChild(preimage);

        let h1 = document.createElement('h1');
        h1.setAttribute('class', "sec-title");
        h1.innerText = title;
        obj.appendChild(h1);

        main.appendChild(obj);
    }
    add_section("computer-structure", "argb.png", "Computer Structure");
    add_section("ubuntu", "argb.png", "Ubuntu");
    add_section("docker", "argb.png", "Docker");
    add_section("algorithm", "argb.png", "Algorithm");
</script>
    
# Section.

- [[computer-structure]] 🧱

- [[ubuntu]] 🐒

- [[docker]] 🐋

- [[algorithm]] ➗


<style>
    .section
    {
        display:flex;
        place-items: center normal;
        padding: 2vw 0vw;
    }
    .sections
    {
        display: flex;
        justify-content: flex-start;
        margin: 4.5vw 5vw;
    }
    .secimg
    {
        display: inline-block;
        width: 16vw;
        height: 16vw;
        border-radius: 10px;
        margin: 0em 0em;
        margin-right: 3vw;
        vertical-align: middle;
    }
    h1
    {
        font-size: 2.5vw;
        margin-top:0em;
    }
</style>


