layout: post
title: Codility - Lesson 7 - Maximum Slice Problem
date: 2015/10/25
tags: [codility,algorithms,javascript]
---

> Disclaimer: If you're using this to cheat on job interviews, it's not my fault, however it goes. If you're catching someone cheating, make sure it's not me. 


## 01 MaxDoubleSliceSum

{% codeblock lang:javascript %}
function solution(A) {
    
    // K1[i] will be the max sum for a subarray starting at i
    var K1 = [];
    for (var i = A.length - 2; i >= 0; i--) {
        // A variation on Kadane's algo
        // note: (undefined | x) === x
        K1[i] = Math.max(A[i], (K1[i + 1] | 0) + A[i], 0);
    }

    // K2[i] will be tht max sum for a subarray ending at i
    var K2 = [];
    for (i = 1; i < A.length; i++) {
        // A more straightforward Kadane's
        K2[i] = Math.max(A[i], (K2[i - 1] | 0) + A[i], 0);
    }
    
    // Now we are looking for a given i, max(K2[i-1] + K1[i+1])
    var max = 0;
    for (i = 1; i < A.length - 1; i++) {
        max = Math.max(max, (K2[i - 1] | 0) + (K1[i + 1] | 0));
    }
    return max;
}
{% endcodeblock %}

02 MaxProfit

{% codeblock lang:javascript %}
function solution(A) {
    var entryPrice = A[0];
    var maxProfit = 0;
    for (var i = 0; i < A.length; i++) {
        var possibleExitPrice = A[i];
        var possibleProfit = possibleExitPrice - entryPrice;
        if (possibleProfit < 0) {
            entryPrice = A[i];
        } else {
            maxProfit = Math.max(maxProfit, possibleProfit);
        }
    }
    return maxProfit;
}
{% endcodeblock %}

03 MaxSliceSum

This is just Kadane's algorithm, without zero-lenght subarrays being allowed. 

{% codeblock lang:javascript %}
function solution(A) {
    var currentSliceSum = A[0];
    var maxSliceSum = currentSliceSum;
    for (var i = 1; i < A.length; i++) {
        currentSliceSum = Math.max(A[i], currentSliceSum + A[i]);
        maxSliceSum = Math.max(maxSliceSum, currentSliceSum);
    }
    return maxSliceSum;
}
{% endcodeblock %}