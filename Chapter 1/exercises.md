## Exercise 1.1: 

Below is a sequence of expressions. What is the result printed by the interpreter in response to each expression? Assume that the sequence is to be evaluated in the order in which it is presented.

```scheme
10 --> 10
(+ 5 3 4) --> 12
(- 9 1) --> 8
(/ 6 2) --> 3
(+ (* 2 4) (- 4 6)) --> 6
(define a 3) --> 
(define b (+ a 1)) --> 
(+ a b (* a b)) --> 19
(= a b) --> #f
(if (and (> b a) (< b (* a b)))
    b
    a) --> 4
(cond ((= a 4) 6)
      ((= b 4) (+ 6 7 a))
      (else 25)) --> 16
(+ 2 (if (> b a) b a)) --> 6
(* (cond ((> a b) a)
         ((< a b) b)
         (else -1))
   (+ a 1)) --> 16
```

## Exercise 1.3: 

Define a procedure that takes three numbers as arguments and returns the sum of the squares of the two larger numbers.

```scheme
(define (x a b c)
  (cond ((and (< a b) (< a c)) (sum-of-squares b c))
        ((and (< b a) (< b c)) (sum-of-squares a c))
        (else (sum-of-squares a b))))
```

## Exercise 1.4: 

Observe that our model of evaluation allows for combinations whose operators are compound expressions. Use this observation to describe the behavior of the following procedure:

```scheme
(define (a-plus-abs-b a b)
  ((if (> b 0) + -) a b))
```
					
If b > 0, we add a and b. Otherwise, we subtract b from a.

## Exercise 1.5: 

Ben Bitdiddle has invented a test to determine whether the interpreter he is faced with is using applicative-order evaluation or normal-order evaluation. He defines the following two procedures:

```scheme 
(define (p) (p))

(define (test x y) 
      (if (= x 0) 
      0 
      y))
```

Then he evaluates the expression:

```scheme 
(test 0 (p))
```

1. What behavior will Ben observe with an interpreter that uses applicative-order evaluation? 
2. What behavior will he observe with an interpreter that uses normal-order evaluation? 
3. Explain your answer. (Assume that the evaluation rule for the special form if is the same whether the interpreter is using normal or applicative order: The predicate expression is evaluated first, and the result determines whether to evaluate the consequent or the alternative expression.)
---
1. undefined
2. 0
3. - Applicative: Because we have to evaluate the subexpressions of the combination first, evaluating `(p)` forces us into an infinite loop.

   - Normal: Substituting the operand expressions in for the formal parameters, we get an `if` case analysis. Since `(= 0 0)` returns true, we ONLY evaluate the consequent expression `0`, ignoring the infinite loop of `(p)`.

## Exercise 1.6: 

Alyssa P. Hacker doesn’t see why `if` needs to be provided as a special form. “Why can’t I just define it as an ordinary procedure in terms of cond?” she asks. Alyssa’s friend Eva Lu Ator claims this can indeed be done, and she defines a new version of `if`:

```scheme
(define (new-if predicate 
                then-clause 
                else-clause)
  (cond (predicate then-clause)
        (else else-clause)))
```

Eva demonstrates the program for Alyssa:

``` scheme
(new-if (= 2 3) 0 5)
> 5

(new-if (= 1 1) 0 5)
> 0
```

Delighted, Alyssa uses `new-if` to rewrite the square-root program:

```scheme
(define (sqrt-iter guess x)
  (new-if (good-enough? guess x)
          guess
          (sqrt-iter (improve guess x) x)))
```

What happens when Alyssa attempts to use this to compute square roots? Explain.

---

`new-if` is a function while `if` is a special form. This means that every subexpression in the `new-if` combination will be evaluated before `new-if` is applied, including `sqrt-iter`. This will recursively call itself with no way to stop, resulting in an infinite loop. 

## Exercise 1.7 **

The `good-enough?` test used in computing square roots will not be very effective for finding the square roots of very small numbers. 

Also, in real computers, arithmetic operations are almost always performed with limited precision. This makes our test inadequate for very large numbers. 

1. Explain these statements, with examples showing how the test fails for small and large numbers. 

An alternative strategy for implementing `good-enough?` is to watch how guess changes from one iteration to the next and to stop when the change is a very small fraction of the guess. 

2. Design a square-root procedure that uses this kind of end test. Does this work better for small and large numbers? 

---

`good-enough` is not effective for finding the square roots of very small numbers, because the absolute error tolerance of `0.001` is too large for very small numbers. Hence, `improve` doesn't run for many iterations.

On the other hand, the absolute error tolerance is too small for very large number. The computer runs out of significant digits before the programs gets an error less than `0.001`. This causes all further iterations of `improve` to be the same as the last iteration, leading to an infinite loop.

```scheme
(define (good-enough? prev-guess guess)
  (< (abs (/ (- guess prev-guess) guess)) 0.000000001))

(define (sqrt-iter guess x)
  (if (good-enough? guess (improve guess x))
      guess
      (sqrt-iter (improve guess x) x)))
```
This works better for both small and large numbers.

## Exercise 1.8: 

Newton’s method for cube roots is based on the fact that if $y$ is an approximation to the cube root of $x$, then a better approximation is given by the value

$$ \frac{x / y^2 + 2 y}{ 3 }. $$

Use this formula to implement a cube-root procedure analogous to the square-root procedure.

---

```scheme
(define (improve y x)
  (/ 3 (+ (/ x (* y y)) (* 2 y))))

(define (cube x) 
  (* x x x))

(define (good-enough? prev-guess guess)
  (< (abs (/ (- guess prev-guess) guess)) 0.000000001))

(define (cube-iter guess x)
  (if (good-enough? guess (improve guess x))
      guess
      (cube-iter (improve guess x) x)))

(define (cube-root x)
  (cube-iter 1.0 x))
```
