## Midterm Exam Preparation

The problems below are similar to what you can expect for the midterm exam.


### Structural Recursion and Induction.

1. Use structural recursion to define a function

   `take: NN x List -> List`

   that takes a natural number `n` and a list `l`, and returns the 
   list consisting of the first `n` values of `l`. If `n` is greater than the
   length of `l`, then `take` returns `l` itself. For example,

   `take(2, 1 :: 4 :: 2 :: 5 :: nil) = 1 :: 4 :: nil`

   `take(3, 5 :: 1 :: nil) = 5 :: 1 :: nil`

1. Transform your function from part 1. into a Scala function

   ```scala
   def take(n: Int, l: List): List
   ```
   
   where `List` is the Scala representation of lists of integers:

   ```scala
   enum List:
     case Nil
     case Cons(hd: Int, tl: List)
   ```

   Is your function tail-recursive? If not, make it tail-recursive.


1. Using the definitions of the functions append and reverse given in
   class, prove the following properties by structural induction:

   a. for all lists `l`: `append(l, nil) = l`

   b. for all lists `l1, l2, l3`:
      `append(append(l1, l2), l3) = append(l1, append(l2, l3))`

   c. for all lists `l1, l2`:
      `reverse(append(l1, l2)) = append(reverse(l2), reverse(l1))`


   Hint: the base case in the proof of c. needs property a. The
   induction step of c. needs b.

   Note that c. is the property we assumed in the proofs of Homework 4.


### Binding, Scoping, and Substitutions

1. For each of the following expressions, overline all defining
   occurrences of variables. Moreover, draw an arrow between each
   bound using occurrence of a variable x and the corresponding
   defining occurrence of x. Finally, compute the set of free
   variables of the expression.

   a. 
      ```javascript
      const x = x + y; const y = x + y; y
      ```
      
   b. 
      ```javascript
      const f = (function (x) (const y = x; y)); f(x)
      ```
      
   c. 
      ```javascript
      function (x) ((function (x) (x(y)))(x))
      ```

   You can also mark defining occurrences with ^

1. Compute the following substitutions. Be careful about potential variable capturing:

   a. 
      ```javascript
      (x === y)[3/z]
      ```
            
   b. 
      ```javascript
      (x === y)[y/x]
      ```
      
   c. 
      ```javascript
      (const x = x + y; x + y)[2/x]
      ```
      
   d. 
      ```javascript
      (const x = x + y; x + y)[2/y]
      ```
      
   e. 
      ```javascript
      (const x = x + y; x + y)[x/y]
      ```
      
   f. 
      ```javascript
      ((x => (x + y))(y))[2/y]
      ```
      
   g. 
      ```javascript
      (x => (x + y))(y))[x/y]
      
      ```
      
   h. 
      ```javascript
      ((x => (x + y))(y))[x(y)/y]
      ```
      

### Operational Semantics

Evaluate the following program using (a) the small-step semantics with
static binding; (b) the big-step semantics with dynamic binding

```javascript
const x = 2 + 3;
const f = y => x + y;
const x = 3;
f(0)
```

Show the individual evaluation steps for the small-step semantics,
respectively, the derivation tree for the big-step semantics. 


