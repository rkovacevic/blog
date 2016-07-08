layout: post
title: Codility - Lesson 2 - Counting Elements
date: 2015/10/10
tags: [codility,algorithms,javascript]
---

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
        // Special case, if you get a number larger than array length,
        // you can be sure it's not a permutation
        if (v >= A.length) return 0;
        // If we got the same index second time, it's not a permutation
        if (perm[v] === true) return 0;
        // Mark that index was visited
        perm[v] = true;
    }
    // If we passed all and no index was visited twice,
    // it must be a permutation
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

    // In case A was a sequence of numbers, like [1,2,3],
    // this will return the next int (that's just how JS arrays work)
    // Also checks special case when there were no positive integers.
    return integers.length === 0 ? 1 : integers.length;
}
{% endcodeblock %}

## 04 MaxCounters

{% codeblock lang:javascript %}
function solution(N, A) {
    var counters = []
    for (var i = 0; i < N; i++) {
        counters.push(0)
    }
    var maxCounter = 0
    var lastMaxCounterOperation
    A.forEach(n => {
        if (n !== N + 1) {
            if (lastMaxCounterOperation !== undefined &&
                counters[n - 1] < lastMaxCounterOperation) {
                counters[n - 1] = lastMaxCounterOperation        
            }
            counters[n - 1] += 1
            maxCounter = Math.max(maxCounter, counters[n - 1])
        } else {
            lastMaxCounterOperation = maxCounter
        }
    })
    if (lastMaxCounterOperation !== undefined) {
        for (i = 0; i < N; i++) {
            if (counters[i] < lastMaxCounterOperation) {
                counters[i] = lastMaxCounterOperation
            }
        }
    }
    return counters
}
{% endcodeblock %}
