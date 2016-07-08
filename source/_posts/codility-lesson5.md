layout: post
title: Codility - Lesson 5 - Stacks and Queues
date: 2015/10/17
tags: [codility,algorithms,javascript]
---

> Disclaimer: If you're using this to cheat on job interviews, it's not my fault, however it goes. If you're catching someone cheating, make sure it's not me. 


## 01 Nesting

{% codeblock lang:javascript %}
function solution(S) {
    var counter = 0;
    for (var i = 0; i < S.length; ++i) {
        if (S.charAt(i) === '(') counter++;
        if (S.charAt(i) === ')') counter--;
        if (counter < 0) return 0;
    }
    if (counter === 0) return 1;
    return 0;
}
{% endcodeblock %}

## 02 Stonewall

{% codeblock lang:javascript %}
function peek(H) {
    if (H.length === 0) return undefined;
    return H[H.length - 1];
}

function solution(H) {
    var stack = [];
    var blocks = 0;
    var handleHeight = function(h) {
        var l = peek(stack);
        if (l === h) return;
        if (l === undefined || l < h) {
            blocks++;
            stack.push(h);
        } else {
            stack.pop();
            handleHeight(h);
        }
    };
    H.forEach(handleHeight);
    return blocks;
}
{% endcodeblock %}

## 03 Brackets

{% codeblock lang:javascript %}
function solution(S) {
    var stack = [];
    
    for (var i = 0; i < S.length; i++) {
        var x = S.charAt(i);
        if (x === '(' || x === '[' || x === '{') stack.push(x);
        else {
            var y = stack.pop();
            if (!(x === ')' && y === '(' || 
                x === ']' && y === '[' ||
                x === '}' && y === '{')) return 0;
        }
    }
    
    if (stack.length === 0) return 1;
    return 0;
}
{% endcodeblock %}

## 04 Fish

There is one performance test that fails with this, probably because of the recursion. I could rewrite it as a loop, but I believe the recursion problem will be solved in ES6. 

{% codeblock lang:javascript %}
const UPSTREAM = 0;
const DOWNSTREAM = 1;

function solution(A, B) {
    var upstreamStack = [];
    var downstreamStack = [];
    
    var handleFish = function(i) {
        var size = A[i];
        var direction = B[i];
        
        if (direction === UPSTREAM) {
            if (downstreamStack.length === 0) {
                upstreamStack.push(i);
            } else {
                var opponent = downstreamStack.pop();
                if (A[opponent] > size) {
                    downstreamStack.push(opponent);
                } else {
                    handleFish(i);
                }
            }
        } else {
            downstreamStack.push(i);
        }
    };
    
    for (var i = 0; i < A.length; i++) {
        handleFish(i);
    }
    
    return upstreamStack.length + downstreamStack.length;
}
{% endcodeblock %}