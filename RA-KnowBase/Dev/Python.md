# 1. What a Function _Is_ in Python

In older languages, a function often means:

- a block of code
- stored in a fixed memory address
- sometimes separate from data
- with fixed parameter types
- possibly passed by pointer (Pascal procedures, C function pointers)

In Python:

- **Functions are objects** (first-class citizens).
- You can store them in variables.
- You can pass them to other functions.
- You can return functions from functions.
- They have **no declared types** for arguments or return values.
- They capture surrounding variables (closures).

---

def greet(name):
    return f"Hello, {name}"

greet("Roman")

### A) Positional parameters

`def add(a, b):     
	`return a + b`
>add(3, 5)
>  8
> 
`add(a=10, b=30)
>  40

def power(base, exp=2):
    return base ** exp
> power(3)
>   9
>  power(3, 3)
>   27

def stats(a, b):
    return a + b, a * b
> s, p = stats(3, 4)
> s = 7
> p = 12

---

**Safe pattern:**

`def func(arg=None):     
	`if arg is None:         
	`	arg = []     ...`

**Reason:**

- Default arguments in Python are stored once.
- Mutable defaults cause shared state.
- Using `None` ensures a new list is created per function call.

