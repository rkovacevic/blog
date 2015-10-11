layout: post
title: Codility - Lesson 3 - Prefix Sum
date: 2015/10/10
tags: [codility,algorithms,javascript]
---

> Disclaimer: If you're using this to cheat on job interviews, it's not my fault, however it goes. If you're catching someone cheating, make sure it's not me. 


## 01 CountDiv

This one took me a while, and I'm still not really sure why this solution works, but it does. Dividing the difference by K is the intuitive part, but I also had to align A and B to first divisible integer, and then add 1 to the result. 

I didn't get this on the first try. If you need to get these on the first try, write *at least* 10 tests. 

{% codeblock lang:javascript %}
function solution(A, B, K) {
    if (A % K !== 0) A = A + (K - A % K);
    if (B % K !== 0) B = B - (B % K);
    return (B - A) / K + 1;
}
{% endcodeblock %}

## 02 PassingCars

Hated this one, so poorly worded task.

{% codeblock lang:javascript %}
function solution(A) {
    var zerosCount = 0;
    var res = 0;
    for (var i = 0; i < A.length; i++) {
        if (A[i] === 0) zerosCount++;
        if (A[i] === 1) res += zerosCount;
        if (res > 1000000000) return -1;
    }
    return res;
}
{% endcodeblock %}

// This lesson is still in progress //