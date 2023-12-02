---
title: Home
---

<h1>나를 죽이지 못하는 것은 나를 강하게 만든다 🍀</h1>
<div  width="1500vw" height="200vh" style="background: #E8F3D6">
<p style="padding: 2vw 1vh; border-radius: 4px;">
  💰 핀테크에 관심이 있는 <span style="font-weight: bold">신입 개발자</span>
  <br><br>
  👩‍🦯  사용자만을 생각해서, 최적의 기술을 제공합니다
  <br><br>
  🗣  효율적인 소통과 협업을 통한 지속 가능한 개발 생활
</p>
</div>

<div style="padding:1vw 1vh; display:flex; justify-content:flex-start;">
<img src="../assets/image.jpg" height="250em" width="250em" style="border-radius:4px; margin: 0em 0em; padding-right:2em;">
<div style="padding:1vw 0vh;">
<span style="font-size: larger; padding: 2vw 0vh; color:#FAAB78;">Contact 📟. <br><strong style="font-size: x-larger; color: black;">giananews@gmail.com</strong></span>
<br><br>
<span style="font-size: larger; padding: 2vw 0vh; color: #FAAB78;">Github 🫙.<br> <strong style="font-size: x-larger; color: black;">
<a href="https://github.com/su-giana/">Github Link</a>
</strong></span>
</div>
</div>

<hr>

### Tech. stack
- Computer basics : ```Computer Structure```, ```Algorithm```, ```Ubuntu Linux```, ```Docker```

<hr>

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
    add_posting("whameleon-kor", "cameleon.png" ,"Whameleon 프로젝트", "유학생들을 위한 체크카드 충전 시스템");
    add_posting("pintos-kaist-kor", "pintos.png", "Pintos Kaist", "Computer Science 심화 이해를 위한 운영체제, 네트워크, 데이터 베이스");
    add_posting("woowa-tech-kor", "woowa.png", "우아한 테크코스", "제발 붙여주세요");
    add_posting("conference-kor", "conference.png", "컨퍼런스", "컨퍼런스 후기");
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