layout: post
title: Codility - Lesson 2 - Counting Elements
date: 2015/10/10
tags: [codility,algorithms]
---

> Disclaimer: If you're using this to cheat on job interviews, it's not my fault, however it goes. If you're catching someone cheating, make sure it's not me. 

Alright, lesson 2, let's go. 

## 01 FrogRiverOne

Lessons here:
* You can name loops in JS, which can be used to 'continue' or 'break' outer loops. This is not something that should be used in normal code, but comes in handy in trick shots. 
* Reading and understanding the problem is always most important.
* Converting between zero-indexed and one-indexed is always source of bugs. 

{% codeblock lang:javascript %}
function solution(X, A) {
    var fallenLeaves = new Array(X);
    var passableUpTo = 0;
    loop: for (var i = 0; i < A.length; i++) {
        fallenLeaves[A[i] - 1] = true;
        for (var j = passableUpTo + 1; j < X; j++) {
            if (fallenLeaves[j] === true) {
                passableUpTo = j;
            } else {
                continue loop;
            }
        }
        return i;
    }
    return -1;
}
{% endcodeblock %}

## 02 PermCheck

A short lesson in how not to name variables:

{% codeblock lang:javascript %}
function solution(A) {
    // This is the array where we'll store which indexes we visited
    var perm = new Array(A.length);
    for (var i = 0; i < A.length; i++) {
        // Zero index it
        var v = A[i] - 1;
        // Special case, if you get a number larger than array length, you can be sure it's not a permutation
        if (v >= A.length) return 0;
        // If we got the same index second time, it's not a permutation
        if (perm[v] === true) return 0;
        // Mark that index was visited
        perm[v] = true;
    }
    // If we passed all and no index was visited twice, it must be a permutation
    return 1;
}
{% endcodeblock %}

## 03 MissingInteger

The way Javascript implements arrays, which is pretty much just objects with integers as keys, makes this a *lot* easier. Don't try this with C arrays. 

{% codeblock lang:javascript %}
function solution(A) {
    var integers = [];
    
    A.forEach(function(element) {
        // We're only interested in positive integers
        if (element > 0) integers[element] = true;
    });
    
    for (var i = 1; i < integers.length; i++) {
        if (integers[i] !== true) return i;
    }
    
    // In case A was a sequence of numbers, like [1,2,3], this will return the next int
    // (that's just how JS arrays work)
    // Also checks special case when there were no positive integers. 
    return integers.length === 0 ? 1 : integers.length;
}
{% endcodeblock %}

## 04 MaxCounters

This solution fails one performance check, when you have a large array of all max_counters operations. I could make a special optimization for just that case, but I'm not going to spend a lot of time searching for an elegant solution. 

{% codeblock lang:javascript %}
// In ES6, you could just use Array.prototype.fill
function setAllElementsInArray(array, value) {
    for (var i = 0; i < array.length; i++) {
        array[i] = value;
    }
}

var maxCounter = 0;

function applyOperation(array, x, n) {
    if (x <= n) {
        array[x-1] = array[x-1] + 1;
        if (array[x-1] > maxCounter) maxCounter = array[x-1];
    } else if (x === n + 1) {
        setAllElementsInArray(array, maxCounter);
    } else {
        throw new Error('Invalid x: ' + x);   
    }
}

function solution(N, A) {
    var counters = new Array(N);
    setAllElementsInArray(counters, 0);
    A.forEach(function(x) {
        applyOperation(counters, x, N);
    });
    return counters;
}
{% endcodeblock %}