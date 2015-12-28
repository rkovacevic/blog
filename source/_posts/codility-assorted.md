layout: post
title: Codility - Assorted lessons
date: 2015/12/27
tags: [codility,algorithms,javascript]
---

> Disclaimer: If you're using this to cheat on job interviews, it's not my fault, however it goes. If you're catching someone cheating, make sure it's not me.

These are solutions to some assorted Codility lessons.

## BinaryGap

Make sure you take into account that only gaps that are bordered by '1' count. For example, binary number 1000 does not have any gaps, and should return 0.

{% codeblock lang:javascript %}
function solution(N) {
    var counter = -1
    var maxCounter = 0
    while (N > 0) {
        if (N % 2 === 0 && counter >= 0) {
            counter++
            if (counter > maxCounter) maxCounter = counter
        } else {
            if (N % 2 !== 0) counter = 0
        }
        N = Math.floor(N / 2)
    }
    return maxCounter
}
{% endcodeblock %}

## OddOccurrencesInArray

{% codeblock lang:javascript %}
function solution(A) {
    var  pairs = []

    A.forEach(n => {
        pairs[n] = (pairs[n] | 0) + 1
    })

    var keys = Object.keys(pairs)
    for (var i = 0; i < keys.length; i++) {
        if (pairs[keys[i]] % 2 !== 0) return parseInt(keys[i], 10)
    }
}
{% endcodeblock %}
