
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

## 1.4 If-expressions
Written with the syntax `if then else`
* The **guard** of the `if` expression must have type `bool` ⟶ can't do `if 0`
* `e2` and `e3` must be the same type ⟶ can't do `if true then "yay" else 1`
![](../../attachments/Pasted%20image%2020250408171338.png)

* If expression are like ternary operators ⟶ can place them anywhere in another expression
![](../../attachments/Pasted%20image%2020250408171523.png)

* The final `else` is mandatory, though you can also have `else if`
![](../../attachments/Pasted%20image%2020250408171555.png)

### 1.4.1 Semantics
* Evaluation is the **dynamic semantics**, and type checking is the **static semantics**
![](../../attachments/Pasted%20image%2020250408171734.png)
![](../../attachments/Pasted%20image%2020250408171657.png)
![](../../attachments/Pasted%20image%2020250408171701.png)

## 1.5 Let Definitions
* Allows us to put a name to a value ⟶ used in the toplevel to define variables
* `let x = 42;;` ⟶ read from right to left
	* We have a value `42` of type `int` that is bound to the name `x`
![](../../attachments/Pasted%20image%2020250408172017.png)

### 1.5.1 What Are Definitions?
* It is **not an expression**
![](../../attachments/Pasted%20image%2020250408172039.png)

### 1.5.2 Semantics
![](../../attachments/Pasted%20image%2020250408172335.png)

## 1.6 Let Expressions
* Like `let` definitions, but are syntatically expressions ⟶ allows nesting in other expressions
![](../../attachments/Pasted%20image%2020250408172442.png)
![](../../attachments/Pasted%20image%2020250408172611.png)

* These `let` expressions are scoped by the `in`:
![](../../attachments/Pasted%20image%2020250408172559.png)
![](../../attachments/Pasted%20image%2020250408172746.png)
### 1.6.1 Semantics
![](../../attachments/Pasted%20image%2020250408173205.png)
![](../../attachments/Pasted%20image%2020250408173212.png)
![](../../attachments/Pasted%20image%2020250408173236.png)

## 1.7 Scope
The scope of a variable is where its name is meaninful
![](../../attachments/Pasted%20image%2020250408173309.png)

### 1.7.1 Overlapping Bindings
![](../../attachments/Pasted%20image%2020250408173322.png)
The above evaluates to the below, which equals 11.
![](../../attachments/Pasted%20image%2020250408173411.png)
**Alpha Equivalence:** two functions are equivalent up to renaming of variables
* The expression above is the exact same if we change the inner `x` to `y` for clarity
![](../../attachments/Pasted%20image%2020250408173440.png)

> [!NOTE] Shadowing
> This is known as shadowing ⟶ a new binding of a variable **shadows** any old binding of the variable name. In the above, the innermost `x` shadows the out binding of `x`

### 1.7.2 Top Level
Recall that OCaml cannot mutate variables. However, if we do the following in the top level, it seems like we are:
![](../../attachments/Pasted%20image%2020250408173808.png)
![](../../attachments/Pasted%20image%2020250408173846.png)
