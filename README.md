Write a function, `infix->prefix`, which takes an infix arithmetic expression
(given as a Scheme list) and returns the corresponding prefix expression.
It gives an error if you attempt to operate on an empty list.
You may assume that all sub-expressions are parenthesized so that you don’t
need to worry about precedence.

Here are some test cases:
```scheme
> (infix->prefix 42)
42
> (infix->prefix '(1 + 2))
(+ 1 2)
> (infix->prefix '(1 + (2 * 8)))
(+ 1 (* 2 8))
> (infix->prefix '((((2 + 3) * 2) / 5) + (17 - 1))
(+ (/ (* (+ 2 3) 2) 5) (- 17 1))
> (infix->prefix '())
infix->prefix: Cannot process an empty expression
```

# Starter code
```scheme
#lang scheme
(define infix->prefix
  (lambda (expr)
    (cond
      ((null? expr)
       (error "Cannot process an empty expression")))))
```

# Hints
We assume all operators operate on two operands. Given an expression `'(1 + 2)`, we can get the operands and the operator using `(car lst)`, `(caddr lst)`, and `(cadr lst)` respectively.
```
(car '(1 + 2)) => 1
(caddr '(1 + 2)) => 2
(cadr '(1 + 2)) => '+
```

Our goal is to turn `'(left operator right)` to `'(operator left right)`. "left" and "right" can, themselves, be nested expressions of the form '(left operator right). But, we are defining a procedure that can turn `'(left operator right)` into `'(operator left right)`, so we can recursively apply the procedure to solve such sub-problems for us and put their results back together to form the solution to the parent problem. When an expression is simply a number (as shown in the first test case), its prefix notation is the same as its infix notation. You can use `number?` predicate to test whether a variable is a number:
```scheme
(number? '1) => #t
(number? 1) => #t
```

Here are two equivalent ways to construct a list:
```scheme
(cons 'a (cons 'b (cons 'c '()))) => '(a b c)
(list 'a 'b 'c) => '(a b c)
```
