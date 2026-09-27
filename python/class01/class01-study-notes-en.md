# Python for DevOps — Class 01 Study Notes

**Topic:** From raw data to your first Python program
**Covers:** data, variables, data types (`str`, `int`, `float`, `bool`, `list`, `dict`), how Python runs code, type casting, string interpolation, user input
**Companion files in this folder:** `1. data-types-basics.py`, `2. list-data-operations.py`, `3. dictionary-data-type.py`, `Student-Lab-Exercises1.md`

---

## Table of Contents

1. [The Big Picture: Why We Program](#1-the-big-picture-why-we-program)
2. [What Is a Programming Language?](#2-what-is-a-programming-language)
3. [Meet Python](#3-meet-python)
4. [Setting Up and Running Python](#4-setting-up-and-running-python)
5. [Data, Data Structures and Algorithms](#5-data-data-structures-and-algorithms)
6. [Variables](#6-variables)
7. [Data Types](#7-data-types)
8. [Lists in Depth](#8-lists-in-depth)
9. [Dictionaries in Depth](#9-dictionaries-in-depth)
10. [Type Casting](#10-type-casting)
11. [Joining Strings: Concatenation](#11-joining-strings-concatenation)
12. [String Interpolation](#12-string-interpolation)
13. [User Input](#13-user-input)
14. [Common Misconceptions, Corrected](#14-common-misconceptions-corrected)
15. [Lab 1 Walkthrough](#15-lab-1-walkthrough)
16. [Interview Corner](#16-interview-corner)
17. [Glossary](#17-glossary)
18. [One-Page Cheat Sheet](#18-one-page-cheat-sheet)

---

## 1. The Big Picture: Why We Program

The class opened with a census-style survey of one person: name, age, occupation, married or not, spouse's name, kids, address, phone, family size, yearly income. A national statistics bureau spends crores of taka sending people door to door to collect exactly this kind of information.

Everything collected that way is **data**. So is a photo, a video, this class recording, employee records, financial accounts, tax files. In a DevOps job, your daily data looks like this:

| Real-world thing | Example value |
|---|---|
| IP address | `192.168.1.50` |
| CPU usage | `75.1` (%) |
| Docker service status | `active` |
| Output of `pwd` / `ls` | a directory path, a list of files |

The chain of ideas the whole class builds on:

```
Raw data  →  organised (structured)  →  stored in a database / memory
                         ↑
            done by a PROGRAM (instructions)
                         ↑
       written in a PROGRAMMING LANGUAGE (e.g. Python)
                         ↑
              run on a COMPUTER (anything that computes)
```

- **Database:** a "box" that holds your data and gives it back when you need it. Scan an NID or passport and the whole record comes back — that is a database at work.
- You *could* organise data by hand, but the preferred way is a **program**.
- A **computer** is anything that can compute: laptop, server, smartphone.

---

## 2. What Is a Programming Language?

The instructor said a sentence in Japanese and asked if anyone understood. Nobody did. A computer has the same problem with human language.

> **A programming language is a language a computer (machine) can understand, so it can process and execute our instructions.**

**Proof from class:** typing `2 + 2` directly into a Linux/Bash terminal fails. Bash is a shell, not a calculator, so it doesn't understand the instruction. Start the Python interpreter first (`python3`) and the same `2 + 2` returns `4`. Once you give the computer a language, it understands you.

### Input and Output (I/O)

Every interaction with a computer is **input → processing → output**.

| You give (input) | Computer gives back (output) |
|---|---|
| `2 + 2` | `4` |
| `print("Hello there")` | `Hello there` on screen |
| keyboard typing, mouse clicks | whatever the program does |

Other languages you'll hear about: C++, C#, Java, JavaScript, PHP (Laravel is a PHP *framework*, not a language). This course uses **Python**, one of the most popular languages in the world.

---

## 3. Meet Python

"Python is easy and reads like English" is true, but it's a weak interview answer. An engineer should describe Python by its **technical characteristics**:

### 3.1 High-level language

| Level | Meaning | Example |
|---|---|---|
| **Low-level** | Works close to the hardware ("iron and metal"); you manage memory and CPU details | Assembly, C (partly) |
| **High-level** | Works far from the hardware and reads close to plain English; the language handles the machine details | Python, JavaScript |

"High-level" does **not** mean "elite" or "better". It only describes how far the language sits from the hardware.

### 3.2 Dynamically typed

In many languages you must declare the type yourself:

```c
int x = 2;        // C / Java style — type written explicitly
```

In Python you just write:

```python
x = 2             # Python works out on the fly that this is an int
name = "Noor"     # ...and that this is a str
```

Python detects each value's type **at runtime** ("on the fly"). That is what *dynamically typed* means.

### 3.3 Both compiled and interpreted

An **interpreter** is a translator (দোভাষী), like a person translating Bengali to French for you.

What actually happens when you run `python3 my_program.py` (in CPython, the standard Python):

1. **Compile step:** Python reads the *whole file* and compiles it into **bytecode**, a compact intermediate form (sometimes cached as `.pyc` files in `__pycache__/`).
2. **Interpret step:** the **Python Virtual Machine (PVM)** executes that bytecode **statement by statement, top to bottom**.

That is why the interview answer is **"Python is both compiled and interpreted."** The instructor also gave a tip: say terms like this deliberately. The interviewer will ask you to explain them, and a question you can answer well keeps the ball in your court.

> **Precision note:** in class, bytecode was simplified to "machine code". Strictly speaking, bytecode is **not** CPU machine code. It is an instruction set for the Python Virtual Machine, which then drives the real hardware. For a first class, "Python translates your English-like code into something the machine can run" is a fine mental model.

### 3.4 Case-sensitive

Uppercase and lowercase letters are different to Python.

```python
x = "Noor"
y = "noor"
print(x == y)   # False — "N" and "n" are different characters

age = 60
Age = 61        # a completely separate variable from `age`
```

`==` means **"is equal to?"** (a comparison that returns `True`/`False`). A single `=` means **"store this value"** (assignment). Don't mix them up.

### 3.5 Runs top to bottom, one statement at a time

```python
# This is a python program      ← comment: skipped
                                 ← blank line: nothing to do
print("line 1")                  ← runs 1st
                                 
print("line 3")                  ← runs 2nd (the label is just text!)
                                 
print("line 2")                  ← runs 3rd
x = 3                            ← runs 4th: stores 3, prints nothing
```

Output:
```
line 1
line 3
line 2
```

Python doesn't care what your text *says*. It follows the **order of the lines** in the file.

### 3.6 Comments

A **comment** is a note for humans. Python ignores it.

```python
# This whole line is a comment
port = 22   # a comment can also sit at the end of a line
```

- Use `#` at the start of each line you want commented.
- **VS Code shortcut:** select lines and press **`Ctrl + /`** (Mac: `Cmd + /`) to comment or uncomment them all at once.

---

## 4. Setting Up and Running Python

### 4.1 Check whether Python is installed

```bash
python3 --version      # Linux / macOS
python --version       # Windows (or: py --version)
which python3          # Linux / macOS: where is it installed?
```

Two dashes: `--version`. Some tools use `-v`, some `--version`, some neither. It varies, so check each tool's help (`--help`) when unsure.

If it's missing, download it from **python.org → Downloads**. The site detects your OS (Windows / macOS / Linux). A computer doesn't ship with every language pre-installed, because that would waste disk space.

### 4.2 Quick practice vs. the engineer's way

- **Quick practice:** the online shell at python.org, or type `python3` in a terminal to open the interactive **REPL** (`>>>` prompt). Good for trying one-liners.
- **The engineer's way** (what we do in this course): a project folder, a `.py` file, an editor, and running it from the terminal. The instructor's line: *"If you've come to dance, don't dance behind a veil"*. Work the way software engineers actually work.

### 4.3 Your first project, step by step

```bash
cd ~/Desktop
mkdir python-playground      # make a folder (directory)
cd python-playground
code .                       # open it in Visual Studio Code
```

Create a file named `my_python.py`. Every Python file ends in **`.py`**, the same way PDFs end in `.pdf` and web pages in `.html`.

```python
print("We are learning Python. It is awesome.")
print(6 / 3)
```

Run it in either of two ways:

1. Click the **▶ Run Python File** button in VS Code, **or**
2. In the terminal (the proper way):

```bash
pwd                   # confirm you're in the right folder
ls                    # confirm my_python.py is there
python3 my_python.py  # "Computer, run this file using the Python language"
```

Output:
```
We are learning Python. It is awesome.
2.0
```

> **Why `2.0` and not `2`?** In Python 3, the `/` operator **always returns a float**, even when the division is exact. Use `//` for whole-number (floor) division: `6 // 3` → `2`.

🎉 **Congratulations. That's your first Python program.**

---

## 5. Data, Data Structures and Algorithms

| Term | Plain definition |
|---|---|
| **Data** | Raw information stored in your computer's memory so a program can work with it. |
| **Data manipulation** | Changing stored data: e.g. updating Noor's age from 60 to 62, deleting a record. |
| **Data structure** | A specialised format for **organising, managing, storing and accessing** data. Organise raw data in a structured way so you can get information back out quickly. |
| **Algorithm** | The **steps** you follow to complete a task. |

Storing data isn't enough. If you need Noor's age tomorrow, you must be able to **find it fast**, **update** it, or **delete** it. That's what a good data structure gives you.

**An algorithm, demystified.** Adding two numbers is already an algorithm:

```python
x = 2          # step 1
y = 3          # step 2
z = x + y      # step 3
print(z)       # step 4  → 5
```

"Algorithm" sounds scary, but it's a recipe: step 1, step 2, step 3, done.

To learn data structures, you first need **data types** (Section 7), and to use data types you need **variables**.

---

## 6. Variables

> **A variable is a name that holds a value, so you can use that value again later.**

Think about your own name. Every time someone says "Noor bhai, how are you?" Noor knows who is being called. The name points to the person. A variable name points to a value in memory in the same way.

```python
name = "Noor"
age = 62
married = True
```

Anatomy:

```
  age    =    62
  ───    ─    ──
  │      │    └── variable VALUE (right side)
  │      └─────── assignment operator (the "=" sign)
  └────────────── variable NAME (left side)
```

**Style rule (best practice, PEP 8):** put **one space on each side** of `=`. Write `x = 20`, not `x=20`. Both run, but the first is the standard.

### Why variables exist: reuse

```python
name = "Noor"
age = 60

print(f"My name is {name} and my age is {age}")
# ... 200 lines later ...
print(f"{name} is {age} years old")
```

Store the value **once**, use it **as many times as you need**. If the age changes, you change it in one place. The alternative is retyping `"Noor"` and `60` everywhere, which is slow and error-prone.

---

## 7. Data Types

The same person's survey answers are not all the same *kind* of data. `"Noor"` is text, `62` is a number, "married?" is yes/no. Python needs to know which kind it is dealing with, and that's what **data types** are for.

Check any value's type with the built-in `type()` function:

```python
type("hello")    # <class 'str'>
type(60)         # <class 'int'>
type(12.3)       # <class 'float'>
type(4 > 6)      # <class 'bool'>
```

(Don't worry about the word `class` in that output yet. Everything in Python is an *object*, and that's a later topic.)

### 7.1 Overview

| Type | Short name | What it holds | Written like | General example | DevOps example |
|---|---|---|---|---|---|
| String | `str` | Text | inside quotes | `"Alice Smith"` | `"us-east-1"`, `"192.168.1.50"`, `'main'` |
| Integer | `int` | Whole numbers (±) | plain digits | `34`, `2` | `ssh_port = 22`, `active_containers = 5` |
| Float | `float` | Numbers with a decimal point | digits with `.` | `45.50`, `1.75` | `cpu_utilization_percent = 78.4` |
| Boolean | `bool` | Only `True` or `False` | `True` / `False` | `has_wife = False` | `is_healthy = True` |
| List | `list` | Ordered collection of items | `[ ]` | `["Liam", "Maya"]` | `["10.0.0.1", "10.0.0.2"]` |
| Dictionary | `dict` | Labelled key → value pairs | `{ key: value }` | `{"name": "Alice"}` | `{"image": "nginx:latest"}` |

### 7.2 String (`str`)

A **string** is a *sequence of characters* wrapped in matching quotes. Think of it as a sandwich: the text is the filling, the quotes are the bread.

```python
a = 'single quotes'
b = "double quotes"
c = '''triple single quotes'''
d = """triple double quotes —
can span
multiple lines"""
```

- A string can be one letter, a word, a sentence or a whole poem.
- **Every** character counts, including a space `" "`, a `$` sign or any symbol.
- **Quotes turn anything into text:** `"60"` is a string, not a number. `type("60")` → `str`.

**Convention:** pick **one** quote style for your codebase (double or single) and use it **everywhere**. Mixing them isn't an error, but it isn't clean.

**What happens without quotes?**

```python
print(Hello World)     # ❌ SyntaxError
print("Hello World")   # ✅ Hello World
```

**Syntax** is the grammar of a programming language. "I Noor" is broken English, because it needs "am". `print(Hello World)` is broken Python, because text needs quotes. A missing piece of grammar is a **syntax error**.

### 7.3 Integer (`int`) and Float (`float`)

```python
age = 60             # int: whole number, no quotes
price = 102.050      # float: has a decimal point
```

- Numbers are written **without** quotes.
- `int` and `float` are **two separate types** in Python, not one type with two flavours.
- Decimals matter a lot in finance and accounts, where `102.05` ≠ `102`.

### 7.4 Boolean (`bool`)

Only **two** possible values in the whole world: `True` or `False`. Capital first letter, no quotes.

```python
married = True
print(4 > 6)          # False — Python evaluates the comparison
print(type(4 > 6))    # <class 'bool'>
```

⚠️ `"True"` (with quotes) is a **string**, not a boolean. And lowercase `true` is an error in Python.

### 7.5 List and Dictionary

These two are big enough to get their own sections. See [Section 8](#8-lists-in-depth) and [Section 9](#9-dictionaries-in-depth).

---

## 8. Lists in Depth

> **A list is an ordered, changeable (mutable) collection of items, wrapped in square brackets `[ ]` and separated by commas.**

```python
fruits = ["apple", "banana", "orange"]
```

### 8.1 Characteristics

- Wrapped in **square brackets** `[ ]`.
- Items are separated by **commas** (and, by style, a space after each comma).
- Can hold **any data type**, even mixed together, even another list:

```python
mixed = ["Noor", 60, 12.3, True, ["nested", "list"]]
```

- Can hold **duplicates**: `["a", "a", "b"]` is fine.
- Each thing inside is called an **item** or **element**.

A string is a *sequence of characters*. A list is a *sequence of items*.

### 8.2 Index: the position of an item

At school, three students share a bench: seat 1, seat 2, seat 3. A list has seats too, called the **index**, but **counting starts at 0**. Lists are **zero-based**.

```
fruits  =  [ "orange", "banana", "apple" ]
index:          0         1         2
negative:      -3        -2        -1
```

```python
print(fruits[0])    # orange
print(fruits[1])    # banana
print(fruits[-1])   # apple  (negative index counts from the end)
print(fruits[4])    # ❌ IndexError: list index out of range
```

To use index `4` you need at least 5 items (0, 1, 2, 3, 4). Python tells you clearly when you ask for a position that doesn't exist.

> Writing `fruits[1]` on its own line in a `.py` file **shows nothing**. To see a value, you must `print()` it. (The interactive `>>>` REPL echoes values automatically. A script doesn't.)

### 8.3 Everyday list operations (from `2. list-data-operations.py`)

```python
grocery_list = ["Eggs", "Milk", "Bread"]

# ADD
grocery_list.append("Apples")      # add to the END
grocery_list.insert(1, "Butter")   # add at position 1
# → ['Eggs', 'Butter', 'Milk', 'Bread', 'Apples']

# REMOVE
grocery_list.remove("Milk")        # remove by VALUE (first match)
removed_item = grocery_list.pop(2) # remove by POSITION, and RETURN it
print(f"I took this out of the list: {removed_item}")   # Bread

# CHECK / COUNT / SORT
if "Eggs" in grocery_list:         # membership test → True/False
    print("Yes, you need to buy eggs.")
print(len(grocery_list))           # number of items
grocery_list.sort()                # A → Z, changes the list in place
```

| Method | Does what | Removes by | Gives back the item? |
|---|---|---|---|
| `append(x)` | Adds `x` at the end | — | — |
| `insert(i, x)` | Adds `x` at index `i` | — | — |
| `remove(x)` | Deletes first `x` found | **value** | No |
| `pop(i)` | Deletes item at index `i` (last item if no `i`) | **position** | **Yes** |
| `len(list)` | Counts items | — | — |
| `sort()` | Sorts in place (A→Z, small→large) | — | — |

**DevOps use:** a list of server IPs in a cluster, or pipeline stages:

```python
web_cluster_ips = ["10.0.0.1", "10.0.0.2", "10.0.0.3"]
deployment_pipeline_stages = ["Build", "Test", "Deploy"]
```

---

## 9. Dictionaries in Depth

Remember your childhood English-to-Bengali dictionary? *Father → বাবা*. You look up a **word** (key) to find its **meaning** (value). Python's `dict` works the same way.

> **A dictionary is a collection of `key: value` pairs, wrapped in curly braces `{ }`. You look data up by its key (a label) instead of by a position number.**

```python
person = {
    "name": "Noor",
    "number_of_kids": 2
}
```

### 9.1 Anatomy

```
{ "name"  :  "Noor" ,  "number_of_kids" : 2 }
  ──────  ─  ──────  ─
   KEY    │  VALUE   └── comma separates ITEMS
          └── COLON separates key from value
```

- Curly braces `{ }` (also called *second brackets* in Bangladesh).
- Each item = **key : value**, separated by a **colon** `:`.
- Multiple items are separated by **commas** `,`.
- **Values** can be anything: str, int, bool, list, even another dict.
- **Keys** are usually strings. They must be **unique** and **unchangeable (immutable)**: `str`, `int`, `float`, `bool` or `tuple` work. A list **cannot** be a key.

Writing each item on its own line is only for readability. Python treats both layouts the same.

### 9.2 A whole survey in one variable

```python
human_survey_profile = {
    "name": "Alice Smith",
    "age": 34,
    "has_wife": False,
    "kids": ["Liam", "Maya"]      # a list inside a dict
}

print(human_survey_profile["name"])    # Alice Smith
```

**DevOps version:** a container's configuration.

```python
container_config = {
    "image": "nginx:latest",
    "exposed_port": 80,
    "environment": "production",
    "is_healthy": True
}
print(container_config["exposed_port"])   # 80
```

This is the whole point of a data structure: **organise the data first, then pull out exactly what you need.** You'll see this `key: value` shape everywhere in DevOps: JSON API responses, YAML files for Docker Compose and Kubernetes, `.env` files.

### 9.3 Look up, add, update (from `3. dictionary-data-type.py`)

```python
employee_directory = {
    "EMP101": "Anis Rahman",
    "EMP102": "Sultana Kamal",
    "EMP103": "Tamim Iqbal"
}

print(employee_directory["EMP102"])              # look up → Sultana Kamal
employee_directory["EMP104"] = "Sakib Al Hasan"  # new key   → ADD
employee_directory["EMP101"] = "Khalilur Hasan"  # existing key → UPDATE (overwrite)
```

The same syntax `dict[key] = value` **adds** if the key is new and **overwrites** if it already exists.

Asking for a key that doesn't exist raises `KeyError`.

### 9.4 List vs Dictionary

| | List `[ ]` | Dictionary `{ }` |
|---|---|---|
| Access by | position number (index) | label (key) |
| Example | `ips[0]` | `config["image"]` |
| Best when | order matters; items are "more of the same" | each value has a meaning/label |
| Duplicates | allowed | keys must be unique |

---

## 10. Type Casting

### 10.1 The problem

```python
x = 10
y = "20"
print(x + y)
# ❌ TypeError: unsupported operand type(s) for +: 'int' and 'str'
```

Read the error slowly: **TypeError** means a *data type* problem. You can't add a number to text, just as a tiger and a cow don't drink from the same pond. Python noticed this by itself because it's dynamically typed.

### 10.2 The fix: convert the type

**Type casting** = converting a value from one data type to another.

```python
print(x + int(y))     # 30  → "20" became the number 20
```

### 10.3 Going the other way

Goal: join `20` and `"26"` into the year `"2026"`.

```python
x = 20
y = "26"
print(x + y)          # ❌ TypeError
print(str(x) + y)     # ✅ 2026  → 20 became the text "20"
```

Same `+` sign, different job:
- `int + int` → **math** (`20 + 26 = 46`)
- `str + str` → **joining text** (`"20" + "26" = "2026"`)

### 10.4 The casting functions

| Function | Converts to | Example | Result |
|---|---|---|---|
| `int()` | integer | `int("20")` | `20` |
| `float()` | float | `float("3.5")` | `3.5` |
| `str()` | string | `str(20)` | `"20"` |
| `bool()` | boolean | `bool(0)` | `False` |

The most common real-world cases are **string → int** and **int → string**.

⚠️ `int("hello")` or `int("3.5")` → `ValueError`. Python can only convert text that actually looks like a whole number.

---

## 11. Joining Strings: Concatenation

**Concatenation** = gluing strings together with `+`. It's joining, not math.

```python
f_name = "Noor"
l_name = "Siddique"

print(f_name + l_name)          # NoorSiddique   ← no space!
print(f_name + " " + l_name)    # Noor Siddique  ← add a space string
```

- `""` is an **empty string** (nothing inside the quotes).
- `" "` is a string containing **one space**. That's what you need between words.

---

## 12. String Interpolation

**String interpolation** = inserting variable values *into* a string.

### 12.1 f-strings (recommended)

```python
name = "Noor"
age = 60

bio = f"My name is {name} and my age is {age}"
print(bio)    # My name is Noor and my age is 60
```

Two parts, both required:
1. The letter **`f`** right before the opening quote.
2. Variable names inside **curly braces `{ }`** within the string.

Forget the `f` and Python prints the braces literally: `My name is {name}...`

> **Terminology fix:** in class this was called the "f function". It's actually a **string prefix**, and the result is called a **formatted string literal**, or **f-string**. It's not a function.

Bonus: f-strings handle numbers for you, so no `str()` casting is needed: `f"Age: {age}"` works even though `age` is an `int`.

### 12.2 `str.format()` (the older way)

```python
bio = "My name is {} and my age is {}".format(name, age)
```

The empty `{}` are **placeholders**, filled **in order** from the values inside `.format( )`.

⚠️ **Order matters.** Python doesn't know which value "means" a name and which an age:

```python
"My name is {} and my age is {}".format(age, name)
# → My name is 60 and my age is Noor   😅
```

| | f-string | `.format()` |
|---|---|---|
| Syntax | `f"Hi {name}"` | `"Hi {}".format(name)` |
| Readability | Variable name visible right where it's used | Must match order mentally |
| Recommendation | ✅ Use this | Know it; you'll see it in older code |

---

## 13. User Input

Login forms ask for email and password. Python's version is the built-in **`input()`** function: it shows a message, waits for the user to type, and returns what they typed.

```python
first_name = input("Enter your first name: ")
last_name = input("Enter your last name: ")

full_name = first_name + " " + last_name
print(full_name)
```

```
Enter your first name: Noor
Enter your last name: Siddique
Noor Siddique
```

The text inside `input("...")` is a **prompt**, the instruction the user sees. It's optional (`input()` works too), but always include one so the user knows what to type.

### ⚠️ The golden rule of `input()`

> **`input()` always returns a `str`, even if the user types a number.**

```python
age = input("Enter your age: ")        # user types 60
print(type(age))                       # <class 'str'>  → "60"
print(age + 1)                         # ❌ TypeError

age = int(input("Enter your age: "))   # cast it immediately
print(age + 1)                         # ✅ 61
```

Wrap `input()` in `int()` (or `float()`) whenever you need a number.

**Built-in functions so far:** `print()`, `input()`, `type()`, `len()`, `int()`, `str()`, `float()`, `bool()`. Python ships these with the installation, like the audio and video drivers that come preinstalled on a new computer. Functions get a full class of their own later.

---

## 14. Common Misconceptions, Corrected

| ❌ Misconception | ✅ Reality |
|---|---|
| "Python is just easy, English-like." | True but shallow. Say: **high-level, dynamically typed, both compiled and interpreted, case-sensitive.** |
| "`"60"` is a number." | Quotes make it a **string**. `type("60")` → `str`. |
| "`=` and `==` are the same." | `=` **stores** a value; `==` **compares** two values and returns `True`/`False`. |
| "`int` and `float` are one number type." | They are **two separate types** in Python. |
| "`input()` gives me a number if I type one." | It **always** returns a `str`. Cast with `int()`/`float()`. |
| "The `f` in f-strings is a function." | It's a **prefix** that makes the string a *formatted string literal*. |
| "Python runs the file blindly line by line, no compilation." | It **compiles the whole file to bytecode first**, then executes statement by statement. That's why a `SyntaxError` anywhere stops even line 1 from running, while a `TypeError` on line 10 only appears after lines 1–9 have run. |
| "Bytecode is machine code." | Bytecode is for the **Python Virtual Machine**, not directly for the CPU. |
| "A line like `x = 3` that prints nothing didn't run." | It **did** run: it stored `3` in memory. It just produces no *output*. |
| "Lists start counting at 1." | Lists are **zero-based**: the first item is index `0`. |
| "Dictionary keys can be any type." | Keys must be **immutable** (str, int, float, bool, tuple). A list can't be a key. Values can be anything. |
| "`6 / 3` gives `2`." | It gives **`2.0`**. `/` always returns a float. Use `//` for `2`. |
| "`Noor` and `noor` are the same." | Python is **case-sensitive**. Different values, and `Name`/`name` would be different variables. |
| "Mixing `'` and `"` is an error." | It's allowed but not clean. Stay **consistent** in one codebase. |

---

## 15. Lab 1 Walkthrough

**File:** `Student-Lab-Exercises1.md`, "The Smart Survey Onboarding Engine"

The lab uses two things from beyond today's class, so here is a primer first.

### 15.1 Mini primer: `if / elif / else` and `and / or`

```python
if condition_1:
    # runs if condition_1 is True
elif condition_2:
    # runs if condition_1 was False AND condition_2 is True
else:
    # runs if nothing above was True
```

- The **colon** `:` ends each condition line.
- The code *inside* must be **indented** (4 spaces). In Python, indentation is the grammar.
- Python checks top to bottom and runs **only the first** matching block.

| Operator | `True` when… | Example |
|---|---|---|
| `and` | **both** sides are True | `age >= 18 and developer == "yes"` |
| `or` | **at least one** side is True | `role == "admin" or role == "root"` |

Comparison operators: `<`, `>`, `<=`, `>=`, `==`, `!=` (not equal).

### 15.2 Plan it like an algorithm

1. Ask for name → keep as `str`.
2. Ask for age → **cast to `int`** (remember the golden rule).
3. Ask "developer? yes/no" → keep as `str`.
4. Decide the tier:
   - age < 18 → `"Tier 3: Guest"`
   - age ≥ 18 **and** developer → `"Tier 1: Admin Infrastructure Access"`
   - age ≥ 18 **and not** developer → `"Tier 2: Standard Executive Access"`
5. Print a summary with an **f-string**.

### 15.3 Reference solution

> Try it yourself first. Compare afterwards.

<details>
<summary>Show solution</summary>

```python
# 1. Capture inputs from user (Name, Age, Developer Status)
name = input("Enter your name: ")
age = int(input("Enter your age: "))
developer = input("Are you a developer? (yes/no): ")

# 2. Evaluate conditional logic to determine the clearance tier
if age < 18:
    tier = "Tier 3: Guest"
elif age >= 18 and developer == "yes":
    tier = "Tier 1: Admin Infrastructure Access"
else:
    tier = "Tier 2: Standard Executive Access"

# 3. Print out the final profile card using an f-string
print(f"Profile -> Name: {name} | Age: {age} | Developer: {developer} | Access: {tier}")
```

Sample run:
```
Enter your name: Noor
Enter your age: 60
Are you a developer? (yes/no): yes
Profile -> Name: Noor | Age: 60 | Developer: yes | Access: Tier 1: Admin Infrastructure Access
```

**Why the order of checks matters:** checking `age < 18` first means every later branch already knows the person is 18+. The `age >= 18` in the `elif` is technically redundant but makes the rule easy to read.

**Stretch goals:**
- If the user types `Yes` or `YES`, the check fails (case-sensitive!). Fix it with `developer.lower() == "yes"`.
- What happens if someone types `twenty` for age? (`ValueError`, since `int()` can't convert it. Error handling comes later.)
- Store the final result in a **dictionary** (`{"name": ..., "age": ..., "tier": ...}`) and print one key from it.

</details>

---

## 16. Interview Corner

**Q: What is Python?**
A high-level, dynamically typed, general-purpose programming language. CPython compiles source code to bytecode and the Python Virtual Machine interprets it, so it is both compiled and interpreted. It is also case-sensitive.

**Q: What does "dynamically typed" mean?**
You don't declare a variable's type. Python determines it at runtime from the value assigned (`x = 5` → `int`).

**Q: Is Python compiled or interpreted?**
Both. Source → bytecode (compile), bytecode → executed by the PVM statement by statement (interpret).

**Q: Difference between a list and a dictionary?**
A list is an ordered collection accessed by numeric index. A dictionary maps unique keys to values and is accessed by key.

**Q: What type does `input()` return?**
Always `str`.

**Q: What's type casting? Give an example.**
Converting a value from one type to another: `int("20")` → `20`, `str(20)` → `"20"`.

**Q: Why does `print(10 + "20")` fail?**
`TypeError`: Python won't add an `int` and a `str`. Cast one side first.

---

## 17. Glossary

| Term | Meaning |
|---|---|
| **Algorithm** | Ordered steps to complete a task. |
| **Assignment (`=`)** | Storing a value in a variable. |
| **Boolean (`bool`)** | Type with only two values: `True` / `False`. |
| **Built-in function** | A function that comes with Python (`print`, `input`, `len`, `type`…). |
| **Bytecode** | Intermediate form Python compiles your code into, run by the PVM. |
| **Case-sensitive** | Upper and lower case are treated as different. |
| **Comment (`#`)** | A note for humans; Python ignores it. |
| **Concatenation** | Joining strings with `+`. |
| **Data** | Raw information stored in memory. |
| **Data structure** | A format for organising, managing, storing and accessing data. |
| **Data type** | The kind of a value: `str`, `int`, `float`, `bool`, `list`, `dict`… |
| **Database** | A system that stores data and returns it on request. |
| **Dictionary (`dict`)** | Collection of unique `key: value` pairs in `{ }`. |
| **Dynamically typed** | Types are detected at runtime, not declared. |
| **Element / Item** | One thing inside a list (or dict). |
| **Empty string (`""`)** | A string with no characters. |
| **f-string** | `f"..."` string with `{variables}` inserted. |
| **Float (`float`)** | Number with a decimal point. |
| **High-level language** | Far from hardware, close to human language. |
| **Index** | An item's position in a list, starting at 0. |
| **Input / Output (I/O)** | What you give the computer / what it gives back. |
| **Integer (`int`)** | Whole number, positive or negative. |
| **Interpreter** | Translates and runs code (like a human translator). |
| **Key / Value** | Label and its data in a dictionary. |
| **List (`list`)** | Ordered, changeable collection in `[ ]`. |
| **Low-level language** | Close to the hardware. |
| **Mutable** | Can be changed after creation (lists, dicts). |
| **Placeholder** | `{}` in a string waiting to be filled by `.format()`. |
| **Program** | A set of instructions for a computer. |
| **Prompt** | The message shown by `input()`. |
| **REPL** | Interactive Python shell (`>>>`), Read–Eval–Print Loop. |
| **String (`str`)** | Sequence of characters inside quotes. |
| **String interpolation** | Inserting variable values into a string. |
| **Syntax / SyntaxError** | Grammar of the language / breaking that grammar. |
| **Type casting** | Converting a value from one type to another. |
| **TypeError** | Error from using incompatible types together. |
| **Variable** | A name that holds a value for later reuse. |
| **Zero-based** | Counting starts at 0. |

---

## 18. One-Page Cheat Sheet

```python
# ── RUN ───────────────────────────────────────────
# python3 --version          check install
# python3 file.py            run a file
# Ctrl + /                   comment/uncomment in VS Code

# ── VARIABLES & TYPES ─────────────────────────────
name   = "Noor"         # str
age    = 60             # int
cpu    = 75.1           # float
active = True           # bool
ips    = ["10.0.0.1", "10.0.0.2"]            # list
cfg    = {"image": "nginx", "port": 80}      # dict
type(age)               # <class 'int'>

# ── LIST ──────────────────────────────────────────
ips[0]        ips[-1]        len(ips)
ips.append(x) ips.insert(i, x) ips.remove(x) ips.pop(i)  ips.sort()
"10.0.0.1" in ips       # True

# ── DICT ──────────────────────────────────────────
cfg["port"]             # read
cfg["env"] = "prod"     # add (new key)
cfg["port"] = 8080      # update (existing key)

# ── CASTING ───────────────────────────────────────
int("20")  str(20)  float("3.5")  bool(0)

# ── STRINGS ───────────────────────────────────────
"Noor" + " " + "Siddique"                    # concatenation
f"My name is {name}, age {age}"              # f-string ✅
"My name is {}, age {}".format(name, age)    # order matters

# ── INPUT ─────────────────────────────────────────
city = input("City: ")                 # always str
age  = int(input("Age: "))             # cast for numbers

# ── REMEMBER ──────────────────────────────────────
# =  stores     ==  compares
# /  → float    // → whole number
# lists start at index 0
# Python is case-sensitive
```

---

*Next class: operations in Python. Later in the course: working with JSON, reading files, REST APIs and objects/classes.*
