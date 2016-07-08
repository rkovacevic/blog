layout: post
title: Codility - Lesson 8 - Prime and composite numbers
date: 2015/10/26
tags: [codility,algorithms,javascript]
---

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

{% codeblock lang:javascript %}
function solution(A) {
    var peaks = [0]
    for (var i = 1; i < A.length - 1; i++) {
        if (A[i - 1] < A[i] && A[i] > A[i + 1]) peaks.push(peaks[i - 1] + 1)
        else peaks.push(peaks[i - 1])
    }
    peaks[-1] = 0
    peaks[peaks.length] = peaks[peaks.length - 1]
    loop: for (i = peaks[peaks.length -1]; i > 0; i--) {
        if (A.length % i !== 0) continue loop
        var blockLength = A.length / i
        for (var j = 0; j < A.length; j += blockLength) {
            if (peaks[j - 1 + blockLength] - peaks[j - 1] === 0) continue loop
        }
        return i
    }
    return 0
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
