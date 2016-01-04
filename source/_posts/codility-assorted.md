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

## FibFrog

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

And a 100% correct solution, using dynamic programming:

{% codeblock lang:javascript %}
function solution(A) {
    var fibs = [0, 1]
    while (fibs[fibs.length - 1] < A.length + 2) {
        fibs[fibs.length] = fibs[fibs.length - 1] + fibs[fibs.length - 2]
    }

    A[-1] = 1
    var minJumpsToHere = []
    minJumpsToHere[-1] = 0
    for (var i = -1; i <= A.length; i++) {
        if (A[i] === 0 || minJumpsToHere[i] === undefined) continue
        for (var j = 2; j < fibs.length; j++) {
            if (A[i + fibs[j]] === 1 || i + fibs[j] === A.length) {
                minJumpsToHere[i + fibs[j]] = Math.min(
                    (minJumpsToHere[i + fibs[j]] || Number.MAX_SAFE_INTEGER),
                    minJumpsToHere[i] + 1
                )
            }
        }
    }
    return minJumpsToHere[A.length] || -1
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

## CommonPrimeDivisors

NOT CORRECT

{% codeblock lang:javascript %}
function gcd(a, b) {
    if (a % b === 0) return b
    else return gcd(b, a % b)
}

function hasSamePrimeDivisors(a, b) {
    var initialA = a
    var g = gcd(a, b)
    while (g !== 1) {
        a = a / g
        b = b / g
        g = gcd(a, b)
    }
    return initialA % b === 0
}

function solution(A, B) {
    var count = 0
    for (var i = 0; i < A.length; i++) {
        if (hasSamePrimeDivisors(
            Math.min(A[i], B[i]),
            Math.max(A[i], B[i])
        )) count++
    }
    return count
}
{% endcodeblock %}


## CountSemiprimes

This solution is ineffiecient, but correct. It creates a prime sieve, then semiprime sieve, then a prefix sum of semiprimes.

{% codeblock lang:javascript %}
function solution(N, P, Q) {
    var primeSieve = [false, false]
    for (var i = 2; i < Math.sqrt(N); i++) {
        for (var j = i + i; j <= N; j += i) {
            primeSieve[j] = false
        }
    }
    var primes = []
    for (i = 0; i <= N; i++) {
        if (primeSieve[i] !== false) primes.push(i)
    }

    var semiprimesSieve = []
    for (i = 0; i < primes.length; i++) {
        for (j = i; j < primes.length; j++) {
            semiprimesSieve[primes[i] * primes[j]] = true
        }
    }
    var semiprimesPrefixSum = []
    for (i = 0; i <= N; i++) {
        if (semiprimesSieve[i] === true) {
            semiprimesPrefixSum[i] = (semiprimesPrefixSum[i -1] | 0) + 1
        } else {
            semiprimesPrefixSum[i] = (semiprimesPrefixSum[i -1] | 0)
        }
    }

    var solution = []
    for (i = 0; i < P.length; i++) {
        solution.push(semiprimesPrefixSum[Q[i]] - (semiprimesPrefixSum[P[i] - 1] | 0))
    }
    return solution
}
{% endcodeblock %}

Then I optimized it, but still fails some performance tests:

{% codeblock lang:javascript %}
function solution(N, P, Q) {
    var sieve = [false, false]
    // Create a prime sieve
    for (var i = 2; i < Math.sqrt(N); i++) {
        for (var j = i + i; j <= N; j += i) {
            sieve[j] = false
        }
    }
    // Create a semiprime sieve (in the same one, but sets true for semiprimes)
    for (i = 0; i <= N; i++) {
        loop: for (j = i; j <= N; j++) {
            if (sieve[i] === undefined && sieve[j] === undefined)
            sieve[i * j] = true
            if (i * j > N) break loop
        }
    }
    // Prefix sum semiprimes
    var semiprimesPrefixSum = []
    for (i = 0; i <= N; i++) {
        if (sieve[i] === true) {
            semiprimesPrefixSum[i] = (semiprimesPrefixSum[i -1] | 0) + 1
        } else {
            semiprimesPrefixSum[i] = (semiprimesPrefixSum[i -1] | 0)
        }
    }

    var solution = []
    for (i = 0; i < P.length; i++) {
        solution.push(semiprimesPrefixSum[Q[i]] - (semiprimesPrefixSum[P[i] - 1] | 0))
    }
    return solution
}
{% endcodeblock %}

## CountNonDivisible

{% codeblock lang:javascript %}
function solution(A) {
    // count[i] = number of times i appears in A
    var count = []
    // divisors[i] = number of divisors for i
    var divisors = []
    var max = A[0]
    A.forEach(n => {
        count[n] = (count[n] | 0) + 1
        divisors[n] = []
        max = Math.max(n, max)
    })

    // Sieve of E. for calculating divisors for numbers up to max
    for (var i = 1; i*i <= max; i++) {
        for (var j = i; j <= max; j += i) {
            if (divisors[j] !== undefined) {
                if (divisors[j].indexOf(i) < 0) divisors[j].push(i)
                if (divisors[j].indexOf(j / i) < 0) divisors[j].push(j / i)
            }
        }
    }

    var solution = []
    A.forEach(n => {
        // We take every divisor, see how many times it appears in A, and add them all up
        var divisorsForElement = 0
        divisors[n].forEach(d => divisorsForElement += (count[d] | 0))
        // Then we subtract number of divisors from total number of elements in A
        solution.push(A.length - divisorsForElement)
    })
    return solution
}
{% endcodeblock %}

## NumberSolitare

{% codeblock lang:javascript %}
function solution(A) {
    var max = [A[0]]
    for (var i = 0; i < A.length; i++) {
        for (var j = 1; j <= 6; j++) {
            if (i + j >= A.length) continue
            max[i + j] = Math.max(
                (max[i + j] || Number.MIN_SAFE_INTEGER),
                max[i] + A[i + j]
            )
        }
    }
    return max[A.length - 1]
}
{% endcodeblock %}
