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

----

def analyze(n):
    is_even = (n % 2 == 0)
    return n * 2, is_even
    
def analyze(n):
    if n % 2 == 0:
        return n * 2, True
    else:
        return n * 2, False

def analyze(n):
    return n * 2, (n % 2 == 0)

def analyze(n):
    return (n * 2, True) if n % 2 == 0 else (None, False)

def analyze(n):
    if n % 2 == 0:
        return n * 2, True
    return None, False

----

- argument types
- default parameters
- keyword vs positional
- variable-length arguments
- pure vs stateful functions
- functional patterns (map/filter/lambda)

------

2. **Data validation inside functions**

def safe_divide(a, b):
	if not isinstance(a, (int, float)):
		raise TypeError("a must be numeric")
	if not isinstance(b, (int, float)):
		 raise TypeError("b must be numeric")
	if b == 0:
		raise ZeroDivisionError("divide by zero")
	return a / b


def temperature_c_to_f(celsius: float) -> float:
    return celsius * 9/5 + 32

---

4. **Returning structured data (tuple vs dict vs dataclass)**

*tuple:* ...

def analyze(n):
    if n % 2 == 0:
        return n * 2, True
    return None, False

*dict:* ...

def analyze_word(word: str) -> dict:
    return {
        "original": word,
        "length": len(word),
        "is_alpha": word.isalpha(),
    }
x = analyze("Sentence")
> x
> {'original': 'Sentence', 'length': 8, 'is_alpha': True}
> x["original"]
> 'Sentence'


*dataclass: ...*

from dataclasses import dataclass

@dataclass
class AnalysisResult:
    original: str
    length: int
    is_alpha: bool

def analyze_word(word: str) -> AnalysisResult:
    return AnalysisResult(word, len(word), word.isalpha())

x = analyze("Sentence")
> x
> AnalysisResult(original='Sentence', length=8, is_alpha=True)
> x.original
> 'Sentence'

------

from dataclasses import dataclass

@dataclass
class UserInfo:
    username: str
    age: int
    email: str
    is_active: bool = True

def create_user_info(uname: str, uage: int, uemail: str) -> UserInfo:
    return UserInfo(username=uname, age=uage, email=uemail)

---

