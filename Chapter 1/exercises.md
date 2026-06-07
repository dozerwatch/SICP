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
1. 0
2. undefined
3. Applicative: (= 0 0) is true, so test returns the consequent expression and doesn't evaluate the alternative expression.
Normal: The interpreter would not evaluate the if statement and will continuously sub (p) for (p).
