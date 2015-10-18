layout: post
title: Codility - Lesson 6 - Leader
date: 2015/10/18
tags: [codility,algorithms,javascript]
---

> Disclaimer: If you're using this to cheat on job interviews, it's not my fault, however it goes. If you're catching someone cheating, make sure it's not me. 


## 01 Dominator

Solution is explained in the lesson. 

{% codeblock lang:javascript %}
function solution(A) {
    var stackHeight = 0;
    var stackTop;
    
    for (var i = 0; i < A.length; ++i) {
        if (stackHeight === 0) {
            stackHeight++;
            stackTop = A[i];
        } else {
            if (stackTop !== A[i]) {
                if (stackHeight > 0) stackHeight--;
                if (stackHeight === 0) stackTop = undefined;
            } else {
                stackHeight++;
            }
        }
    }

    if (stackTop === undefined) return -1;
    
    var leaderIndex;
    var count = A.reduce(function(p, i, index) {
        if (i === stackTop) {
            leaderIndex = index;
            return p + 1;
        }
        return p;
    }, 0);

    if (count > A.length / 2) return leaderIndex;
    return -1;
}
{% endcodeblock %}

## 02 EquiLeader

Here we just extend the previous solution, with a prefix sum array, that we can use to calculate how many leaders there are to the left and to the right of a given index. 

Might be worth noting that, since you have the same leader in both parts, that's obviously the leader for the whole array. 

{% codeblock lang:javascript %}
function solution(A) {
    var stackHeight = 0;
    var stackTop;
    
    for (var i = 0; i < A.length; ++i) {
        if (stackHeight === 0) {
            stackHeight++;
            stackTop = A[i];
        } else {
            if (stackTop !== A[i]) {
                if (stackHeight > 0) stackHeight--;
                if (stackHeight === 0) stackTop = undefined;
            } else {
                stackHeight++;
            }
        }
    }

    if (stackTop === undefined) return 0;
    
    var leaderPrefixSum = [];
    var leaderIndex;
    var count = A.reduce(function(p, i, index) {
        leaderPrefixSum[index] = leaderPrefixSum[index - 1] | 0;
        if (i === stackTop) {
            leaderPrefixSum[index] = leaderPrefixSum[index] + 1;
            leaderIndex = index;
            return p + 1;
        }
        return p;
    }, 0);
    
    if (count <= A.length / 2) return 0;
    
    var equiLeadersCount = 0;
    for (i = 0; i < leaderPrefixSum.length; i++) {
        var leadersLeft = leaderPrefixSum[i];
        if (leadersLeft <= ((i + 1) / 2)) continue;
        var leadersRight = leaderPrefixSum[A.length - 1] - leaderPrefixSum[i];
        if (leadersRight <= ((A.length - (i + 1)) / 2)) continue;
        equiLeadersCount++;
    }
    return equiLeadersCount;
}
{% endcodeblock %}
