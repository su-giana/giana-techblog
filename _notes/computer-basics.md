---
title: "Computer Basics"
---

<div id="sections"></div>

<script>
    function add_section(url, image, title)
    {
        let main = document.getElementById('sections');

        let body = document.createElement('div');
        let obj = document.createElement('a');
        obj.setAttribute('class', 'section');
        let url_ = "https://giana-blog.netlify.app/" + url + "/";
        obj.setAttribute('href', url_);

        let preimage = document.createElement('img');
        preimage.setAttribute('class', 'secimg');
        preimage.setAttribute('src', "https://giana-blog.netlify.app/assets/"+image);
        obj.appendChild(preimage);

        let h2 = document.createElement('h2');
        h2.setAttribute('class', "sec-title");
        h2.innerText = title;
        obj.appendChild(h2);

        body.appendChild(obj);
        main.appendChild(body);
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
    display: grid;
    place-items: center normal;
    padding: 2vw 2vw;
    margin: 0vw;
    width: 16vw;
    transition: all 300ms linear;

    &:hover h2
    {
        transition: all 300ms linear;
        color: #faab78;
    }

    &:after {
    position: relative;
    top: -0.5em;
    font-size: 0.7em;
    content: "↗";
    color: #aaaaaa;
    }
    &.internal-link:after,
    &.footnote:after,
    &.reversefootnote:after {
        content: "";
    }
}
.section:hover {
        transition: all 300ms linear;
        transform: translate(0px, -10px);
        box-shadow: 0 17px 20px -18px rgba(0, 0, 0, 1);
    }

.sections
{
    display: grid;
    margin: 4.5vw 0vw;
}
.secimg
{
    width: 16vw;
    height: 13vw;
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
h2
{
    height:3vw;
    width:16vw;
}
</style>


