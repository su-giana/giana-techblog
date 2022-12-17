---
title: "Project"
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

    add_posting('netlify와-github를-이용하여-블로그-만들기' , 'netlify.jpg', 'netlify와 github를 이용하여 블로그 만들기', 
    '다양한 기록 플렛폼 유목민으로 살아가다가 직접 블로그를 만들어 정리해 보았습니다. ');
    add_posting('시선트래킹-기술을-이용한-학습-관리-플렛폼' , 'icanseeyou.png', '시선트래킹 기술을 이용한 학습 관리 플렛폼', 
    '2022년 하반기 소프트웨어 보안 개발 경진대회에 출품했던 "시선 트래킹 기술을 이용한 학습 관리 플렛폼" 프로젝트요회고를 뒤늦게 적어보았습니다. 팀프로젝트를 하며 있었던 일, 팀원들간의 의사소통 문제에 대해 다시 한 번 생각해보는 시간을 가졌어요');
    add_posting('IoT-포렌식-연구과제-참여' , 'research.jpeg', 'IoT 포렌식 연구과제 참여', 
    '학교에서 진행했던 IoT 포렌식 연구과제를 참여하며 느꼈던 일, 그 과정에서 맡았던 역할, 앞으로의 프로젝트에서 어떻게 행동해야 하는지 정리했습니다.');


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

