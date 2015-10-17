layout: post
title: Codility - Lesson 4 - Sorting
date: 2015/10/16
tags: [codility,algorithms,javascript]
---

> Disclaimer: If you're using this to cheat on job interviews, it's not my fault, however it goes. If you're catching someone cheating, make sure it's not me. 


## 01 MaxProductOfThree

We just sort the array, take the 3 largest numbers, multiply them and that's it? Well, almost. One other possiblity is that there are 2 negative numbers, that multiplied yield a larger number. 

Note that you need to explicitly define the comparator function, when sorting an integer array. JS will sort them alphabetically otherwise. Yes, yes, it's easy to make fun of JS, but don't sweat the small stuff, it got the big things right. 

{% codeblock lang:javascript %}
function solution(A) {
    A.sort(function(a, b) {
        return a - b;
    });
    
    var l = A.length;
    return Math.max(A[l - 1] * A[l - 2] * A[l - 3], A[l - 1] * A[0] * A[1]);
}
{% endcodeblock %}

## 02 Triangle

We have these 3 conditions:
```
(1) A[P] + A[Q] > A[R]
(2) A[Q] + A[R] > A[P]
(3) A[R] + A[P] > A[Q]
```
Since these are supposed to be edges of a triangle, we can intuitively conclude that all 3 numbers must be positive, but we can also mathematically show that, by substitution:
```
If we take (1):
A[P] + A[Q] > A[R]
rewrite (2) to be:
A[R] > A[P] - A[Q]
and put the second in the first one:
A[P] + A[Q] > A[P] - A[Q]
we come up with:
2 * A[Q] > 0
which is only valid for positive numbers
```
Analog proof can be constructed for all 3 numbers. 

Next we're supposed to figure out that it would be easier to solve this, if we were working with a sorted array. I'm not exactly sure I'd guess that, if this lesson wasn't called "Sorting" :)

With a sorted array, we know that this holds:
```
A[P] >= A[Q] >= A[R]
```
Using that, we can rewrite the original 3 conditions as:
```
(1a) A[P] + A[P] > A[R]
(2a) A[P] + A[P] > A[P]
(3a) A[P] + A[P] > A[R]
```
We note that (1a) and (3a) are equivalent, and that (2a) is valid for any positive number. Thus, to find a triangle, all we need is to find 3 positive numbers that satisfy (1). 

Taking all that into account, this is the solution:

{% codeblock lang:javascript %}
function solution(A) {
    if (A.length < 3) return 0;
    
    A.sort(function(a, b) { return a - b; });
    
    for (var i = 0; i < A.length - 2; ++i) {
        if (A[i] <= 0 || A[i + 1] <= 0 || A[i + 2] <= 0) continue;
        if (A[i] + A[i + 1] > A[i + 2]) return 1;
    }
    return 0;
}
{% endcodeblock %}

## 03 Distinct

Here's a very Javascripty solution, that was the first thing that popped into my head:

{% codeblock lang:javascript %}
function solution(A) {
    var uniques = [];
    A.forEach(function(i) {
        uniques[i] = 1;
    });
    return Object.keys(uniques).length;
}
{% endcodeblock %}

Sadly, performance is not good enough, I think Object.keys is doing a copy. 

This is the efficient solution:

{% codeblock lang:javascript %}
function solution(A) {
    A.sort(function(a, b) { return a - b; });
    
    var last;
    var uniques = 0;
    A.forEach(function(i) {
        if (last === undefined || last !== i) {
            uniques++;
            last = i;
        }
    });
    return uniques;
}
{% endcodeblock %}

## 04 NumberOfDiscIntersections

Brute force:

{% codeblock lang:javascript %}
function solution(A) {
    if (A.length < 2) return 0;
    var counter = 0;
    for (var i = 0; i < A.length - 1; ++i) {
        for (var j = i + 1; j < A.length; ++j) {
            // console.log(i + ' ' + A[i] + ' - ' + j + ' ' + A[j]);
            if ((i < j && i + A[i] >= j - A[j]) || 
                (i > j && i - A[i] <= j + A[j])) {
                counter++;
                if (counter > 10000000) return -1;
            }
        }
    }
    return counter;
}
{% endcodeblock %}

More efficient way is to make a list of all points that intersect the X axis, and walk that list, keeping count in how many circles we are:

{% codeblock lang:javascript %}
const CIRCLE_START = -1;
const CIRCLE_END = 1;

function solution(A) {
    if (A.length < 2) return 0;
    
    var events = [];
    for (var i = 0; i < A.length; ++i) {
        events.push({
            event: CIRCLE_START,
            location: i - A[i]
        });
        events.push({
            event: CIRCLE_END,
            location: i + A[i]
        });
    }
    events.sort(function(a, b) {
        if (a.location !== b.location) return a.location - b.location;
        return a.event;
    });
    var intersectingCircles = 0;
    var circleCounter = 0;
    for (i = 0; i < events.length; ++i) {
        var e = events[i];
        if (e.event === CIRCLE_START) {
            intersectingCircles += circleCounter;
            if (intersectingCircles > 10000000) return -1;
            circleCounter++;
        } else {
            circleCounter--;
        }
    }
    return intersectingCircles;
}
{% endcodeblock %}