layout: post
title: Codility - Lesson 1 - Time Complexity
description: "Codility coding problems"
date: 2015/10/09
category: articles
tags: [codility,algorithms]
image:
  feature: texture-feature-05.jpg
---

I decided to go through the Codility coding lessons, using Javascript. I'll be posting my solutions here. 

## 01 TapeEquilibrium

Yikes, as a first problem that was supposed to be trivial, this took some effort. I guess it's a good lesson for O-notation, since O(n2) solution is so easy, and O(n) much harder, so you'll surely remember the difference. 

I knew I can only iterate the array once, so I knew I had to store the sums of elements from the top and from the bottom, on the first and only pass. Once you pass half the array, you can actually start comparing the sum differences and looking for the minimum. 

It would take me too long to fully explain the solution here, so I'm just posting the code for posterity, with no illusion anyone, including myself, will be able to make any sense of it, in any reasonable amount of time. 

It does give you 100% score, though. 

{% codeblock lang:javascript %}
function solution(A) {
    var minimalDiff = null;
    
    var sumUpTo = [];
    var sumDownTo = [];
    
    A.forEach(function(i, index) {
        var previousSumUpTo = (index === 0) ? 0 : sumUpTo[index - 1];
        sumUpTo[index] = previousSumUpTo + i;
        
        var previousSumDownTo = (index === 0) ? 0 : sumDownTo[index - 1];
        sumDownTo[index] = previousSumDownTo + A[A.length - 1 - index];
        
        if (index >= Math.floor(A.length / 2)) {
            var diff = Math.abs(previousSumUpTo - sumDownTo[A.length - 1 - index]);
            if (minimalDiff === null || diff < minimalDiff) minimalDiff = diff;
            
            if (A.length - index - 2 >= 0) {
                diff = Math.abs(sumUpTo[A.length - index - 2] - sumDownTo[index]);
                if (minimalDiff === null || diff < minimalDiff) minimalDiff = diff;
            }
        }
    });
    
    return minimalDiff;
}
{% endcodeblock %}

## 02 FrogJmp

Phew, froggy, that should be easier. Only thing to notice is that we need to come up with an O(1) solution, so we can't just have a loop, where we'll jump until we're over the target. Fortunately, we can just calculate the distance (Y - X), divide it by jump length (D), and take the 'ceil' value of the result (since we need to jump *over* the target). 

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