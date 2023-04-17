---
title: "About coding and debugging"
---
from [[algorithm]]

## Principle to code better
- Short coding
We can change
```
for(int i=0 ; i<array.size() ; i++)
```
to
```
#define FOR(i, n) for(int i=0 l i<(n) ; i++)
```
to prevent mistakes such as ```for(int i=0 ; i<n ; j++)```

- Reuse previous code (modulation)

- Get used to standard library

- Always use similar way to code

- Consistent and clear nomenclature
🙅‍♀️
``` bool judge(int y, int x, ...)```
👍
``` bool isInsideCircle(int y, int x, ...)```

- Normalize every stored data
 : unify units, timezond and etc.

- Seperate code and data
ex)
```const string monthName[] = {"Jan", "Feb", ...}```

<hr>

## Common Mistake
- out of index

- Inconsistent expression of range
 : That's why most programming languages use half-open interval
    * Easy to present empty range with same index
    * Easy to chekc whether those range are consistent or not
        b=c or a=d
    * Easy to get the size of range (b-a)

- Off-by-one
 : Though mathematical expression is right, wrong result came with one more or less index.

- mistake of a const
    * wrong letter in string
    * more or less 0 in number such as 10000001
    * did not designate a const to 64 bit. -> need to put ll behind the integer const

- Stack Overflow

- Shuffle the order of index

- Wrong operator comparison function
### strict weak ordering
1. a<a is false always. (irreflexivity)
2. If a<b is true, b<a is false. (Asymmetry)
3. If a<b and b<c are true, a<c is true. (Transitivity)
4. If a<b and b<a are false, a=b. If a=b and b=c, a=c. (transitivity of equivalence)

- Failed to handle max, min and exception

- Operator priority

- Too slow input and output

- Variable initialization

<hr>

## Debugging and Testing

- Check if the statement works appropriately with small inputs

- Use assertion

- Print step by step output of a program

<hr>

## Understanding of variable's range
- Overflow

- Too big result

- Too big value while in programming

- Too big infintie value

# Do not use FLOAT COMPUTATION

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



