****
# 1 Expressions
* Primary building block of OCaml programs ⟶ kinda like statements or commands
* Semantics of expressions:
	* **Type-checking rules (static semantics)** ⟶ produce a *type*, or fail with an error message
	* **Evaluation rules (dynamic semantics)** ⟶ produce a *value*, or exception or infinite loop

## 1.1 Values
* An expression that does not need any further evaluation ⟶ all values are expressions, but not all expressions are values
![](../../attachments/Pasted%20image%2020250408170330.png)

## 1.2 Primitive Types
* `char` is written with single quotes like `a`
* `string` is a sequence of characters written with double quotes
	* String concat uses `^`
* Notice that operators are not overloaded ⟶ `*` is for ints, `*.` is for floats
![](../../attachments/Pasted%20image%2020250408170428.png)

* There are a few built in functions to convert from strings to other types and vice versa
![](../../attachments/Pasted%20image%2020250408171056.png)
![](../../attachments/Pasted%20image%2020250408171105.png)
![](../../attachments/Pasted%20image%2020250408171123.png)

## 1.3 Type Inference
* The OCaml compiler **infers** types ⟶ you don't have to always write down the types of programs
	* Type checking is done at **compile time**; compilation fails with *type error* if it can't
* You can manually specify the type via **type annotations**
![](../../attachments/Pasted%20image%2020250408174152.png)

## 1.4 Equality
In OCaml, the equality operators are:
* `=` is used for **structural equality** (comparing contents/values)
	* recursively compares data structures
	* returns true if values are identical, even if they are at different memory
	* most commonly used equality operator
* `==` is used for **physical equality** (comparing memory addresses/references)
	* checks if two expressions refer to same memory location
	* much faster but less useful
	* returns true only if the two operands are same object in mem

```ocaml
let a = [1; 2; 3]
let b = [1; 2; 3]
let c = a

(* Structural equality *)
a = b  (* true - same values *)
a = c  (* true - same values *)

(* Physical equality *)
a == b (* false - different lists in memory *)
a == c (* true - c is an alias for a *)
```

## 1.5 If-expressions
Written with the syntax `if then else`
* The **guard** of the `if` expression must have type `bool` ⟶ can't do `if 0`
* `e2` and `e3` must be the same type ⟶ can't do `if true then "yay" else 1`
![](../../attachments/Pasted%20image%2020250408171338.png)

* If expression are like ternary operators ⟶ can place them anywhere in another expression
![](../../attachments/Pasted%20image%2020250408171523.png)

* The final `else` is mandatory, though you can also have `else if`
![](../../attachments/Pasted%20image%2020250408171555.png)

### 1.5.1 Semantics
* Evaluation is the **dynamic semantics**, and type checking is the **static semantics**
![](../../attachments/Pasted%20image%2020250408171734.png)
![](../../attachments/Pasted%20image%2020250408171657.png)
![](../../attachments/Pasted%20image%2020250408171701.png)

## 1.6 Let Definitions
* Allows us to put a name to a value ⟶ used in the toplevel to define variables
* `let x = 42;;` ⟶ read from right to left
	* We have a value `42` of type `int` that is bound to the name `x`
![](../../attachments/Pasted%20image%2020250408172017.png)

### 1.6.1 What Are Definitions?
* It is **not an expression**
![](../../attachments/Pasted%20image%2020250408172039.png)

### 1.6.2 Semantics
![](../../attachments/Pasted%20image%2020250408172335.png)

## 1.7 Let Expressions
* Like `let` definitions, but are syntatically expressions ⟶ allows nesting in other expressions
![|550](../../attachments/Pasted%20image%2020250408172442.png)
![](../../attachments/Pasted%20image%2020250408172611.png)

* These `let` expressions are scoped by the `in`:
![](../../attachments/Pasted%20image%2020250408172559.png)
![](../../attachments/Pasted%20image%2020250408172746.png)
### 1.7.1 Semantics
![](../../attachments/Pasted%20image%2020250408173205.png)
![](../../attachments/Pasted%20image%2020250408173212.png)
![](../../attachments/Pasted%20image%2020250408173236.png)

## 1.8 Scope
The scope of a variable is where its name is meaninful
![](../../attachments/Pasted%20image%2020250408173309.png)

### 1.8.1 Overlapping Bindings
![](../../attachments/Pasted%20image%2020250408173322.png)
The above evaluates to the below, which equals 11.
![](../../attachments/Pasted%20image%2020250408173411.png)
**Alpha Equivalence:** two functions are equivalent up to renaming of variables
* The expression above is the exact same if we change the inner `x` to `y` for clarity
![](../../attachments/Pasted%20image%2020250408173440.png)

> [!NOTE] Shadowing
> This is known as shadowing ⟶ a new binding of a variable **shadows** any old binding of the variable name. In the above, the innermost `x` shadows the out binding of `x`

### 1.8.2 Top Level
Recall that OCaml cannot mutate variables. However, if we do the following in the top level, it seems like we are:
![](../../attachments/Pasted%20image%2020250408173808.png)
![](../../attachments/Pasted%20image%2020250408173846.png)

# 2 Functions
Functions are **values** ⟶ can we use them anywhere we would use values.
* Functions can take functions as arguments
* Functions can return functions as results

## 2.1 Anonymous Functions
This is an anonymous function that takes in argument $x$ and returns $x + 1$
* anonymous because we have not bound it to a name
```ocaml
(fun x -> x + 1);;
- : int -> int = <fun>

(fun x -> x + 1) 3;;
- : int = 4

(fun x y -> (x +. y) /. 2.);;
- : float -> float -> float = <fun>

(fun x y -> (x +. y) /. 2.) 5. 7.;;
- : float = 6.
```

The syntax of anonymous functions is:
```ocaml
fun x1 ... xn -> e
```

The evaluation is:
* A function is already a value ⟶ no further computation necessary
* Body $e$ is not evaluated until the function is applied to arguments

### 2.1.1 Lambdas
Anonymous functions are also known as lambda expressions with the notation:
$$
\lambda x . e
$$
The lambda means "what follows is an anonymous function". The above is: `fun x -> e`

### 2.1.2 Function Application
You can apply functions by just putting expressions next to each other.
* No parentheses unless you need to force a particular order of evaluation

The syntax is:
```ocaml
e0 e1 ... en
```

The evaluation of `e0 e1 … en`:
1. Evaluate every subexpression of the application:
	* `e0 ==> v0, …, en ==> vn`
	* `v0` must be a function of the form `fun x1 … xn -> e`
2. Substitute `vi` for `xi` in the arguments of `v0`
	* This means replace `xi` with `vi` in `e`, yielding new expression `e'`
3. Evaluate `e' ==> v`. This `v` is the result.

For example:
```
(fun x -> x + 1) (2 + 3)
1. Evaluate v0 -> fun x is already an anonymous function which is a value
2. Evaluate (2 + 3) ==> 5 which is a value

(fun x -> x + 1) 5
1. Substitute vi for xi the body of v0 (i.e. sub 5 for x)

e' = 5 + 1
1. Evaluate e' to get v

v = 6
```

## 2.2 Named Functions
Since anonymous functions are values, we can name them just like how we would name a variable!
```ocaml
(* This is anonymous *)
fun x -> x + 1;;
- : int -> int = <fun>

(* This is the same function but named inc *)
let inc = fun x -> x + 1;;
val inc : int -> int = <fun>

(* We can now use the name to apply the function *)
inc 5;;
- : int = 6
```

There is *syntactic sugar* to simplify the above:
```ocaml
(* This is the same as the above *)
let inc x = x + 1

(* These two evaluate to the exact same thing *)
let avg x y = (x +. y) /. 2.;;
let avg = fun x y -> (x +. y) /. 2.;;
val avg : float -> float -> float = <fun>

avg 0. 1.;;
- : float = 0.5

(* The whole sequence above in toplevel is equal to *)
let avg = fun x y -> (x +. y) /. 2. in
	avg 0. 1.;;
- : float = 0.5
```

We can also use this function application for function application:
* This implies that the let expressions are syntatic sugar for the application of anonymous functions
```ocaml
(fun x -> x + 1) 2;;
- : int = 3

let x = 2 in x + 1;;
- : int = 3
```

## 2.3 Recursive Functions
In order for a function to be recursive, you **must explicit state it** using `rec`
```ocaml
(* factorial, requires [n >= 0] *)
let rec fact n =
	if n = 0 then 1
	else n * fact(n - 1);;
val fact : int -> int = <fun>

fact 5;;
- : int = 120
```

## 2.4 Function Types
![](../../attachments/Pasted%20image%2020250421142701.png)
![](../../attachments/Pasted%20image%2020250421142717.png)
![](../../attachments/Pasted%20image%2020250421142959.png)

**Example:**
```
fun (x : int) -> x + 1
(* x has type int, x + 1 must have type int, so *)
(fun x -> x + 1) has type int -> int

fun x y -> x + y
(* x has type int, y has type int, x + y must have type int *)
The type has to be : int -> int -> int

```

## 2.5 Partial Application and Currying
For multi-argument functions, we can partially apply some arguments to produce other functions:
```ocaml
(* this is a function that takes in two args *)
let add x  y = x + y;;
val add : int -> int -> int = <fun>

(* we can fully apply it with two arguments *)
add 2 3;;
- : int = 5

(* we can partially apply the function to get another fun that just adds 2 *)
add 2;;
- : int -> int = <fun>

(* we can assign this to a name to use it *)
let add2 = add 2;;
val add2 : int -> int = <fun>

add2 5;;
- : int = 7
```

This works because OCaml automatically **curries** multiargument functions:
* Currying is a tecnhique where a multi-argument function is transformed into a sequence of single-argument functions
* This is reflected in the type system: `int -> int -> int` means a functino that takes an int and returns a function of type `(int -> int
* In OCaml, there are no true multi-argument functions. Every function takes exactly one argument and is curried.
```ocaml
fun x y z -> e
(* is syntactic sugar for *)
fun x -> (fun y -> (fun z -> e))

let add x y = x + y
(* is syntactic sugar for *)
let add = fun x -> (fun y -> x + y
```
