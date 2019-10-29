# Section 4: Asymptotics and Array Lists
Raymond Guo

Disclaimer: I'm a little rusty in 61B and actually relearning with y'all :). These are just my personal notes, so there may be errors in this document; if so, let me know! Also, this isn't affilianted with CSM or any Berkeley organization :).

## Asymptotics (Q1 & Q2)
In computer science, we often deal with data on the extremely large scale; whenever this is the case, we usually have two scarce resources: time and space. Thus, to determine how "good" an algorithm, we usually try to estimate the amount of time and space an algorithm will take, on the conceptual large scale. This is what we call *analysis of algorithms*. We call the estimated amount of time an algorithm takes to run the **runtime complexity**, and the estimated amount of space the **space complexity**. For this week, we'll focus on time complexity.

There are typically 3 ways we measure the runtime complexity (this also expands to space complexity as well):
- Big Omega: Describes the "slowest" a program can run (lower bound).
- Big O: Describes the "fastest" a program can run (upper bound).
- Big Theta: Describes how a program runs in general (tight bound).

### Some General Rules
When we are considering runtime, we get rid of the following:
- Coefficents
- Lower Degree Terms
- Bases of Logarithms

This means that `O(5n^2 + 3n + 2)` actually is the same runtime `O(n^2)`. 
Also, the order of the most common runtimes, from fastest to slowest, is:
> O(1) [constant] < O(log(n)) [logarithmic] < O(n) [linear] < O(n^k) [polynomial] < O(k^n) [exponential]

### Big Omega (Ω)
We say that Big Omega provides the lower bound of an algorithm's runtime. For example, let's say an algorithm has a runtime of `Ω(n)`. Notice that there aren't any units; that's because we're just comparing the rate that the runtime of the algorithm grows as `n` increases; more specifically for this example, we're saying that as `n` increases, the runtime of the algorithm grows at the same rate or slower than the rate that `k ⋅ n` is increasing. 

More generally, the mathematical definition of Big Omega is: 
> For an algorithm with runtime R(N):
R(N) ∈ Ω(f(N)) → ∃ k, k ⋅ f(N) ≤ R(N)

Visually, this means that the the runtime of the algorithm is always "above" f(N).
![image not appearing](https://cdn.kastatic.org/ka-perseus-images/c02e6916d15bacae7a936381af8c6e5a0068f4fd.png)

### Big O
Big O provides the upper bound of an algorithm's runtime. Let's say an algorithm has a runtime of `O(n)`. This means that as `n` increases, the runtime of the algorithm grows at the same rate or faster than the rate that `k ⋅ n` is increasing. We typically care about the Big O of an algorithm much more than the Big Omega, since it provides us with the "worst case scenario" of how much time an algorithm will take to run.

The mathematical definition of Big O is: 
> For an algorithm with runtime R(N):
R(N) ∈ O(f(N)) → ∃ k, k ⋅ f(N) ≥ R(N)

Visually, this means that the the runtime of the algorithm is always "below" f(N).
![image not appearing](https://cdn.kastatic.org/ka-perseus-images/501211c02f4c6765f60f23842450e1151cfd9c89.png)

### Big Theta (Θ)
Big Theta provides the tight bound of an algorithm's runtime. What this means is that the algorithm's runtime growth can be modelled with a single function. As you may guess, even though every algorithm has a (or infinite) Big O and a Big Omega runtime (since the bounds can be arbitrarily loose), not every algorithm has a Big Theta runtime. 

Let's say an algorithm has a runtime of `Θ(n)`. This means that as `n` increases, the runtime of the algorithm grows at the same rate that `k ⋅ n` is increasing. If there exists a function `f(n)` where the algorith has both a Big O and Big Omega of `f(n)`, then the algorithm has a Big Theta of `f(n)` as well. Otherwise, if there does not exist a function that can satisfy both the upper bound and lower bound at once, then the algorithm doesn't have a Big Theta runtime.

The mathematical definition of Big Theta is: 
> For an algorithm with runtime R(N):
R(N) ∈ Θ(f(N)) → ∃ k_1, k_2, k_1 ⋅ f(N) ≤ R(N) ≤ k_2 ⋅ f(N)

Visually, this means that the the runtime of the algorithm is always "below" f(N).
![image not appearing](https://cdn.kastatic.org/ka-perseus-images/2bdc25c7eda8486d05b8031c5a63535684ecb5a1.png)

### Calculating Runtime - Iterative Algorithms
In general, calculating the runtime of an iterative algorithm is typically more simple than for recursive algorithms. The general rule of thumb is to trace the execution of the program and count the number of times an operation is being looped over. For example, if you're iterating over list, the runtime is probably `O(n)`. If you're iterating over a 2D matrix (with dimension `m x n`), the runtime is probably `O(mn)`. However, be careful! Just like on the worksheet, sometimes the program terminates early, leading to a much faster runtime than one would expect. So in the end, make sure to not just look at the number of `for` or `while` loops; instead, trace through the execution!

### Calculating Runtime - Recursive Algorithms
The key to calculating the runtime of a recursive algorithm is to draw a tree. Each node in the tree represents a new call to the recursive function, with the value of the node being the input size (usually in terms of `n`, the original input size). There are 3 main steps in calculating the runtime:
1. Draw the tree. Get the general shape.
2. Determine the tree's height. 
    - If the `parent -> child` relationship is `n -> n-1`, then the height is most likely `n`. One example of this is Fibonacci.
    - If the `parent -> child` relationship is `n -> n/2`, then the height is most likely `log(n)`. One example of this is Binary Search.
    - If the `parent -> child` relationship is `n -> sqrt(n)`, then the height is most likely `log(log(n))`. This is typically more rare than the other two.
    - There are others, but these 3 are the most common ones I personally run into.
3. Determine the amount of work that's being done at each node. This is equal to the amount of work that's being done in each function call, outside of the recursive relations.
4. From step 3, determine the amount of work being done at each level of the tree. Just multiply the amount of work done at each node by the number of nodes in that level.
5. Sum up all the levels from step 4, taking into account the number of levels (from step 2) to determine the runtime. Here are a few useful sums to know:
    - `1 + 2 + 3 + ... + n = O(n^2)`
    - `1 + 4 + 9 + ... + n^2 = O(n^3)`
    - `1 + 2 + 4 + ... + 2^n = O(2^n)`
    - `1 + 2 + 4 + ... + n = O(n)`
    - `1 + 2 + 4 + ... + 2^(log n) = O(n)`

#### Example: Fibonacci
In section, I went over Fibonacci as an example. Recall the code for Fibonacci:
```
static int fib(int n)  { 
    if (n <= 1) 
        return n; 
    return fib(n-1) + fib(n-2); 
} 
```

Let's draw out a tree with a sample input `7`: 

![image not appearing](https://i.stack.imgur.com/QVSdv.png)

If we generalize this with an arbitrary input `n`, we get:

![image not appearing](https://www.ibm.com/developerworks/library/l-performance-of-inline-assembly-trs/image001.jpg)

Now, to determine the tree's height, we notice that the `parent -> child` relationship is `n -> n-1`, so the height is `n`.

At each node, we're only doing addition, which is a `O(1)` operation. Thus, at level `k`, the runtime of that level is `O(2^k)`, since there are `O(2^k)` nodes and each node is running at `O(1)` time. 

Now, we just need to sum all the levels up: `2^0 + 2^1 + 2^2 + ... 2^n = 1 + 2 + 4 + ... + 2^n = O(2^n)`, so the overall runtime of recursive Fibonacci is `O(2^n)`.

## Array Lists (Q3)
In this class, we'll mainly be dealing with two types of lists: linked lists and array lists. As we learned before, linked lists are essentially nodes that are connected to each other through a `next` pointer. Array lists, on the other hand, are lists that are in essence arrays "behind-the-scenes" -- behind every array list lies an array. 
![image not appearing](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAATYAAACjCAMAAAA3vsLfAAABy1BMVEX///9/f38AAADt7e1ubm7a2tqKiorp6el1dXWioqLGxsZ6enqFhYX5+fne3t67u7u1wM+ZqL6tra3MzMxnZ2ebm5sZOV1ge5+RkZHCwsKnp6cUKUEbQWrt3cdzc3OxsbG8qJWEkKbk9OeEb29pOxH///bi38q3zdjl0LYhUofR3/PU8f9qh6r1//9vbn93dYHg6vgAN2QURHUuAAAZAAAAACoAACJcOQDn9////+oAABYAADeInK/DzNkARH0IDRMPGieThYPfyaf77eEvWYqmn6fSxbTGua/N3efO6v+UbzIAT4b11LVbIgBJCAD/8NWlxdm4l2gAMD5SPClNa46fgF8yQ1VURi1cfJowNT5PJABjj7pwUTBGV2tgT0BOGABzoL/KrYuGYDYUTXCde0gXNjGqxuafg24qOlU5V3eIaVJIfagAAEu1moEyDwCfkYA/OzpFKyI2Hy7UuJSMrMh4hpZVS12hw8REPDRkUVSvkHCNbkdWbomnhF5DRGpEMEpfPjimnYlzUjxnUSujjG5bf44AJE0pLkSFbkwAADBcQRIyAADm5MyCcWEwHiU1Lh4AIlZqcF5KOUc/JQpIQUcyVG1kgItwk5KejWJT3b5mAAALKUlEQVR4nO2djV8TRx6Hh4l53byQV5LAEtAGAhIRRA2aKBBeqxY8gUIBS9RDwFJiJZ5C8A601dpqe4r1/tyb2RDY2d3Z5aMIaH+PEmTznd3ZJzOzk5CJCAEAcORxka9sX7L0Q3+l/K5sMCUlaGRg8KArdqQRcTNCV/FQ6aevr8jvu4pP0m/XqhCKfzNV2lYH+iiSNmSlTcqE0J0m5CLfTOSL/GNb21ct5Mbkoptdw3jEdaj1PSJI2hL45PV/3MCj6E5v55kqNDb+7QRKdODJXW3x76bi0zM3vd9jPHvINT4S7Gg7m8pdSN3pvTWBes6lRDx0rYhyMm3np67iRoQ68chh1/hIsKPtNkoQbf8ko1zurMVpOd4xh4YZbegunh8EbSVEXOl272rrzZ1L9eBGkwddW7AubmubsLqJtngSfX+vE9fD2IaoNox/EEvaLqfIJeHW7eQSxhNI/BEvl7RNk0gl6aQX8uOn0RiMbQAAAAAAAJ8NVkO0p8QfWM51wOU+Ef5jNgNq7VrlIsblYlrlYobljkW0ytlrDY8X3k8vBvg1pTCJoNZWj6YUORGf1lafphQ5MY/W1qDfqJzbYpTg49JBs4C2FOOER1OKccKnKcU4YazNrK1tL0b0ulyt5ul/6dqCOkaOlffpECr4aPb9L11bWEeI4NgOqbUJQoUNtMmxUScG2oTGuvrfbXvW1i94EYp3t6a4iTKMlAEhnUTZ7tZBbmIHRko8LP2Sbfk0N7GDQtuS+gXWPWprPX6isU7Q1CYIVCj9S7U1cLS5wm6FlNyCNZKM369syMg2qrRZw1aFFI91aRb5B90/yXWrtLnMSCHlwUUqerhjRR5TaTPbXR+kTTq/cLnT2QQqpELgaxO8PrO/otAvCI4TOtqQv8LukkuJ/0Qf/NyKIqVqbQ7B51JML8Qqert4UrZJpc0kBE2slBMNE1TB4MOkbKN6ihITIipt/+o+lWJT6tbmF+yukrZWf591wObdEAoDp/jaRrKWTP+EaJPM7mircCqwOAUhLJu3iQt387Nore/R+Gnm4Kpy5HELy+dtov1YCsV9y155pSNhv4PBH3EKHrtM2/AEepxE8YdotVkuSV3PCpvFyWrLzaca5pPMJrdN4/zKPTPVb0kuj9QJ6QY9bXW2tFhQarO4lQRrfR5ZWxKrU+jW0K0VlBiVawuryoVtsZhcW9ZxlzRRfyQgbwARp11B1GJzhmVtaTVtXptDiaJ5qUWuzac8nNkpOBStLUc66QtmSERmp8b5xSzlAc3WuGSkjWwn2rK1rDbV2OYknV/eBYcXSH3m1ptR5xO5NmUnNVWQwU3RBeP/Tm6fzA7qS0JM8DDPAf5jD24soLH3QftzWcNRjW1uW9Sl7KT0SJsKbaqxjZ5fuKxNoNpqjbW1pgYynbpjGx2iGSlr9QPPk2LIsSgf3lTapKFdLmU47bjbguoiA5PyE1Fpc9tNjJSee+Rm0z1Pbq/P7cZU2kxkZFdpq448G2Vj6rGN1nNXW/NSOu47LfK1VQSXhUJUKDjsPsG+XPAJe56ADJDHFWXtGX6iDDsBiWZosaiLm9hBJqWfShb99GDZjGZChkKb2+qrVCT0JiBCrELoWxaijnC0NUac8Odt0vRDuuiWt33h013tp/LhXRvS3L/0R6mtVuCjuVsNKZVehvoJ6ZviwT0C2lyKehZL361MyKIjpLaszeTRQfPx0tB2KUQohnZoP0VvL7GhI6DNeikko2a7xu3s4+vXM2JV7XOvaGhrwoQ+vMPFZ+Smq4kNHYHX20xNpfodozeFye7STzXKMe+T4LcoJqUOR+9l+oaH4LOQfeNcuO5ctG9VS5tyMqvEH9TWFjQqF9bWFlUlPb2SpwuZxZnoZuvBarMq56R2u6StDwfT3dGblv6Zpz9otTa3upwSzUHBbFzOrFXOr84FS9pwGAfSqwfc2jRo2tb282I+vRmZXy5qtbYjQLmTBt/ULawW8oet7Uo1YaZ65uxE4JdC4GwxsEU3XDEueMBYpYpWV4cCC61b4/PjpZ/aD0tblTaHVBs+Ls1qjmaMSwIAAAAAAAAAAAAAAPwNyTpOc+/yuxASlW84ASjreIJ3VwITo4nnJW2JmQOr0mdA/EzruSTKRkyZrMOUQQ2eQUR/6+2gbyOTtGX9SWT2VGZfVEeShnv72/Dg4vEzK6gHT46Sr6qeri08kvs1KUofcSFp68Gp6794TiXw5XHQtsNXLWjtNnEzhx7QZfOeyMt7nXgu10bvK2v7OkTa3vXbh13VI4Qo/e5mqAefRA9+TaG1ov/pPXRn4pr0BqKytnh3Rxtok7P+Z8qU/W2KflLKg3Op+PniQMc94vKidBlI4D5fJdHmp+5yF/qgk24Tl96XVlf8PZBCCTLTEB+nN8iW86WLqxgIBKrEQHI1MJNBrlenQJsuCdxsHAIUrMOHVgDAUcL0JXFgy5H9xwyx7UOitvbjE3s4jPOgtDmihpHAPiR8jo9PmATDhM2wIvuEw3AJ9160GXaOqOE7SY0TVkNtxon9ArR9EKDtgwBtHwSrre715CDK3swX5Bvl2vofTaYRWswzCyEV2rJn6HOz1V75NrmU+Mt8fgXluvLjKU4Ciff/mCVPmNnDyKVk7+fzHSNLk/lHf3ISnxZGW2KBfn7xy2bEaJBrMyE0fZLc3TPFJJj84n+JNrGWWRfGaJNeXs/NIcRNvEmizaHcLLveUillkVpPrOgkPh2MtluD7hS6Oppl37DOdtL4Y/WCZUbbg9nrzSTVydf2kByF7MGNeIksXW+5sj6E4s9lCYWUq+rlz4ekbbrVcr+5Z8b5ivmtjFxb9tvLIwgt5Z8wLyLJtXXO0KXFqyM62jYsf8wh0fJqgZdAa4WBtyvi89grnS64Rl+3F5mjHJK271JoeLSnhVRJPqYoLglrdORieg+j7ZbXPOYVF8yJJ/L1SYoBv5MuCUXrI9zEQIZ04qzHLTfLSskW6e3YEOImPiWMtrE5lKiiK5I3dbRdpyfLrFZmtNmDwZtFfzC8/LxRllBoG5Z0rA3yE/E3dOQiw9surJRVqRrsbyEPSVv2UeBhCq2ObzEr/+Xa1gNbE0jcCmwxL2QqJiDSyuJO7pV0eCsw2YyeBfJpXoLemSGHuckMFowUetUg7tjryqHN26TzV3wmFNPapPuUHxqlOW9jtjFSXFpHYVubVoKV4lId4/Ob7hom9uOpvLEU4yf7+4Wetu3FvkUv/z9JyDT19pK/vU38BThuGqFf3g9PlBZI08XG/OOQRFo/sY/oacu0h0JT4baa9npuxFvT9c78qKtLZ5Vcpr2rOh173RXiSpESvq6uUJqXQCa67rhlPlTDXchFEm2zleOhmlFeYj/Re71tsB2/e/HXNOafMKoMYdw6jXEN/zEme8FYdy9SovBW7ziuJozHHd/gaq42mqArHfmJ/cRhs3Ap9OK/Xi4s4lCRGynWkBO+QbQV9PaCz2506OyFJt7rJizOXnwh/P43XD2qk8D1sbcHpM1l5uKubMc/P323iUP1bl4kTbW9JNq83AjZS1ewA/P3QhOv8fsbOscx+3vxaF3jIpHCi/jpyuS2/x2QNj1I5+mKLpNWwB9zvCE8bg+e0VsBnGnHM/aNH3W6YIaKfYX1Oqm07pi2Nt1OGvYdVGvTI3OppqatrUb3ktC+HdHRVt4LV75xggz4NSRDItwB31TeyYFcEvRwVW7Dn4BYyxHTR+xlD8dB5YjOBMQwAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAHzZ/B/l5fKJSsAQmgAAAABJRU5ErkJggg==)

The biggest functionality that an array list has that an array doesn't is the ability to add elements. Arrays aren't able to do this because they aren't resizeable. Thus, in order to overcome this, whenever the underlying array of an array list becomes full and a new element is inserted, the array list goes through a resizing procedure:
1. Make a new array with double the size.
2. Copy over the elements from the old array to the new array.
3. Delete the old array.

 The advantage of linked lists is that it makes appending to the beginning / end of the list extreme fast (`O(1)` time). Removing is also a lot faster for linked lists. However, array lists are very fast at retrieving elements at a specified index (`O(1)` time), whereas linked lists have to iterate through every node from the beginning to reach the right index before returning the value (`O(n) time`). 
 
 Here's the runtimes for linked list, array, and array list (called dynamic array below):
 ![image not appearing](https://i.stack.imgur.com/Ly4Fp.jpg)
