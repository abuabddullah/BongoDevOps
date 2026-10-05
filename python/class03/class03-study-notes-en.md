# Python for DevOps — Class 03 Study Notes

**Topic:** From Python basics to a real application — modules, JSON, files, REST APIs, a Flask web app, and debugging
**Covers:** custom and built-in modules, `import` styles, JSON format, reading and writing files, `json.load` / `json.dump`, nested data access, loops and filters on real data, APIs and CRUD, the `requests` library, `try / except`, web servers, Flask, SQLite, virtual environments, `pdb`
**Builds on:** [Class 01 notes](../class01/class01-study-notes-en.md) (data types, variables, f-strings, `input()`) and [Class 02 notes](../class02/class02-study-notes-en.md) (loops, conditions, functions, classes and objects)
**Course repo folders used today:** `python-genius-main/11.JSON-Data`, `12.modules`, `13.flast-student-management-app`, `14.pdb-set-trace-debugger.py`

> **About these notes:** they are built from the class recording (4 October) and the course repository. **Every file from folders 11–14 is copied in full inside these notes**, so you never need to open the folders to follow along. Every program was run on Python 3 (Flask 3.1, requests 2.34) and the outputs shown are real. Where the class used a simplified or loose explanation, a **Precision note** gives the exact version. Where the repo code has a bug, a **Bug alert** shows the problem and a fixed version.

---

## Table of Contents

1. [Recap and Today's Roadmap](#1-recap-and-todays-roadmap)
2. [The Project Map: Every File We Use Today](#2-the-project-map-every-file-we-use-today)
3. [Modules](#3-modules)
4. [JSON: The Language of Data Exchange](#4-json-the-language-of-data-exchange)
5. [Working with Files](#5-working-with-files)
6. [Reading JSON and Exploring Nested Data](#6-reading-json-and-exploring-nested-data)
7. [Writing JSON: Adding a New Student](#7-writing-json-adding-a-new-student)
8. [APIs, REST and CRUD](#8-apis-rest-and-crud)
9. [Calling a Real API with `requests`](#9-calling-a-real-api-with-requests)
10. [Web Development Basics](#10-web-development-basics)
11. [The Flask Student Management App: Full Code Walkthrough](#11-the-flask-student-management-app-full-code-walkthrough)
12. [Virtual Environments and Running the App](#12-virtual-environments-and-running-the-app)
13. [CRUD in Action: Following One Request End to End](#13-crud-in-action-following-one-request-end-to-end)
14. [Debugging with `pdb`](#14-debugging-with-pdb)
15. [Questions Asked in Class](#15-questions-asked-in-class)
16. [Bugs and Gotchas in the Course Repo](#16-bugs-and-gotchas-in-the-course-repo)
17. [Common Misconceptions, Corrected](#17-common-misconceptions-corrected)
18. [Practice Exercises](#18-practice-exercises)
19. [Interview Corner](#19-interview-corner)
20. [Glossary](#20-glossary)
21. [One-Page Cheat Sheet](#21-one-page-cheat-sheet)

---

## 1. Recap and Today's Roadmap

The class opened with an analogy. Learning from YouTube is like reading the whole Ramayana overnight and then asking "so who was Sita?" You studied everything, but you can't do anything with it.

> **Buying rice, lentils and spices is not cooking biryani.**

Classes 01 and 02 bought the ingredients: data types, lists, dictionaries, loops, conditions, functions, classes. **Class 03 cooks the biryani:** a real **Student Management System** you can use for a school, a college or any organisation. It stores students in a database through classes and objects, lets you add, edit and delete them from the browser, and serves the data as JSON through an API.

**Today's path:**

```
Modules  →  JSON  →  Files (read / write)  →  APIs & CRUD  →  requests library
        →  Web servers & Flask  →  The full Student Management App  →  Debugging with pdb
```

The instructor's request for today: **don't stop to ask questions; try to understand the flow.** Doubt-clearing sessions come later.

---

## 2. The Project Map: Every File We Use Today

This is the part of the course repo that Class 03 uses. Every file listed here is reproduced in full later in these notes.

```text
python-genius-main/
│
├── 11.JSON-Data/                      ← Sections 4–7 and 9
│   ├── students.json                  data: a course with 10 students
│   ├── app.py                         read JSON, check types, loop, filter, search
│   ├── inser-data-in-json.py          add a student and save; turn dicts into objects
│   └── json-api.py                    fetch posts from a public REST API
│
├── 12.modules/                        ← Section 3
│   ├── modules.py                     a custom module: math functions + a variable
│   └── app.py                         imports and uses modules.py (written live in class)
│
├── 13.flast-student-management-app/   ← Sections 10–13  ("flast" is a typo for "flask")
│   ├── README.md                      how to set up and run the app
│   ├── requirements.txt               which packages the app needs
│   ├── app.py                         Flask routes: web pages + REST API (entry point)
│   ├── database.py                    connect to SQLite, create the table
│   ├── students.db                    the SQLite database file (created automatically)
│   ├── models/
│   │   ├── __init__.py                empty file: marks "models" as a package
│   │   └── student.py                 Student class + StudentRepository class
│   ├── templates/
│   │   ├── index.html                 home page: add form + student table
│   │   └── edit.html                  edit form for one student
│   └── static/
│       └── style.css                  page styling
│
└── 14.pdb-set-trace-debugger.py       ← Section 14: pause a script and inspect it
```

| Folder | One-line purpose | Where in these notes |
|---|---|---|
| `12.modules` | Split code into files and reuse it with `import` | [Section 3](#3-modules) |
| `11.JSON-Data` | Read, explore, filter and write JSON; call a public API | [Sections 4–7](#4-json-the-language-of-data-exchange), [9](#9-calling-a-real-api-with-requests) |
| `13.flast-student-management-app` | A complete web app: browser → Flask → class → SQLite → JSON | [Sections 10–13](#10-web-development-basics) |
| `14.pdb-set-trace-debugger.py` | Stop a program mid-way and step through it | [Section 14](#14-debugging-with-pdb) |

> The class taught modules first (folder 12), then JSON (folder 11). These notes follow the class order.

---

## 3. Modules

### 3.1 What is a module?

> **A module is simply a Python file (`.py`). It can contain functions, classes and variables, and other files can `import` it to reuse them.**

That's all. Any `.py` file you write is already a module. Name it anything: `modules.py`, `hello.py`, `utils.py`.

### 3.2 Two kinds of modules

| Kind | Where it comes from | Examples | Install needed? |
|---|---|---|---|
| **Built-in module** | Python's **standard library**, which is installed together with Python | `os`, `sys`, `shutil`, `json`, `datetime`, `sqlite3`, `pdb` | No, just `import` it |
| **Custom module** | A `.py` file **you** wrote | `modules.py` in folder 12 | No, it's your own file |
| **Third-party package** *(seen later today)* | Downloaded from the internet (PyPI) | `requests`, `flask` | **Yes**, with `pip install` |

**The standard library** is everything that comes with Python by default, like the apps that come preinstalled on a new phone. What it gives you:

| Module | Used for |
|---|---|
| `os` | Operating-system tasks: files, folders, environment variables |
| `sys` | Information about the running Python (arguments, version, import path) |
| `shutil` | Copying, moving and deleting files and folders |
| `json` | Reading and writing JSON data |
| `datetime` | Dates and times |
| `sqlite3` | A complete database, with no database server to install |
| `pdb` | The Python debugger |

The instructor's favourite point: **Python ships with a database (`sqlite3`).** You never install MongoDB, PostgreSQL or MySQL to start building.

There are hundreds of modules. Nobody memorises them all. Learn the popular ones as you need them.

### 3.3 The module file: `12.modules/modules.py` (full file)

```python
def add(a, b):
    """Returns the sum of two numbers."""
    return a + b

def subtract(a, b):
    """Returns the difference of two numbers."""
    return a - b

def multiply(a, b):
    """Returns the product of two numbers."""
    return a * b

def divide(a, b):
    """Returns the quotient. Handles division by zero."""
    if b == 0:
        return "Error: Cannot divide by zero"
    return a / b

def remainder(a, b):
    """Returns the remainder (modulus)."""
    if b == 0:
        return "Error: Cannot calculate remainder of zero"
    return a % b


name = 'Nure'
```

`a` and `b` are **parameters**: the inputs each function expects. The text in triple quotes under each `def` is a **docstring**, a built-in description of the function.

### 3.4 Using it: `12.modules/app.py`

> In the repo this file is **empty**. It was typed live in class. Here is the reconstructed version, matching the outputs shown on screen.

```python
import modules

print(modules.add(10, 5))
print(modules.divide(10, 5))
print(modules.remainder(10, 3))
print(modules.name)
```

Run it from inside the folder:

```bash
cd 12.modules
python3 app.py
```

Output:
```
15
2.0
1
Nure
```

- `import modules` uses the **file name without `.py`**. If the file were `hello.py`, you'd write `import hello`.
- After importing, use `modulename.thing`: `modules.add(...)`, `modules.name`.
- **VS Code tip:** hold `Ctrl` and click `add` in `modules.add`. The editor jumps to the function inside `modules.py`. The code lives in one file and is reused in another.

### 3.5 Three ways to import

```python
# 1. Import the whole module
import modules
print(modules.add(10, 5))

# 2. Import with an alias (a nickname)
import modules as calc
print(calc.add(10, 5))

# 3. Named import: pick only what you need
from modules import add, subtract, name
print(add(10, 5))        # no "modules." prefix needed
print(subtract(10, 5))
print(name)
```

Output of style 3:
```
15
5
Nure
```

| Style | Syntax | You then write | Good for |
|---|---|---|---|
| Whole module | `import modules` | `modules.add()` | Clear where each name comes from |
| Alias | `import modules as calc` | `calc.add()` | Long module names (`import numpy as np`) |
| Named import | `from modules import add, name` | `add()` | Using a few names often |

**Alias analogy from class:** the instructor's full name is Nur Alam Siddique, but back home everyone calls him by a nickname. An alias is a nickname for a module.

> **Precision note: aliases.** In class the alias was `import modules as math`. Avoid that name, because `math` is already a standard-library module. Your alias hides the real `math` in that file, which can confuse you later. Pick something unique, like `calc`.

> **Precision note: "named import saves memory".** In class, named imports were described as loading only the chosen functions to save memory. In reality, Python **runs and loads the entire module either way**. `from modules import add` only decides which names appear in *your* file. The real benefits are **readability** and **avoiding name clutter**, not memory.

### 3.6 Why didn't we need a path?

A student asked: *"How does Python find `modules.py` without a path?"*

Python looks for modules in a list of places called **`sys.path`**. The **first** place is the folder of the script you're running. `app.py` and `modules.py` sit in the same folder, so Python finds it immediately. Put the module in another folder and the import fails unless you make it a package or adjust the path.

### 3.7 Why use modules at all?

| Principle | Meaning | How modules help |
|---|---|---|
| **DRY** (Don't Repeat Yourself) | Write logic once, reuse it everywhere | Write `add()` once, import it into any file |
| **Clean code** | Code that is easy to read and maintain | Small files with one job each, not one 5,000-line file |
| **Separation of concerns** | Each part of the program handles one responsibility | The Flask app splits routes, database and models into separate files (Section 11) |

> "Knowing how to code is not enough. You also need to know the best practices."

> **Bug alert: `divide()` returns a string on error.** `divide(4, 0)` returns `"Error: Cannot divide by zero"`, a `str`, while normal results are `float`. Code that expects a number will break later in a confusing place. A cleaner design is to let the error happen (`ZeroDivisionError`) or `raise ValueError("b must not be zero")`, and handle it with `try / except` (Section 9.3).

---

## 4. JSON: The Language of Data Exchange

### 4.1 What is JSON?

**JSON = JavaScript Object Notation.** It's a text format for storing and exchanging data as **key: value pairs**, almost identical to a Python dictionary.

- JSON files end in **`.json`**, just as Python files end in `.py`.
- Most data you get from the internet (API responses) is JSON.
- It's the most common format for configuration and data exchange in DevOps work.

### 4.2 The data file: `11.JSON-Data/students.json`

Shape of the file (first student only):

```json
{
    "course": "DevOps & Cloud Engineering",
    "batch": 6,
    "active": true,
    "students": [
        {
            "id": 101,
            "name": "Rahim",
            "age": 24,
            "cgpa": 3.65,
            "skills": ["Linux", "Python", "Docker"],
            "address": {
                "city": "Dhaka",
                "country": "Bangladesh"
            }
        }
    ]
}
```

<details>
<summary><b>Show the full <code>students.json</code> (all 10 students)</b></summary>

```json
{
    "course": "DevOps & Cloud Engineering",
    "batch": 6,
    "active": true,
    "students": [
        {
            "id": 101,
            "name": "Rahim",
            "age": 24,
            "cgpa": 3.65,
            "skills": ["Linux", "Python", "Docker"],
            "address": {"city": "Dhaka", "country": "Bangladesh"}
        },
        {
            "id": 102,
            "name": "Karim",
            "age": 27,
            "cgpa": 3.82,
            "skills": ["Python", "AWS", "Terraform"],
            "address": {"city": "Chattogram", "country": "Bangladesh"}
        },
        {
            "id": 103,
            "name": "Ayesha",
            "age": 22,
            "cgpa": 3.91,
            "skills": ["Python", "Git", "Linux"],
            "address": {"city": "Sylhet", "country": "Bangladesh"}
        },
        {
            "id": 104,
            "name": "Hasan",
            "age": 26,
            "cgpa": 3.48,
            "skills": ["Docker", "Kubernetes", "AWS"],
            "address": {"city": "Rajshahi", "country": "Bangladesh"}
        },
        {
            "id": 105,
            "name": "Nusrat",
            "age": 23,
            "cgpa": 3.74,
            "skills": ["Python", "Ansible", "Terraform"],
            "address": {"city": "Khulna", "country": "Bangladesh"}
        },
        {
            "id": 106,
            "name": "Sakib",
            "age": 25,
            "cgpa": 3.56,
            "skills": ["Linux", "Docker", "GitHub Actions"],
            "address": {"city": "Barishal", "country": "Bangladesh"}
        },
        {
            "id": 107,
            "name": "Mim",
            "age": 21,
            "cgpa": 3.88,
            "skills": ["Python", "AWS", "Docker"],
            "address": {"city": "Cumilla", "country": "Bangladesh"}
        },
        {
            "id": 108,
            "name": "Tanvir",
            "age": 28,
            "cgpa": 3.41,
            "skills": ["Linux", "Kubernetes", "Prometheus"],
            "address": {"city": "Rangpur", "country": "Bangladesh"}
        },
        {
            "id": 109,
            "name": "Farzana",
            "age": 24,
            "cgpa": 3.95,
            "skills": ["Python", "Terraform", "AWS"],
            "address": {"city": "Mymensingh", "country": "Bangladesh"}
        },
        {
            "id": 110,
            "name": "Arif",
            "age": 29,
            "cgpa": 3.69,
            "skills": ["Docker", "Kubernetes", "Grafana"],
            "address": {"city": "Dhaka", "country": "Bangladesh"}
        }
    ]
}
```

*(In the repo each list is spread over several lines with `indent=4`. The content is identical.)*

</details>

### 4.3 Every data structure you've learned, in one file

```
data  (dict)
├── "course"   → "DevOps & Cloud Engineering"   (str)
├── "batch"    → 6                              (int)
├── "active"   → true                           (bool)
└── "students" → [ ... ]                        (list of 10 dicts)
                   └── each student (dict)
                       ├── "id"      → 101                    (int)
                       ├── "name"    → "Rahim"                (str)
                       ├── "age"     → 24                     (int)
                       ├── "cgpa"    → 3.65                   (float)
                       ├── "skills"  → ["Linux", ...]         (list inside a dict)
                       └── "address" → {"city": ..., ...}     (dict inside a dict)
```

A dictionary holding a list, holding dictionaries, holding lists and more dictionaries. **This is why data structures matter.** Real backend data looks exactly like this, whether it describes students, products or servers.

### 4.4 JSON vs Python: small but important differences

| JSON (text in a file) | Python (after loading) |
|---|---|
| object `{ }` | `dict` |
| array `[ ]` | `list` |
| string `"text"` | `str` |
| number `24` / `3.65` | `int` / `float` |
| `true` / `false` | `True` / `False` |
| `null` | `None` |

- JSON strings **must** use **double quotes**. `{'name': 'Rahim'}` is valid Python but **invalid JSON**:
  ```
  JSONDecodeError: Expecting property name enclosed in double quotes
  ```
- JSON has no comments and no trailing commas.

**Tip from class:** paste JSON into an online **JSON viewer / formatter** (search "JSON viewer") to see it as a collapsible tree. In class the viewer rejected the pasted data. That's because `print(data)` shows a **Python dict** (single quotes, `True`), which is not valid JSON. Paste the original `.json` file, or print `json.dumps(data, indent=4)` instead.

---

## 5. Working with Files

Opening a file by hand: double-click, work, close the window. A program does the same: **open → work → close**.

### 5.1 `with open(...)` — the safe way

```python
with open("students.json", "r") as file:
    data = json.load(file)
# here the file is already closed
```

Reading it word by word:

| Part | Meaning |
|---|---|
| `with` | "Handle cleanup for me": the file is **closed automatically** when the indented block ends, even if an error happens |
| `open("students.json", "r")` | Open this file (name is a **string**) in **read** mode |
| `as file` | Give the opened file a short name (an alias) to use inside the block |
| `:` + indented block | Everything indented runs while the file is open |

### 5.2 File modes

| Mode | Name | What it does | If file doesn't exist |
|---|---|---|---|
| `"r"` | read | Read only (default) | Error |
| `"w"` | write | **Erases** the file, then writes | Creates it |
| `"a"` | append | Adds to the end | Creates it |
| `"x"` | create | Creates a new file only | Creates it (error if it exists) |

⚠️ `"w"` empties the file **the moment it opens**. Read what you need first (Section 7).

### 5.3 Relative paths depend on where you run from

`open("students.json")` looks in the **current working directory** (the folder your terminal is in), **not** the folder the script lives in. That's why the instructor always did `cd 11.JSON-Data` before running. Run from the parent folder instead and you get:

```
FileNotFoundError: [Errno 2] No such file or directory: 'students.json'
```

Notice that every folder has its own `app.py`. They are different files, and you choose which one runs by `cd`-ing into the right folder.

---

## 6. Reading JSON and Exploring Nested Data

### 6.1 The full file: `11.JSON-Data/app.py`

```python
import json

with open("students.json", "r") as file:  # it opens the file and closes after the operation
    data = json.load(file)

print(data)


# Data checking
print(type(data))
print(type(data["course"]))
print(type(data["batch"]))
print(type(data["students"]))

student = data["students"][0]

print(type(student))
print(type(student["name"]))
print(type(student["age"]))
print(type(student["cgpa"]))
print(type(student["skills"]))


# Accessing nested information:
print(data["course"])

print(data["students"][0]["name"])

print(data["students"][0]["skills"])

print(data["students"][0]["address"]["city"])


# Running loop on data:
for student in data["students"]:
    print(student["name"], student["age"])



# Running extended loop on data:
for student in data["students"]:
    print(f"{student['name']} knows: ")

    for skill in student["skills"]:
        print(f"- {skill}")

# Filtering data using loop:
for student in data["students"]:
    if student["cgpa"] > 3.7:
        print(student["name"])


# Find for a skill and find all students who know that skill.
skill_to_find = input("Enter a skill: ").lower()

for student in data["students"]:
    for skill in student["skills"]:
        if skill_to_find == skill.lower():
            print(student["name"])
        else:
            print("No students at bongoDev has the target skill")
```

Run it:
```bash
cd 11.JSON-Data
python3 app.py
```

In class the instructor commented parts out (`Ctrl + /`) and ran one block at a time. We'll do the same below.

### 6.2 `json.load` vs `json.loads`

| Function | Reads from | Example |
|---|---|---|
| `json.load(file)` | an open **file** | `data = json.load(file)` |
| `json.loads(text)` | a **string** (the "s" = string) | `data = json.loads('{"name": "Rahim"}')` |

```python
import json
text = '{"name": "Rahim", "age": 24, "active": true, "manager": null}'
d = json.loads(text)
print(d, type(d))
# {'name': 'Rahim', 'age': 24, 'active': True, 'manager': None} <class 'dict'>
```

See how `true` became `True` and `null` became `None`.

### 6.3 Step 1 — check the types

Output of the type-checking block:
```
<class 'dict'>      ← data: the whole file
<class 'str'>       ← data["course"]
<class 'int'>       ← data["batch"]
<class 'list'>      ← data["students"]
<class 'dict'>      ← one student
<class 'str'>       ← name
<class 'int'>       ← age
<class 'float'>     ← cgpa
<class 'list'>      ← skills
```

When you receive unfamiliar data, **check types first**. Then you know whether to use `["key"]`, `[index]` or a loop.

### 6.4 Step 2 — reach inside nested data

Remember the Class 02 rule: **dict → use the key, list → use the index.** Chain them to go deeper:

```python
print(data["course"])                           # DevOps & Cloud Engineering
print(data["students"][0]["name"])              # Rahim
print(data["students"][0]["skills"])            # ['Linux', 'Python', 'Docker']
print(data["students"][0]["address"]["city"])   # Dhaka
```

Read `data["students"][0]["address"]["city"]` left to right:

```
data                → the whole dict
["students"]        → the list of 10 students
[0]                 → the first student (a dict)
["address"]         → that student's address (a dict)
["city"]            → "Dhaka"
```

**Real-world parallel:** a census database. Find the person named Nur, open their address, read the home district. Same chain.

### 6.5 Step 3 — loop through every student

```python
print(len(data["students"]))      # added in class: how many students?

for student in data["students"]:
    print(student["name"], student["age"])
```

Output:
```
10
Rahim 24
Karim 27
Ayesha 22
Hasan 26
Nusrat 23
Sakib 25
Mim 21
Tanvir 28
Farzana 24
Arif 29
```

`student` is just a **loop variable**. Each time round the loop it holds the next student dict: first index 0, then 1, then 2… (a student asked about this in class; see [Section 15](#15-questions-asked-in-class)).

### 6.6 Step 4 — a loop inside a loop

Each student has a `skills` **list**, so we loop over it too:

```python
for student in data["students"]:
    print(f"{student['name']} knows: ")

    for skill in student["skills"]:
        print(f"- {skill}")
```

Output (first three students):
```
Rahim knows: 
- Linux
- Python
- Docker
Karim knows: 
- Python
- AWS
- Terraform
Ayesha knows: 
- Python
- Git
- Linux
```

Note the quotes: the f-string uses double quotes outside, so the keys inside use **single** quotes: `f"{student['name']}"`. Using the same quote type inside and outside is an error in older Pythons.

### 6.7 Step 5 — filter: who has CGPA above 3.7?

**HR scenario from class:** HR receives 5,000 CVs, converts them to JSON, and wants to call everyone with CGPA above 3.7 for interview. (Or, less happily, keep only employees above 80% performance.)

```python
for student in data["students"]:
    if student["cgpa"] > 3.7:
        print(student["name"])
```

Output:
```
Karim
Ayesha
Nusrat
Mim
Farzana
```

`>` means "strictly greater than". Use `>=` if 3.7 itself should qualify.

### 6.8 Step 6 — search by skill (user input) and handle case

**Bank scenario from class:** you give the bank your account number, and it checks whether an account exists. Here the user types a skill and we list the students who have it.

**The edge case:** Python is **case-sensitive** (Class 01). If the user types `lInUx` and the data says `Linux`, a plain `==` says "not equal". Fix: **lowercase both sides** before comparing.

```python
skill_to_find = input("Enter a skill: ").lower()   # user's text → lowercase
...
if skill_to_find == skill.lower():                 # data's text → lowercase
```

An **edge case** is an unusual input that breaks naive code. Handling edge cases is a big part of real programming.

> **Bug alert: the "No students" message repeats.** The `else` sits **inside the inner loop**, so it runs for **every skill that doesn't match**, even when the search succeeds. Real output for `Python`:
>
> ```
> Enter a skill: Python
> No students at bongoDev has the target skill
> Rahim
> No students at bongoDev has the target skill
> Karim
> No students at bongoDev has the target skill
> No students at bongoDev has the target skill
> Ayesha
> ...
> ```
>
> Searching `Python` finds the 6 right students but prints "No students…" **24 times** along the way. Searching `lInUx` prints the 4 correct names and the message **26 times**.
>
> **Fixed version:** collect matches first, decide afterwards.
>
> ```python
> import json
>
> with open("students.json", "r") as file:
>     data = json.load(file)
>
> skill_to_find = input("Enter a skill: ").strip().lower()
> found = []
>
> for student in data["students"]:
>     for skill in student["skills"]:
>         if skill.lower() == skill_to_find:
>             found.append(student["name"])
>             break          # this student matched; stop checking their other skills
>
> if found:
>     print(f"{len(found)} student(s) know {skill_to_find}:")
>     for name in found:
>         print(f"- {name}")
> else:
>     print("No students at bongoDev has the target skill")
> ```
>
> ```
> Enter a skill:   lInUx 
> 4 student(s) know linux:
> - Rahim
> - Ayesha
> - Sakib
> - Tanvir
>
> Enter a skill: ML
> No students at bongoDev has the target skill
> ```
>
> `.strip()` also removes accidental spaces around the input, another edge case.

---

## 7. Writing JSON: Adding a New Student

So far we only **read**. Now five more students arrive. How do we **write** them into the same file?

### 7.1 The full file: `11.JSON-Data/inser-data-in-json.py`

```python
import json


# 1. Create the Student class
class Student:
    def __init__(self, name, age, cgpa, skills):
        self.name = name
        self.age = age
        self.cgpa = cgpa
        self.skills = skills

    def display(self):
        print(f"Name: {self.name}")
        print(f"Age: {self.age}")
        print(f"CGPA: {self.cgpa}")
        print(f"Skills: {', '.join(self.skills)}")
        print("-" * 30)


# 2. Read existing student data from JSON file
with open("students.json", "r") as file:
    data = json.load(file)


# 3. Create a new student as a dictionary
new_student = {
    "id": 111,
    "name": "Roman",
    "age": 23,
    "cgpa": 3.9,
    "skills": ["Python", "Docker"],
    "address": {
        "city": "Dhaka",
        "country": "Bangladesh"
    }
}


# 4. Add the new student to the students list
data["students"].append(new_student)


# 5. Save the updated data back to the JSON file
with open("students.json", "w") as file:
    json.dump(data, file, indent=4)


# 6. Create an empty list for Student objects
students = []


# 7. Convert each dictionary into a Student object
for item in data["students"]:
    student = Student(
        item["name"],
        item["age"],
        item["cgpa"],
        item["skills"]
    )

    students.append(student)


# 8. Display all Student objects
for student in students:
    student.display()
```

### 7.2 What happens, step by step

```
students.json ──(open "r", json.load)──► data in MEMORY (10 students)
                                              │
                                    append(new_student)
                                              ▼
                                   data in MEMORY (11 students)
                                              │
students.json ◄──(open "w", json.dump)────────┘   file now has 11 students
```

1. **Read** the file into `data` (the file is closed after the `with` block).
2. Build `new_student`, a plain **dict** with the same shape as the others.
3. `data["students"].append(new_student)` adds it **to the end** of the list. (Class question: *start, middle or end?* **End.** `append` always adds to the end.)
4. ⚠️ At this point the change exists **only in memory**. The file on disk still has 10 students.
5. **Open the file again in `"w"` mode** and `json.dump` the whole updated `data` into it. Now the file has 11 students.
6. Steps 6–8 turn each dict into a `Student` **object** (Class 02 OOP) and call `display()` on it. This shows the bridge between **raw data (dict)** and **objects (class instances)**, the same bridge the Flask app uses.

Last lines of the output:
```
------------------------------
Name: Roman
Age: 23
CGPA: 3.9
Skills: Python, Docker
------------------------------
```

`', '.join(self.skills)` glues the list into one string: `["Python", "Docker"]` → `"Python, Docker"`. `"-" * 30` repeats the dash 30 times.

### 7.3 `json.dump` vs `json.dumps`

| Function | Writes to | Returns |
|---|---|---|
| `json.dump(data, file, indent=4)` | an open **file** | nothing |
| `json.dumps(data, indent=4)` | — | a **string** of JSON |

`indent=4` makes the output human-readable, with nested items indented 4 spaces. Without it, everything goes on one long line.

**Memory trick:** the **s** stands for **s**tring. `load`/`dump` work with files, `loads`/`dumps` work with strings.

### 7.4 When does the file close? (class question)

A student asked: *"You said the file closes immediately after reading. How does writing work then?"*

There are **two separate `with` blocks**:
1. The first opens the file for reading, loads the data **into memory**, and closes the file when the block ends.
2. The program changes the data **in memory**.
3. The second `with` opens the file again for writing, dumps the data, and closes it.

A file stays open **only while its `with` block is running.**

### 7.5 Run it twice and Roman appears twice

In class the script ran twice and Roman was added **twice** (12 students instead of 11). The instructor deleted the extra entry by hand. Verified:

```
after run 1: 11 students
after run 2: 12 students  ['Arif', 'Roman', 'Roman']
```

The script never checks whether the student already exists. A **safe version** with a duplicate check:

```python
import json

with open("students.json", "r") as file:
    data = json.load(file)

new_student = {"id": 111, "name": "Roman", "age": 23, "cgpa": 3.9,
               "skills": ["Python", "Docker"],
               "address": {"city": "Dhaka", "country": "Bangladesh"}}

existing_ids = [student["id"] for student in data["students"]]

if new_student["id"] in existing_ids:
    print(f"Student {new_student['id']} already exists. Nothing to add.")
else:
    data["students"].append(new_student)
    with open("students.json", "w") as file:
        json.dump(data, file, indent=4)
    print(f"Added {new_student['name']}. Total students: {len(data['students'])}")
```

```
Added Roman. Total students: 11          ← first run
Student 111 already exists. Nothing to add.   ← second run
```

This idea has a name in DevOps: **idempotency**. Running something twice gives the same result as running it once. Ansible, Terraform and Kubernetes are all built around it.

---

## 8. APIs, REST and CRUD

> "Is the real world this neat? Will someone hand us a tidy JSON file?" No. In the real world, data usually comes through **APIs**.

### 8.1 What is an API?

**API = Application Programming Interface.** It's the **messenger between two pieces of software**: it carries a request from one side and brings the answer back.

**Analogies from class:**
- **Phone call:** you call a friend. Your call goes to a tower (GP, Airtel), the tower finds your friend's number, and connects you. You never talk to the tower yourself; it sits in the middle.
- **Mediator:** when two countries negotiate through a third country, the third country carries messages both ways.

In a web application:

```
 Browser (Frontend)  ──request──►  Backend (Flask / Python)  ──query──►  Database (SQLite)
                     ◄─response──                             ◄─rows───
                         (JSON / HTML)
```

The **frontend** is what you see in the browser. The **backend** is the code on the server. The API is how the frontend talks to the backend (and how one service talks to another).

### 8.2 CRUD and the HTTP methods

Almost everything an app does with data is one of four operations, called **CRUD**:

| CRUD | Meaning | HTTP method | Everyday example |
|---|---|---|---|
| **C**reate | Add new data | `POST` | Sign up; add a student; submit a login form |
| **R**ead | Get data | `GET` | Open a page; list all students |
| **U**pdate | Change existing data | `PUT` (whole thing) / `PATCH` (part of it) | Edit a student's age |
| **D**elete | Remove data | `DELETE` | Remove a student |

A **REST API** is an API that uses these HTTP methods on URLs (`/api/students`, `/api/students/1`) to perform CRUD, usually exchanging JSON.

### 8.3 HTTP status codes you'll see today

| Code | Meaning | When our app returns it |
|---|---|---|
| `200 OK` | Success | Reading students |
| `201 Created` | Something new was created | Creating a student via the API |
| `302 Found` | Redirect: "go to this other page" | After saving a form, back to the home page |
| `400 Bad Request` | Your request is missing or invalid | API POST without required fields |
| `404 Not Found` | That thing doesn't exist | Student id 99; a misspelled API path |
| `405 Method Not Allowed` | Right URL, wrong method | Sending `PUT` to a route that only accepts `GET`/`POST` |
| `500 Internal Server Error` | The server's code crashed | Typing `abc` as age in the form |

### 8.4 API URLs in the real world

A common pattern:

```
https://bongodev.com/api/students        ← all students
https://bongodev.com/api/students/1      ← one student, id 1
```

The `/api/` part isn't required, but it makes it obvious that the URL returns data, not a web page.

**Security point from class:** real companies **don't expose APIs like this publicly**. If `/api/users` were open to anyone, a competitor could download every user's name and phone number. Real APIs require authentication and restrict access.

### 8.5 A free practice API: JSONPlaceholder

**https://jsonplaceholder.typicode.com** is a free fake API for practice (bookmark it). It offers `/posts`, `/comments`, `/users` and more. Each post has `userId`, `id`, `title` and `body`.

> **Precision note:** the class said JSONPlaceholder "only allows GET". It actually **accepts** POST, PUT and DELETE and returns a realistic answer, but **nothing is saved**. Verified: a POST returns `201` with `"id": 101`, yet `GET /posts/101` returns `404` and the list still has 100 posts. It's perfect for practice because you can't break anything.

---

## 9. Calling a Real API with `requests`

### 9.1 Installing `requests`

`requests` is **not** in the standard library. It's a **third-party package** from PyPI, so install it first:

```bash
pip install requests
```

Without it you get `ModuleNotFoundError: No module named 'requests'`. (Install it inside a virtual environment; see [Section 12](#12-virtual-environments-and-running-the-app).)

`requests` has a function for each HTTP method: `requests.get()`, `requests.post()`, `requests.put()`, `requests.delete()`.

### 9.2 The full file: `11.JSON-Data/json-api.py`

```python
import requests
import json
import pdb

# 1. The URL that provides the fake post data
url = "https://jsonplaceholder.typicode.com/posts"

try:
    # 2. Fetch the data
    # pdb.set_trace()
    response = requests.get(url)
    response.raise_for_status() # Check for errors

    # 3. Parse the data
    # This specific URL returns a LIST of posts
    posts = response.json()

    print(f"Successfully fetched {len(posts)} posts.\n")

    # 4. Professional DevOps Logic: Only print the first 5 titles
    # We use a slice [:5] so we don't flood the terminal
    print("--- Latest 5 Post Titles ---")
    for post in posts[:5]:
        print(f"ID {post['id']}: {post['title']}")

except Exception as e:
    print(f"Failed to fetch posts: {e}")
```

```bash
python3 json-api.py
```

Output:
```
Successfully fetched 100 posts.

--- Latest 5 Post Titles ---
ID 1: sunt aut facere repellat provident occaecati excepturi optio reprehenderit
ID 2: qui est esse
ID 3: ea molestias quasi exercitationem repellat qui ipsa sit aut
ID 4: eum et est occaecati
ID 5: nesciunt quas odio
```

(The titles are placeholder Latin, so don't try to read them.)

### 9.3 Line by line

| Line | What it does |
|---|---|
| `requests.get(url)` | Send a **GET** request; the server's answer is stored in `response` |
| `response.raise_for_status()` | If the status code is an error (4xx/5xx), **raise an exception** instead of continuing silently |
| `response.json()` | Convert the JSON text of the response into Python objects (here a **list** of dicts) |
| `posts[:5]` | **Slice**: take items from index 0 up to (not including) 5 |
| `try:` | "Try this code…" |
| `except Exception as e:` | "…and if **anything** goes wrong, run this instead." `e` holds the error message |

**Why `try / except`?** You can never be sure a URL will work. It might be renamed tomorrow, the network might be down, or you might mistype it. Without `try`, the program crashes with a long traceback. With it, you print a clear message and decide what to do.

`import json` and `import pdb` aren't actually used here (`response.json()` is built into `requests`, and the `pdb` line is commented out). They're left over from the lesson.

> **Precision note: "Latest 5".** `posts[:5]` takes the **first** five (IDs 1–5), not the latest. For the last five, use `posts[-5:]`.

### 9.4 List slicing, a quick reference

| Slice | Result | Type |
|---|---|---|
| `posts[0]` | first item | a **dict** |
| `posts[-1]` | last item | a **dict** |
| `posts[:5]` | first 5 items | a **list** |
| `posts[-5:]` | last 5 items | a **list** |
| `posts[1:3]` | items at index 1 and 2 | a **list** |

### 9.5 Two errors from class, explained

**Error 1: `posts[-1]` in the loop.** The instructor changed `posts[:5]` to `posts[-1]` to get "the last one" and got:

```
Failed to fetch posts: string indices must be integers, not 'str'
```

**Why:** `posts[-1]` is **one dict**, not a list. Looping over a dict gives its **keys** (`"userId"`, `"id"`, `"title"`, `"body"`), which are strings. So `post` became the string `"userId"`, and `"userId"['id']` is nonsense. **Fix:** slice instead, `posts[-1:]` (a list with one item), or skip the loop:

```python
last = posts[-1]
print(f"ID {last['id']}: {last['title']}")
```

Then `posts[1:3]` worked and printed IDs 2 and 3.

**Error 2: a misspelled URL.** To show `try / except`, the URL was changed (`typicode` → `typecode`):

```
Failed to fetch posts: HTTPSConnectionPool(host='jsonplaceholder.typecode.com', port=443):
Max retries exceeded with url: /posts (Caused by NameResolutionError(...))
```

A wrong **path** on a correct host gives a different error, caught by `raise_for_status()`:

```
Failed to fetch posts: 404 Client Error: Not Found for url: https://jsonplaceholder.typicode.com/postz
```

In both cases the program didn't crash; `except` caught it and printed a clear message.

---

## 10. Web Development Basics

### 10.1 Website vs web app vs mobile app vs desktop app

| Type | What it is | Example |
|---|---|---|
| **Website** (static) | Pages you only *look at*; no login, no interaction | The sweet shop next door's info page |
| **Web app** | Runs in the browser; you log in and interact | BongoDev, Facebook, our Student Management System |
| **Mobile app** | Installed on a phone | WhatsApp on Android |
| **Desktop app** | Installed on a computer | Microsoft Teams desktop |

Many products (Teams, WhatsApp) come in all three forms. Today we build a **web app**.

### 10.2 Client, server, request, response

```
 Client (browser)                         Server (computer running the app)
 ───────────────                          ─────────────────────────────────
 You type a URL, press Enter ──REQUEST──►  The app runs Python code,
                                           asks the database,
 The page appears          ◄──RESPONSE──  sends back HTML or JSON
```

- **Client:** the side that *asks*, usually a browser (the frontend).
- **Server:** the side that *answers* (the backend).
- **Request:** "please give me / save / change / delete this."
- **Response:** the answer (a page, JSON data, or an error code).

### 10.3 Web servers

A **web server** is a program that receives requests from clients and returns responses.

| Web server | Typical use |
|---|---|
| **Flask development server** | Built into Flask; what we use today. **Development only.** |
| **Gunicorn** | Production server for Flask and Django (WSGI apps) |
| **Uvicorn** | Production server for FastAPI and other async (ASGI) apps |
| **Nginx** | Popular front-door server: serves static files and forwards traffic to the app |
| **Apache** | Older, still widely used general web server |

A typical production setup for a Python app:

```
Internet ──► Nginx (port 80/443) ──► Gunicorn / Uvicorn ──► your Flask / FastAPI code
```

**Class Q&A:** *Can Python apps run directly on Nginx?* Not directly. We run Flask with Gunicorn (or FastAPI with Uvicorn), and Nginx sits in front as a **reverse proxy**. JavaScript and PHP apps are also commonly served through Nginx.

When you start the app, Flask itself prints this warning (verified):

```
WARNING: This is a development server. Do not use it in a production deployment.
Use a production WSGI server instead.
```

### 10.4 Python web frameworks

| Framework | Character | Best for |
|---|---|---|
| **Flask** | Lightweight "micro-framework"; you add only what you need | Small-to-medium apps, quick tools, microservices |
| **Django** | "Batteries included": admin panel, ORM, auth, templates | Large full-stack applications |
| **FastAPI** | Modern, fast, built for APIs | Backends and REST APIs |

**Analogy from class:** Django is a **cannon**, Flask is a **mosquito coil**. To get rid of mosquitoes you don't fire a cannon. Light a coil. Flask being lightweight doesn't mean it's a toy, though: many enterprises use it for focused services.

### 10.5 What the browser understands

A browser only understands **HTML** (structure), **CSS** (style) and **JavaScript** (behaviour). The class showed this with a tiny `index.html` (an `<h1>` and a `<p>`) dragged into the browser. It displays, but it's just a **web page**, not an app.

> **Precision note:** the class said Python code "is converted to JavaScript by the browser". That's not how it works. **Python never runs in the browser.** Flask runs Python **on the server**, uses it to build **HTML**, and sends that HTML to the browser. The browser only ever receives HTML, CSS, JavaScript or JSON.

### 10.6 SQLite in one paragraph

**SQLite** is a complete **relational (SQL) database** stored in **a single file** (here `students.db`). There's no separate database server to install, because Python's `sqlite3` module talks to it directly. Data lives in **tables** of **rows** and **columns**, like a spreadsheet.

> **Precision note:** in class SQLite was called a "sequential database". The correct term is **relational database** (tables with rows and columns, queried with SQL). The spreadsheet comparison from class is the right mental picture.

---

## 11. The Flask Student Management App: Full Code Walkthrough

### 11.1 What the app does

- Add a student from a browser form
- See all students in a table
- Edit or delete any student
- Store everything permanently in SQLite (`students.db`)
- Return all students, or one student, as **JSON** through a REST API
- Create a student through the JSON API

Each student has: **ID, Name, Age, Email, CGPA, Course, Skills.**

### 11.2 Project structure and architecture

```text
13.flast-student-management-app/
├── app.py               ← controller: all routes (web pages + API). ENTRY POINT.
├── database.py          ← opens SQLite, creates the table
├── requirements.txt     ← Flask>=3.0,<4.0
├── README.md            ← setup and run instructions
├── students.db          ← created automatically on first run
├── models/
│   ├── __init__.py      ← empty; makes "models" an importable package
│   └── student.py       ← Student (data) + StudentRepository (database operations)
├── templates/           ← HTML pages with Jinja2 placeholders
│   ├── index.html
│   └── edit.html
└── static/
    └── style.css        ← CSS served as-is
```

```
Browser / Frontend
        │   HTML form or JSON request
        ▼
Flask Route (app.py)
        │   builds a Student object
        ▼
Student Class / Object (models/student.py)
        │   handed to the repository
        ▼
StudentRepository (models/student.py)
        │   runs SQL
        ▼
SQLite Database (students.db)
```

**Separation of concerns** in practice: routes don't write SQL, the repository doesn't know about HTML, and the `Student` class only describes a student.

### 11.3 `requirements.txt` (full file)

```text
Flask>=3.0,<4.0
```

This lists the packages the project needs. `>=3.0,<4.0` means "any Flask 3.x version": at least 3.0, below 4.0. Install everything listed with one command: `pip install -r requirements.txt`.

### 11.4 `database.py` (full file)

```python
import sqlite3

DATABASE_NAME = "students.db"

# Think of it as: open database → prepare SQL → run SQL → save → close.

def get_connection():
    """
    Open a connection to SQLite.

    sqlite3 is included with Python, so no separate database
    server such as MySQL or PostgreSQL is required.
    """
    connection = sqlite3.connect(DATABASE_NAME)
    connection.row_factory = sqlite3.Row
    return connection


def init_db():
    """Create the students table if it does not already exist."""
    connection = get_connection()
    
    connection.execute(
        """
        CREATE TABLE IF NOT EXISTS students (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL,
            age INTEGER NOT NULL,
            email TEXT NOT NULL,
            cgpa REAL NOT NULL,
            course TEXT NOT NULL,
            skills TEXT NOT NULL
        )
        """
    )

    connection.commit()
    connection.close()
```

| Piece | Meaning |
|---|---|
| `DATABASE_NAME` in CAPITALS | A **constant**: a value not meant to change (naming convention) |
| `sqlite3.connect("students.db")` | Open the database file; **creates it if it doesn't exist** |
| `row_factory = sqlite3.Row` | Lets you read a row by column name: `row["name"]` instead of `row[1]` |
| `CREATE TABLE IF NOT EXISTS` | Create the table only the first time; safe to run on every start |
| `id INTEGER PRIMARY KEY AUTOINCREMENT` | A unique id that the database fills in automatically: 1, 2, 3… |
| `NOT NULL` | This column can't be empty |
| `connection.commit()` | **Save** the changes, like `git commit` saves a version |
| `connection.close()` | Release the connection |

**Python ↔ SQLite types:**

| Python | SQLite |
|---|---|
| `str` | `TEXT` |
| `int` | `INTEGER` |
| `float` | `REAL` |

### 11.5 `models/__init__.py` (full file)

```python
```

It's **empty on purpose**. Its presence tells Python "the `models` folder is a **package**", so `from models.student import Student` works. A package is a folder of modules.

### 11.6 `models/student.py` (full file)

```python
from database import get_connection


class Student:
    """
    Student is the model class.

    Every Student(...) we create is an OBJECT made from this CLASS.
    """

    def __init__(
        self,
        name,
        age,
        email,
        cgpa,
        course,
        skills,
        student_id=None,
    ):
        self.student_id = student_id
        self.name = name
        self.age = age
        self.email = email
        self.cgpa = cgpa
        self.course = course
        self.skills = skills

    def to_dict(self):
        """Convert the Student object into a Python dictionary."""
        return {
            "id": self.student_id,
            "name": self.name,
            "age": self.age,
            "email": self.email,
            "cgpa": self.cgpa,
            "course": self.course,
            "skills": self.skills,
        }


class StudentRepository:
    """
    This class contains database operations.

    Keeping database code here helps students see separation of concerns:
    Flask routes -> Student objects -> Repository -> SQLite database
    """

    # Use @staticmethod when a function logically belongs inside a class, but it does not need to access any object-specific (self) data.
    @staticmethod
    def create(student):
        connection = get_connection()

        cursor = connection.execute(
            """
            INSERT INTO students (name, age, email, cgpa, course, skills)
            VALUES (?, ?, ?, ?, ?, ?)
            """,
            (
                student.name,
                student.age,
                student.email,
                student.cgpa,
                student.course,
                student.skills,
            ),
        )

        connection.commit()
        student_id = cursor.lastrowid
        connection.close()

        return student_id

    @staticmethod
    def get_all():
        connection = get_connection()

        rows = connection.execute(
            "SELECT * FROM students ORDER BY id DESC"
        ).fetchall()

        connection.close()

        students = []

        for row in rows:
            student = Student(
                student_id=row["id"],
                name=row["name"],
                age=row["age"],
                email=row["email"],
                cgpa=row["cgpa"],
                course=row["course"],
                skills=row["skills"],
            )

            students.append(student)

        return students

    @staticmethod
    def get_by_id(student_id):
        connection = get_connection()

        row = connection.execute(
            "SELECT * FROM students WHERE id = ?",
            (student_id,),
        ).fetchone()

        connection.close()

        if row is None:
            return None

        return Student(
            student_id=row["id"],
            name=row["name"],
            age=row["age"],
            email=row["email"],
            cgpa=row["cgpa"],
            course=row["course"],
            skills=row["skills"],
        )

    @staticmethod
    def update(student):
        connection = get_connection()

        connection.execute(
            """
            UPDATE students
            SET name = ?, age = ?, email = ?, cgpa = ?, course = ?, skills = ?
            WHERE id = ?
            """,
            (
                student.name,
                student.age,
                student.email,
                student.cgpa,
                student.course,
                student.skills,
                student.student_id,
            ),
        )

        connection.commit()
        connection.close()

    @staticmethod
    def delete(student_id):
        connection = get_connection()

        connection.execute(
            "DELETE FROM students WHERE id = ?",
            (student_id,),
        )

        connection.commit()
        connection.close()
```

**The `Student` class** (the **model**). This is the Class 02 constructor pattern again:
- `__init__` stores each value on `self`.
- `student_id=None` is a **default parameter**. A new student has no id yet, because the database assigns it.
- `to_dict()` turns the object into a dictionary, which Flask can turn into JSON.

**The `StudentRepository` class** holds all the SQL, one method per CRUD operation:

| Method | CRUD | SQL used | Returns |
|---|---|---|---|
| `create(student)` | Create | `INSERT INTO ... VALUES (?, ...)` | the new id (`cursor.lastrowid`) |
| `get_all()` | Read | `SELECT * ... ORDER BY id DESC` | list of `Student` objects (newest first) |
| `get_by_id(id)` | Read | `SELECT * ... WHERE id = ?` | one `Student`, or `None` |
| `update(student)` | Update | `UPDATE ... SET ... WHERE id = ?` | nothing |
| `delete(id)` | Delete | `DELETE ... WHERE id = ?` | nothing |

Database rows come back and are turned into `Student` objects. This is the same dict → object bridge you saw in Section 7.

**What is `@staticmethod`?** The `@` line is a **decorator**: it modifies the function below it. A static method **belongs to the class but doesn't receive `self`**, because it needs no data from any particular object. So you call it on the class itself: `StudentRepository.get_all()`, with no `StudentRepository()` object needed.

> **Precision note:** in class, static methods were described as "protected". They aren't protected or private in any way. "Static" only means **no `self` (and no `cls`) is passed in**.

**Why the `?` marks?** They're **placeholders**. The values are passed separately in a tuple, and SQLite inserts them **safely**. Never build SQL with f-strings like `f"... WHERE id = {student_id}"`. That allows **SQL injection**, where a malicious input can rewrite your query (for example, delete your table). Placeholders prevent it.

`(student_id,)` has a trailing comma because a **one-item tuple** needs it. `(student_id)` without the comma is just a number in brackets.

### 11.7 `app.py` (full file)

```python
from flask import Flask, jsonify, redirect, render_template, request, url_for

from database import init_db
from models.student import Student, StudentRepository

app = Flask(__name__)

# Create the database/table automatically when the app starts.
init_db()


@app.route("/")
def index():
    """Show the frontend form and all saved students."""
    students = StudentRepository.get_all()
    return render_template("index.html", students=students)


@app.route("/students", methods=["POST"])
def create_student():
    """Create a Student object from frontend form data and save it."""
    student = Student(
        name=request.form["name"],
        age=int(request.form["age"]),
        email=request.form["email"],
        cgpa=float(request.form["cgpa"]),
        course=request.form["course"],
        skills=request.form["skills"],
    )

    StudentRepository.create(student)
    return redirect(url_for("index"))


@app.route("/students/<int:student_id>/edit", methods=["GET"])
def edit_student_page(student_id):
    """Show the edit form for one student."""
    student = StudentRepository.get_by_id(student_id)

    if student is None:
        return "Student not found", 404

    return render_template("edit.html", student=student)


@app.route("/students/<int:student_id>/edit", methods=["POST"])
def update_student(student_id):
    """Update an existing student."""
    student = Student(
        student_id=student_id,
        name=request.form["name"],
        age=int(request.form["age"]),
        email=request.form["email"],
        cgpa=float(request.form["cgpa"]),
        course=request.form["course"],
        skills=request.form["skills"],
    )

    StudentRepository.update(student)
    return redirect(url_for("index"))


@app.route("/students/<int:student_id>/delete", methods=["POST"])
def delete_student(student_id):
    """Delete a student."""
    StudentRepository.delete(student_id)
    return redirect(url_for("index"))


# -------------------------
# REST API endpoints
# -------------------------

@app.route("/api/students", methods=["GET"])
def api_get_students():
    students = StudentRepository.get_all()
    return jsonify([student.to_dict() for student in students])


@app.route("/api/students/<int:student_id>", methods=["GET"])
def api_get_student(student_id):
    student = StudentRepository.get_by_id(student_id)

    if student is None:
        return jsonify({"error": "Student not found"}), 404

    return jsonify(student.to_dict())


@app.route("/api/students", methods=["POST"])
def api_create_student():
    data = request.get_json()

    required_fields = ["name", "age", "email", "cgpa", "course", "skills"]

    if not data or not all(field in data for field in required_fields):
        return jsonify({"error": "Missing required student information"}), 400

    student = Student(
        name=data["name"],
        age=int(data["age"]),
        email=data["email"],
        cgpa=float(data["cgpa"]),
        course=data["course"],
        skills=data["skills"],
    )

    student_id = StudentRepository.create(student)
    student.student_id = student_id

    return jsonify(student.to_dict()), 201


if __name__ == "__main__":
    app.run(debug=True)
```

**Key ideas, top to bottom:**

| Code | Meaning |
|---|---|
| `from flask import Flask, jsonify, ...` | Named import (Section 3.5) from the Flask package |
| `from models.student import ...` | Import from `models/student.py`: package `models`, module `student` |
| `app = Flask(__name__)` | Create the app. `__name__` (a "dunder", meaning double underscore, variable) tells Flask where the app lives, so it can find `templates/` and `static/` |
| `init_db()` | Create the table on start-up if needed |
| `@app.route("/")` | A **decorator** that connects a **URL path** (route) to the function below it |
| `methods=["POST"]` | Which HTTP methods this route accepts (default: `GET` only) |
| `<int:student_id>` | A **URL variable**: `/students/5/edit` calls the function with `student_id=5` (as an `int`) |
| `request.form["name"]` | A value from a submitted HTML form, **always a string**, hence `int(...)` and `float(...)` (Class 01 type casting) |
| `request.get_json()` | The JSON body of an API request, as a dict |
| `render_template("index.html", students=students)` | Fill an HTML template with data and return it |
| `redirect(url_for("index"))` | Send the browser to the home page (`302`) |
| `jsonify(...)` | Turn a dict/list into a JSON response |
| `return ..., 404` | Return a response **with** a specific status code |
| `if __name__ == "__main__": app.run(debug=True)` | **Entry point**: start the server only when this file is run directly |

**What is a route?** A **route** is a path on your site. `bongodev.com/` is the home page; `/success` or `/contact` is another route, another page. Each `@app.route` handles one of them.

**Every route in the app:**

| Method | Route | Function | What it does | Response |
|---|---|---|---|---|
| `GET` | `/` | `index` | Home page: form + table of students | HTML |
| `POST` | `/students` | `create_student` | Save a new student from the form | `302` → `/` |
| `GET` | `/students/<id>/edit` | `edit_student_page` | Show the edit form | HTML or `404` |
| `POST` | `/students/<id>/edit` | `update_student` | Save the edited student | `302` → `/` |
| `POST` | `/students/<id>/delete` | `delete_student` | Delete the student | `302` → `/` |
| `GET` | `/api/students` | `api_get_students` | All students as JSON | `200` |
| `GET` | `/api/students/<id>` | `api_get_student` | One student as JSON | `200` or `404` |
| `POST` | `/api/students` | `api_create_student` | Create a student from JSON | `201` or `400` |

**`if __name__ == "__main__":`** Every module has a built-in variable `__name__`. When you **run** a file directly (`python3 app.py`), its `__name__` is `"__main__"`. When the file is **imported** by another file, `__name__` is the module's name instead. So this line means "start the server only if this file was run directly, not if it was imported." It's the **entry point** of the program.

> **`debug=True`** gives auto-reload when you save a file and a detailed error page in the browser. Great for learning, **dangerous in production**: the debug page can let visitors run code on your server. Always turn it off and use Gunicorn in production.

### 11.8 Templates and Jinja2

Flask fills HTML files using **Jinja2**, a template language. Think of it as **f-strings for HTML**:

| Jinja2 syntax | Meaning | Python equivalent |
|---|---|---|
| `{{ student.name }}` | Insert a value | `f"{student.name}"` |
| `{% for student in students %} ... {% endfor %}` | Loop | `for student in students:` |
| `{% if students %} ... {% else %} ... {% endif %}` | Condition | `if students: ... else: ...` |
| `{{ url_for('static', filename='style.css') }}` | Build a URL to a static file | — |

`templates/` holds HTML with Jinja2 placeholders. `static/` holds files sent **as they are** (CSS, images, JavaScript).

<details>
<summary><b>Show <code>templates/index.html</code> (full file)</b></summary>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Student Management App</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>

<div class="container">
    <header>
        <h1>Student Management System</h1>
        <p>Flask + Class & Object + SQLite + REST API</p>
    </header>

    <section class="card">
        <h2>Add Student</h2>

        <form action="/students" method="POST" class="student-form">

            <label>Name</label>
            <input
                type="text"
                name="name"
                placeholder="e.g. Rahim Ahmed"
                required
            >

            <label>Age</label>
            <input
                type="number"
                name="age"
                min="1"
                placeholder="e.g. 24"
                required
            >

            <label>Email</label>
            <input
                type="email"
                name="email"
                placeholder="rahim@example.com"
                required
            >

            <label>CGPA</label>
            <input
                type="number"
                name="cgpa"
                step="0.01"
                min="0"
                max="4"
                placeholder="e.g. 3.75"
                required
            >

            <label>Course</label>
            <input
                type="text"
                name="course"
                placeholder="e.g. DevOps & Cloud Engineering"
                required
            >

            <label>Skills</label>
            <input
                type="text"
                name="skills"
                placeholder="Python, Linux, Docker"
                required
            >

            <button type="submit">Save Student</button>

        </form>
    </section>

    <section class="card">
        <div class="section-heading">
            <h2>Students</h2>
            <a href="/api/students" target="_blank">View JSON API</a>
        </div>

        {% if students %}

        <div class="table-wrapper">
            <table>
                <thead>
                    <tr>
                        <th>ID</th>
                        <th>Name</th>
                        <th>Age</th>
                        <th>Email</th>
                        <th>CGPA</th>
                        <th>Course</th>
                        <th>Skills</th>
                        <th>Actions</th>
                    </tr>
                </thead>

                <tbody>
                    {% for student in students %}
                    <tr>
                        <td>{{ student.student_id }}</td>
                        <td>{{ student.name }}</td>
                        <td>{{ student.age }}</td>
                        <td>{{ student.email }}</td>
                        <td>{{ student.cgpa }}</td>
                        <td>{{ student.course }}</td>
                        <td>{{ student.skills }}</td>
                        <td class="actions">
                            <a
                                class="edit"
                                href="/students/{{ student.student_id }}/edit"
                            >
                                Edit
                            </a>

                            <form
                                action="/students/{{ student.student_id }}/delete"
                                method="POST"
                                class="inline-form"
                            >
                                <button
                                    type="submit"
                                    class="delete"
                                    onclick="return confirm('Delete this student?')"
                                >
                                    Delete
                                </button>
                            </form>
                        </td>
                    </tr>
                    {% endfor %}
                </tbody>
            </table>
        </div>

        {% else %}

        <p>No students have been added yet.</p>

        {% endif %}
    </section>

    <section class="card learning">
        <h2>What Students Learn</h2>

        <div class="flow">
            HTML Form
            <span>→</span>
            Flask Route
            <span>→</span>
            Student Object
            <span>→</span>
            SQLite
            <span>→</span>
            HTML / JSON Response
        </div>
    </section>

</div>

</body>
</html>
```

</details>

**How the form connects to Python:** `<form action="/students" method="POST">` sends the form to the `create_student` route. Each `<input name="age">` becomes `request.form["age"]` in Python. **The `name` attribute is the dictionary key.**

<details>
<summary><b>Show <code>templates/edit.html</code> (full file)</b></summary>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Edit Student</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>

<div class="container small-container">

    <section class="card">
        <h1>Edit Student</h1>

        <form
            action="/students/{{ student.student_id }}/edit"
            method="POST"
            class="student-form"
        >

            <label>Name</label>
            <input
                type="text"
                name="name"
                value="{{ student.name }}"
                required
            >

            <label>Age</label>
            <input
                type="number"
                name="age"
                value="{{ student.age }}"
                required
            >

            <label>Email</label>
            <input
                type="email"
                name="email"
                value="{{ student.email }}"
                required
            >

            <label>CGPA</label>
            <input
                type="number"
                step="0.01"
                min="0"
                max="4"
                name="cgpa"
                value="{{ student.cgpa }}"
                required
            >

            <label>Course</label>
            <input
                type="text"
                name="course"
                value="{{ student.course }}"
                required
            >

            <label>Skills</label>
            <input
                type="text"
                name="skills"
                value="{{ student.skills }}"
                required
            >

            <button type="submit">Update Student</button>
            <a href="/" class="secondary-button">Cancel</a>

        </form>
    </section>

</div>

</body>
</html>
```

</details>

`value="{{ student.age }}"` pre-fills each box with the current value, so you only change what's wrong.

<details>
<summary><b>Show <code>static/style.css</code> (full file)</b></summary>

```css
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    font-family: Arial, Helvetica, sans-serif;
    background: #f3f5f8;
    color: #1f2937;
}

.container {
    width: min(1200px, 94%);
    margin: 40px auto;
}

.small-container {
    width: min(700px, 94%);
}

header {
    margin-bottom: 24px;
}

header h1 {
    margin-bottom: 6px;
}

header p {
    margin-top: 0;
    color: #6b7280;
}

.card {
    background: white;
    padding: 24px;
    margin-bottom: 24px;
    border-radius: 12px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.06);
}

.student-form {
    display: grid;
    gap: 10px;
}

.student-form label {
    font-weight: 600;
    margin-top: 8px;
}

.student-form input {
    padding: 12px;
    border: 1px solid #d1d5db;
    border-radius: 7px;
    font-size: 15px;
}

button,
.secondary-button {
    margin-top: 14px;
    padding: 12px 16px;
    border: 0;
    border-radius: 7px;
    cursor: pointer;
    font-size: 15px;
}

.student-form button {
    background: #111827;
    color: white;
}

.secondary-button {
    text-decoration: none;
    text-align: center;
    background: #e5e7eb;
    color: #111827;
}

.section-heading {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.section-heading a {
    text-decoration: none;
}

.table-wrapper {
    overflow-x: auto;
}

table {
    width: 100%;
    border-collapse: collapse;
}

th,
td {
    padding: 12px;
    border-bottom: 1px solid #e5e7eb;
    text-align: left;
}

th {
    background: #f9fafb;
}

.actions {
    white-space: nowrap;
}

.edit {
    display: inline-block;
    padding: 7px 10px;
    text-decoration: none;
    background: #e5e7eb;
    color: #111827;
    border-radius: 5px;
}

.inline-form {
    display: inline;
}

.delete {
    margin: 0 0 0 5px;
    padding: 7px 10px;
    background: #111827;
    color: white;
}

.flow {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    align-items: center;
    font-weight: 600;
}

.flow span {
    color: #6b7280;
}

.learning {
    border-left: 5px solid #111827;
}

@media (max-width: 700px) {
    .container {
        margin-top: 20px;
    }

    .section-heading {
        align-items: flex-start;
        flex-direction: column;
        gap: 8px;
    }
}
```

</details>

### 11.9 What the README adds

The project's `README.md` contains the setup steps (covered in [Section 12](#12-virtual-environments-and-running-the-app)), the architecture diagrams (Section 11.2), the API examples (Section 12.4) and six exercises ([Section 18](#18-practice-exercises)). Its learning goal is that you can explain this whole flow:

```text
HTML form → HTTP request → Flask route → Python dictionary → Student class → Student object
→ SQLite → Student object → Dictionary → JSON / HTML
```

---

## 12. Virtual Environments and Running the App

### 12.1 Why a virtual environment?

Imagine two customers:
- Customer A's app needs **Flask 3.x**.
- Customer B's older app needs **Flask 2.x**.

A computer normally has one global set of Python packages, so you can't install both versions at once. A **virtual environment (venv)** is a **separate, private package folder for one project**. Each project gets its own "world", and versions never clash.

```
Your computer
├── project-A/venv  → Flask 3.1
└── project-B/venv  → Flask 2.3
```

### 12.2 Commands

| Step | Linux / macOS | Windows (cmd / PowerShell) |
|---|---|---|
| Create | `python3 -m venv venv` | `python -m venv venv` |
| Activate | `source venv/bin/activate` | `venv\Scripts\activate` |
| Install packages | `pip install -r requirements.txt` | `pip install -r requirements.txt` |
| Run the app | `python3 app.py` | `python app.py` |
| Stop the app | `Ctrl + C` | `Ctrl + C` |
| Leave the venv | `deactivate` | `deactivate` |

- In `python3 -m venv venv`, `-m venv` runs Python's built-in `venv` module. The last `venv` is the **folder name**, and you can pick any name (in class: `noor-venv`).
- After activating, your prompt shows the name in brackets, e.g. `(noor-venv) $`. That's how you know you're inside.
- On Windows **Git Bash**, activate with `source venv/Scripts/activate`.
- **Mistake from class:** the instructor first typed `source .venv/bin/activate` (with a dot) and it failed. The folder he'd created was called `noor-venv`. **The path must match the name you created.**
- Never commit the venv folder to Git. Add it to `.gitignore`, because `requirements.txt` is enough to rebuild it.

### 12.3 Run it

```bash
cd 13.flast-student-management-app
python3 -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate
pip install -r requirements.txt
python3 app.py
```

Real output:
```
 * Serving Flask app 'app'
 * Debug mode: on
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on http://127.0.0.1:5000
Press CTRL+C to quit
 * Restarting with stat
 * Debugger is active!
```

Open **http://127.0.0.1:5000** in the browser. `127.0.0.1` means "this computer" (also called `localhost`), and `5000` is Flask's default **port**.

### 12.4 Talking to the API from the terminal with `curl`

`curl` is a popular command-line tool for sending HTTP requests. Keep the app running in one terminal and open a second terminal tab. All outputs below are real:

```bash
# Read all (empty at first)
curl http://127.0.0.1:5000/api/students
```
```json
[]
```

```bash
# Create through the API
curl -X POST http://127.0.0.1:5000/api/students \
  -H "Content-Type: application/json" \
  -d '{"name": "Harrison", "age": 24, "email": "harrison@example.com", "cgpa": 2.7, "course": "DevOps", "skills": "Ansible, JavaScript"}'
```
```json
{
  "age": 24,
  "cgpa": 2.7,
  "course": "DevOps",
  "email": "harrison@example.com",
  "id": 2,
  "name": "Harrison",
  "skills": "Ansible, JavaScript"
}
```
Status: **201 Created**. (Its id is 2 because one student had already been added through the form.)

```bash
# Missing fields
curl -X POST http://127.0.0.1:5000/api/students -H "Content-Type: application/json" -d '{"name": "X"}'
```
```json
{
  "error": "Missing required student information"
}
```
Status: **400 Bad Request**.

```bash
# A student that doesn't exist
curl http://127.0.0.1:5000/api/students/99
```
```json
{
  "error": "Student not found"
}
```
Status: **404 Not Found**.

| `curl` flag | Meaning |
|---|---|
| `-X POST` | Use this HTTP method (default is GET) |
| `-H "Content-Type: application/json"` | Add a header: "the body I'm sending is JSON" |
| `-d '...'` | The request body (data) |

Notice that Flask **sorts the JSON keys alphabetically** (`age`, `cgpa`, `course`…). The order of keys in JSON doesn't matter. **Postman** is a graphical alternative to `curl`.

### 12.5 Where DevOps fits in

Developers build the app and hand over the `README.md`, `requirements.txt`, the port, and how to start it. **DevOps engineers** use that to set up test environments, **CI/CD pipelines**, containers and production servers (Gunicorn + Nginx). You don't need to write every app yourself, but you **must understand how it runs**. That's why we built one end to end.

---

## 13. CRUD in Action: Following One Request End to End

In class, students were added through the browser (Harrison, then Farhan Tanvir), edited and deleted. Here's what happens behind each click.

### 13.1 Create — "Save Student"

```
1. Browser    You fill the form, click "Save Student"
2. HTML       <form action="/students" method="POST"> sends the fields
3. Flask      @app.route("/students", methods=["POST"]) → create_student()
4. Python     request.form["age"] is "24" (str) → int("24") → 24
5. Object     student = Student(name=..., age=24, ...)
6. Repository StudentRepository.create(student) → INSERT INTO students ... ; commit
7. Database   SQLite gives the row id 1, 2, 3...
8. Response   redirect(url_for("index")) → 302 → browser asks for "/"
9. Read       index() → get_all() → render_template → table shows the new student
```

### 13.2 Update — "Edit" (Harrison's age 24 → 27, CGPA 2.7 → 3.9)

`GET /students/2/edit` shows the pre-filled form, then `POST /students/2/edit` → `update_student(2)` → `UPDATE students SET ... WHERE id = 2`. Verified result:

```json
{"age": 27, "cgpa": 3.9, "course": "DevOps", "email": "harrison@example.com", "id": 2, "name": "Harrison", "skills": "Ansible, JavaScript"}
```

### 13.3 Delete — "Delete" ("Noor bhai, Sunday-Monday closed!")

`POST /students/1/delete` → `delete_student(1)` → `DELETE FROM students WHERE id = 1`. Student 1 disappears from the table and from `/api/students`.

### 13.4 Why does this app update and delete with POST, not PUT and DELETE?

**HTML forms can only send `GET` or `POST`.** The browser pages therefore use `POST` for edit and delete. Real REST APIs usually expose:

```
PUT    /api/students/<id>    ← update
PATCH  /api/students/<id>    ← partial update
DELETE /api/students/<id>    ← delete (README Exercise 5, solved in Section 18)
```

Sending a `PUT` to this app returns **405 Method Not Allowed** (verified), because no route accepts it.

> **Precision note: POST vs PUT vs PATCH.** In class: "POST rewrites the whole object, PUT updates only the field you change, and there's also PATCH." The standard meanings are:
> - **POST** → **create** a new resource (or trigger an action).
> - **PUT** → **replace** an existing resource with the full new version.
> - **PATCH** → **partially update** an existing resource (only the fields you send).
>
> The instructor's point stands: an update should ideally use `PUT`/`PATCH`, not `POST`.

### 13.5 What happens with bad input?

Type `abc` into the age box (possible if you bypass the browser's number field, e.g. with curl) and `int("abc")` raises `ValueError`. Flask returns **500 Internal Server Error** (verified). Production code would validate input and return `400` with a helpful message. That's a good use of `try / except`.

---

## 14. Debugging with `pdb`

### 14.1 The full file: `14.pdb-set-trace-debugger.py`

```python
import pdb

def calculate_server_ratio(servers, users):
    # The script stops here!
    pdb.set_trace() 
    
    ratio = servers / users
    return ratio

print(calculate_server_ratio(10, 0))
```

**`pdb`** is Python's built-in **debugger** (from the standard library). `pdb.set_trace()` is a **breakpoint**: when the program reaches this line, it **pauses** and gives you a `(Pdb)` prompt. You can inspect variables and run the next lines one at a time.

Why? Big programs fail in confusing ways. The traceback tells you *where* it crashed, but not always *why*. Pausing just before the problem line lets you look around.

### 14.2 Reading a traceback first

In class the instructor first introduced a typo (`ratio` vs a misspelled name), ran the script, and read the **traceback**: Python's error report. Read it **bottom-up**: the last line is the error type and message, and the lines above show the file and **line number** where it happened. Then he placed the breakpoint just before that line.

### 14.3 A real session (with `10, 0`)

```
> ...14.pdb-set-trace-debugger.py(5)calculate_server_ratio()
-> pdb.set_trace()
(Pdb) p servers, users
(10, 0)
(Pdb) n
> ...14.pdb-set-trace-debugger.py(7)calculate_server_ratio()
-> ratio = servers / users
(Pdb) c
Traceback (most recent call last):
  ...
    ratio = servers / users
            ~~~~~~~~^~~~~~~
ZeroDivisionError: division by zero
```

Seeing `users = 0` *before* the division explains the crash. **Fix:** pass a non-zero number, or check `if users == 0:` first.

### 14.4 A session with `10, 4` (the class demo)

```
(Pdb) l                        ← list the code around the current line
  5  ->	    pdb.set_trace()
  7  	    ratio = servers / users
  8  	    return ratio
 10  	print(calculate_server_ratio(10, 4))
(Pdb) n                        ← run one line
-> ratio = servers / users
(Pdb) p ratio
*** NameError: name 'ratio' is not defined     ← not created yet: line 7 hasn't run
(Pdb) n
-> return ratio
(Pdb) c                        ← continue to the end
2.5
```

### 14.5 pdb commands

| Command | Name | Does |
|---|---|---|
| `n` | next | Run the current line, stop at the next one |
| `s` | step | Like `n`, but **steps into** a function call |
| `c` | continue | Run until the next breakpoint or the end |
| `p x` | print | Show the value of `x` |
| `l` | list | Show the code around the current line |
| `q` | quit | Stop the program and exit the debugger |
| `h` | help | List all commands |

`Ctrl + C` also gets you out, as in class.

**Modern shortcut:** since Python 3.7, the built-in **`breakpoint()`** does the same as `import pdb; pdb.set_trace()`, with no import needed. VS Code also has a graphical debugger (click left of a line number to set a red-dot breakpoint). The instructor still prefers `pdb.set_trace()`.

⚠️ **Remove breakpoints before committing code.** A forgotten `set_trace()` will freeze a server or CI pipeline while it waits for input that never comes.

---

## 15. Questions Asked in Class

**Q: How does `import modules` work without a path?**
Both files are in the same folder, and Python always searches the running script's folder first (`sys.path`). See [Section 3.6](#36-why-didnt-we-need-a-path).

**Q: In `for student in students`, is `student` just a variable?**
Yes. It's a **loop variable**. On each round it holds the next item: first `students[0]`, then `students[1]`, and so on. You can name it anything; `student` simply reads well.

**Q: You said the file closes right after reading. How does writing work then?**
Two separate `with` blocks: read and close, change the data in memory, then reopen in `"w"` mode, write and close. See [Section 7.4](#74-when-does-the-file-close-class-question).

**Q: The API idea is clear, but how does it work specifically in this app?** *(Mustafizur bhai)*
That's [Sections 11–13](#11-the-flask-student-management-app-full-code-walkthrough): every route is an API endpoint, the browser form calls `POST /students`, and `/api/students` returns JSON.

**Q: Can Python apps be served directly by Nginx?**
Not directly. Python apps run under Gunicorn (Flask/Django) or Uvicorn (FastAPI), with Nginx in front as a reverse proxy.

**Q: Will we cover objects (OOP) in more depth?**
Yes. A full OOP treatment needs 3–4 classes, and it will come up again in the project work.

**Q: Is this much Python needed for DevOps interviews?**
For DevOps roles, understanding and correlating these ideas is enough. For a Python engineer role you'd go deeper. You don't need to memorise everything, but you do need the basics.

---

## 16. Bugs and Gotchas in the Course Repo

| # | File | Issue | Effect | Fix |
|---|---|---|---|---|
| 1 | `11.JSON-Data/app.py` | `else` inside the inner skill loop | "No students…" printed once per non-matching skill, even on success (26 times for Linux) | Collect matches, print after the loops ([6.8](#68-step-6--search-by-skill-user-input-and-handle-case)) |
| 2 | `11.JSON-Data/inser-data-in-json.py` | No duplicate check | Each run adds Roman again | Check existing ids first ([7.5](#75-run-it-twice-and-roman-appears-twice)) |
| 3 | `11.JSON-Data/*.py` | Relative path `"students.json"` | `FileNotFoundError` unless run from inside the folder | `cd 11.JSON-Data` first |
| 4 | `11.JSON-Data/json-api.py` | Needs `requests`; heading says "Latest 5" | `ModuleNotFoundError` if not installed; shows the first 5, not the latest | `pip install requests`; use `posts[-5:]` for the latest |
| 5 | `12.modules/modules.py` | `divide()`/`remainder()` return a string on error | Mixed return types | Raise an exception instead |
| 6 | `12.modules/app.py` | Empty in the repo | Nothing to run | Use the version in [3.4](#34-using-it-12modulesapppy) |
| 7 | `13.flast-student-management-app` | `debug=True`; no input validation | Unsafe in production; `abc` as age → 500 | Turn debug off; validate and return 400 |
| 8 | `13.flast-student-management-app` | Folder name typo "flast"; `students.db` and `__pycache__/` committed | Confusing name; a real email inside the committed sample DB | Rename; add `*.db` and `__pycache__/` to `.gitignore` |
| 9 | `14.pdb-set-trace-debugger.py` | Calls `calculate_server_ratio(10, 0)` | `ZeroDivisionError`, **on purpose**, to practise debugging | Pass non-zero, or guard `users == 0` |

---

## 17. Common Misconceptions, Corrected

| ❌ Misconception | ✅ Reality |
|---|---|
| "A module is some special Python feature." | A module is **just a `.py` file**. |
| "`from x import y` loads only `y` and saves memory." | The whole module is executed and loaded either way. It only changes which names you can use directly. |
| "`import modules as math` is a fine alias." | It hides Python's real `math` module. Use a unique alias. |
| "JSON and Python dicts are the same thing." | JSON is **text**: double quotes only, `true`/`false`/`null`. Python dicts allow single quotes and use `True`/`False`/`None`. |
| "`print(data)` gives valid JSON." | It prints a **Python dict**. Use `json.dumps(data, indent=4)` for real JSON. |
| "`json.load` and `json.loads` are the same." | `load` reads a **file**; `loads` reads a **string**. Same for `dump`/`dumps`. |
| "`append()` puts the item at the start." | `append()` always adds to the **end**. |
| "After `append`, the file is updated." | Only memory changes. You must write the file again (`"w"` + `json.dump`). |
| "Opening a file with `"w"` adds to it." | `"w"` **erases** the file first. Use `"a"` to append text. |
| "`posts[-1]` gives me a list to loop over." | It gives **one item** (a dict). Use `posts[-1:]` for a one-item list. |
| "`requests` comes with Python." | It's a **third-party** package. `pip install requests`. |
| "JSONPlaceholder only allows GET." | It accepts POST/PUT/DELETE but **doesn't save** anything. |
| "The browser converts Python to JavaScript." | Python runs **on the server**. The browser receives HTML/CSS/JS/JSON only. |
| "SQLite is a sequential database." | It's a **relational** (SQL) database in a single file. |
| "`@staticmethod` makes a method protected." | It only means the method gets **no `self`**. Nothing is protected. |
| "POST updates the whole object, PUT updates one field." | **POST = create, PUT = replace, PATCH = partial update.** |
| "The Flask dev server is fine for production." | It's for development only. Use Gunicorn behind Nginx and turn `debug` off. |
| "A virtual environment is a virtual machine." | It's only a **separate folder of Python packages** for one project. |

---

## 18. Practice Exercises

These six exercises come from the project's `README.md`. Exercises 4–6 include solutions that were tested against the app.

**Exercise 1 — Add a `phone` field.** Update **all four** layers: the `Student` class (`__init__` + `to_dict`), the database table (`CREATE TABLE`, plus `INSERT`/`UPDATE`/`SELECT` handling in the repository), both HTML forms, and the API.
*Hint:* `CREATE TABLE IF NOT EXISTS` won't change an existing table. Delete `students.db` while developing so it's recreated.

**Exercise 2 — Add a `department` field.** Same steps as Exercise 1. Doing it twice makes the pattern stick.

**Exercise 3 — Search by name.** Add a search box; show students whose name contains the text.
*Hint:* SQL `WHERE name LIKE ?` with the value `f"%{text}%"`, or filter `get_all()` in Python using `.lower()` on both sides (like [6.8](#68-step-6--search-by-skill-user-input-and-handle-case)).

**Exercise 4 — `GET /api/students/cgpa/3.5`.** Return students with CGPA ≥ the number.

<details>
<summary>Show solution</summary>

```python
@app.route("/api/students/cgpa/<float:min_cgpa>", methods=["GET"])
def api_students_by_cgpa(min_cgpa):
    students = StudentRepository.get_all()
    return jsonify([s.to_dict() for s in students if s.cgpa >= min_cgpa])
```

Tested with Rahim (3.75) and Hasan (3.2): `/api/students/cgpa/3.5` → `["Rahim"]`.

⚠️ Flask's `<float:...>` converter needs a decimal point: `/cgpa/3` returns **404**. Use `/cgpa/3.0`.

</details>

**Exercise 5 — `DELETE /api/students/<id>`.**

<details>
<summary>Show solution</summary>

```python
@app.route("/api/students/<int:student_id>", methods=["DELETE"])
def api_delete_student(student_id):
    if StudentRepository.get_by_id(student_id) is None:
        return jsonify({"error": "Student not found"}), 404
    StudentRepository.delete(student_id)
    return jsonify({"message": f"Student {student_id} deleted"}), 200
```

Tested: first `DELETE /api/students/2` → `200 {"message": "Student 2 deleted"}`; second time → `404 {"error": "Student not found"}`.

```bash
curl -X DELETE http://127.0.0.1:5000/api/students/2
```

</details>

**Exercise 6 — Return skills as a JSON array.** Turn `"Python, Linux, Docker"` into `["Python", "Linux", "Docker"]` in the API.

<details>
<summary>Show solution</summary>

In `Student.to_dict()` replace the `"skills"` line:

```python
"skills": [skill.strip() for skill in self.skills.split(",") if skill.strip()],
```

`split(",")` cuts the string at commas, `.strip()` removes spaces, and `if skill.strip()` drops empty pieces. Tested: `"Python, Linux,Docker , "` → `['Python', 'Linux', 'Docker']`.

</details>

**Bonus:**
- Make `11.JSON-Data/app.py` print the **count** of students who know a skill.
- Change `json-api.py` to fetch `https://jsonplaceholder.typicode.com/users` and print each user's name and city (hint: `user["address"]["city"]`, the same nested access as Section 6.4).
- Put `breakpoint()` inside `create_student()` in the Flask app, submit the form, and inspect `request.form` with `p request.form`.

---

## 19. Interview Corner

**Q: What is a module? A package?**
A module is a single `.py` file. A package is a folder of modules (traditionally with an `__init__.py`), like `models/` in our app.

**Q: Standard library vs third-party packages?**
The standard library ships with Python (`json`, `os`, `sqlite3`). Third-party packages are installed with `pip` (`requests`, `flask`).

**Q: What does `if __name__ == "__main__":` do?**
It runs the code below it only when the file is executed directly, not when it's imported.

**Q: Difference between `json.load`, `json.loads`, `json.dump`, `json.dumps`?**
`load`/`dump` work with files; `loads`/`dumps` work with strings. `load(s)` reads JSON into Python; `dump(s)` writes Python as JSON.

**Q: Why use `with open(...)`?**
It closes the file automatically, even if an error happens inside the block.

**Q: What is an API? What is REST?**
An API is the interface through which programs talk to each other. A REST API uses HTTP methods (GET, POST, PUT/PATCH, DELETE) on URLs to perform CRUD, usually with JSON.

**Q: Map CRUD to HTTP methods.**
Create → POST, Read → GET, Update → PUT/PATCH, Delete → DELETE.

**Q: What do 200, 201, 400, 404, 405 and 500 mean?**
OK, Created, Bad Request, Not Found, Method Not Allowed, Internal Server Error.

**Q: Flask vs Django vs FastAPI?**
Flask: lightweight micro-framework. Django: full-stack, batteries included. FastAPI: modern framework focused on fast APIs.

**Q: Why not run the Flask dev server in production? What do you use instead?**
It's built for development, not for heavy traffic, and with debug on it exposes an interactive console. Use Gunicorn (WSGI) behind Nginx (reverse proxy), or Uvicorn for async apps.

**Q: What is a virtual environment and why use it?**
An isolated package folder per project, so different projects can use different package versions without conflict.

**Q: How do you prevent SQL injection in Python?**
Use parameterised queries (`?` placeholders with a tuple of values). Never build SQL with f-strings.

**Q: How do you debug a Python script?**
Read the traceback bottom-up, then set a breakpoint (`breakpoint()` / `pdb.set_trace()`), step with `n`/`s`, inspect with `p`, continue with `c`.

---

## 20. Glossary

| Term | Meaning |
|---|---|
| **Alias** | A nickname for an imported module: `import modules as calc` |
| **API** | Application Programming Interface: how programs talk to each other |
| **Backend** | Server-side code that handles logic and data |
| **Breakpoint** | A point where the program pauses for debugging |
| **Built-in module** | A module from Python's standard library |
| **Client** | The side that sends requests (usually the browser) |
| **Commit (database)** | Permanently save changes to the database |
| **Constant** | A value not meant to change, written in CAPITALS by convention |
| **CRUD** | Create, Read, Update, Delete |
| **`curl`** | Command-line tool for sending HTTP requests |
| **Custom module** | A `.py` file you wrote and import |
| **Debugger** | A tool to pause and inspect a running program (`pdb`) |
| **Decorator** | An `@something` line that modifies the function below it (`@app.route`, `@staticmethod`) |
| **Docstring** | Triple-quoted description right under a `def` or `class` |
| **DRY** | Don't Repeat Yourself |
| **Dunder** | "Double underscore" names like `__name__`, `__init__` |
| **Edge case** | An unusual input that can break naive code |
| **Endpoint** | A specific URL + method an API responds to (`GET /api/students`) |
| **Entry point** | Where the program starts (`if __name__ == "__main__":`) |
| **Exception** | An error raised while the program runs; caught with `try / except` |
| **Flask** | Lightweight Python web framework |
| **Frontend** | What the user sees and interacts with in the browser |
| **HTTP method** | The type of request: GET, POST, PUT, PATCH, DELETE |
| **Idempotent** | Running it twice gives the same result as once |
| **Jinja2** | Flask's template language: `{{ }}` and `{% %}` |
| **JSON** | JavaScript Object Notation: text format of key-value data |
| **Localhost / 127.0.0.1** | This computer |
| **Model** | A class that represents a kind of data (`Student`) |
| **Module** | A Python file that can be imported |
| **Named import** | `from module import name1, name2` |
| **Package** | A folder of modules (e.g. `models/`) |
| **Parameterised query** | SQL with `?` placeholders; prevents SQL injection |
| **`pip`** | Python's package installer |
| **Port** | A numbered door on a computer for network traffic (Flask: 5000) |
| **Repository (pattern)** | A class that holds all database operations for a model |
| **Request / Response** | What the client asks / what the server answers |
| **`requirements.txt`** | List of packages a project needs |
| **REST API** | An API using HTTP methods on URLs for CRUD |
| **Reverse proxy** | A front server (Nginx) that forwards requests to the app |
| **Route** | A URL path handled by a function in Flask |
| **Separation of concerns** | Each part of the code has one responsibility |
| **Server** | The side that answers requests |
| **Slice** | Part of a list: `posts[1:3]` |
| **SQL injection** | An attack where input rewrites your SQL query |
| **SQLite** | Single-file relational database built into Python (`sqlite3`) |
| **Standard library** | Modules installed together with Python |
| **Static method** | A method inside a class that receives no `self` |
| **Status code** | Number describing a response's result (200, 404, …) |
| **Template** | HTML file with placeholders filled by Flask |
| **Third-party package** | Code installed from PyPI with `pip` |
| **Traceback** | Python's error report: read it bottom-up |
| **Virtual environment** | An isolated set of packages for one project |
| **Web server** | A program that receives HTTP requests and returns responses |
| **WSGI / ASGI** | Standard interfaces between Python apps and web servers (sync / async) |

---

## 21. One-Page Cheat Sheet

```python
# ── MODULES ───────────────────────────────────────────────
import modules                      # whole file: modules.add(1, 2)
import modules as calc              # alias:      calc.add(1, 2)
from modules import add, name       # named:      add(1, 2)
# built-in: json os sys shutil datetime sqlite3 pdb   third-party: pip install requests flask

# ── FILES ─────────────────────────────────────────────────
with open("file.json", "r") as f:   # r=read  w=erase+write  a=append  x=create new
    ...                             # file closes automatically here

# ── JSON ──────────────────────────────────────────────────
import json
data = json.load(f)                 # file   → Python
data = json.loads(text)             # string → Python
json.dump(data, f, indent=4)        # Python → file
text = json.dumps(data, indent=4)   # Python → string
# JSON true/false/null  ⇄  Python True/False/None ; JSON needs "double quotes"

# ── NESTED ACCESS, LOOP, FILTER ───────────────────────────
data["students"][0]["address"]["city"]            # dict → key, list → index
for s in data["students"]:
    if s["cgpa"] > 3.7: print(s["name"])
if user_text.strip().lower() == skill.lower(): ...  # case-insensitive compare

# ── WRITE BACK ────────────────────────────────────────────
data["students"].append(new_student)              # memory only
with open("students.json", "w") as f:
    json.dump(data, f, indent=4)                  # now saved

# ── API WITH requests ─────────────────────────────────────
import requests
try:
    r = requests.get(url)
    r.raise_for_status()            # error on 4xx / 5xx
    posts = r.json()                # list / dict
except Exception as e:
    print(f"Failed: {e}")
posts[:5]  posts[-5:]  posts[1:3]  posts[-1]  # slices → list ; [-1] → one item

# ── CRUD ↔ HTTP ───────────────────────────────────────────
# Create=POST  Read=GET  Update=PUT/PATCH  Delete=DELETE
# 200 OK  201 Created  302 Redirect  400 Bad  404 Not found  405 Wrong method  500 Crash

# ── FLASK ─────────────────────────────────────────────────
from flask import Flask, jsonify, request, render_template, redirect, url_for
app = Flask(__name__)
@app.route("/api/students/<int:student_id>", methods=["GET"])
def get_one(student_id):
    return jsonify({...}), 200
if __name__ == "__main__":
    app.run(debug=True)             # dev only! production: gunicorn + nginx

# ── SQLITE ────────────────────────────────────────────────
conn = sqlite3.connect("students.db"); conn.row_factory = sqlite3.Row
conn.execute("SELECT * FROM students WHERE id = ?", (sid,))   # ? = safe from SQL injection
conn.commit(); conn.close()

# ── VENV & RUN ────────────────────────────────────────────
# python3 -m venv venv ; source venv/bin/activate   (Win: venv\Scripts\activate)
# pip install -r requirements.txt ; python3 app.py ; open http://127.0.0.1:5000
# curl http://127.0.0.1:5000/api/students ; Ctrl+C to stop ; deactivate

# ── DEBUG ─────────────────────────────────────────────────
import pdb; pdb.set_trace()         # or just: breakpoint()
# n=next  s=step into  c=continue  p x=print  l=list  q=quit
```

---

*Practise by running each folder yourself: modules → JSON read → JSON write → API → Flask app → pdb. "Nothing is hard unless you decide it's hard."*
