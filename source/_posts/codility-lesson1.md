layout: post
title: Codility - Lesson 1 - Time Complexity
date: 2015/10/09
tags: [codility,algorithms,javascript]
---

> Disclaimer: If you're using this to cheat on job interviews, it's not my fault, however it goes. If you're catching someone cheating, make sure it's not me.

I decided to go through the Codility coding lessons, using Javascript. I'll be posting my solutions here.

## 01 TapeEquilibrium

{% codeblock lang:javascript %}
function solution(A) {
    var prefixSum = []
    for (var i = 0; i < A.length; i++) {
        prefixSum[i] = (prefixSum[i-1] | 0) + A[i]
    }
    var minDiff
    for (i = 0; i < prefixSum.length - 1; i++) {
        var diff = Math.abs(prefixSum[i] - (prefixSum[A.length - 1] - prefixSum[i]))
        if (minDiff === undefined || diff < minDiff) minDiff = diff
    }
    return minDiff
}
{% endcodeblock %}

## 02 FrogJmp

Only thing to notice is that we need to come up with an O(1) solution, so we can't just have a loop, where we'll jump until we're over the target. Fortunately, we can just calculate the distance (Y - X), divide it by jump length (D), and take the 'ceil' value of the result (since we need to jump *over* the target).

{% codeblock lang:javascript %}
function solution(X, Y, D) {
    return Math.ceil((Y - X) / D);
}
{% endcodeblock %}

## 03 PermMissingElem

The trick here is to figure out what the requirement actually is. We have an array with elements 1 .. n, with one element missing. All that we need, is the value of that one element, so if we subtracted the sum of that array, from the sum of an array with *all* elements, we would get exactly it. Calculating the sum of first n integers is fairly simple, and explained in the PDF that comes with the lesson.

Summing an array in JS is easily done with 'reduce'. Once you have that, all you need is to subtract.

{% codeblock lang:javascript %}
function solution(A) {
    var sumA = A.reduce(function(a, b) {
        return a + b;
    }, 0);

    var sum = (A.length + 1) * (A.length + 2) / 2;

    return sum - sumA;
}
{% endcodeblock %}
