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

   <details><summary>Solution</summary>
   <p>
      
   ```
   take(n, nil) = nil
   take(0, hd :: tl) = nil
   take(n, hd :: tl) = hd :: take(n - 1, tl) if n > 0
   ```
   </p>
   </details>

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

   <details><summary>Solution</summary>
   <p>
      
   ```scala
   def take(n: Int, l: List): List =
     require(n >= 0)
     def takeLoop(n: Int, l: List, acc: List): List = l match
       case Cons(hd, tl) if n > 0 => 
         takeLoop(n - 1, tl, Cons(hd, acc))
       case _ => acc
     reverse(takeLoop(n, l, Nil))
   ```
   Here, `reverse` is the function that reverses the given list.
   </p>
   </details>

1. Using the definitions of the functions append and reverse given in
   class, prove the following properties by structural induction:

   a. for all lists `l`: `append(l, nil) = l`

      <details><summary>Solution</summary>
      <p>

      Proof by structural induction on `l`:

      Let `l = nil`:
      ```
        append(l, nil)
      = append(nil, nil)  -- Def. of append
      = nil
      = l
      ```

      Let `l = hd :: tl`:
 
      induction hypothesis: `append(tl, nil) = tl`

      ```
        append(l, nil)
      = append(hd :: tl, nil)       
      = hd :: append(tl, nil) -- Def. of append
      = hd :: tl              -- ind. hyp.
      = l
      ```
      </p>
      </details>

   b. for all lists `l1, l2, l3`:
      `append(append(l1, l2), l3) = append(l1, append(l2, l3))`

      <details><summary>Solution</summary>
      <p>
      
      Proof by structural induction on `l1`:

      Let `l1 = nil`:
      ```
        append(append(l1, l2), l3)
      = append(append(nil, l2), l3)   
      = append(l2, l3)                -- Def. of append
      = append(nil, append(l2, l3))   -- Def. of append
      = append(l1, append(l2, l3))
      ```

      Let `l1 = hd :: tl`

      ind. hyp.: `append(append(tl, l2), l3) = append(tl, append(l2, l3))`
      ```
        append(append(l1, l2), l3)
      = append(append(hd :: tl, l2), l3)   
      = append(hd :: append(tl, l2), l3)  -- Def. of append
      = hd :: append(append(tl, l2), l3)  -- Def. of append
      = hd :: append(tl, append(l2, l3))  -- ind. hyp.
      = append(hd :: tl, append(l2, l3))  -- Def. of append
      = append(l1, append(l2, l3))
      ```
      </p>
      </details>

   c. for all lists `l1, l2`:
      `reverse(append(l1, l2)) = append(reverse(l2), reverse(l1))`

      <details><summary>Solution</summary>
      <p>
      
      Proof by structural induction on `l1`:

      Let `l1 = nil`:
      ```
        reverse(append(l1, l2))
      = reverse(append(nil, l2))
      = reverse(l2)                        -- Def. of append
      = append(reverse(l2), nil)           -- part a
      = append(reverse(l2), reverse(nil))  -- Def. of reverse
      = append(reverse(l2), reverse(l1))
      ```

      Let `l1 = hd :: tl`

      ind. hyp.: `reverse(append(tl, l2)) = append(reverse(l2), reverse(tl))`
      ```
        reverse(append(l1,l2))
      = reverse(append(hd :: tl, l2))
      = reverse(hd :: append(tl, l2))               -- Def. of append
      = append(reverse(append(tl, l2)), hd :: nil)  -- Def of reverse
      = append(append(reverse(l2), reverse(tl)), hd :: nil) -- ind. hyp.
      = append(reverse(l2), append(reverse(tl), hd :: nil)) -- part b
      = append(reverse(l2), reverse(hd :: tl))      -- Def. of reverse
      = append(reverse(l2), reverse(l1))
      ```
      </p>
      </details>

   Hint: the base case in the proof of c. needs property a. The
   induction step of c. needs b.

   Note that c. is the property we assumed in the proofs of Homework 3.


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
      
      <details><summary>Solution</summary>
      <p>
      
      ```javascript
      const ^x = x + y; const ^y1 = x1 + y; y1
      ```
      Free variables: `{x, y}`
      </p>
      </details>

   b. 
      ```javascript
      const f = (x => const y = x; y); f(x)
      ```
      
      <details><summary>Solution</summary>
      <p>
      
      ```javascript
      const ^f1 = (^x1 => const ^y1 = x1; y1); f1(x)
      ```
      Free variables: `{x}`

      </p>
      </details>

   c. 
      ```javascript
      x => (x => x(y))(x)
      ```

      <details><summary>Solution</summary>
      <p>
      
      ```javascript
      ^x1 => (^x2 => x2(y))(x1)
      ```
      Free variables: `{y}`

      </p>
      </details>

   You can also mark defining occurrences with ^

1. Compute the following substitutions. Be careful about potential variable capturing:

   a. 
      ```javascript
      (x === y)[3/z]
      ```
      
      <details><summary>Solution</summary>
      <p>
      
      ```javascript
      (x === y)
      ```
      </p>
      </details>
      
   b. 
      ```javascript
      (x === y)[y/x]
      ```
      
      <details><summary>Solution</summary>
      <p>
      
      ```javascript
      (y === y)
      ```
      </p>
      </details>

   c. 
      ```javascript
      (const x = x + y; x + y)[2/x]
      ```
      
      <details><summary>Solution</summary>
      <p>
      
      ```javascript
      (const x = 2 + y; x + y)
      ```
      </p>
      </details>

   d. 
      ```javascript
      (const x = x + y; x + y)[2/y]
      ```
      
      <details><summary>Solution</summary>
      <p>
      
      ```javascript
      (const x = x + 2; x + 2)
      ```
      </p>
      </details>

   e. 
      ```javascript
      (const x = x + y; x + y)[x/y]
      ```
      
      <details><summary>Solution</summary>
      <p>
      
      ```javascript
      (const z = x + x; z + x)
      ```
      </p>
      </details>

   f. 
      ```javascript
      (x => x + y)(y)[2/y]
      ```
      
      <details><summary>Solution</summary>
      <p>
      
      ```javascript
      (x => x + 2)(2)
      ```
      </p>
      </details>

   g. 
      ```javascript
      (x => x + y)(y)[x/y]
      
      ```
      
      <details><summary>Solution</summary>
      <p>
      ```javascript
      (z => z + x)(x)
      ```
      </p>
      </details>

   h. 
      ```javascript
      (x => x + y)(y)[x(y)/y]
      ```
      
      <details><summary>Solution</summary>
      <p>
      
      ```javascript
      (z => z + x(y))(x(y))
      ```
      </p>
      </details>


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

<details><summary>Solution</summary>
<p>

Small-step with static binding:

```javascript
const x = 2 + 3;
const f = y => x + y;
const x = 3;
f(0)
```

-> with rules SearchConstDecl1, DoPlus

```javascript
const x = 5;
const f = y => x + y;
const x = 3;
f(0)
```

-> with rule DoConstDecl

```javascript
const f = y => 5 + y;
const x = 3;
f(0)
```

-> with rule DoConstDecl

```javascript
const x = 3;
(y => 5 + y)(0)
```

-> with rule DoConstDecl

```javascript
(y => 5 + y)(0)
```

-> with rule DoCall

```javascript
5 + 0
```

-> with rule DoPlus

```javascript
5
```

Big-step semantics with dynamic binding:

Define `e1 = (const f = y => x + y; const x = 3; f(0))`

```javascript
------------ EvalVal
{} |- 2 => 2         toNum(2)=2

------------ EvalVal
{} |- 3 => 3         toNum(3)=3
------------------------- EvalPlus
    {} |- 2 + 3 => 5                A:{x -> 5} |- e1 => 3
------------------------------------------------------------EvalConstDecl
             {} |- const x = 2 + 3; e1 => 3
```

Derivation of A:

Define `fdef = (y => x + y)`
Define `e2 = (const x = 3; f(0))`

```javascript
------------------------------- EvalVal
    {x -> 5} |- fdef => fdef            B:{x -> 5, f -> fdef} |- e2 => 3 
-------------------------------------------------EvalConstDecl
     {x -> 5} |- const f = fdef; e2 => 3
```

Derivation of B:

```javascript
------------------------------ EvalVal
{x -> 5, f -> fdef } |- 3 => 3          C:{x -> 3, f -> fdef} |- f(0) => 3
------------------------------------------------------------EvalConstDecl
     {x -> 5, f -> fdef } |- const x = 3; f(0) => 3
```

Derivation of C:

```javascript
------------------------------ EvalVar   
{x -> 3, f -> fdef } |- f => fdef

------------------------------ EvalVal
{x -> 3, f -> fdef } |- 0 => 0

D:{x -> 3, f -> fdef, y -> 0} |- x + y => 3
---------------------------------------------------- EvalCall
        {x -> 3, f -> fdef} |- f(0) => 3
```

Derivation of D:

```javascript
------------------------------------- EvalVar
{x -> 3, f -> fdef, y -> 0} |- x => 3          toNum(3) = 3

------------------------------------- EvalVar
{x -> 3, f -> fdef, y -> 0} |- y => 0          toNum(0) = 0
----------------------------------------------------------- EvalPlus
      {x -> 3, f -> fdef, y -> 0} |- x + y => 3
```

