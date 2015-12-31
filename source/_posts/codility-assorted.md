layout: post
title: Codility - Assorted lessons
date: 2015/12/27
tags: [codility,algorithms,javascript]
---

> Disclaimer: If you're using this to cheat on job interviews, it's not my fault, however it goes. If you're catching someone cheating, make sure it's not me.

These are solutions to some assorted Codility problems.

## BinaryGap

Make sure you take into account that only gaps that are bordered by '1' count. For example, binary number 1000 does not have any gaps, and should return 0.

{% codeblock lang:javascript %}
function solution(N) {
    var counter = -1
    var maxCounter = 0
    while (N > 0) {
        if (N % 2 === 0 && counter >= 0) {
            counter++
            if (counter > maxCounter) maxCounter = counter
        } else {
            if (N % 2 !== 0) counter = 0
        }
        N = Math.floor(N / 2)
    }
    return maxCounter
}
{% endcodeblock %}

## OddOccurrencesInArray

{% codeblock lang:javascript %}
function solution(A) {
    var  pairs = []

    A.forEach(n => {
        pairs[n] = (pairs[n] | 0) + 1
    })

    var keys = Object.keys(pairs)
    for (var i = 0; i < keys.length; i++) {
        if (pairs[keys[i]] % 2 !== 0) return parseInt(keys[i], 10)
    }
}
{% endcodeblock %}

## PermMissingElem

{% codeblock lang:javascript %}
function solution(A) {
    var sum = (A.length + 1) * (A.length / 2)
    var elementsSum = 0
    A.forEach(n => {
        elementsSum += n
    })
    return  sum + (A.length + 1) - elementsSum
}
{% endcodeblock %}

## AbsDistinct

{% codeblock lang:javascript %}
function solution(A) {
    var count = 1
    var arr = []
    if (A[0] < 0) arr[Math.abs(A[0])] = true
    for (var i = 1; i < A.length; i++) {
        if (A[i] < 0) {
            arr[Math.abs(A[i])] = true
            if (A[i - 1] !== A[i]) count++
        } else {
            if (A[i - 1] !== A[i] && arr[A[i]] !== true) count++
        }
    }
    return count
}
{% endcodeblock %}

## MinAbsSumOfTwo

{% codeblock lang:javascript %}
function abs(A, back, front) {
    return Math.abs(A[back] + A[front])
}

function solution(A) {
    A.sort((a, b) => a - b)

    if (A[0] >= 0) return 2 * A[0]
    if (A[A.length - 1] <= 0) return -2 * A[A.length - 1]

    var back = 0
    var front = A.length - 1
    var min = abs(A, back, front)
    while(back <= front) {
        var current = abs(A, back, front)
        min = Math.min(current, min)

        if (abs(A, back + 1, front) < current) back++
        else if (abs(A, back, front - 1) < current) front--
        else {
            back++
            front--
        }
    }
    return min
}
{% endcodeblock %}

## TieRopes

{% codeblock lang:javascript %}
function solution(K, A) {   
    var count = 0
    var currentRopeLength = 0

    for (var i = 0; i < A.length; i++) {
        if (A[i] + currentRopeLength >= K) {
            count++
            currentRopeLength = 0
        } else {
            currentRopeLength += A[i]
        }
    }

    return count
}
{% endcodeblock %}

## FibFron

About 80% correct, because it's a greedy solution.

{% codeblock lang:javascript %}
function solution(A) {

    var fibs = [0, 1]

    while(fibs[fibs.length -1] <= A.length) {
        fibs.push(fibs[fibs.length - 1] + fibs[fibs.length - 2])
    }

    var position = -1
    var fib = fibs.length - 1
    var hops = []
    while(position >= -1 && position !== A.length) {
        if (A[position + fibs[fib]] === 1 ||
            position + fibs[fib] === A.length) {
            position += fibs[fib]
            hops.push(fib)
            fib = fibs.length - 1
        } else {
            if (fib > 2) {
                fib--
            } else {
                fib = hops.pop()
                position -= fibs[fib]
                fib--
            }
        }
    }
    if (position === A.length) return hops.length
    return -1
}
{% endcodeblock %}

## MinPerimeterRectangle

{% codeblock lang:javascript %}
function solution(N) {
    var min
    for (var i = 0; i <= Math.ceil(Math.sqrt(N)); i++) {
        if (N % i === 0) {
            if (min === undefined) min = 2 * i  + 2 * (N / i)
            else min = Math.min(min, 2 * i  + 2 * (N / i))
        }
    }
    return min
}
{% endcodeblock %}

## MaxNonoverlapingSegments

{% codeblock lang:javascript %}
function solution(A, B) {
    var count = 0
    var currentEnd
    for (var i = 0; i < A.length; i++) {
        if (currentEnd === undefined || currentEnd < A[i]) {
            count++
            currentEnd = B[i]
        }
    }
    return count
}
{% endcodeblock %}
