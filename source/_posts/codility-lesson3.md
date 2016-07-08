layout: post
title: Codility - Lesson 3 - Prefix Sum
date: 2015/10/15
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

## 03 MinAvgTwoSlice

This solution gives 90%, there's a failing 'performance' test case. I have not been able to find out why it's failing, and codality won't give me the actual test case.

{% codeblock lang:javascript %}
function Slice(startPos, sum, length) {
    this.startPos = startPos;
    this.sum = sum;
    this.length = length;
}

Slice.create = function(A, startPos) {
    return new Slice(startPos, A[startPos] + A[startPos + 1], 2);
};

Slice.prototype.avg = function() {
    return this.sum / this.length;
};

Slice.prototype.add = function(number) {
    this.sum += number;
    this.length++;
};

Slice.prototype.copy = function() {
    return new Slice(this.startPos, this.sum, this.length);
};

function minSlice(slice1, slice2) {
    if (slice2.avg() < slice1.avg()) return slice2;
    return slice1;
}

function solution(A) {
    var currentSlice = Slice.create(A, 0);
    var bestSlice = currentSlice;
    for (var i = 2; i < A.length; ++i) {
        var newSlice = Slice.create(A, i - 1);
        currentSlice.add(A[i]);
        currentSlice = minSlice(currentSlice, newSlice);
        bestSlice = minSlice(bestSlice, currentSlice).copy();
    }
    return bestSlice.startPos;
}

function solution(A) {
    var currentSliceStart = 0
    var currentSliceAvg = A[0]
    var minSliceStart = 0
    var minSliceAvg = currentSliceAvg

    for (var i = 1; i < A.length - 1; i++) {
        currentSliceAvg = (currentSliceAvg + A[i]) / 2
        var newSliceAvg = (A[i] + A[i + 1]) / 2
        if (newSliceAvg < currentSliceAvg) {
            currentSliceAvg = newSliceAvg
            currentSliceStart = i
        }
        if (currentSliceAvg < minSliceAvg) {
            minSliceStart = currentSliceStart
            minSliceAvg = currentSliceAvg
        }
    }
    return minSliceStart
}
{% endcodeblock %}

## 04 GenomicRangeQuery

{% codeblock lang:javascript %}
function solution(S, P, Q) {
    var prefixSums = {
        'A': [],
        'C': [],
        'G': [],
        'T': []
    }
    for (var i = 0; i < S.length; i++) {
        var n = S.charAt(i)
        Object.keys(prefixSums).forEach(key => {
            if (key === n) {
                prefixSums[key][i] = (prefixSums[key][i - 1] | 0) + 1
            } else {
                prefixSums[key][i] = (prefixSums[key][i - 1] | 0)
            }
        })
    }
    var solution = []
    for (i = 0; i < P.length; i++) {
        if (prefixSums['A'][Q[i]] - (prefixSums['A'][P[i] - 1] | 0) > 0) solution.push(1)
        else if (prefixSums['C'][Q[i]] - (prefixSums['C'][P[i] - 1] | 0) > 0) solution.push(2)
        else if (prefixSums['G'][Q[i]] - (prefixSums['G'][P[i] - 1] | 0) > 0) solution.push(3)
        else if (prefixSums['T'][Q[i]] - (prefixSums['T'][P[i] - 1] | 0) > 0) solution.push(4)
    }
    return solution
}
{% endcodeblock %}
