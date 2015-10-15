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
{% endcodeblock %}

## 04 GenomicRangeQuery

This was my first attempt, with a brute force solution:

{% codeblock lang:javascript %}
function impactOf(char) {
    if (char === 'A') return 1;
    if (char === 'C') return 2;
    if (char === 'G') return 3;
    if (char === 'T') return 4;
    throw new Error('Unknown nucleotide');
}

function min(num1, num2) {
    if (num1 === undefined) return num2;
    if (num2 === undefined) return num1;
    return Math.min(num1, num2);
}

function solution(S, P, Q) {
    var minImpacts = new Array(P.length);
    for (var i = 0; i < P.length; ++i) {
        for (var j = P[i]; j <= Q[i]; ++j) {
            var impact = impactOf(S.charAt(j));
            minImpacts[i] = min(minImpacts[i], impact);
        }
    }
    return minImpacts;
}
{% endcodeblock %}

After some brainstorming, it became clear that checks for each query had to be done in O(1). Obviously, prefix sum was a solution, but the tricky part to figure out was that you actually need a separate prefix sum for each nucleotide. 

Multidimensional arrays are poorly supported in JS, so I also created a handy Matrix object, that also has a feature to set a default value. 

{% codeblock lang:javascript %}
function Matrix(defaultValue) {
    this.data = [];
    this.defaultValue = defaultValue;
}

Matrix.prototype.put = function(i, j, value) {
    var row = this.data[i];
    if (row === undefined) {
        row = [];
        this.data[i] = row;
    }
    row[j] = value;
};

Matrix.prototype.get = function(i, j) {
    var row = this.data[i];
    if (row === undefined) return this.defaultValue;
    var value = row[j];
    if (value === undefined) return this.defaultValue;
    return value;
};

function solution(S, P, Q) {
    var prefixSums = new Matrix(0);
    
    for (var i = 0; i < S.length; i++) {
        var delta = { A: 0, C: 0, G: 0, T: 0 };
        delta[S.charAt(i)] = 1;
        prefixSums.put('A', i, prefixSums.get('A', i - 1) + delta.A);
        prefixSums.put('C', i, prefixSums.get('C', i - 1) + delta.C);
        prefixSums.put('G', i, prefixSums.get('G', i - 1) + delta.G);
        prefixSums.put('T', i, prefixSums.get('T', i - 1) + delta.T);
    }
    var result = [];
    for (i = 0; i < P.length; i++) {
        var from = P[i];
        var to = Q[i];
        if (prefixSums.get('A', to) - prefixSums.get('A', from - 1) > 0) result[i] = 1;
        else if (prefixSums.get('C', to) - prefixSums.get('C', from - 1) > 0) result[i] = 2;
        else if (prefixSums.get('G', to) - prefixSums.get('G', from - 1) > 0) result[i] = 3;
        else result[i] = 4;
    }
    return result;
}
{% endcodeblock %}