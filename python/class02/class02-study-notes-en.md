# Python for DevOps — Class 02 Study Notes

**Topic:** From storing data to making decisions and building reusable code
**Covers:** list operations, dictionary manipulation, `for` loops, indentation, list comprehension, arithmetic / comparison / logical operators, `if / elif / else`, functions, naming conventions, classes and objects (OOP)
**Builds on:** [Class 01 notes](../class01/class01-study-notes-en.md) (data types, variables, type casting, f-strings, `input()`)
**Practice files to create as you read:** `list.py`, `dict.py`, `loop.py`, `arithmetic.py`, `condition.py`, `user_function.py`, `python_class.py`

> **About these notes:** they are built from the class recording. Code that was typed on screen has been reconstructed and every example here was run on Python 3 to confirm the output shown. Where the class used a simplified or loose explanation, a **Precision note** gives the exact version.

---

## Table of Contents

1. [Recap and Today's Roadmap](#1-recap-and-todays-roadmap)
2. [Lists: Access and Traversal](#2-lists-access-and-traversal)
3. [List Operations](#3-list-operations)
4. [Algorithm Thinking: Why Am I Learning This?](#4-algorithm-thinking-why-am-i-learning-this)
5. [Dictionaries: Access, Add, Update](#5-dictionaries-access-add-update)
6. [Loops](#6-loops)
7. [List Comprehension](#7-list-comprehension)
8. [Arithmetic Operators](#8-arithmetic-operators)
9. [Comparison Operators](#9-comparison-operators)
10. [Conditions: if, elif, else](#10-conditions-if-elif-else)
11. [Logical Operators](#11-logical-operators)
12. [Functions](#12-functions)
13. [Naming Conventions](#13-naming-conventions)
14. [Classes and Objects](#14-classes-and-objects)
15. [Questions Asked in Class](#15-questions-asked-in-class)
16. [Common Misconceptions, Corrected](#16-common-misconceptions-corrected)
17. [Practice Problems](#17-practice-problems)
18. [Interview Corner](#18-interview-corner)
19. [Glossary](#19-glossary)
20. [One-Page Cheat Sheet](#20-one-page-cheat-sheet)

---

## 1. Recap and Today's Roadmap

Milestones finished so far in the course: Git and GitHub, Linux and shell scripting, computer networking. Python is the current milestone, and class 01 stopped at `input()`.

The class opened with a quick quiz on data types:

| Value on screen | Type | DevOps example |
|---|---|---|
| `"us-east-1"` | `str` | region, IP address, Git branch name |
| `22` | `int` | port number |
| `78.4` | `float` | CPU usage percent |
| `["10.0.0.1", "10.0.0.2"]` | `list` — can hold **any** data type | server IPs |
| `{"image": "nginx"}` | `dict` — `key: value` pairs, can hold any type, even another dict or list | container config |

Class 01 was about **holding** data. Class 02 is about **doing things** with it:

```
Data types (class 01)
      ↓
Operate on collections      → list and dict operations
      ↓
Repeat work                 → loops
      ↓
Calculate and compare       → arithmetic and comparison operators
      ↓
Decide                      → if / elif / else, and / or
      ↓
Package and reuse           → functions
      ↓
Model real things           → classes and objects
```

The course repository has seven lab exercises. The instructor's advice: understand the concepts first, then do the labs slowly. No deadline has been set yet.

---

## 2. Lists: Access and Traversal

```python
grocery_list = ["Eggs", "Milk", "Bread"]
```

- The things inside the square brackets are **items** or **elements**.
- Positions are called **indexes** and start at **0** (zero-based).
- `grocery_list` itself is a **variable** that holds a list.

### 2.1 Access by index

```python
print(grocery_list[0])    # Eggs
print(grocery_list[1])    # Milk
print(grocery_list[-1])   # Bread  ← last item, however long the list is
```

Run it the engineer's way:

```bash
python3 list.py
```

**Terminal tip:** type `python3 li` and press **Tab**. The shell completes the file name for you. This works for every command-line tool.

### 2.2 Why `-1` matters

With three items you can see the last one with your eyes. A real program can't. Imagine the list of every student enrolled in the current batch. To find out who joined **last**, you don't count: you ask for index `-1` and get back that student's record, something like `{"name": "Rahim", "age": 21}`.

### 2.3 Traverse / iterate

When you **don't** know the position, for example a database returning 100,000 items, the program must visit the items one by one: go to the first, then the next, then the next. Visiting every item once is called **traversing** or **iterating** the list. Section 6 shows how.

> **Precision note:** in class, reading one item by position was called "indexing or slicing". These are two different things. **Indexing** returns **one** item: `grocery_list[1]` → `"Milk"`. **Slicing** returns a **sub-list** using a `start:stop` range: `grocery_list[1:3]` → `["Milk", "Bread"]` (the stop position is not included).

### 2.4 Commenting out

Select lines and press `Ctrl + /`. Python goes line by line, sees the `#`, and skips those lines. If every line except one `print()` is commented out, only that one action runs.

---

## 3. List Operations

Tomorrow a new student enrols. Someone leaves. You need to check whether a person is on the list. Lists are **mutable** (changeable), and these are the tools.

### 3.1 Add: `append()` and `insert()`

```python
grocery_list = ["Eggs", "Milk", "Bread"]

grocery_list.append("Apples")       # always goes to the END
print(grocery_list)                 # ['Eggs', 'Milk', 'Bread', 'Apples']

grocery_list.insert(1, "Butter")    # goes to the position YOU choose
print(grocery_list)                 # ['Eggs', 'Butter', 'Milk', 'Bread', 'Apples']
```

| | `append(x)` | `insert(i, x)` |
|---|---|---|
| Where does the item go? | Always at the **end** | At index `i`; everything after it shifts right |
| Arguments | the item | the position, then the item |

### 3.2 Remove: `remove()` and `pop()`

```python
grocery_list.remove("Milk")         # remove by VALUE
print(grocery_list)                 # ['Eggs', 'Butter', 'Bread', 'Apples']

removed = grocery_list.pop(2)       # remove by POSITION and hand the item back
print(removed)                      # Bread
print(grocery_list)                 # ['Eggs', 'Butter', 'Apples']
```

**The jar picture:** think of a jar. Items go in, and "pop" takes one out. `pop()` with no number takes out the **last** item, which is exactly how a **stack** works.

> **Precision note:** the class called the two actions "pop in" and "pop out". The standard names are **push** (put in) and **pop** (take out). In Python, `append()` is the push.

### 3.3 Search: the `in` operator

Fifty-seven people are in the class and the instructor calls out "Is Iftekhar bhai here?" That's a search. A second picture: your spouse hands you a shopping list; at the shop you look at the list to check "were eggs on it?" before buying.

```python
if "Eggs" in grocery_list:
    print("Yes, you need to buy eggs.")
```

`"Eggs" in grocery_list` is a **condition**. It evaluates to `True` or `False`.

`in` also works on any **sequence**, including strings, where it finds a **substring** (a smaller piece of text inside a bigger one):

```python
print("Ifti" in "Iftekhar")     # True
print("ifti" in "Iftekhar")     # False  ← case-sensitive!
```

**Real use:** a student complains "I can't see the course videos." First step: is their email `in` the enrolled-students list?

### 3.4 Count: `len()`

```python
print(len(grocery_list))    # 3
```

With three items you can count by eye. With a few hundred thousand, "you'd go grey counting". `len` is short for *length*, the same way `str` is short for string and `int` for integer.

**Don't confuse count with index.** The instructor asked "how many elements?" on purpose to check this:

| List | Count (`len`) | Indexes |
|---|---|---|
| `["Eggs", "Milk", "Bread"]` | **3** | 0, 1, 2 |

The last index is always `len(list) - 1`.

### 3.5 Sort: `sort()`

Humans naturally arrange things A, B, C or 1, 2, 3. That is **sorting**.

```python
items = ["Milk", "Bread", "Cocoa", "Apples", "Eggs"]
items.sort()
print(items)        # ['Apples', 'Bread', 'Cocoa', 'Eggs', 'Milk']

numbers = [4, 3, 7, 1]
numbers.sort()
print(numbers)      # [1, 3, 4, 7]
```

| Order | Meaning | How |
|---|---|---|
| **Ascending** (default) | A → Z, small → large | `numbers.sort()` |
| **Descending** | Z → A, large → small | `numbers.sort(reverse=True)` |

### 3.6 Join two lists: `+` (concatenation)

Two batches are being invited to one reunion. Each batch is a list. You want one combined list so you can email and call everyone.

```python
list1 = [4, 3, 7, 1]
list2 = [10, 6, 2]

full_list = list1 + list2
full_list.sort()
print(full_list)    # [1, 2, 3, 4, 6, 7, 10]
```

The same `+` that joins strings (class 01) joins lists. **It only works when both sides are lists.**

### 3.7 ⚠️ The `None` trap

`append()`, `insert()`, `remove()` and `sort()` change the list **in place** and give back **nothing** (`None`). This mistake happened live in class:

```python
print(full_list.sort())             # None   ← the list got sorted, but sort() returns nothing

all_ips = prod_ips.append(dev_ip)   # all_ips is None!
```

Correct pattern: do the operation on one line, then use the list on the next.

```python
full_list.sort()
print(full_list)
```

If you want a **new** sorted list and the original untouched, use the built-in `sorted()`:

```python
print(sorted([3, 1, 2]))    # [1, 2, 3]
```

### 3.8 DevOps example: collecting IPs

```python
prod_ips = ["10.0.1.10", "10.0.1.11"]
dev_ip = "10.0.2.20"                    # a single string

prod_ips.append(dev_ip)                 # one item → append
print(prod_ips)                         # ['10.0.1.10', '10.0.1.11', '10.0.2.20']
```

`prod_ips` is already a list, so there is no need to build a new one. Just append to it.

If the dev IPs are also a **list**, use `+`:

```python
prod_ips = ["10.0.1.10", "10.0.1.11"]
dev_ips = ["10.0.2.20", "10.0.2.21"]

all_ips = prod_ips + dev_ips
print(all_ips)      # ['10.0.1.10', '10.0.1.11', '10.0.2.20', '10.0.2.21']
```

And if you mix them up:

```python
prod_ips + dev_ip
# ❌ TypeError: can only concatenate list (not "str") to list
```

### 3.9 Summary table

| You want to… | Use | Changes the original? | Gives back |
|---|---|---|---|
| Add one item at the end | `lst.append(x)` | Yes | `None` |
| Add one item at a position | `lst.insert(i, x)` | Yes | `None` |
| Remove by value | `lst.remove(x)` | Yes | `None` |
| Remove by position | `lst.pop(i)` | Yes | the removed item |
| Check membership | `x in lst` | No | `True` / `False` |
| Count items | `len(lst)` | No | a number |
| Sort | `lst.sort()` | Yes | `None` |
| Sorted copy | `sorted(lst)` | No | a new list |
| Join two lists | `lst1 + lst2` | No | a new list |

> **Precision note:** in class, `append`, `insert`, `in`, `len` and `sort` were all loosely called "keywords". The exact names matter in interviews:
> - **Keyword:** a word reserved by the language itself: `in`, `for`, `if`, `def`, `class`, `and`, `or`.
> - **Built-in function:** called on its own: `len(x)`, `print(x)`, `sorted(x)`.
> - **Method:** a function that belongs to an object, called with a dot: `grocery_list.append(x)`, `grocery_list.sort()`, `name.upper()`.

---

## 4. Algorithm Thinking: Why Am I Learning This?

Knowing syntax is important. But the instructor's point was that syntax is the easy part ("পানি ভাত"). What matters is the **algorithm**:

> **An algorithm is the steps through which we accomplish a task.**

The reunion example as an algorithm:

1. I have `list1`.
2. I have `list2`.
3. Concatenate them with `+`.
4. Sort the result.
5. Print it.

That step-by-step plan is what turns syntax into a result.

### The lesson behind the lesson

The instructor once studied Python for 12 hours straight and still had no idea what a list was *for*. "What do I do with a list in real life?" The reunion example answers it: two batches, joined into one list, sorted, and then everyone gets called.

> **The real learning is how you are thinking.** After each topic, stop and ask: *"Why am I learning this? What is a real-life example of this?"* If you only copy-type tutorials without asking **why**, the learning never becomes smooth.

---

## 5. Dictionaries: Access, Add, Update

```python
employee_directory = {
    "EMP101": "Anis Rahman",
    "EMP102": "Sultana Kamal",
    "EMP103": "Tamim Iqbal"
}
```

**How to recognise a dictionary:** curly braces `{ }` (second brackets) with `key: value` pairs inside. If you see square brackets `[ ]` instead, it's a list. Knowing the structure by sight is the first skill.

### 5.1 Access by key

A list gives you an item by **position** (`0`, `1`, `2`). A dictionary has no positions. You give it the **key** and it returns the **corresponding value**, the same way you look up "father" and find "বাবা".

```python
print(employee_directory["EMP102"])     # Sultana Kamal
print(employee_directory["EMP103"])     # Tamim Iqbal
```

### 5.2 Add a new item

The batch has 160 students. Five more enrol tomorrow. To add one:

```python
employee_directory["EMP104"] = "Sakib Al Hasan"
```

Dictionary name, the new key in square brackets, `=`, the value. That's all.

### 5.3 Update an existing item: data manipulation

Employee `EMP101` was entered wrongly. It should be Khalilur Hasan.

```python
employee_directory["EMP101"] = "Khalilur Hasan"
print(employee_directory)
# {'EMP101': 'Khalilur Hasan', 'EMP102': 'Sultana Kamal',
#  'EMP103': 'Tamim Iqbal', 'EMP104': 'Sakib Al Hasan'}
```

Everything in a dictionary happens **through the key**. Give a key that exists and the value is **overwritten**. Give a key that doesn't exist and a new item is **added**. Changing stored data like this is **data manipulation**.

**DevOps use:** assigning a dynamic IP: "use this IP now", overwriting the old value under the same key.

### 5.4 Many names, one shape

| Where | What they call it |
|---|---|
| Python | **dictionary** (`dict`) |
| JavaScript | object |
| Computer science in general | hash map |
| Files and APIs | JSON (JavaScript Object Notation) |

Curly braces around key-value pairs is a global convention. The instructor's claim: in an engineering career this is the shape of data you will work with **most**, and engineers smile widest when data arrives as JSON. JSON gets its own session next class.

> **Precision note:** a Python dict and JSON *look* alike but are not the same thing. **JSON is a text format** used in files and API responses. A **dict is a live data structure** in Python's memory. Python converts between the two (`json.loads()` / `json.dumps()`), which is next class's topic. And in Python the word "object" is wider than "dictionary": **everything** in Python is an object (see Section 14.8).

> **Precision note:** the class mentioned that Python is annoying because you must `print()` everything, while a browser's JavaScript console shows `2 + 2` immediately. Python has the same convenience: the interactive shell (`python3` → `>>>`) echoes results without `print()`. It's **script files** that need `print()`, and that is true for JavaScript files too.

---

## 6. Loops

Searching a list is `in`. But what if you want to **visit every item and do something with each one**? The simple way to traverse (iterate) is a **loop**. Python has two: **`for`** and **`while`**.

### 6.1 The `for` loop

```python
fruits = ["apple", "banana", "orange"]

for item in fruits:
    print(item)
```

Output:
```
apple
banana
orange
```

Read it aloud as: **"for each item in fruits, do something."**

Anatomy:

```
for   item   in   fruits  :
───   ────   ──   ──────  ─
 │     │     │      │     └── colon: "a block of code starts here"
 │     │     │      └──────── the list to walk through
 │     │     └─────────────── keyword
 │     └───────────────────── loop variable: holds the CURRENT item
 └─────────────────────────── keyword
    print(item)      ← indented block: runs once per item
```

What happens when it runs:

| Round (iteration) | `item` holds | Action |
|---|---|---|
| 1 | `"apple"` | prints `apple` |
| 2 | `"banana"` | prints `banana` |
| 3 | `"orange"` | prints `orange` |
| — | no items left | loop says "I'm done" and exits |

Each round is called an **iteration**. What you do with the item inside the loop is entirely up to you: print it, add to it, multiply it, store it somewhere.

### 6.2 The loop variable is only a placeholder

```python
for x in fruits:
    print(x)        # exactly the same result
```

`item`, `element`, `x`, `y`: Python doesn't care. The name has no special meaning. It is a placeholder that receives each item in turn.

**Convention for readable code:** name the list in the **plural** and the loop variable in the **singular**.

```python
for student in students:
for price in dollar_prices:
for ip in server_ips:
```

### 6.3 Indentation and code blocks

After the colon, press Enter. The editor moves the cursor in by **four spaces**. That gap is **indentation**, and the indented lines form a **block of code**.

- The standard is **4 spaces**. In VS Code the **Tab** key inserts them for you.
- 2 spaces also runs, as long as you are consistent inside the block. 4 is the standard (PEP 8).
- **Zero indentation does not work.** Writing the loop body level with the `for` is an error:

```python
for item in fruits:
print(item)
# ❌ IndentationError: expected an indented block after 'for' statement
```

You will meet blocks in exactly four places today: **loops**, **conditions** (`if / elif / else`), **functions** (`def`) and **classes** (`class`). The pattern is always the same: a line ending in a **colon**, then an **indented block**.

Indentation also decides what is *inside* the loop and what is *after* it:

```python
for item in fruits:
    print(item)      # inside: runs 3 times
print("Done")        # outside: runs once, after the loop finishes
```

### 6.4 Worked example: dollars to cents

Prices are stored in dollars: shoes, a belt, a cap. `1.50` dollars means 1 dollar 50 cents, or 150 cents. Convert every price to cents.

**Algorithm:** make an empty list → loop over the prices → multiply each by 100 → append the result → print.

```python
dollar_prices = [1.50, 4.99, 10.00, 25.50]
cent_prices = []                        # an EMPTY list, to be filled later

for price in dollar_prices:
    cents = price * 100
    cent_prices.append(cents)

print(cent_prices)      # [150.0, 499.0, 1000.0, 2550.0]
```

`[]` is another way to create a list: start empty, then `append`, `insert` or concatenate into it later.

Trace of a smaller version, as drawn in class:

| Iteration | `price` | `price * 100` | `cent_prices` after append |
|---|---|---|---|
| 1 | `1.5` | `150.0` | `[150.0]` |
| 2 | `2.5` | `250.0` | `[150.0, 250.0]` |
| 3 | `3.5` | `350.0` | `[150.0, 250.0, 350.0]` |

Three items, three rounds, then the loop exits and the final `print` runs.

> **Precision note:** the results are floats (`150.0`, not `150`) because float × int gives a float. Floats can also carry tiny rounding errors (`1.1 * 100` gives `110.00000000000001`). For whole cents write `round(price * 100)`, which gives `[150, 499, 1000, 2550]`.

### 6.5 Why loops matter in DevOps: reading a log file

Remember log files from the Linux classes? Line after line, each marked `INFO`, `WARNING`, `ERROR` or `CRITICAL`. The algorithm for a Python log scanner:

1. Open (read) the file.
2. Read all the lines. This gives you a **list** of lines.
3. Loop through the lines.
4. `if "ERROR" in line` → print the line, or append it to another list.

Here is that algorithm on a list of lines (reading the actual file comes next class):

```python
log_lines = [
    "INFO server started",
    "ERROR disk full",
    "WARNING cpu high",
    "ERROR db connection lost",
]

error_lines = []
for line in log_lines:
    if "ERROR" in line:
        error_lines.append(line)

print(error_lines)      # ['ERROR disk full', 'ERROR db connection lost']
```

This is what `grep ERROR app.log` does for you behind the scenes. Everything you did with `grep` and `sed` can also be written as a Python script. Notice that this small program uses a **list**, a **loop**, the **`in`** substring search, a **condition** and **`append`** together.

### 6.6 Looping over other things

A loop works on any sequence, not only lists:

```python
for ch in "dev":
    print(ch)           # d, e, v on separate lines
```

### 6.7 The `while` loop (named in class, not demonstrated)

`for` and `while` have different uses. `for` walks through a collection. `while` keeps repeating **as long as a condition stays true**:

```python
count = 1
while count <= 3:
    print("Health check", count)
    count += 1          # same as: count = count + 1
```

Output:
```
Health check 1
Health check 2
Health check 3
```

If the condition never becomes false, the loop never stops (an **infinite loop**). Press `Ctrl + C` in the terminal to stop it.

---

## 7. List Comprehension

*(Advanced. Beginners: get comfortable with the plain `for` loop first. That was the instructor's advice too.)*

The dollars-to-cents loop took four lines. A **list comprehension** does the same job in one:

```python
cent_prices = [price * 100 for price in dollar_prices]
```

How it maps to the loop:

```
[  price * 100      for price in dollar_prices  ]
   ───────────      ──────────────────────────
   what to put      the same for-loop header
   in the new list
└─ the square brackets play the role of the empty list + append ─┘
```

### 7.1 With a filter: `if` at the END

"Only convert prices greater than 2 dollars":

```python
[round(price * 100) for price in dollar_prices if price > 2]
# [499, 1000, 2550]         ← 1.50 was left out
```

The equivalent loop:

```python
for price in dollar_prices:
    if price > 2:
        cent_prices.append(round(price * 100))
```

### 7.2 With `if ... else`: BEFORE the `for`

A student asked: "What if I want an `else`?" For example, multiply by 100 when the price is above 2, **otherwise** multiply by 10. Writing `else` at the end is a `SyntaxError`, which is what happened live in class. When there is an `else`, the whole condition moves **in front of** the `for`:

```python
[round(price * 100) if price > 2 else round(price * 10) for price in dollar_prices]
# [15, 499, 1000, 2550]
```

| Form | Pattern | Effect |
|---|---|---|
| Filter | `[value for x in items if condition]` | **Skips** items that fail the condition |
| Choose | `[a if condition else b for x in items]` | **Keeps every item**, picks between two values |

### 7.3 When *not* to use it

A student asked what to do if the loop body needs several lines of work. Answer: comprehensions are for compressing short 3–5 line loops. If the body has five or seven operations, use a normal `for` loop. Readable beats clever.

---

## 8. Arithmetic Operators

```python
print(7 + 2)     # 9     addition
print(7 - 2)     # 5     subtraction
print(5 * 2)     # 10    multiplication (asterisk / star)
print(4 / 2)     # 2.0   division → always a float
print(5 % 2)     # 1     modulus → the REMAINDER (ভাগশেষ)
print(2 ** 3)    # 8     power → 2 to the power 3
print(2 ** 5)    # 32
```

| Operator | Name | Example | Result |
|---|---|---|---|
| `+` | add | `7 + 2` | `9` |
| `-` | subtract | `7 - 2` | `5` |
| `*` | multiply | `5 * 2` | `10` |
| `/` | divide | `4 / 2` | `2.0` |
| `//` | floor divide (class 01) | `7 // 2` | `3` |
| `%` | modulus (mod) | `5 % 2` | `1` |
| `**` | power (exponent) | `2 ** 3` | `8` |

### 8.1 Why the modulus matters: even or odd

`%` gives the **remainder** after division. Divide any number by 2: if the remainder is `0` the number is **even**, otherwise it is **odd**.

```python
x = 5

if x % 2 == 0:
    print("The number is even")
else:
    print("The number is odd")      # ← this runs: 5 % 2 is 1
```

This tiny program is where competitive programmers start, and it packs in five ideas at once: a **variable**, an **arithmetic operator** (`%`), a **comparison operator** (`==`), a **condition** (`if / else`) and **indentation** (blocks of code).

### 8.2 One star or two

```python
print(5 * 2)     # 10   ← multiplication
print(5 ** 2)    # 25   ← power
```

One character changes the meaning completely. Knowing Python's syntax is not optional.

> **Precision note:** the word is **remainder** (what is left over), not "reminder". And in many calculators and spreadsheets `^` means power, but in Python `^` is a different operator (bitwise XOR): `5 ^ 2` gives `7`, not `25`. For power, always use `**`.

---

## 9. Comparison Operators

A comparison asks a question and answers `True` or `False`.

```python
print(2 == 2)    # True
print(2 != 2)    # False
print(2 > 3)     # False
print(2 >= 3)    # False
print(3 <= 3)    # True
print(2 < 3)     # True
```

| Operator | Meaning | Example | Result |
|---|---|---|---|
| `==` | equal to | `2 == 2` | `True` |
| `!=` | not equal to | `2 != 2` | `False` |
| `>` | greater than | `2 > 3` | `False` |
| `<` | less than | `2 < 3` | `True` |
| `>=` | greater than **or** equal to | `3 >= 3` | `True` |
| `<=` | less than **or** equal to | `3 <= 3` | `True` |

Two rules:

1. **`=` stores, `==` compares.** `x = 5` puts 5 into `x`. `x == 5` asks "is the left side equal to the right side?"
2. **In two-character operators the `=` always comes last:** `>=`, `<=`, `!=`. Writing them reversed (`=>`, `=<`, `=!`) is a `SyntaxError`. Python will complain. This is a very common beginner mistake.

---

## 10. Conditions: if, elif, else

A condition lets the program **choose** what to do based on whether something is true or false. Three keywords:

| Keyword | Read as | Runs when |
|---|---|---|
| `if` | "if" | its condition is `True` |
| `elif` | "else if" (Python's short form) | every condition above was `False` **and** this one is `True` |
| `else` | "otherwise" | nothing above was `True` |

### 10.1 Two branches: pass or fail

You need 60% to earn the course certificate.

```python
pass_mark = 60
your_mark = 89

if your_mark >= pass_mark:
    print("You get a certificate")
else:
    print("You don't deserve a certificate yet. Practice more.")
```

Syntax pattern (the same as a loop): keyword, condition, **colon**, **indented block**.

### 10.2 Three branches: adding `elif`

Someone who scored 50–59 came close. Give them an encouraging message instead of the harsh one.

```python
pass_mark = 60
your_mark = 59

if your_mark >= pass_mark:
    print("You get a certificate")
elif your_mark >= 50 and your_mark < 60:
    print("So close (কানের পাশ দিয়ে গেছে)! Work a little harder.")
else:
    print("You don't deserve a certificate yet. Practice more.")
```

### 10.3 How Python walks through it

Python checks from the top and runs **only the first branch that matches**. Then it leaves the whole structure. Later branches are not even looked at.

| `your_mark` | `>= 60`? | `>= 50 and < 60`? | What runs |
|---|---|---|---|
| `89` | ✅ True | *(not checked)* | "You get a certificate" |
| `59` | ❌ False | ✅ True and ✅ True | "So close!" |
| `49` | ❌ False | ❌ False and ✅ True → False | `else` → "Practice more" |

Notes:

- You can have **as many `elif` branches as you need**.
- `else` is **optional**. Use it when you want a final "otherwise".
- In the `elif` above, `your_mark < 60` is technically unnecessary: if Python reached that line, the mark is already known to be below 60. It was kept in class to demonstrate `and`. It does no harm.
- Marks can also come from the user: `your_mark = int(input("What mark did you obtain? "))`. Remember to cast, because `input()` always returns a `str`.

### 10.4 Conditions in DevOps

**Alerting.** CPU at 90% is a problem. Someone must be told.

```python
cpu_usage = 93

if cpu_usage >= 90:
    print("ALERT: CPU is too high. Sending email...")
else:
    print("All normal. Nothing to do.")
```

**Access control.** Different roles get different permissions.

```python
role = "collaborator"

if role == "admin":
    print("Full access")
elif role == "collaborator":
    print("Read-only access")
else:
    print("No access")
```

---

## 11. Logical Operators

Logical operators combine conditions.

| Operator | Rule | `True` when… |
|---|---|---|
| `and` | **both** the left side and the right side must be true | `True and True` |
| `or` | **either** the left side or the right side must be true | at least one side is `True` |

### 11.1 The same marks example, `and` versus `or`

With `your_mark = 49`:

```python
your_mark >= 50 and your_mark < 60
#    False      and     True         →  False   → falls through to else
```

```python
your_mark >= 50 or your_mark < 60
#    False      or      True         →  True    → prints "So close!"
```

Changing one word changes who gets the encouraging message. With `or`, a score of 49 (or even 10) matches, because the right side `your_mark < 60` is true by itself. That is the wrong logic for this problem, which is why the example uses `and`.

### 11.2 Truth table

| A | B | `A and B` | `A or B` |
|---|---|---|---|
| True | True | **True** | **True** |
| True | False | False | **True** |
| False | True | False | **True** |
| False | False | False | False |

> **Beyond the class:** there is a third logical operator, `not`, which flips a value: `not True` → `False`. Python also lets you write a range check in one go: `50 <= your_mark < 60`.

---

## 12. Functions

### 12.1 Built-in functions: the ones Python ships with

`print()`, `input()`, `len()`, `type()`, `sorted()`. When you install Python, these come along "like the screws and batteries in the box", there to make life easier. Someone already wrote the many lines behind `print`, so you don't have to.

**How to spot a function:** a name followed by **parentheses**, with or without something inside: `print("hi")`, `len(x)`, `make_sum()`. Whether Python made it or a human did, the parentheses are the sign.

> **Precision note:** `upper()`, `lower()` and `sort()` were listed as built-in functions in class. They are **methods**: functions attached to a value and called with a dot (`name.upper()`, `items.sort()`). The parentheses rule still holds for them. Parentheses mean something is being **called**: a function, a method, or (Section 14) a class.

### 12.2 Writing your own function

Functions you create are **user-defined** (custom) functions.

```python
def make_sum():
    total = 3 + 2
    print(total)
```

Anatomy:

```
def   make_sum  ( )  :
───   ────────  ───  ─
 │       │       │   └── colon → a block of code follows
 │       │       └────── parentheses (parameters go inside)
 │       └────────────── function name, in snake_case
 └────────────────────── keyword: "define"
    total = 3 + 2     ← indented block = the function BODY
    print(total)
```

> **Naming tip:** in class the variable inside was named `sum`. It works, but `sum` is already the name of a built-in function (`sum([1, 2, 3])` → `6`), and reusing the name hides it. `total` is the safer choice.

### 12.3 A function must be *called*

Run the file above and **nothing happens**. No output at all.

Why? Think about `print` and `len`. They exist inside Python all the time, but they never do anything until your code uses them. Your function is the same. `def` only **declares** it. To run it, **call** it by name with parentheses:

```python
make_sum()      # 5
make_sum()      # 5   ← call it as many times as you want
```

### 12.4 Parameters and arguments

Problem: the function above can only ever add 3 and 2. To add 4 and 5, would you write a second function? You want to write it **once** and change the numbers a million times.

```python
def make_sum(a, b):         # a and b are PARAMETERS
    total = a + b
    print(total)

make_sum(2, 3)              # 5    ← 2 and 3 are ARGUMENTS
make_sum(5, 6)              # 11
```

| Term | What it is | Where it lives |
|---|---|---|
| **Parameter** | a placeholder name | in the `def` line: `def make_sum(a, b)` |
| **Argument** | the actual value you pass | in the call: `make_sum(2, 3)` |

The instructor's view: don't lose sleep over which is which; many people just say "params". For interviews, the table above is the exact distinction.

This is how `print` works too. It was written once and you pass it different things every time.

> **Beyond the class: `print` versus `return`.** The functions above only *display* the answer. To hand the answer **back** so other code can use it, a function uses `return`:
>
> ```python
> def add(a, b):
>     return a + b
>
> result = add(2, 3)      # result is 5
> print(result * 10)      # 50
> ```
>
> A function with no `return` gives back `None`. That is the same reason `list.sort()` gave `None` in Section 3.7.

### 12.5 Why write functions: reuse

> **A function is a reusable block of code that performs a task. Write it once, call it again and again.**

`print` was written once by the Python team. Millions of people call it. Your functions can serve the same purpose: fewer repeated lines, one place to fix a bug.

**Reuse across files.** A function written in one file can be **imported** into another. Files used this way are called **modules**:

```python
# in another file in the same folder
from user_function import make_sum

make_sum(10, 20)        # 30
```

(More on modules and installing packages in the next class.)

### 12.6 DevOps example: function + condition + comparison

```python
def check_usage(usage):
    if usage >= 80:
        print("ALERT: server in danger!")
    else:
        print("All good. DevOps engineer can sleep.")

check_usage(90)     # ALERT: server in danger!
check_usage(70)     # All good. DevOps engineer can sleep.
```

Picture this function running automatically every minute with the current usage. At 90 it raises an alert. At 70 it stays quiet and nobody gets woken at 3 a.m. One small example hooks together a **function**, a **parameter**, a **condition** and a **comparison operator**. Real programs are built by combining these pieces.

### 12.7 Variable versus function: noun versus verb

```python
memory = 60     # just holds a value; it cannot DO anything
```

| | Variable | Function |
|---|---|---|
| Job | **Holds** a value | **Performs** an action |
| English grammar | noun / adjective | **verb** |
| Example | `memory = 60` | `check_usage(60)` |

(Side note from class: to a software engineer, **memory** means **RAM**. Hard disk space is **disk** usage. Don't mix them up.)

Hold on to noun and verb. The next section is built from exactly those two things.

---

## 13. Naming Conventions

When a name has more than one word you can't use a space. There are four common styles ("cases"):

| Case | Looks like | Why the name | Use in Python |
|---|---|---|---|
| **snake_case** | `my_name` | lies flat and low like a snake | ✅ variables, functions, file names |
| **camelCase** | `myName` | a hump in the middle like a camel | Valid, but it's the Java / JavaScript habit |
| **PascalCase** | `MyName` | every word starts with a capital | ✅ class names |
| **kebab-case** | `my-name` | words on a skewer like a kebab | ❌ cannot be used for Python names |

```python
def make_sum():         # function → snake_case
total_price = 100       # variable → snake_case

class HumanSurvey:      # class → PascalCase
```

> **Precision note:** the class said "don't use kebab-case" as a matter of standards. It is stricter than that: in Python, kebab-case is **impossible**. `my-name = 1` is read as "`my` minus `name`" and raises a `SyntaxError`. You will still see kebab-case in places that aren't Python code: URLs, Git branch names, Docker image names, Kubernetes resource names.

---

## 14. Classes and Objects

This is "your Python class about Python classes".

### 14.1 Two ways to organise a program

If a program needs 50 functions, you can write function after function after function. Or you can group related data and functions together into **classes**. That second style is **Object-Oriented Programming (OOP)**.

> **Precision note:** the first style was called "functional programming" in class, and a student suggested "procedural". The student's word is the accurate one: a program organised as a sequence of functions is **procedural programming**. ("Functional programming" is the name of a different, more specialised style.) So the two styles to know are **procedural** and **object-oriented**.

### 14.2 The blueprint idea

**The house.** Your grandfather leaves you three katha of land. You sketch what you want on paper: four rooms, a big drawing-dining space, two windows and two doors per room. You hand the sketch to an architect or civil engineer, who produces a **blueprint**. From that one blueprint, on plots of the same size, you could build thousands of houses.

**The car.** Ford designs a three-wheeler: three wheels (two at the back, one in front), one engine, one fuel system. That design is a blueprint. The factory builds car after car from it.

**BongoDev enrolment.** You enrol with an email, a password and a payment. The backend does not contain a separate Python file per student. It has **one** `Student` blueprint. Each enrolment creates one new student record from it, and thousands of students come from a single blueprint.

**AWS.** Behind the EC2 console sits a blueprint: "if the user selects these options, build them a machine". The actual machine you get is called an **EC2 instance**. Now you know where that word comes from.

### 14.3 Getting the three words right

| Term | Meaning | House analogy |
|---|---|---|
| **Class** | the **blueprint** | the architect's drawing |
| **Object** | a **real thing** built from the class | an actual house |
| **Instance** | the same thing as an object: "an instance *of* the class" | "this house is an instance of that blueprint" |
| **Instantiate** | to create an object from a class | to build a house |

> **Precision note:** in class, "object" and "class" were used as if they were the same thing ("class = object = blueprint", and the real thing = "instance of the object"). The standard definitions, and the ones interviewers expect, are:
> - **Class = the blueprint.**
> - **Object = instance = the real thing built from the blueprint.**
>
> So `noor` below is **an object**, and equally **an instance of the `HumanSurvey` class**. The two words describe the same thing from two angles.

### 14.4 What goes inside a class: nouns and verbs

Back to the three-wheeler:

```
wheels = 3            ┐
engine = 1            ├─ hold values  → VARIABLES  (nouns)  → called ATTRIBUTES
fuel_system = 1       ┘

switch_on()           ┐
switch_off()          ┴─ do something → FUNCTIONS  (verbs)  → called METHODS
```

Turning the switch on triggers the fuel system, which drives the engine, which turns the wheels, and the car moves. That's an action, so it's a function.

| Inside a class | It's really a… | Grammar | Example |
|---|---|---|---|
| **Attribute** | variable | noun | `self.name`, `self.status` |
| **Method** | function | verb | `introduce()`, `stop_server()` |

Inside a class we normally say **method** instead of function. Saying "function" is not wrong.

### 14.5 Writing a class

Class 01's census survey, as a class:

```python
class HumanSurvey:
    def __init__(self, name, age, has_wife, kids):
        self.name = name
        self.age = age
        self.has_wife = has_wife
        self.kids = kids

    def introduce(self):
        print(f"I am {self.name}. I am {self.age} years old and I have {len(self.kids)} kids.")
```

Piece by piece:

| Piece | Meaning |
|---|---|
| `class` | Keyword that starts a class, the way `def` starts a function. |
| `HumanSurvey` | The class name in **PascalCase**: every word starts with a capital letter. |
| `:` + indentation | A block of code again. |
| `__init__` | The **constructor**: a special method that Python runs **automatically** each time a new object is created. Two underscores on each side ("dunder init"). |
| `self` | The object being created or used. See the note below. |
| `name, age, has_wife, kids` | Parameters: the information you must supply when creating an object. |
| `self.name = name` | "Store the `name` I was given **on this object**." These stored values are the **attributes**. |
| `introduce(self)` | A **method**. It reads the object's own data through `self`. |

> **Precision note on `self`:** the class explanation was "`self` defines the class itself". More exactly, `self` refers to **the particular object (instance)** being created or used, not to the class. When you call `noor.introduce()`, `self` *is* `noor`. When you call `bob.introduce()`, `self` *is* `bob`. That is how one method can serve thousands of objects while each keeps its own data. You never pass `self` yourself. Python fills it in.

### 14.6 Creating objects (instances)

A surveyor goes door to door. At each house the **same form** is filled in with **different data**. A class is called just like a function, with the values passed in:

```python
noor = HumanSurvey("Noor", 60, False, ["Liam", "Maya"])
bob = HumanSurvey("Bob", 45, True, [])

noor.introduce()        # I am Noor. I am 60 years old and I have 2 kids.
print(bob.name)         # Bob
```

- `noor` and `bob` are two separate **objects** made from **one** class.
- `noor.introduce()` calls a **method** (with parentheses).
- `bob.name` reads an **attribute** (no parentheses).
- You never write "Noor" inside `introduce`. The method picks up whatever name the object was created with.

### 14.7 DevOps example: an EC2 server class

```python
class EC2Server:
    def __init__(self, server_id, ip_address, instance_type):
        self.server_id = server_id
        self.ip_address = ip_address
        self.instance_type = instance_type      # t2.micro, t2.small, t2.large ...
        self.status = "running"                 # every new server starts as running

    def start_server(self):
        self.status = "running"
        return f"Server {self.server_id} ({self.instance_type}) is running at {self.ip_address}"

    def stop_server(self):
        self.status = "stopped"


web_server = EC2Server("i-0a1b2c3d", "10.0.1.10", "t2.micro")
db_server = EC2Server("i-0e4f5a6b", "10.0.1.20", "t2.large")

print(web_server.start_server())
# Server i-0a1b2c3d (t2.micro) is running at 10.0.1.10

db_server.stop_server()
print(db_server.status)     # stopped
print(web_server.status)    # running   ← each object keeps its OWN status
```

One blueprint, two servers. Stopping the database server does not touch the web server.

The same thing *could* be written with loose functions and loose variables. Classes give you a **cleaner** way to organise it: the data and the actions that belong together stay together.

### 14.8 In Python, everything is an object

```python
x = "hello"
```

In VS Code, type `x.` and look at the suggestion list: `count`, `find`, `format`, `index`, `join`, `lower`, `upper`, `title`… All of those are **methods** that every string carries.

```python
print(x.upper())        # HELLO
print(x.title())        # Hello
print(x.count("l"))     # 2
print(type(x))          # <class 'str'>
```

That `<class 'str'>` from class 01 now makes sense. `str` is a **class**, a blueprint written by the Python team. `"hello"` is an **object** built from it, and it comes with attributes and methods. The same is true of numbers (`<class 'int'>`), lists (`<class 'list'>`) and even functions. In Python, everything is an object.

---

## 15. Questions Asked in Class

**Q: `prod_ips + dev_ips` and `append` gave the same-looking result. How do I know which to use?**
It's your choice, guided by what you have in hand:
- You have **two lists** → `+` joins them.
- You have a list and **one item** → `append` (at the end) or `insert` (at a position).
- `+` only works list-to-list. `list + "string"` is a `TypeError`.

*One extra warning:* `append` adds its argument as **one single item**. If you append a list to a list, you get a list **inside** a list:

```python
prod_ips = ["10.0.1.10", "10.0.1.11"]
prod_ips.append(["10.0.2.20", "10.0.2.21"])
print(prod_ips)     # ['10.0.1.10', '10.0.1.11', ['10.0.2.20', '10.0.2.21']]   ← 3 items, not 4
```

To pour the items of one list into another in place, use `extend()`: `prod_ips.extend(dev_ips)`.

**Q: What if a loop needs several lines of work? Can a list comprehension do that?**
Comprehensions are for short loops. Add a filter with `if` at the end, or a choice with `if ... else` before the `for`. Beyond that, write a normal `for` loop.

**Q: From a DevOps point of view, shell scripting and Python do similar jobs. Do we use them interchangeably?**
Use **shell (Bash) scripts** for what they are good at: running commands and gluing tools together. When the work becomes more **operation-intensive** (real logic, data processing, JSON, APIs), reach for **Python**. And in the age of AI, knowing Python is essential for a DevOps engineer.

---

## 16. Common Misconceptions, Corrected

| ❌ Misconception | ✅ Reality |
|---|---|
| "A list with 3 items has indexes 1 to 3." | Count is 3; indexes are 0, 1, 2. Last index = `len(list) - 1`. |
| "`grocery_list[1]` is slicing." | That's **indexing** (one item). **Slicing** uses a range: `grocery_list[1:3]`. |
| "`append` lets me choose where the item goes." | `append` always adds at the **end**. `insert(i, x)` takes a position. |
| "`remove` and `pop` are the same." | `remove(x)` deletes by **value**. `pop(i)` deletes by **position** and **returns** the item. |
| "`new_list = old_list.append(x)` gives me the updated list." | `append`, `insert`, `remove`, `sort` change the list in place and return **`None`**. |
| "`print(my_list.sort())` prints the sorted list." | It prints `None`. Sort first, then print. Or use `sorted(my_list)`. |
| "`+` can add a single item to a list." | `+` joins **list + list** only. `list + "text"` is a `TypeError`. |
| "`in` only works on lists." | It works on any sequence. On strings it finds a **substring**. It is case-sensitive. |
| "`append`, `len`, `sort` are keywords." | `in`, `for`, `if`, `def`, `class` are **keywords**. `len`, `print` are **built-in functions**. `append`, `sort`, `upper` are **methods**. |
| "A dictionary and JSON are the same thing." | They look alike. JSON is a **text format**; a dict is a **Python data structure**. Python converts between them. |
| "Adding and updating a dict item use different syntax." | Same syntax: `d[key] = value`. New key → add. Existing key → overwrite. |
| "The loop variable must be called `item`." | Any name works. It's a placeholder. Prefer a meaningful singular name. |
| "Indentation is just for looks." | In Python, indentation **is** the grammar. It defines what belongs to a block. Missing it is an `IndentationError`. |
| "In a comprehension, `else` goes at the end." | Filter-only `if` goes at the **end**. `if ... else` goes **before** the `for`. |
| "It's called the *reminder*." | It's the **remainder** (ভাগশেষ), and the operator is modulus `%`. |
| "`^` means power." | In Python, power is `**`. `5 ^ 2` is `7` (bitwise XOR), not `25`. |
| "`=>` and `>=` both work." | Only `>=`, `<=`, `!=`. The `=` always comes **last**. |
| "After `if` matches, Python still checks the `elif`s." | Only the **first** matching branch runs. The rest are skipped. |
| "`and` and `or` are interchangeable." | `and` needs **both** sides true. `or` needs **at least one**. Swapping them changes the logic. |
| "Defining a function runs it." | `def` only declares it. Nothing happens until you **call** it: `make_sum()`. |
| "Parameter and argument are the same word." | **Parameter:** the name in the definition. **Argument:** the value in the call. |
| "kebab-case is just bad style in Python." | It's **invalid**: `my-name` means `my` minus `name`. |
| "Class and object are the same thing." | **Class** = blueprint. **Object** = **instance** = the real thing built from it. |
| "`self` means the class." | `self` is the **specific object** the method is working on. |
| "Python has two styles: functional and OOP." | The function-after-function style is **procedural**. The class-based style is **object-oriented**. |
| "Python always needs `print()`; JavaScript doesn't." | Both have an interactive console that echoes results. Both need an explicit print in script files. |

---

## 17. Practice Problems

> Try each one yourself first. Open the solution only to compare.

### Problem 1 — Even or odd, 1 to 10

Print each number from 1 to 10 with the word `even` or `odd` next to it.

*Hint:* `range(1, 11)` produces the numbers 1 to 10 (the stop value is not included).

<details>
<summary>Show solution</summary>

```python
for number in range(1, 11):
    if number % 2 == 0:
        print(number, "even")
    else:
        print(number, "odd")
```

</details>

### Problem 2 — The reunion list

Batch 4 emails: `["rahim@x.com", "karim@x.com"]`. Batch 5 emails: `["sadia@x.com", "anis@x.com"]`. Build one sorted list, print it, print how many people are invited, and check whether `"anis@x.com"` is invited.

<details>
<summary>Show solution</summary>

```python
batch_4 = ["rahim@x.com", "karim@x.com"]
batch_5 = ["sadia@x.com", "anis@x.com"]

invite_list = batch_4 + batch_5
invite_list.sort()

print(invite_list)
print(len(invite_list))

if "anis@x.com" in invite_list:
    print("Anis is invited")
```

</details>

### Problem 3 — High-CPU servers

Given `cpu_readings = [45.2, 91.5, 78.0, 95.1]`, build a new list with only the readings that are 90 or above. Do it once with a `for` loop and once with a list comprehension.

<details>
<summary>Show solution</summary>

```python
cpu_readings = [45.2, 91.5, 78.0, 95.1]

# for loop
high = []
for reading in cpu_readings:
    if reading >= 90:
        high.append(reading)
print(high)                                             # [91.5, 95.1]

# list comprehension
high = [reading for reading in cpu_readings if reading >= 90]
print(high)                                             # [91.5, 95.1]
```

</details>

### Problem 4 — Three-level alert function

Write `check_cpu(usage)`: 90 or more → `CRITICAL`, 70 to 89 → `WARNING`, otherwise → `OK`. Call it with 95, 75 and 40.

<details>
<summary>Show solution</summary>

```python
def check_cpu(usage):
    if usage >= 90:
        print("CRITICAL")
    elif usage >= 70:
        print("WARNING")
    else:
        print("OK")

check_cpu(95)   # CRITICAL
check_cpu(75)   # WARNING
check_cpu(40)   # OK
```

The `elif` doesn't need `and usage < 90`: Python only reaches it when `usage >= 90` was already false.

</details>

### Problem 5 — A `Student` class

Create a `Student` class with `name`, `email` and `paid` (True/False), and a method `show_status()` that prints whether the student has course access. Create two students and call the method on both.

<details>
<summary>Show solution</summary>

```python
class Student:
    def __init__(self, name, email, paid):
        self.name = name
        self.email = email
        self.paid = paid

    def show_status(self):
        if self.paid:
            print(f"{self.name} has course access")
        else:
            print(f"{self.name} needs to complete payment")


rahim = Student("Rahim", "rahim@x.com", True)
sadia = Student("Sadia", "sadia@x.com", False)

rahim.show_status()     # Rahim has course access
sadia.show_status()     # Sadia needs to complete payment
```

</details>

---

## 18. Interview Corner

**Q: Difference between `append()` and `insert()`?**
`append(x)` adds `x` at the end of the list. `insert(i, x)` adds `x` at index `i` and shifts later items right.

**Q: Difference between `remove()` and `pop()`?**
`remove(x)` deletes the first item equal to `x` and returns nothing. `pop(i)` deletes the item at index `i` (the last item if no index is given) and returns it.

**Q: Difference between `sort()` and `sorted()`?**
`list.sort()` is a method that sorts the list in place and returns `None`. `sorted(iterable)` is a built-in function that returns a new sorted list and leaves the original unchanged.

**Q: How do you check whether a value exists in a list or a string?**
With the `in` operator: `"x" in my_list`, `"err" in "error log"`. It returns a boolean.

**Q: What is a `for` loop?**
A construct that iterates over a sequence, running an indented block once for each item, with the loop variable holding the current item.

**Q: Why does indentation matter in Python?**
Python uses indentation instead of braces to mark blocks of code. The lines indented under a `for`, `if`, `def` or `class` belong to it. Wrong or missing indentation is an `IndentationError`.

**Q: What is a list comprehension?**
A one-line way to build a new list from an existing sequence: `[expression for item in sequence if condition]`.

**Q: What does `%` do, and where is it useful?**
It returns the remainder of a division. `n % 2 == 0` tests whether `n` is even.

**Q: Difference between `=` and `==`?**
`=` assigns a value to a variable. `==` compares two values and returns `True` or `False`.

**Q: Difference between `and` and `or`?**
`and` is true only when both conditions are true. `or` is true when at least one is.

**Q: What is a function and why use one?**
A named, reusable block of code that performs a task. Define it once with `def`, call it many times, optionally with different arguments. It removes repetition.

**Q: Parameter versus argument?**
A parameter is the variable name in the function definition. An argument is the actual value passed when calling the function.

**Q: What is a class? What is an object?**
A class is a blueprint that defines attributes (data) and methods (behaviour). An object, also called an instance, is a concrete thing created from that class. Many objects can be created from one class.

**Q: What is `__init__`?**
The constructor (initialiser) method. Python calls it automatically when an object is created, and it sets up the object's attributes.

**Q: What is `self`?**
The reference to the current object inside a class's methods. It lets each object read and change its own attributes.

**Q: When would you choose Python over a shell script?**
Shell is ideal for running commands and simple automation. Python is the better choice when the task involves real logic, data structures, JSON, APIs or anything that has to be maintained and reused.

---

## 19. Glossary

| Term | Meaning |
|---|---|
| **Algorithm** | The steps through which a task is accomplished. |
| **`and` / `or`** | Logical operators: both sides true / at least one side true. |
| **Append** | Add an item to the end of a list. |
| **Argument** | The actual value passed to a function when calling it. |
| **Arithmetic operator** | `+ - * / // % **`. |
| **Ascending / Descending** | Small → large (A → Z) / large → small (Z → A). |
| **Attribute** | A variable that belongs to an object (`self.name`). |
| **Block of code** | Indented lines that belong to a `for`, `if`, `def` or `class`. |
| **Blueprint** | The design from which real things are built; what a class is. |
| **Built-in function** | A function that comes with Python (`print`, `len`, `sorted`). |
| **Call** | Run a function by writing its name with parentheses. |
| **camelCase** | Naming style: `myName`. |
| **Class** | A blueprint for creating objects. |
| **Comparison operator** | `== != > < >= <=`; produces `True` or `False`. |
| **Concatenation** | Joining two lists (or two strings) with `+`. |
| **Condition** | An expression that is `True` or `False`, used to decide what runs. |
| **Constructor (`__init__`)** | The method that runs automatically when an object is created. |
| **Data manipulation** | Changing stored data: adding, updating, deleting. |
| **`def`** | Keyword that defines a function. |
| **`elif`** | "Else if": an additional condition checked when earlier ones failed. |
| **Empty list (`[]`)** | A list with no items, ready to be filled. |
| **Indentation** | Leading spaces (standard: 4) that mark a block of code. |
| **Indexing** | Reading one item by position: `lst[1]`. |
| **Instance** | An object created from a class. |
| **Iterate / Traverse** | Visit each item of a collection one by one. |
| **Iteration** | One round of a loop. |
| **JSON** | JavaScript Object Notation: a text format of key-value data. |
| **kebab-case** | Naming style: `my-name`. Not valid for Python names. |
| **Keyword** | A word reserved by Python (`for`, `in`, `if`, `def`, `class`). |
| **List comprehension** | One-line syntax for building a list from a loop. |
| **Logical operator** | `and`, `or`, `not`. |
| **Loop** | A structure that repeats a block of code (`for`, `while`). |
| **Loop variable** | The name that holds the current item in a `for` loop. |
| **Method** | A function that belongs to an object, called with a dot. |
| **Module** | A Python file whose code can be imported into another file. |
| **Modulus (`%`)** | Operator that returns the remainder of a division. |
| **`None`** | Python's "nothing" value; what a function gives back when it returns nothing. |
| **Object** | A real thing created from a class; same as an instance. |
| **OOP** | Object-Oriented Programming: organising code around classes and objects. |
| **Parameter** | A placeholder name in a function definition. |
| **PascalCase** | Naming style: `MyName`. Used for class names. |
| **Pop** | Remove an item by position and return it. |
| **Procedural programming** | Organising a program as a series of functions. |
| **Remainder** | What is left over after division. |
| **`return`** | Hands a value back from a function to the caller. |
| **`self`** | The current object, inside a class's methods. |
| **Slicing** | Taking a sub-list with a range: `lst[1:3]`. |
| **snake_case** | Naming style: `my_name`. Python's standard for variables and functions. |
| **Sorting** | Arranging items in order. |
| **Stack** | A collection where the last item put in is the first taken out. |
| **Substring** | A smaller string inside a bigger one. |
| **User-defined function** | A function you write yourself. |
| **`while` loop** | Repeats as long as a condition is true. |

---

## 20. One-Page Cheat Sheet

```python
# ── LIST OPERATIONS ───────────────────────────────
lst = ["Eggs", "Milk", "Bread"]
lst[0]    lst[-1]    lst[1:3]          # first, last, slice
lst.append("Apples")                   # add at end
lst.insert(1, "Butter")                # add at position
lst.remove("Milk")                     # delete by value
item = lst.pop(2)                      # delete by position, returns item
"Eggs" in lst                          # True / False
len(lst)                               # count
lst.sort()    lst.sort(reverse=True)   # in place → returns None
sorted(lst)                            # new sorted list
all_items = list1 + list2              # list + list only

# ── DICT OPERATIONS ───────────────────────────────
emp["EMP102"]                          # read by key
emp["EMP104"] = "Sakib"                # new key → add
emp["EMP101"] = "Khalil"               # existing key → update

# ── LOOP ──────────────────────────────────────────
for price in prices:                   # colon + 4-space indent
    print(price * 100)

while count <= 3:
    count += 1

# ── LIST COMPREHENSION ────────────────────────────
[p * 100 for p in prices]                          # transform
[p * 100 for p in prices if p > 2]                 # filter  (if at END)
[p * 100 if p > 2 else p * 10 for p in prices]     # choose  (if/else BEFORE for)

# ── OPERATORS ─────────────────────────────────────
# arithmetic :  +  -  *  /  //  %  **        (5 % 2 → 1,  2 ** 3 → 8)
# comparison :  ==  !=  >  <  >=  <=         (= always LAST)
# logical    :  and (both)   or (either)   not

# ── CONDITION ─────────────────────────────────────
if mark >= 60:
    print("pass")
elif mark >= 50:
    print("close")
else:
    print("fail")

# ── FUNCTION ──────────────────────────────────────
def make_sum(a, b):                    # a, b = parameters
    return a + b

make_sum(2, 3)                         # 2, 3 = arguments → 5

# ── CLASS ─────────────────────────────────────────
class EC2Server:                       # PascalCase
    def __init__(self, server_id):     # constructor, runs automatically
        self.server_id = server_id     # attribute (noun)
        self.status = "running"

    def stop_server(self):             # method (verb)
        self.status = "stopped"

web = EC2Server("i-0a1b")              # object / instance
web.stop_server()                      # call a method
web.status                             # read an attribute → "stopped"

# ── REMEMBER ──────────────────────────────────────
# colon + indentation = block of code (for, if, def, class)
# append/insert/remove/sort return None — don't assign or print them
# def only declares; a function runs when CALLED
# class = blueprint; object = instance = the real thing
# snake_case for variables/functions, PascalCase for classes
```

---

*Next class (Sunday): more advanced Python: working with files, reading JSON, calling APIs, and installing packages.*
