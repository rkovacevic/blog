layout: post
title: Codility - Lesson 8 - Prime and composite numbers
date: 2015/10/26
tags: [codility,algorithms,javascript]
---

> Disclaimer: If you're using this to cheat on job interviews, it's not my fault, however it goes. If you're catching someone cheating, make sure it's not me.


## 01 CountFactors

{% codeblock lang:javascript %}
function solution(N) {
    if (N === 1) return 1;
    var sqrtN = Math.sqrt(N);
    var count = 1;
    for (var i = 2; i < sqrtN; i++) {
        if (N % i === 0) count++;
    }
    var square = Number.isInteger(sqrtN) ? 1 : 0;
    return count * 2 + square;
}
{% endcodeblock %}

## 02 MinPerimeterRectangle

{% codeblock lang:javascript %}
function solution(N) {
    var greatestPrimeDivisor = 1;
    for (var i = 1; i <= Math.sqrt(N); i++) {
        if (N % i === 0) greatestPrimeDivisor = i;
    }
    return 2 * (greatestPrimeDivisor + N / greatestPrimeDivisor);
}
{% endcodeblock %}

## 03 Peaks

Correct, but inefficient solution, that tests every divisor, by checking if each section contains peaks.

{% codeblock lang:javascript %}
function hasPeak(peaks, from, to) {
    for (var i in peaks) {
        var peak = peaks[i];
        if (peak >= from && peak < to) return true;
    }
    return false;
}

function solution(A) {
    var peaks = [];
    var divisors = [];
    for (var i = 1; i < A.length - 1; i++) {
        if (A.length % i === 0) divisors.push(i);
        if (A[i - 1] < A[i] && A[i] > A[i + 1]) peaks.push(i);
    }
    divisors.push(A.length);

    divisorsLoop: for (i = divisors.length - 1; i >= 0; i--) {
        var d = divisors[i];
        if (d <= peaks.length) {
            var x = 0;
            while (x < A.length) {
                if (!hasPeak(peaks, x, x + (A.length / d))) continue divisorsLoop;
                x += A.length / d;
            }
            return d;
        }
    }
    return 0;
}
{% endcodeblock %}

Here's a better one, that instead of checking each group (or section), goes through the peaks, and checks if there's a peak for each group.

Still not good enough, only gets 60% for performance, but I'm moving on for now.

{% codeblock lang:javascript %}
function isPeakInEachGroupForDivisor(A, peaks, d) {
    if (peaks.length === 0) return false;
    var currentGroup = 0;
    var groupsWithPeaksFound = 1;
    for (var i in peaks) {
        var peak = peaks[i];
        var currentGroupStart = currentGroup * (A.length / d);
        var currentGroupEnd = (currentGroup + 1) * (A.length / d);
        if (peak >= currentGroupStart && peak < currentGroupEnd) continue;
        currentGroup++;
        groupsWithPeaksFound++;
    }
    return groupsWithPeaksFound === d;
}

function solution(A) {
    var peaks = [];
    var divisors = [];
    for (var i = 1; i < A.length - 1; i++) {
        if (A.length % i === 0) divisors.push(i);
        if (A[i - 1] < A[i] && A[i] > A[i + 1]) peaks.push(i);
    }
    divisors.push(A.length);

    for (i = divisors.length - 1; i >= 0; i--) {
        var d = divisors[i];
        if (d <= peaks.length) {
            if (isPeakInEachGroupForDivisor(A, peaks, d)) return d;
        }
    }
    return 0;
}
{% endcodeblock %}

## 04 Flags

** NOT DONE ** This solution is incorrect, maybe I'll come back to this.

{% codeblock lang:javascript %}
function solution(A) {
    var peaks = []
    var lastPeak
    var numberOfPeaks = 0
    for (var i = 1; i < A.length - 1; i++) {
        if (A[i-1] < A[i] && A[i] > A[i+1]) {
            numberOfPeaks++
            if (lastPeak !== undefined) {
                peaks.push(i - lastPeak)
            }
            lastPeak = i
        }
    }
    if (numberOfPeaks === 0) return 0
    if (numberOfPeaks === 1) return 1

    var numberOfFlags = Math.min(peaks.length + 1, Math.floor(Math.sqrt(A.length)))

    while (numberOfFlags > 0) {
        var placedFlags = 1
        var distanceFromLastFlag = 0
        for (i = 0; i < peaks.length; i++) {
            distanceFromLastFlag += peaks[i]
            if (distanceFromLastFlag >= numberOfFlags) {
                placedFlags++
                distanceFromLastFlag = 0
            }
        }
        if (placedFlags >= numberOfFlags) return placedFlags
        numberOfFlags--
    }

    return numberOfFlags
}
{% endcodeblock %}
