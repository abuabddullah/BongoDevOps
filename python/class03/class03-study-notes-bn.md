# ডেভঅপসের জন্য পাইথন — ক্লাস ০৩ স্টাডি নোট

**বিষয়:** পাইথনের বেসিক থেকে একটা আসল অ্যাপ্লিকেশন — মডিউল, JSON, ফাইল, REST API, Flask ওয়েব অ্যাপ এবং ডিবাগিং
**যা যা আছে:** কাস্টম ও বিল্ট-ইন মডিউল, `import`-এর ধরন, JSON ফরম্যাট, ফাইল পড়া ও লেখা, `json.load` / `json.dump`, নেস্টেড ডাটা অ্যাক্সেস, আসল ডাটায় লুপ ও ফিল্টার, API ও CRUD, `requests` লাইব্রেরি, `try / except`, ওয়েব সার্ভার, Flask, SQLite, ভার্চুয়াল এনভায়রনমেন্ট, `pdb`
**আগের ভিত্তি:** [ক্লাস ০১ নোট](../class01/class01-study-notes-bn.md) (ডাটা টাইপ, ভেরিয়েবল, f-স্ট্রিং, `input()`) এবং [ক্লাস ০২ নোট](../class02/class02-study-notes-bn.md) (লুপ, কন্ডিশন, ফাংশন, ক্লাস ও অবজেক্ট)
**কোর্স রিপোর যে ফোল্ডারগুলো আজ লাগবে:** `python-genius-main/11.JSON-Data`, `12.modules`, `13.flast-student-management-app`, `14.pdb-set-trace-debugger.py`

> **পড়ার নিয়ম:** প্রতিটি টেকনিক্যাল টার্ম প্রথমবার এসেছে **বাংলা (English)** আকারে, যেমন **মডিউল (Module)**। কোড সবসময় ইংরেজিতে। রিপো থেকে হুবহু কপি করা ফাইলগুলোর কমেন্টও ইংরেজিতে রাখা হয়েছে, যাতে আপনার ফাইলের সাথে অক্ষরে অক্ষরে মেলে। নোটের নিজস্ব উদাহরণগুলোর কমেন্ট বাংলায়।

> **এই নোট সম্পর্কে:** নোটটি ক্লাসের রেকর্ডিং (৪ অক্টোবর) আর কোর্স রিপোজিটরি থেকে তৈরি। **১১–১৪ নম্বর ফোল্ডারের প্রতিটি ফাইল এই নোটের ভেতরে পুরোটা দেওয়া আছে**, তাই সাথে সাথে বুঝতে কোনো ফোল্ডার খুলতে হবে না। প্রতিটি প্রোগ্রাম Python 3-এ (Flask 3.1, requests 2.34) চালিয়ে দেখা হয়েছে, এখানের আউটপুটগুলো আসল। ক্লাসে যেখানে সহজ করে বা আলগাভাবে কিছু বলা হয়েছে, সেখানে **সূক্ষ্ম সংশোধন** অংশে সঠিক রূপ দেওয়া আছে। রিপোর কোডে যেখানে বাগ আছে, সেখানে **বাগ সতর্কতা** অংশে সমস্যা ও ঠিক করা কোড দেওয়া আছে।

---

## সূচিপত্র

1. [রিক্যাপ ও আজকের রোডম্যাপ](#১-রিক্যাপ-ও-আজকের-রোডম্যাপ)
2. [প্রজেক্ট ম্যাপ: আজকের সব ফাইল](#২-প্রজেক্ট-ম্যাপ-আজকের-সব-ফাইল)
3. [মডিউল](#৩-মডিউল)
4. [JSON: ডাটা আদান-প্রদানের ভাষা](#৪-json-ডাটা-আদান-প্রদানের-ভাষা)
5. [ফাইল নিয়ে কাজ](#৫-ফাইল-নিয়ে-কাজ)
6. [JSON পড়া ও নেস্টেড ডাটা ঘাঁটা](#৬-json-পড়া-ও-নেস্টেড-ডাটা-ঘাঁটা)
7. [JSON লেখা: নতুন স্টুডেন্ট যোগ করা](#৭-json-লেখা-নতুন-স্টুডেন্ট-যোগ-করা)
8. [API, REST ও CRUD](#৮-api-rest-ও-crud)
9. [`requests` দিয়ে আসল API কল](#৯-requests-দিয়ে-আসল-api-কল)
10. [ওয়েব ডেভেলপমেন্টের বেসিক](#১০-ওয়েব-ডেভেলপমেন্টের-বেসিক)
11. [Flask স্টুডেন্ট ম্যানেজমেন্ট অ্যাপ: পুরো কোড বিশ্লেষণ](#১১-flask-স্টুডেন্ট-ম্যানেজমেন্ট-অ্যাপ-পুরো-কোড-বিশ্লেষণ)
12. [ভার্চুয়াল এনভায়রনমেন্ট ও অ্যাপ চালানো](#১২-ভার্চুয়াল-এনভায়রনমেন্ট-ও-অ্যাপ-চালানো)
13. [CRUD বাস্তবে: একটা রিকোয়েস্টের পুরো যাত্রা](#১৩-crud-বাস্তবে-একটা-রিকোয়েস্টের-পুরো-যাত্রা)
14. [`pdb` দিয়ে ডিবাগিং](#১৪-pdb-দিয়ে-ডিবাগিং)
15. [ক্লাসে যে প্রশ্নগুলো এসেছিল](#১৫-ক্লাসে-যে-প্রশ্নগুলো-এসেছিল)
16. [কোর্স রিপোর বাগ ও সতর্কতা](#১৬-কোর্স-রিপোর-বাগ-ও-সতর্কতা)
17. [প্রচলিত ভুল ধারণা ও সংশোধন](#১৭-প্রচলিত-ভুল-ধারণা-ও-সংশোধন)
18. [অনুশীলনী](#১৮-অনুশীলনী)
19. [ইন্টারভিউ কর্নার](#১৯-ইন্টারভিউ-কর্নার)
20. [পরিভাষা (Glossary)](#২০-পরিভাষা-glossary)
21. [এক পাতার চিট শিট](#২১-এক-পাতার-চিট-শিট)

---

## ১. রিক্যাপ ও আজকের রোডম্যাপ

ক্লাস শুরু হয়েছিল একটা উপমা দিয়ে। ইউটিউবে শেখা অনেক সময় "সারারাত রামায়ণ পড়ে সকালে জিজ্ঞেস করা, সীতা কার বাপ?"-এর মতো। সব পড়লেন, কিন্তু কিছু করতে পারছেন না।

> **চাল, ডাল, মশলা কেনা মানেই বিরিয়ানি রান্না নয়।**

ক্লাস ০১ আর ০২-এ আমরা উপকরণ কিনেছি: ডাটা টাইপ, লিস্ট, ডিকশনারি, লুপ, কন্ডিশন, ফাংশন, ক্লাস। **ক্লাস ০৩-এ বিরিয়ানি রান্না:** একটা আসল **স্টুডেন্ট ম্যানেজমেন্ট সিস্টেম (Student Management System)**, যা স্কুল, কলেজ বা যেকোনো প্রতিষ্ঠানে ব্যবহার করা যায়। এটা ক্লাস-অবজেক্ট দিয়ে স্টুডেন্টদের ডাটাবেসে রাখে, ব্রাউজার থেকে যোগ, এডিট ও ডিলিট করতে দেয়, এবং API দিয়ে ডাটা JSON আকারে দেয়।

**আজকের পথ:**

```
মডিউল  →  JSON  →  ফাইল (পড়া / লেখা)  →  API ও CRUD  →  requests লাইব্রেরি
       →  ওয়েব সার্ভার ও Flask  →  পুরো স্টুডেন্ট ম্যানেজমেন্ট অ্যাপ  →  pdb দিয়ে ডিবাগিং
```

আজকের জন্য শিক্ষকের অনুরোধ: **প্রশ্ন করে থামবেন না, পুরো প্রবাহটা বোঝার চেষ্টা করুন।** সন্দেহ দূর করার সেশন পরে হবে।

---

## ২. প্রজেক্ট ম্যাপ: আজকের সব ফাইল

কোর্স রিপোর যে অংশটা ক্লাস ০৩-এ লাগে, সেটা নিচে দেওয়া হলো। এখানের প্রতিটি ফাইল পরে এই নোটে পুরোটা দেওয়া আছে।

```text
python-genius-main/
│
├── 11.JSON-Data/                      ← অনুচ্ছেদ ৪–৭ ও ৯
│   ├── students.json                  ডাটা: একটা কোর্স, ১০ জন স্টুডেন্ট
│   ├── app.py                         JSON পড়া, টাইপ দেখা, লুপ, ফিল্টার, সার্চ
│   ├── inser-data-in-json.py          স্টুডেন্ট যোগ করে সেভ; ডিকশনারিকে অবজেক্ট বানানো
│   └── json-api.py                    একটা পাবলিক REST API থেকে পোস্ট আনা
│
├── 12.modules/                        ← অনুচ্ছেদ ৩
│   ├── modules.py                     কাস্টম মডিউল: অঙ্কের ফাংশন + একটা ভেরিয়েবল
│   └── app.py                         modules.py ইমপোর্ট করে ব্যবহার (ক্লাসে সরাসরি লেখা)
│
├── 13.flast-student-management-app/   ← অনুচ্ছেদ ১০–১৩  ("flast" আসলে "flask"-এর বানান ভুল)
│   ├── README.md                      সেটআপ ও চালানোর নির্দেশনা
│   ├── requirements.txt               অ্যাপের কোন প্যাকেজ লাগবে
│   ├── app.py                         Flask রাউট: ওয়েব পেজ + REST API (এন্ট্রি পয়েন্ট)
│   ├── database.py                    SQLite-এ কানেক্ট, টেবিল তৈরি
│   ├── students.db                    SQLite ডাটাবেস ফাইল (নিজে থেকে তৈরি হয়)
│   ├── models/
│   │   ├── __init__.py                খালি ফাইল: "models"-কে প্যাকেজ বানায়
│   │   └── student.py                 Student ক্লাস + StudentRepository ক্লাস
│   ├── templates/
│   │   ├── index.html                 হোম পেজ: যোগ করার ফর্ম + স্টুডেন্টের টেবিল
│   │   └── edit.html                  একজন স্টুডেন্ট এডিটের ফর্ম
│   └── static/
│       └── style.css                  পেজের ডিজাইন
│
└── 14.pdb-set-trace-debugger.py       ← অনুচ্ছেদ ১৪: স্ক্রিপ্ট থামিয়ে পরীক্ষা করা
```

| ফোল্ডার | এক লাইনে কাজ | নোটের কোথায় |
|---|---|---|
| `12.modules` | কোড আলাদা ফাইলে ভাগ করে `import` দিয়ে পুনর্ব্যবহার | [অনুচ্ছেদ ৩](#৩-মডিউল) |
| `11.JSON-Data` | JSON পড়া, ঘাঁটা, ফিল্টার ও লেখা; পাবলিক API কল | [অনুচ্ছেদ ৪–৭](#৪-json-ডাটা-আদান-প্রদানের-ভাষা), [৯](#৯-requests-দিয়ে-আসল-api-কল) |
| `13.flast-student-management-app` | পূর্ণাঙ্গ ওয়েব অ্যাপ: ব্রাউজার → Flask → ক্লাস → SQLite → JSON | [অনুচ্ছেদ ১০–১৩](#১০-ওয়েব-ডেভেলপমেন্টের-বেসিক) |
| `14.pdb-set-trace-debugger.py` | প্রোগ্রাম মাঝপথে থামিয়ে লাইন ধরে ধরে চালানো | [অনুচ্ছেদ ১৪](#১৪-pdb-দিয়ে-ডিবাগিং) |

> ক্লাসে আগে মডিউল (ফোল্ডার ১২), তারপর JSON (ফোল্ডার ১১) পড়ানো হয়েছে। নোটও ক্লাসের ক্রম মেনে চলেছে।

---

## ৩. মডিউল

### ৩.১ মডিউল কী?

> **মডিউল (Module) মানে শুধু একটা পাইথন ফাইল (`.py`)। এর ভেতরে ফাংশন, ক্লাস ও ভেরিয়েবল থাকতে পারে, আর অন্য ফাইল `import` করে সেগুলো আবার ব্যবহার করতে পারে।**

ব্যস, এটুকুই। আপনার লেখা যেকোনো `.py` ফাইলই একটা মডিউল। নাম যা খুশি দিতে পারেন: `modules.py`, `hello.py`, `utils.py`।

### ৩.২ মডিউল দুই রকম

| ধরন | কোথা থেকে আসে | উদাহরণ | ইনস্টল লাগে? |
|---|---|---|---|
| **বিল্ট-ইন মডিউল (Built-in Module)** | পাইথনের **স্ট্যান্ডার্ড লাইব্রেরি (Standard Library)**, যা পাইথনের সাথেই ইনস্টল হয় | `os`, `sys`, `shutil`, `json`, `datetime`, `sqlite3`, `pdb` | না, শুধু `import` |
| **কাস্টম মডিউল (Custom Module)** | **আপনার** লেখা `.py` ফাইল | ফোল্ডার ১২-এর `modules.py` | না, নিজেরই ফাইল |
| **থার্ড-পার্টি প্যাকেজ (Third-party Package)** *(আজ পরে দেখব)* | ইন্টারনেট (PyPI) থেকে নামাতে হয় | `requests`, `flask` | **হ্যাঁ**, `pip install` দিয়ে |

**স্ট্যান্ডার্ড লাইব্রেরি** হলো পাইথনের সাথে ডিফল্টভাবে যা কিছু আসে, যেমন নতুন ফোনে আগে থেকে দেওয়া অ্যাপগুলো। এর ভেতরে যা পাবেন:

| মডিউল | কাজ |
|---|---|
| `os` | অপারেটিং সিস্টেমের কাজ: ফাইল, ফোল্ডার, এনভায়রনমেন্ট ভেরিয়েবল |
| `sys` | চলমান পাইথনের তথ্য (আর্গুমেন্ট, ভার্সন, ইমপোর্ট পাথ) |
| `shutil` | ফাইল-ফোল্ডার কপি, সরানো, মুছে ফেলা |
| `json` | JSON ডাটা পড়া ও লেখা |
| `datetime` | তারিখ ও সময় |
| `sqlite3` | পূর্ণাঙ্গ একটা ডাটাবেস, আলাদা ডাটাবেস সার্ভার ইনস্টল ছাড়াই |
| `pdb` | পাইথনের ডিবাগার |

শিক্ষকের প্রিয় পয়েন্ট: **পাইথনের সাথে একটা ডাটাবেসই চলে আসে (`sqlite3`)।** শুরু করতে MongoDB, PostgreSQL বা MySQL কিছুই ইনস্টল করতে হয় না।

মডিউল আছে শত শত। কেউ সব মুখস্থ করে না। দরকারমতো জনপ্রিয়গুলো শিখে যাবেন।

### ৩.৩ মডিউল ফাইল: `12.modules/modules.py` (পুরো ফাইল)

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

`a` আর `b` হলো **প্যারামিটার (Parameter)**: প্রতিটি ফাংশন যে ইনপুট আশা করে। প্রতিটি `def`-এর নিচে তিন কোটেশনের লেখাটা **ডকস্ট্রিং (Docstring)**, অর্থাৎ ফাংশনের বিবরণ।

### ৩.৪ ব্যবহার: `12.modules/app.py`

> রিপোতে এই ফাইলটা **খালি**, কারণ ক্লাসে সরাসরি টাইপ করা হয়েছিল। স্ক্রিনে দেখানো আউটপুটের সাথে মিলিয়ে নিচে পুনর্গঠিত রূপটা দেওয়া হলো।

```python
import modules

print(modules.add(10, 5))
print(modules.divide(10, 5))
print(modules.remainder(10, 3))
print(modules.name)
```

ফোল্ডারের ভেতরে ঢুকে চালান:

```bash
cd 12.modules
python3 app.py
```

আউটপুট:
```
15
2.0
1
Nure
```

- `import modules` লেখা হয় **ফাইলের নাম দিয়ে, `.py` ছাড়া**। ফাইলের নাম `hello.py` হলে লিখতেন `import hello`।
- ইমপোর্টের পর ব্যবহার করুন `modulename.thing` আকারে: `modules.add(...)`, `modules.name`।
- **ভিএস কোড টিপ:** `Ctrl` চেপে ধরে `modules.add`-এর `add`-এ ক্লিক করুন। এডিটর সরাসরি `modules.py`-এর ফাংশনে চলে যাবে। কোড এক ফাইলে থাকে, ব্যবহার হয় আরেক ফাইলে।

### ৩.৫ ইমপোর্টের তিনটি উপায়

```python
# ১. পুরো মডিউল ইমপোর্ট
import modules
print(modules.add(10, 5))

# ২. ডাকনাম (alias) দিয়ে ইমপোর্ট
import modules as calc
print(calc.add(10, 5))

# ৩. নাম ধরে ইমপোর্ট: যেটুকু দরকার শুধু সেটুকু
from modules import add, subtract, name
print(add(10, 5))        # সামনে "modules." লাগে না
print(subtract(10, 5))
print(name)
```

৩ নম্বর পদ্ধতির আউটপুট:
```
15
5
Nure
```

| পদ্ধতি | লেখার ধরন | এরপর লিখবেন | কখন ভালো |
|---|---|---|---|
| পুরো মডিউল | `import modules` | `modules.add()` | কোন নাম কোথা থেকে এলো, পরিষ্কার বোঝা যায় |
| অ্যালিয়াস (Alias) | `import modules as calc` | `calc.add()` | লম্বা নামের মডিউলে (`import numpy as np`) |
| নেমড ইমপোর্ট (Named Import) | `from modules import add, name` | `add()` | অল্প কয়েকটা জিনিস বারবার লাগলে |

**ক্লাসের অ্যালিয়াস উপমা:** শিক্ষকের পুরো নাম নূর আলম সিদ্দিক, কিন্তু এলাকায় সবাই চেনে ডাকনামে। অ্যালিয়াস মানে মডিউলের ডাকনাম।

> **সূক্ষ্ম সংশোধন: অ্যালিয়াস।** ক্লাসে অ্যালিয়াস দেওয়া হয়েছিল `import modules as math`। এই নামটা এড়িয়ে চলুন, কারণ `math` স্ট্যান্ডার্ড লাইব্রেরির একটা আসল মডিউল। আপনার অ্যালিয়াস ওই ফাইলে আসল `math`-কে ঢেকে দেয়, যা পরে বিভ্রান্তি তৈরি করতে পারে। অনন্য নাম দিন, যেমন `calc`।

> **সূক্ষ্ম সংশোধন: "নেমড ইমপোর্টে মেমোরি বাঁচে"।** ক্লাসে বলা হয়েছে, নাম ধরে ইমপোর্ট করলে শুধু ওই ফাংশনগুলো লোড হয় তাই মেমোরি বাঁচে। বাস্তবে দুই ক্ষেত্রেই পাইথন **পুরো মডিউলটা চালায় ও লোড করে**। `from modules import add` শুধু ঠিক করে *আপনার* ফাইলে কোন নামগুলো দেখা যাবে। আসল লাভ হলো **পড়ার সুবিধা** আর **নামের জট এড়ানো**, মেমোরি নয়।

### ৩.৬ পাথ ছাড়া মডিউল পেল কীভাবে?

একজন স্টুডেন্ট জিজ্ঞেস করেছিলেন: *"পাথ না দিয়েও পাইথন `modules.py` খুঁজে পেল কীভাবে?"*

পাইথন মডিউল খোঁজে একটা তালিকার জায়গাগুলোতে, যার নাম **`sys.path`**। এর **প্রথম** জায়গাই হলো যে স্ক্রিপ্ট চালাচ্ছেন তার ফোল্ডার। `app.py` আর `modules.py` একই ফোল্ডারে, তাই সাথে সাথে পেয়ে যায়। মডিউল অন্য ফোল্ডারে রাখলে, প্যাকেজ না বানালে বা পাথ না বদলালে, ইমপোর্ট ব্যর্থ হবে।

### ৩.৭ মডিউল কেন দরকার?

| নীতি | অর্থ | মডিউল যেভাবে সাহায্য করে |
|---|---|---|
| **DRY** (Don't Repeat Yourself, নিজেকে পুনরাবৃত্তি কোরো না) | লজিক একবার লিখে সবখানে ব্যবহার | `add()` একবার লিখে যেকোনো ফাইলে ইমপোর্ট |
| **ক্লিন কোড (Clean Code)** | পড়তে ও রক্ষণাবেক্ষণে সহজ কোড | ছোট ছোট ফাইল, প্রতিটির একটা কাজ; ৫০০০ লাইনের এক ফাইল নয় |
| **সেপারেশন অফ কনসার্নস (Separation of Concerns)** | প্রোগ্রামের প্রতিটি অংশ একটা দায়িত্ব সামলায় | Flask অ্যাপ রাউট, ডাটাবেস ও মডেল আলাদা ফাইলে রেখেছে (অনুচ্ছেদ ১১) |

> "শুধু কোডিং জানলে হবে না, বেস্ট প্র্যাকটিসগুলোও জানতে হবে।"

> **বাগ সতর্কতা: এরর হলে `divide()` স্ট্রিং ফেরত দেয়।** `divide(4, 0)` ফেরত দেয় `"Error: Cannot divide by zero"`, একটা `str`, অথচ স্বাভাবিক ফলাফল `float`। যে কোড সংখ্যা আশা করে, সেটা পরে কোনো বিভ্রান্তিকর জায়গায় ভেঙে পড়বে। ভালো ডিজাইন হলো এররটা ঘটতে দেওয়া (`ZeroDivisionError`) বা `raise ValueError("b must not be zero")` লেখা, তারপর `try / except` দিয়ে সামলানো (অনুচ্ছেদ ৯.৩)।

---

## ৪. JSON: ডাটা আদান-প্রদানের ভাষা

### ৪.১ JSON কী?

**JSON = JavaScript Object Notation (জাভাস্ক্রিপ্ট অবজেক্ট নোটেশন)।** এটা ডাটা রাখা ও আদান-প্রদানের একটা টেক্সট ফরম্যাট, যেখানে ডাটা থাকে **কী: ভ্যালু (key: value)** জোড়ায়। দেখতে প্রায় হুবহু পাইথনের ডিকশনারির মতো।

- JSON ফাইলের শেষে থাকে **`.json`**, যেমন পাইথন ফাইলের শেষে `.py`।
- ইন্টারনেট থেকে যে ডাটা আসে (API রেসপন্স), তার বেশিরভাগই JSON।
- ডেভঅপসের কাজে কনফিগারেশন আর ডাটা আদান-প্রদানের সবচেয়ে প্রচলিত ফরম্যাট এটাই।

### ৪.২ ডাটা ফাইল: `11.JSON-Data/students.json`

ফাইলের কাঠামো (শুধু প্রথম স্টুডেন্ট):

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
<summary><b>পুরো <code>students.json</code> দেখুন (১০ জন স্টুডেন্ট)</b></summary>

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

*(রিপোতে প্রতিটি লিস্ট `indent=4` দিয়ে কয়েক লাইনে ছড়ানো। কনটেন্ট হুবহু একই।)*

</details>

### ৪.৩ এ যাবৎ শেখা সব ডাটা স্ট্রাকচার, এক ফাইলে

```
data  (dict)
├── "course"   → "DevOps & Cloud Engineering"   (str)
├── "batch"    → 6                              (int)
├── "active"   → true                           (bool)
└── "students" → [ ... ]                        (১০টা dict-এর list)
                   └── প্রতিটি স্টুডেন্ট (dict)
                       ├── "id"      → 101                    (int)
                       ├── "name"    → "Rahim"                (str)
                       ├── "age"     → 24                     (int)
                       ├── "cgpa"    → 3.65                   (float)
                       ├── "skills"  → ["Linux", ...]         (dict-এর ভেতরে list)
                       └── "address" → {"city": ..., ...}     (dict-এর ভেতরে dict)
```

ডিকশনারির ভেতরে লিস্ট, লিস্টের ভেতরে ডিকশনারি, তার ভেতরে আবার লিস্ট আর ডিকশনারি। **এজন্যই ডাটা স্ট্রাকচার এত গুরুত্বপূর্ণ।** আসল ব্যাকএন্ডের ডাটা ঠিক এরকমই দেখায়, সেটা স্টুডেন্ট, প্রোডাক্ট বা সার্ভার যা নিয়েই হোক।

### ৪.৪ JSON বনাম পাইথন: ছোট কিন্তু জরুরি পার্থক্য

| JSON (ফাইলের টেক্সট) | পাইথন (লোড করার পর) |
|---|---|
| অবজেক্ট `{ }` | `dict` |
| অ্যারে `[ ]` | `list` |
| স্ট্রিং `"text"` | `str` |
| সংখ্যা `24` / `3.65` | `int` / `float` |
| `true` / `false` | `True` / `False` |
| `null` | `None` |

- JSON-এর স্ট্রিংয়ে **অবশ্যই** **ডাবল কোটেশন** লাগবে। `{'name': 'Rahim'}` পাইথনে বৈধ, কিন্তু **JSON হিসেবে অবৈধ**:
  ```
  JSONDecodeError: Expecting property name enclosed in double quotes
  ```
- JSON-এ কমেন্ট লেখা যায় না, শেষ আইটেমের পরে বাড়তি কমাও দেওয়া যায় না।

**ক্লাসের টিপ:** অনলাইন **JSON ভিউয়ার / ফরম্যাটারে** ("JSON viewer" লিখে সার্চ করুন) JSON পেস্ট করলে গাছের মতো খোলা-বন্ধ করা যায় এমন রূপে দেখা যায়। ক্লাসে ভিউয়ারটা পেস্ট করা ডাটা নেয়নি। কারণ `print(data)` দেখায় একটা **পাইথন ডিকশনারি** (সিঙ্গেল কোটেশন, `True`), যা বৈধ JSON নয়। আসল `.json` ফাইলটা পেস্ট করুন, অথবা `json.dumps(data, indent=4)` প্রিন্ট করুন।

---

## ৫. ফাইল নিয়ে কাজ

হাতে ফাইল খুলি এভাবে: ডাবল-ক্লিক, কাজ, তারপর ক্রস দিয়ে বন্ধ। প্রোগ্রামও একই কাজ করে: **খোলো → কাজ করো → বন্ধ করো**।

### ৫.১ `with open(...)`: নিরাপদ উপায়

```python
with open("students.json", "r") as file:
    data = json.load(file)
# এখানে আসার আগেই ফাইল বন্ধ হয়ে গেছে
```

শব্দ ধরে ধরে:

| অংশ | অর্থ |
|---|---|
| `with` | "পরিষ্কারের দায়িত্ব তোমার": ইনডেন্ট করা অংশ শেষ হলেই ফাইল **নিজে থেকে বন্ধ** হয়, মাঝে এরর হলেও |
| `open("students.json", "r")` | এই ফাইলটা (নাম সবসময় **স্ট্রিং**) **রিড (Read)** মোডে খোলো |
| `as file` | খোলা ফাইলটার একটা ছোট নাম (অ্যালিয়াস), ব্লকের ভেতরে ব্যবহারের জন্য |
| `:` + ইনডেন্ট করা ব্লক | ইনডেন্টের ভেতরের সবকিছু ফাইল খোলা অবস্থায় চলে |

### ৫.২ ফাইল মোড (File Mode)

| মোড | নাম | কী করে | ফাইল না থাকলে |
|---|---|---|---|
| `"r"` | read (পড়া) | শুধু পড়ে (ডিফল্ট) | এরর |
| `"w"` | write (লেখা) | ফাইল **মুছে ফেলে**, তারপর লেখে | তৈরি করে |
| `"a"` | append (যোগ) | শেষে যোগ করে | তৈরি করে |
| `"x"` | create (নতুন তৈরি) | শুধু নতুন ফাইল বানায় | তৈরি করে (থাকলে এরর) |

⚠️ `"w"` মোড ফাইল **খোলার সাথে সাথেই** খালি করে দেয়। যা লাগবে আগে পড়ে নিন (অনুচ্ছেদ ৭)।

### ৫.৩ রিলেটিভ পাথ (Relative Path) নির্ভর করে কোথা থেকে চালাচ্ছেন তার ওপর

`open("students.json")` ফাইলটা খোঁজে **কারেন্ট ওয়ার্কিং ডিরেক্টরিতে (Current Working Directory)**, অর্থাৎ টার্মিনাল যে ফোল্ডারে আছে সেখানে। স্ক্রিপ্ট যে ফোল্ডারে আছে সেখানে **নয়**। এজন্যই শিক্ষক চালানোর আগে সবসময় `cd 11.JSON-Data` করেছেন। ওপরের ফোল্ডার থেকে চালালে হবে:

```
FileNotFoundError: [Errno 2] No such file or directory: 'students.json'
```

খেয়াল করুন, প্রতিটি ফোল্ডারে আলাদা `app.py` আছে। এগুলো আলাদা ফাইল; কোনটা চলবে, সেটা ঠিক হয় কোন ফোল্ডারে `cd` করলেন তা দিয়ে।

---

## ৬. JSON পড়া ও নেস্টেড ডাটা ঘাঁটা

### ৬.১ পুরো ফাইল: `11.JSON-Data/app.py`

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

চালান:
```bash
cd 11.JSON-Data
python3 app.py
```

ক্লাসে শিক্ষক বাকি অংশ কমেন্ট করে (`Ctrl + /`) একবারে একটা ব্লক চালিয়েছেন। নিচে আমরাও তাই করব।

### ৬.২ `json.load` বনাম `json.loads`

| ফাংশন | কোথা থেকে পড়ে | উদাহরণ |
|---|---|---|
| `json.load(file)` | খোলা একটা **ফাইল** থেকে | `data = json.load(file)` |
| `json.loads(text)` | একটা **স্ট্রিং** থেকে ("s" মানে string) | `data = json.loads('{"name": "Rahim"}')` |

```python
import json
text = '{"name": "Rahim", "age": 24, "active": true, "manager": null}'
d = json.loads(text)
print(d, type(d))
# {'name': 'Rahim', 'age': 24, 'active': True, 'manager': None} <class 'dict'>
```

দেখুন, `true` হয়ে গেছে `True`, আর `null` হয়ে গেছে `None`।

### ৬.৩ ধাপ ১ — টাইপ দেখে নিন

টাইপ পরীক্ষার অংশের আউটপুট:
```
<class 'dict'>      ← data: পুরো ফাইল
<class 'str'>       ← data["course"]
<class 'int'>       ← data["batch"]
<class 'list'>      ← data["students"]
<class 'dict'>      ← একজন স্টুডেন্ট
<class 'str'>       ← name
<class 'int'>       ← age
<class 'float'>     ← cgpa
<class 'list'>      ← skills
```

অচেনা ডাটা হাতে পেলে **আগে টাইপ দেখুন**। তাহলেই বুঝবেন `["key"]`, `[index]` নাকি লুপ লাগবে।

### ৬.৪ ধাপ ২ — নেস্টেড ডাটার ভেতরে ঢোকা

ক্লাস ০২-এর নিয়ম মনে আছে? **dict হলে কী, list হলে ইনডেক্স।** একটার পর একটা জুড়ে আরো গভীরে যান:

```python
print(data["course"])                           # DevOps & Cloud Engineering
print(data["students"][0]["name"])              # Rahim
print(data["students"][0]["skills"])            # ['Linux', 'Python', 'Docker']
print(data["students"][0]["address"]["city"])   # Dhaka
```

`data["students"][0]["address"]["city"]` বাম থেকে ডানে পড়ুন:

```
data                → পুরো dict
["students"]        → ১০ জন স্টুডেন্টের list
[0]                 → প্রথম স্টুডেন্ট (একটা dict)
["address"]         → ওই স্টুডেন্টের ঠিকানা (একটা dict)
["city"]            → "Dhaka"
```

**বাস্তব উদাহরণ:** আদমশুমারির ডাটাবেস। নূর নামের মানুষটাকে খুঁজুন, তার ঠিকানায় ঢুকুন, হোম ডিস্ট্রিক্ট পড়ুন। একই শিকল।

### ৬.৫ ধাপ ৩ — প্রতিটি স্টুডেন্টের ওপর লুপ

```python
print(len(data["students"]))      # ক্লাসে যোগ করা হয়েছিল: মোট কতজন?

for student in data["students"]:
    print(student["name"], student["age"])
```

আউটপুট:
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

`student` শুধু একটা **লুপ ভেরিয়েবল (Loop Variable)**। প্রতিবার লুপ ঘুরলে এতে পরের স্টুডেন্টের dict থাকে: প্রথমে ইনডেক্স ০, তারপর ১, তারপর ২… (ক্লাসে একজন স্টুডেন্ট এটা জিজ্ঞেস করেছিলেন; দেখুন [অনুচ্ছেদ ১৫](#১৫-ক্লাসে-যে-প্রশ্নগুলো-এসেছিল))।

### ৬.৬ ধাপ ৪ — লুপের ভেতরে লুপ (Nested Loop)

প্রতিটি স্টুডেন্টের `skills` একটা **লিস্ট**, তাই তার ওপরও লুপ চালাই:

```python
for student in data["students"]:
    print(f"{student['name']} knows: ")

    for skill in student["skills"]:
        print(f"- {skill}")
```

আউটপুট (প্রথম তিনজন):
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

কোটেশন খেয়াল করুন: f-স্ট্রিংয়ের বাইরে ডাবল কোটেশন, তাই ভেতরের কী-তে **সিঙ্গেল** কোটেশন: `f"{student['name']}"`। ভেতরে-বাইরে একই কোটেশন দিলে পুরনো পাইথনে এরর হয়।

### ৬.৭ ধাপ ৫ — ফিল্টার: কার CGPA ৩.৭-এর বেশি?

**ক্লাসের এইচআর উদাহরণ:** এইচআর ৫০০০ সিভি পেয়েছে, সেগুলো JSON-এ রূপান্তর করেছে, এখন CGPA ৩.৭-এর বেশি যাদের, তাদের ইন্টারভিউতে ডাকবে। (অথবা কম আনন্দের কাজ: পারফরম্যান্স ৮০%-এর ওপরে যারা, শুধু তাদের রাখা।)

```python
for student in data["students"]:
    if student["cgpa"] > 3.7:
        print(student["name"])
```

আউটপুট:
```
Karim
Ayesha
Nusrat
Mim
Farzana
```

`>` মানে "কঠোরভাবে বড়"। ৩.৭ নিজেও গণ্য করতে চাইলে `>=` দিন।

### ৬.৮ ধাপ ৬ — স্কিল দিয়ে সার্চ (ইউজার ইনপুট) ও কেস সামলানো

**ক্লাসের ব্যাংক উদাহরণ:** ব্যাংকে একাউন্ট নম্বর দিলে তারা দেখে ওই নম্বরে একাউন্ট আছে কি না। এখানে ইউজার একটা স্কিল লিখবে, আর আমরা দেখাব কোন স্টুডেন্টদের সেটা আছে।

**এজ কেস (Edge Case):** পাইথন **কেস-সেনসিটিভ** (ক্লাস ০১)। ইউজার যদি লেখে `lInUx` আর ডাটায় থাকে `Linux`, সাধারণ `==` বলবে "সমান নয়"। সমাধান: তুলনার আগে **দুই পাশকেই ছোট হাতের (lowercase) বানিয়ে নিন**।

```python
skill_to_find = input("Enter a skill: ").lower()   # ইউজারের লেখা → lowercase
...
if skill_to_find == skill.lower():                 # ডাটার লেখা → lowercase
```

**এজ কেস** মানে এমন অস্বাভাবিক ইনপুট, যা সরল কোডকে ভেঙে দেয়। আসল প্রোগ্রামিংয়ের বড় একটা অংশ হলো এজ কেস সামলানো।

> **বাগ সতর্কতা: "No students" বার্তা বারবার আসে।** `else` বসে আছে **ভেতরের লুপের ভেতরে**, তাই **প্রতিটি না-মেলা স্কিলের জন্য** এটা চলে, সার্চ সফল হলেও। `Python` লিখলে আসল আউটপুট:
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
> `Python` সার্চে ঠিক ৬ জনকেই পায়, কিন্তু পথে "No students…" ছাপে **২৪ বার**। `lInUx` সার্চে ঠিক ৪টা নাম আসে, আর বার্তাটা আসে **২৬ বার**।
>
> **ঠিক করা কোড:** আগে মিলগুলো জমাও, পরে সিদ্ধান্ত নাও।
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
>             break          # এই স্টুডেন্ট মিলে গেছে; তার বাকি স্কিল দেখার দরকার নেই
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
> `.strip()` ইনপুটের আশেপাশের ভুল করে দেওয়া স্পেসও মুছে দেয়, এটাও একটা এজ কেস।

---

## ৭. JSON লেখা: নতুন স্টুডেন্ট যোগ করা

এ পর্যন্ত আমরা শুধু **পড়েছি**। এখন আরো পাঁচজন স্টুডেন্ট এসেছে। একই ফাইলে তাদের **লিখব** কীভাবে?

### ৭.১ পুরো ফাইল: `11.JSON-Data/inser-data-in-json.py`

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

### ৭.২ ধাপে ধাপে কী ঘটে

```
students.json ──(open "r", json.load)──► মেমোরিতে data (১০ জন)
                                              │
                                    append(new_student)
                                              ▼
                                   মেমোরিতে data (১১ জন)
                                              │
students.json ◄──(open "w", json.dump)────────┘   ফাইলে এখন ১১ জন
```

1. ফাইলটা **পড়ে** `data`-তে রাখা (`with` ব্লক শেষে ফাইল বন্ধ)।
2. `new_student` বানানো: অন্যদের মতো একই কাঠামোর একটা সাধারণ **dict**।
3. `data["students"].append(new_student)` এটাকে লিস্টের **শেষে** যোগ করে। (ক্লাসের প্রশ্ন: *শুরুতে, মাঝে নাকি শেষে?* **শেষে।** `append` সবসময় শেষে যোগ করে।)
4. ⚠️ এই মুহূর্তে পরিবর্তনটা আছে **শুধু মেমোরিতে**। ডিস্কের ফাইলে এখনো ১০ জন।
5. **ফাইলটা আবার `"w"` মোডে খুলে** পুরো হালনাগাদ `data` দিয়ে `json.dump` করা। এবার ফাইলে ১১ জন।
6. ধাপ ৬–৮ প্রতিটি dict-কে `Student` **অবজেক্টে** (ক্লাস ০২-এর OOP) রূপান্তর করে `display()` ডাকে। এটা দেখায় **কাঁচা ডাটা (dict)** আর **অবজেক্ট (ক্লাসের ইনস্ট্যান্স)**-এর মধ্যকার সেতু, যে সেতু Flask অ্যাপও ব্যবহার করে।

আউটপুটের শেষ অংশ:
```
------------------------------
Name: Roman
Age: 23
CGPA: 3.9
Skills: Python, Docker
------------------------------
```

`', '.join(self.skills)` লিস্টকে জুড়ে একটা স্ট্রিং বানায়: `["Python", "Docker"]` → `"Python, Docker"`। `"-" * 30` ড্যাশটাকে ৩০ বার পুনরাবৃত্তি করে।

### ৭.৩ `json.dump` বনাম `json.dumps`

| ফাংশন | কোথায় লেখে | ফেরত দেয় |
|---|---|---|
| `json.dump(data, file, indent=4)` | খোলা একটা **ফাইলে** | কিছু না |
| `json.dumps(data, indent=4)` | — | JSON-এর একটা **স্ট্রিং** |

`indent=4` আউটপুটকে মানুষের পড়ার উপযোগী করে, নেস্টেড আইটেম ৪ স্পেস ভেতরে সরিয়ে। এটা না দিলে সব এক লম্বা লাইনে চলে আসে।

**মনে রাখার কৌশল:** **s** মানে **s**tring। `load`/`dump` কাজ করে ফাইল নিয়ে, `loads`/`dumps` কাজ করে স্ট্রিং নিয়ে।

### ৭.৪ ফাইল কখন বন্ধ হয়? (ক্লাসের প্রশ্ন)

একজন স্টুডেন্ট জিজ্ঞেস করেছিলেন: *"আপনি বললেন পড়ার পরেই ফাইল বন্ধ হয়ে যায়। তাহলে লেখা হলো কীভাবে?"*

এখানে **দুটো আলাদা `with` ব্লক** আছে:
1. প্রথমটা পড়ার জন্য ফাইল খোলে, ডাটা **মেমোরিতে** তোলে, ব্লক শেষ হলে ফাইল বন্ধ করে।
2. প্রোগ্রাম ডাটা বদলায় **মেমোরিতে**।
3. দ্বিতীয় `with` লেখার জন্য ফাইলটা আবার খোলে, ডাটা লেখে, তারপর বন্ধ করে।

ফাইল খোলা থাকে **শুধু তার `with` ব্লক চলার সময়টুকু।**

### ৭.৫ দুবার চালালে রোমান দুবার আসে

ক্লাসে স্ক্রিপ্টটা দুবার চলেছিল, আর রোমান **দুবার** যোগ হয়েছিল (১১ নয়, ১২ জন)। শিক্ষক হাতে বাড়তিটা মুছে দিয়েছিলেন। যাচাই করা ফলাফল:

```
১ম বার চালানোর পর: 11 students
২য় বার চালানোর পর: 12 students  ['Arif', 'Roman', 'Roman']
```

স্ক্রিপ্টটা কখনো দেখে না যে স্টুডেন্ট আগে থেকেই আছে কি না। ডুপ্লিকেট-চেকসহ একটা **নিরাপদ সংস্করণ**:

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
Added Roman. Total students: 11          ← প্রথমবার
Student 111 already exists. Nothing to add.   ← দ্বিতীয়বার
```

ডেভঅপসে এই ধারণার নাম **আইডেমপোটেন্সি (Idempotency)**: কোনো কাজ দুবার চালালেও ফল একবার চালানোর মতোই। Ansible, Terraform, Kubernetes সবই এই নীতির ওপর দাঁড়িয়ে।

---

## ৮. API, REST ও CRUD

> "বাস্তব দুনিয়া কি এত সুন্দর? কেউ কি আমাদের গুছানো JSON ফাইল হাতে দেবে?" না। বাস্তবে ডাটা সাধারণত আসে **API** দিয়ে।

### ৮.১ API কী?

**API = Application Programming Interface (অ্যাপ্লিকেশন প্রোগ্রামিং ইন্টারফেস)।** এটা **দুটো সফটওয়্যারের মাঝের বার্তাবাহক**: এক পাশের রিকোয়েস্ট নিয়ে যায়, উত্তর ফিরিয়ে আনে।

**ক্লাসের উপমা:**
- **ফোন কল:** আপনি বন্ধুকে ফোন করলেন। কলটা যায় টাওয়ারে (জিপি, এয়ারটেল), টাওয়ার বন্ধুর নম্বর খুঁজে সংযোগ দেয়। আপনি নিজে টাওয়ারের সাথে কথা বলেন না; সে মাঝখানে বসে থাকে।
- **মধ্যস্থতাকারী:** দুই দেশ তৃতীয় একটা দেশের মাধ্যমে আলোচনা করলে, তৃতীয় দেশটা দুই দিকেই বার্তা বয়ে নেয়।

একটা ওয়েব অ্যাপ্লিকেশনে:

```
 ব্রাউজার (ফ্রন্টএন্ড)  ──রিকোয়েস্ট──►  ব্যাকএন্ড (Flask / পাইথন)  ──কোয়েরি──►  ডাটাবেস (SQLite)
                       ◄──রেসপন্স────                             ◄──রো───
                           (JSON / HTML)
```

**ফ্রন্টএন্ড (Frontend)** হলো ব্রাউজারে যা দেখেন। **ব্যাকএন্ড (Backend)** হলো সার্ভারে চলা কোড। API হলো ফ্রন্টএন্ড কীভাবে ব্যাকএন্ডের সাথে কথা বলে (এবং এক সার্ভিস কীভাবে আরেক সার্ভিসের সাথে কথা বলে)।

### ৮.২ CRUD ও HTTP মেথড

ডাটা নিয়ে একটা অ্যাপ যা করে, তার প্রায় সবই চারটা কাজের একটা। একসাথে এদের বলে **CRUD**:

| CRUD | অর্থ | HTTP মেথড | দৈনন্দিন উদাহরণ |
|---|---|---|---|
| **C**reate (তৈরি) | নতুন ডাটা যোগ | `POST` | সাইন আপ; স্টুডেন্ট যোগ; লগইন ফর্ম জমা |
| **R**ead (পড়া) | ডাটা আনা | `GET` | পেজ খোলা; সব স্টুডেন্টের তালিকা |
| **U**pdate (হালনাগাদ) | আগের ডাটা বদলানো | `PUT` (পুরোটা) / `PATCH` (অংশবিশেষ) | স্টুডেন্টের বয়স এডিট |
| **D**elete (মুছে ফেলা) | ডাটা সরানো | `DELETE` | স্টুডেন্ট মুছে ফেলা |

**REST API** হলো এমন API, যা URL-এর (`/api/students`, `/api/students/1`) ওপর এই HTTP মেথডগুলো ব্যবহার করে CRUD করে, সাধারণত JSON আদান-প্রদান করে।

### ৮.৩ আজ যে HTTP স্ট্যাটাস কোডগুলো দেখবেন

| কোড | অর্থ | আমাদের অ্যাপ কখন দেয় |
|---|---|---|
| `200 OK` | সফল | স্টুডেন্ট পড়ার সময় |
| `201 Created` | নতুন কিছু তৈরি হয়েছে | API দিয়ে স্টুডেন্ট তৈরি করলে |
| `302 Found` | রিডাইরেক্ট: "অন্য পেজে যাও" | ফর্ম সেভের পর হোম পেজে ফেরত |
| `400 Bad Request` | রিকোয়েস্টে কিছু নেই বা ভুল | দরকারি ফিল্ড ছাড়া API POST |
| `404 Not Found` | জিনিসটা নেই | id ৯৯-এর স্টুডেন্ট; ভুল বানানের API পাথ |
| `405 Method Not Allowed` | URL ঠিক, মেথড ভুল | যে রাউট শুধু `GET`/`POST` নেয় সেখানে `PUT` |
| `500 Internal Server Error` | সার্ভারের কোড ক্র্যাশ করেছে | ফর্মে বয়সের জায়গায় `abc` লিখলে |

### ৮.৪ বাস্তবে API-এর URL

একটা প্রচলিত ধরন:

```
https://bongodev.com/api/students        ← সব স্টুডেন্ট
https://bongodev.com/api/students/1      ← একজন স্টুডেন্ট, id 1
```

`/api/` অংশটা বাধ্যতামূলক নয়, তবে দেখলেই বোঝা যায় URL-টা ওয়েব পেজ নয়, ডাটা দেয়।

**ক্লাসের নিরাপত্তা পয়েন্ট:** আসল কোম্পানিগুলো **এভাবে API সবার জন্য খুলে রাখে না**। `/api/users` যদি সবার জন্য খোলা থাকত, প্রতিযোগীরা সব ইউজারের নাম আর ফোন নম্বর নামিয়ে নিত। আসল API-তে অথেনটিকেশন (Authentication) লাগে, অ্যাক্সেস সীমিত থাকে।

### ৮.৫ প্র্যাকটিসের ফ্রি API: JSONPlaceholder

**https://jsonplaceholder.typicode.com** প্র্যাকটিসের জন্য একটা ফ্রি নকল API (বুকমার্ক করে রাখুন)। এখানে `/posts`, `/comments`, `/users` সহ আরো অনেক কিছু আছে। প্রতিটি পোস্টে থাকে `userId`, `id`, `title` ও `body`।

> **সূক্ষ্ম সংশোধন:** ক্লাসে বলা হয়েছে JSONPlaceholder "শুধু GET দেয়"। আসলে এটা POST, PUT, DELETE **গ্রহণ করে** এবং বাস্তবসম্মত উত্তরও দেয়, কিন্তু **কিছুই সেভ করে না**। যাচাই করা: একটা POST ফেরত দেয় `201` আর `"id": 101`, অথচ `GET /posts/101` দেয় `404`, আর তালিকায় পোস্ট থাকে সেই ১০০টাই। প্র্যাকটিসের জন্য এটা দারুণ, কারণ কিছু ভাঙার ভয় নেই।

---

## ৯. `requests` দিয়ে আসল API কল

### ৯.১ `requests` ইনস্টল

`requests` স্ট্যান্ডার্ড লাইব্রেরিতে **নেই**। এটা PyPI থেকে আসা **থার্ড-পার্টি প্যাকেজ**, তাই আগে ইনস্টল করতে হবে:

```bash
pip install requests
```

না করলে হবে `ModuleNotFoundError: No module named 'requests'`। (ভার্চুয়াল এনভায়রনমেন্টের ভেতরে ইনস্টল করুন; দেখুন [অনুচ্ছেদ ১২](#১২-ভার্চুয়াল-এনভায়রনমেন্ট-ও-অ্যাপ-চালানো)।)

প্রতিটি HTTP মেথডের জন্য `requests`-এ একটা করে ফাংশন আছে: `requests.get()`, `requests.post()`, `requests.put()`, `requests.delete()`।

### ৯.২ পুরো ফাইল: `11.JSON-Data/json-api.py`

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

আউটপুট:
```
Successfully fetched 100 posts.

--- Latest 5 Post Titles ---
ID 1: sunt aut facere repellat provident occaecati excepturi optio reprehenderit
ID 2: qui est esse
ID 3: ea molestias quasi exercitationem repellat qui ipsa sit aut
ID 4: eum et est occaecati
ID 5: nesciunt quas odio
```

(টাইটেলগুলো নকল ল্যাটিন লেখা, পড়ার চেষ্টা করবেন না।)

### ৯.৩ লাইন ধরে ধরে

| লাইন | কী করে |
|---|---|
| `requests.get(url)` | একটা **GET** রিকোয়েস্ট পাঠায়; সার্ভারের উত্তর থাকে `response`-এ |
| `response.raise_for_status()` | স্ট্যাটাস কোড এরর (4xx/5xx) হলে চুপচাপ না এগিয়ে **এক্সেপশন (Exception) তোলে** |
| `response.json()` | রেসপন্সের JSON টেক্সটকে পাইথন অবজেক্টে রূপান্তর করে (এখানে dict-এর একটা **list**) |
| `posts[:5]` | **স্লাইস (Slice)**: ইনডেক্স ০ থেকে ৫-এর আগ পর্যন্ত নেয় |
| `try:` | "এই কোডটা চেষ্টা করো…" |
| `except Exception as e:` | "…আর **যেকোনো** গোলমাল হলে এটা চালাও।" `e`-তে থাকে এররের বার্তা |

**`try / except` কেন?** কোনো URL সবসময় কাজ করবে, এমন নিশ্চয়তা নেই। কাল নাম বদলে যেতে পারে, নেটওয়ার্ক বন্ধ থাকতে পারে, বা আপনি বানান ভুল করতে পারেন। `try` ছাড়া প্রোগ্রাম লম্বা ট্রেসব্যাক দিয়ে ক্র্যাশ করে। `try` থাকলে পরিষ্কার বার্তা দিয়ে সিদ্ধান্ত নিতে পারেন কী করবেন।

`import json` আর `import pdb` এখানে আসলে ব্যবহার হয়নি (`response.json()` `requests`-এর নিজের, আর `pdb`-এর লাইনটা কমেন্ট করা)। এগুলো পাঠের সময়কার অবশিষ্ট।

> **সূক্ষ্ম সংশোধন: "Latest 5"।** `posts[:5]` নেয় **প্রথম** পাঁচটা (ID ১–৫), সর্বশেষ নয়। শেষ পাঁচটা চাইলে `posts[-5:]`।

### ৯.৪ লিস্ট স্লাইসিং, এক নজরে

| স্লাইস | ফলাফল | টাইপ |
|---|---|---|
| `posts[0]` | প্রথম আইটেম | একটা **dict** |
| `posts[-1]` | শেষ আইটেম | একটা **dict** |
| `posts[:5]` | প্রথম ৫টা | একটা **list** |
| `posts[-5:]` | শেষ ৫টা | একটা **list** |
| `posts[1:3]` | ইনডেক্স ১ ও ২ | একটা **list** |

### ৯.৫ ক্লাসের দুটো এরর, ব্যাখ্যাসহ

**এরর ১: লুপে `posts[-1]`।** শিক্ষক "শেষেরটা" পেতে `posts[:5]` বদলে `posts[-1]` দিয়েছিলেন, আর এলো:

```
Failed to fetch posts: string indices must be integers, not 'str'
```

**কারণ:** `posts[-1]` হলো **একটা dict**, লিস্ট নয়। dict-এর ওপর লুপ চালালে আসে তার **কী-গুলো** (`"userId"`, `"id"`, `"title"`, `"body"`), যেগুলো স্ট্রিং। ফলে `post` হয়ে গেল স্ট্রিং `"userId"`, আর `"userId"['id']`-এর কোনো মানে হয় না। **সমাধান:** স্লাইস দিন, `posts[-1:]` (এক আইটেমের লিস্ট), অথবা লুপই বাদ দিন:

```python
last = posts[-1]
print(f"ID {last['id']}: {last['title']}")
```

এরপর `posts[1:3]` দিলে কাজ করেছিল, ID ২ আর ৩ ছাপা হয়েছিল।

**এরর ২: বানান ভুল URL।** `try / except` দেখাতে URL বদলানো হয়েছিল (`typicode` → `typecode`):

```
Failed to fetch posts: HTTPSConnectionPool(host='jsonplaceholder.typecode.com', port=443):
Max retries exceeded with url: /posts (Caused by NameResolutionError(...))
```

হোস্ট ঠিক কিন্তু **পাথ** ভুল হলে আসে অন্য এরর, যা ধরে `raise_for_status()`:

```
Failed to fetch posts: 404 Client Error: Not Found for url: https://jsonplaceholder.typicode.com/postz
```

দুই ক্ষেত্রেই প্রোগ্রাম ক্র্যাশ করেনি; `except` ধরে পরিষ্কার বার্তা দিয়েছে।

---

## ১০. ওয়েব ডেভেলপমেন্টের বেসিক

### ১০.১ ওয়েবসাইট বনাম ওয়েব অ্যাপ বনাম মোবাইল অ্যাপ বনাম ডেস্কটপ অ্যাপ

| ধরন | কী | উদাহরণ |
|---|---|---|
| **ওয়েবসাইট (Website)** (স্ট্যাটিক) | শুধু *দেখার* পেজ; লগইন নেই, ইন্টারঅ্যাকশন নেই | পাশের মিষ্টির দোকানের তথ্যের পেজ |
| **ওয়েব অ্যাপ (Web App)** | ব্রাউজারে চলে; লগইন করে কাজ করা যায় | বঙ্গডেভ, ফেসবুক, আমাদের স্টুডেন্ট ম্যানেজমেন্ট সিস্টেম |
| **মোবাইল অ্যাপ (Mobile App)** | ফোনে ইনস্টল করা | অ্যান্ড্রয়েডে হোয়াটসঅ্যাপ |
| **ডেস্কটপ অ্যাপ (Desktop App)** | কম্পিউটারে ইনস্টল করা | মাইক্রোসফট টিমসের ডেস্কটপ সংস্করণ |

অনেক প্রোডাক্ট (টিমস, হোয়াটসঅ্যাপ) তিন রূপেই আসে। আজ আমরা বানাচ্ছি একটা **ওয়েব অ্যাপ**।

### ১০.২ ক্লায়েন্ট, সার্ভার, রিকোয়েস্ট, রেসপন্স

```
 ক্লায়েন্ট (ব্রাউজার)                         সার্ভার (যে কম্পিউটারে অ্যাপ চলে)
 ─────────────────                          ──────────────────────────────────
 URL লিখে এন্টার দিলেন ──রিকোয়েস্ট──►      অ্যাপ পাইথন কোড চালায়,
                                           ডাটাবেসকে জিজ্ঞেস করে,
 পেজটা দেখা গেল        ◄──রেসপন্স────      HTML বা JSON ফেরত পাঠায়
```

- **ক্লায়েন্ট (Client):** যে পাশ *চায়*, সাধারণত ব্রাউজার (ফ্রন্টএন্ড)।
- **সার্ভার (Server):** যে পাশ *উত্তর দেয়* (ব্যাকএন্ড)।
- **রিকোয়েস্ট (Request):** "এটা দাও / সেভ করো / বদলাও / মোছো।"
- **রেসপন্স (Response):** উত্তর (একটা পেজ, JSON ডাটা, বা একটা এরর কোড)।

### ১০.৩ ওয়েব সার্ভার

**ওয়েব সার্ভার (Web Server)** হলো এমন প্রোগ্রাম, যা ক্লায়েন্টের রিকোয়েস্ট নেয় আর রেসপন্স ফেরত দেয়।

| ওয়েব সার্ভার | সাধারণ ব্যবহার |
|---|---|
| **Flask ডেভেলপমেন্ট সার্ভার** | Flask-এর ভেতরেই থাকে; আজ এটাই চালাচ্ছি। **শুধু ডেভেলপমেন্টের জন্য।** |
| **Gunicorn** | Flask ও Django-র প্রোডাকশন সার্ভার (WSGI অ্যাপ) |
| **Uvicorn** | FastAPI ও অন্যান্য async (ASGI) অ্যাপের প্রোডাকশন সার্ভার |
| **Nginx** | জনপ্রিয় সামনের-দরজা সার্ভার: স্ট্যাটিক ফাইল দেয়, ট্রাফিক অ্যাপে পাঠায় |
| **Apache** | পুরনো, এখনো বহুল ব্যবহৃত সাধারণ ওয়েব সার্ভার |

পাইথন অ্যাপের একটা সাধারণ প্রোডাকশন সেটআপ:

```
ইন্টারনেট ──► Nginx (পোর্ট 80/443) ──► Gunicorn / Uvicorn ──► আপনার Flask / FastAPI কোড
```

**ক্লাসের প্রশ্নোত্তর:** *পাইথন অ্যাপ কি সরাসরি Nginx-এ চালানো যায়?* সরাসরি না। Flask চালাই Gunicorn দিয়ে (বা FastAPI চালাই Uvicorn দিয়ে), আর Nginx সামনে বসে **রিভার্স প্রক্সি (Reverse Proxy)** হিসেবে। জাভাস্ক্রিপ্ট আর PHP অ্যাপও সাধারণত Nginx দিয়ে সার্ভ করা হয়।

অ্যাপ চালু করলে Flask নিজেই এই সতর্কবার্তা দেয় (যাচাই করা):

```
WARNING: This is a development server. Do not use it in a production deployment.
Use a production WSGI server instead.
```

### ১০.৪ পাইথনের ওয়েব ফ্রেমওয়ার্ক

| ফ্রেমওয়ার্ক (Framework) | বৈশিষ্ট্য | কখন ভালো |
|---|---|---|
| **Flask** | হালকা "মাইক্রো-ফ্রেমওয়ার্ক"; যা দরকার শুধু সেটুকু যোগ করেন | ছোট থেকে মাঝারি অ্যাপ, দ্রুত টুল, মাইক্রোসার্ভিস |
| **Django** | "সব সাথে করে আনে": অ্যাডমিন প্যানেল, ORM, অথেনটিকেশন, টেমপ্লেট | বড় ফুল-স্ট্যাক অ্যাপ্লিকেশন |
| **FastAPI** | আধুনিক, দ্রুত, API-এর জন্য বানানো | ব্যাকএন্ড ও REST API |

**ক্লাসের উপমা:** Django হলো **কামান**, Flask হলো **মশার কয়েল**। মশা তাড়াতে কেউ কামান দাগায় না, কয়েল জ্বালায়। তবে Flask হালকা বলে খেলনা নয়: অনেক বড় প্রতিষ্ঠান নির্দিষ্ট কাজের সার্ভিসে এটা ব্যবহার করে।

### ১০.৫ ব্রাউজার যা বোঝে

ব্রাউজার শুধু বোঝে **HTML** (কাঠামো), **CSS** (সাজসজ্জা) আর **JavaScript** (আচরণ)। ক্লাসে একটা ছোট `index.html` (একটা `<h1>` আর একটা `<p>`) ব্রাউজারে টেনে ছেড়ে এটা দেখানো হয়েছিল। পেজটা দেখা যায়, কিন্তু এটা শুধু একটা **ওয়েব পেজ**, অ্যাপ নয়।

> **সূক্ষ্ম সংশোধন:** ক্লাসে বলা হয়েছে পাইথন কোড "ব্রাউজার জাভাস্ক্রিপ্টে কনভার্ট করে নেয়"। ব্যাপারটা এরকম নয়। **পাইথন কখনো ব্রাউজারে চলে না।** Flask পাইথন চালায় **সার্ভারে**, তা দিয়ে **HTML** তৈরি করে, আর সেই HTML ব্রাউজারে পাঠায়। ব্রাউজার পায় শুধু HTML, CSS, JavaScript বা JSON।

### ১০.৬ এক অনুচ্ছেদে SQLite

**SQLite** হলো **একটা ফাইলেই** (এখানে `students.db`) রাখা একটা পূর্ণাঙ্গ **রিলেশনাল (SQL) ডাটাবেস (Relational Database)**। আলাদা ডাটাবেস সার্ভার ইনস্টল লাগে না, পাইথনের `sqlite3` মডিউল সরাসরি এর সাথে কথা বলে। ডাটা থাকে **টেবিলে (Table)**, **রো (Row)** আর **কলাম (Column)** আকারে, ঠিক স্প্রেডশিটের মতো।

> **সূক্ষ্ম সংশোধন:** ক্লাসে SQLite-কে "সিকোয়েন্সিয়াল ডাটাবেস" বলা হয়েছে। সঠিক পরিভাষা **রিলেশনাল ডাটাবেস** (রো-কলামের টেবিল, SQL দিয়ে প্রশ্ন করা হয়)। তবে ক্লাসের স্প্রেডশিটের তুলনাটাই সঠিক মানসিক ছবি।

---

## ১১. Flask স্টুডেন্ট ম্যানেজমেন্ট অ্যাপ: পুরো কোড বিশ্লেষণ

### ১১.১ অ্যাপটা কী করে

- ব্রাউজারের ফর্ম থেকে স্টুডেন্ট যোগ করা
- সব স্টুডেন্টকে একটা টেবিলে দেখা
- যেকোনো স্টুডেন্ট এডিট বা ডিলিট করা
- সবকিছু স্থায়ীভাবে SQLite-এ (`students.db`) রাখা
- REST API দিয়ে সব স্টুডেন্ট বা একজন স্টুডেন্টকে **JSON** আকারে দেওয়া
- JSON API দিয়ে স্টুডেন্ট তৈরি করা

প্রতিটি স্টুডেন্টের থাকে: **ID, Name, Age, Email, CGPA, Course, Skills।**

### ১১.২ প্রজেক্টের কাঠামো ও আর্কিটেকচার

```text
13.flast-student-management-app/
├── app.py               ← কন্ট্রোলার: সব রাউট (ওয়েব পেজ + API)। এন্ট্রি পয়েন্ট।
├── database.py          ← SQLite খোলে, টেবিল বানায়
├── requirements.txt     ← Flask>=3.0,<4.0
├── README.md            ← সেটআপ ও চালানোর নির্দেশনা
├── students.db          ← প্রথমবার চালালে নিজে তৈরি হয়
├── models/
│   ├── __init__.py      ← খালি; "models"-কে ইমপোর্টযোগ্য প্যাকেজ বানায়
│   └── student.py       ← Student (ডাটা) + StudentRepository (ডাটাবেসের কাজ)
├── templates/           ← Jinja2 প্লেসহোল্ডারসহ HTML পেজ
│   ├── index.html
│   └── edit.html
└── static/
    └── style.css        ← CSS, যেমন আছে তেমনই পাঠানো হয়
```

```
ব্রাউজার / ফ্রন্টএন্ড
        │   HTML ফর্ম বা JSON রিকোয়েস্ট
        ▼
Flask রাউট (app.py)
        │   একটা Student অবজেক্ট বানায়
        ▼
Student ক্লাস / অবজেক্ট (models/student.py)
        │   রিপোজিটরির হাতে দেয়
        ▼
StudentRepository (models/student.py)
        │   SQL চালায়
        ▼
SQLite ডাটাবেস (students.db)
```

বাস্তবে **সেপারেশন অফ কনসার্নস**: রাউট SQL লেখে না, রিপোজিটরি HTML-এর কিছু জানে না, আর `Student` ক্লাস শুধু একজন স্টুডেন্টের বর্ণনা দেয়।

### ১১.৩ `requirements.txt` (পুরো ফাইল)

```text
Flask>=3.0,<4.0
```

প্রজেক্টের কোন প্যাকেজ লাগবে তার তালিকা। `>=3.0,<4.0` মানে "Flask-এর যেকোনো 3.x সংস্করণ": অন্তত 3.0, কিন্তু 4.0-এর নিচে। তালিকার সবকিছু এক কমান্ডে ইনস্টল হয়: `pip install -r requirements.txt`।

### ১১.৪ `database.py` (পুরো ফাইল)

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

| অংশ | অর্থ |
|---|---|
| বড় হাতের `DATABASE_NAME` | একটা **কনস্ট্যান্ট (Constant)**: যে মান বদলানোর কথা না (নামকরণের রীতি) |
| `sqlite3.connect("students.db")` | ডাটাবেস ফাইল খোলে; **না থাকলে তৈরি করে** |
| `row_factory = sqlite3.Row` | রো-কে কলামের নাম দিয়ে পড়া যায়: `row[1]`-এর বদলে `row["name"]` |
| `CREATE TABLE IF NOT EXISTS` | শুধু প্রথমবার টেবিল বানায়; প্রতিবার চালু হলেও নিরাপদ |
| `id INTEGER PRIMARY KEY AUTOINCREMENT` | অনন্য একটা id, যা ডাটাবেস নিজেই বসায়: ১, ২, ৩… |
| `NOT NULL` | এই কলাম খালি রাখা যাবে না |
| `connection.commit()` | পরিবর্তন **সেভ** করে, যেমন `git commit` একটা ভার্সন সেভ করে |
| `connection.close()` | কানেকশন ছেড়ে দেয় |

**পাইথন ↔ SQLite টাইপ:**

| পাইথন | SQLite |
|---|---|
| `str` | `TEXT` |
| `int` | `INTEGER` |
| `float` | `REAL` |

### ১১.৫ `models/__init__.py` (পুরো ফাইল)

```python
```

এটা **ইচ্ছা করেই খালি**। এর উপস্থিতি পাইথনকে জানায় "`models` ফোল্ডারটা একটা **প্যাকেজ (Package)**", ফলে `from models.student import Student` কাজ করে। প্যাকেজ মানে মডিউলের একটা ফোল্ডার।

### ১১.৬ `models/student.py` (পুরো ফাইল)

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

**`Student` ক্লাস** (**মডেল, Model**)। এটা ক্লাস ০২-এর কনস্ট্রাক্টরের সেই পরিচিত ধরন:
- `__init__` প্রতিটি মান `self`-এ রাখে।
- `student_id=None` একটা **ডিফল্ট প্যারামিটার (Default Parameter)**। নতুন স্টুডেন্টের এখনো id নেই, কারণ id দেয় ডাটাবেস।
- `to_dict()` অবজেক্টটাকে ডিকশনারি বানায়, যেটাকে Flask JSON-এ রূপান্তর করতে পারে।

**`StudentRepository` ক্লাস** সব SQL ধরে রাখে, প্রতিটি CRUD কাজের জন্য একটা মেথড:

| মেথড | CRUD | যে SQL চলে | ফেরত দেয় |
|---|---|---|---|
| `create(student)` | Create | `INSERT INTO ... VALUES (?, ...)` | নতুন id (`cursor.lastrowid`) |
| `get_all()` | Read | `SELECT * ... ORDER BY id DESC` | `Student` অবজেক্টের list (নতুনটা আগে) |
| `get_by_id(id)` | Read | `SELECT * ... WHERE id = ?` | একটা `Student`, অথবা `None` |
| `update(student)` | Update | `UPDATE ... SET ... WHERE id = ?` | কিছু না |
| `delete(id)` | Delete | `DELETE ... WHERE id = ?` | কিছু না |

ডাটাবেস থেকে রো আসে, আর সেগুলো `Student` অবজেক্টে রূপান্তরিত হয়। অনুচ্ছেদ ৭-এর সেই dict → অবজেক্ট সেতুটাই।

**`@staticmethod` কী?** `@` দিয়ে শুরু লাইনটা একটা **ডেকোরেটর (Decorator)**: নিচের ফাংশনটাকে পরিবর্তন করে। স্ট্যাটিক মেথড (Static Method) **ক্লাসের অংশ, কিন্তু `self` পায় না**, কারণ কোনো নির্দিষ্ট অবজেক্টের ডাটা এর দরকার নেই। তাই একে সরাসরি ক্লাসের নাম দিয়ে ডাকা যায়: `StudentRepository.get_all()`, কোনো `StudentRepository()` অবজেক্ট লাগে না।

> **সূক্ষ্ম সংশোধন:** ক্লাসে স্ট্যাটিক মেথডকে "প্রটেক্টেড" বলা হয়েছে। এগুলো কোনোভাবেই protected বা private নয়। "স্ট্যাটিক" মানে শুধু এটুকু: **`self` (এবং `cls`) পাঠানো হয় না**।

**`?` চিহ্নগুলো কেন?** এগুলো **প্লেসহোল্ডার (Placeholder)**। মানগুলো আলাদা একটা টাপল (Tuple)-এ পাঠানো হয়, আর SQLite **নিরাপদে** বসিয়ে দেয়। কখনো f-স্ট্রিং দিয়ে SQL বানাবেন না, যেমন `f"... WHERE id = {student_id}"`। এতে **SQL ইনজেকশন (SQL Injection)** সম্ভব হয়: খারাপ উদ্দেশ্যের ইনপুট আপনার কোয়েরি বদলে ফেলতে পারে (যেমন পুরো টেবিল মুছে দিতে পারে)। প্লেসহোল্ডার এটা ঠেকায়।

`(student_id,)`-এ শেষে কমা আছে, কারণ **এক আইটেমের টাপলে** কমা লাগে। কমা ছাড়া `(student_id)` শুধু ব্র্যাকেটের ভেতরের একটা সংখ্যা।

### ১১.৭ `app.py` (পুরো ফাইল)

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

**মূল ধারণাগুলো, ওপর থেকে নিচে:**

| কোড | অর্থ |
|---|---|
| `from flask import Flask, jsonify, ...` | Flask প্যাকেজ থেকে নেমড ইমপোর্ট (অনুচ্ছেদ ৩.৫) |
| `from models.student import ...` | `models/student.py` থেকে ইমপোর্ট: প্যাকেজ `models`, মডিউল `student` |
| `app = Flask(__name__)` | অ্যাপ তৈরি। `__name__` (একটা "ডান্ডার" ভেরিয়েবল, মানে ডাবল আন্ডারস্কোর) Flask-কে জানায় অ্যাপ কোথায় আছে, যাতে সে `templates/` আর `static/` খুঁজে পায় |
| `init_db()` | চালু হওয়ার সময় দরকার হলে টেবিল বানায় |
| `@app.route("/")` | একটা **ডেকোরেটর**, যা একটা **URL পাথ** (রাউট) নিচের ফাংশনের সাথে জোড়ে |
| `methods=["POST"]` | এই রাউট কোন HTTP মেথড নেবে (ডিফল্ট: শুধু `GET`) |
| `<int:student_id>` | **URL ভেরিয়েবল**: `/students/5/edit` ফাংশনটাকে ডাকে `student_id=5` দিয়ে (`int` হিসেবে) |
| `request.form["name"]` | জমা দেওয়া HTML ফর্মের একটা মান, **সবসময় স্ট্রিং**, তাই `int(...)` ও `float(...)` (ক্লাস ০১-এর টাইপ কাস্টিং) |
| `request.get_json()` | API রিকোয়েস্টের JSON বডি, dict আকারে |
| `render_template("index.html", students=students)` | ডাটা দিয়ে HTML টেমপ্লেট পূরণ করে ফেরত দেয় |
| `redirect(url_for("index"))` | ব্রাউজারকে হোম পেজে পাঠায় (`302`) |
| `jsonify(...)` | dict/list-কে JSON রেসপন্স বানায় |
| `return ..., 404` | নির্দিষ্ট স্ট্যাটাস কোড **সহ** রেসপন্স ফেরত দেয় |
| `if __name__ == "__main__": app.run(debug=True)` | **এন্ট্রি পয়েন্ট**: ফাইলটা সরাসরি চালালে তবেই সার্ভার চালু হয় |

**রাউট (Route) কী?** **রাউট** হলো সাইটের একটা পথ। `bongodev.com/` হোম পেজ; `/success` বা `/contact` আরেকটা রাউট, আরেকটা পেজ। প্রতিটি `@app.route` এদের একটাকে সামলায়।

**অ্যাপের সব রাউট:**

| মেথড | রাউট | ফাংশন | কী করে | রেসপন্স |
|---|---|---|---|---|
| `GET` | `/` | `index` | হোম পেজ: ফর্ম + স্টুডেন্টের টেবিল | HTML |
| `POST` | `/students` | `create_student` | ফর্ম থেকে নতুন স্টুডেন্ট সেভ | `302` → `/` |
| `GET` | `/students/<id>/edit` | `edit_student_page` | এডিট ফর্ম দেখায় | HTML বা `404` |
| `POST` | `/students/<id>/edit` | `update_student` | এডিট করা স্টুডেন্ট সেভ | `302` → `/` |
| `POST` | `/students/<id>/delete` | `delete_student` | স্টুডেন্ট ডিলিট | `302` → `/` |
| `GET` | `/api/students` | `api_get_students` | সব স্টুডেন্ট JSON-এ | `200` |
| `GET` | `/api/students/<id>` | `api_get_student` | একজন স্টুডেন্ট JSON-এ | `200` বা `404` |
| `POST` | `/api/students` | `api_create_student` | JSON থেকে স্টুডেন্ট তৈরি | `201` বা `400` |

**`if __name__ == "__main__":`** প্রতিটি মডিউলের একটা বিল্ট-ইন ভেরিয়েবল আছে `__name__`। কোনো ফাইল সরাসরি **চালালে** (`python3 app.py`) তার `__name__` হয় `"__main__"`। অন্য ফাইল যখন সেটা **ইমপোর্ট** করে, তখন `__name__` হয় মডিউলের নাম। তাই এই লাইনের মানে: "ফাইলটা সরাসরি চালানো হলে তবেই সার্ভার চালু করো, ইমপোর্ট হলে নয়।" এটাই প্রোগ্রামের **এন্ট্রি পয়েন্ট (Entry Point)**।

> **`debug=True`** ফাইল সেভ করলেই অটো-রিলোড আর ব্রাউজারে বিস্তারিত এরর পেজ দেয়। শেখার জন্য দারুণ, **প্রোডাকশনে বিপজ্জনক**: ডিবাগ পেজ দিয়ে ভিজিটর আপনার সার্ভারে কোড চালাতে পারে। প্রোডাকশনে সবসময় এটা বন্ধ করে Gunicorn ব্যবহার করুন।

### ১১.৮ টেমপ্লেট ও Jinja2

Flask HTML ফাইল পূরণ করে **Jinja2** দিয়ে, যা একটা টেমপ্লেট ভাষা (Template Language)। ভাবুন **HTML-এর জন্য f-স্ট্রিং**:

| Jinja2 লেখা | অর্থ | পাইথনে সমতুল্য |
|---|---|---|
| `{{ student.name }}` | একটা মান বসাও | `f"{student.name}"` |
| `{% for student in students %} ... {% endfor %}` | লুপ | `for student in students:` |
| `{% if students %} ... {% else %} ... {% endif %}` | কন্ডিশন | `if students: ... else: ...` |
| `{{ url_for('static', filename='style.css') }}` | স্ট্যাটিক ফাইলের URL বানাও | — |

`templates/`-এ থাকে Jinja2 প্লেসহোল্ডারসহ HTML। `static/`-এ থাকে **যেমন আছে তেমনই** পাঠানো ফাইল (CSS, ছবি, JavaScript)।

<details>
<summary><b><code>templates/index.html</code> দেখুন (পুরো ফাইল)</b></summary>

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

**ফর্ম কীভাবে পাইথনের সাথে জোড়া লাগে:** `<form action="/students" method="POST">` ফর্মটা পাঠায় `create_student` রাউটে। প্রতিটি `<input name="age">` পাইথনে হয়ে যায় `request.form["age"]`। **`name` অ্যাট্রিবিউটটাই ডিকশনারির কী।**

<details>
<summary><b><code>templates/edit.html</code> দেখুন (পুরো ফাইল)</b></summary>

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

`value="{{ student.age }}"` প্রতিটি বক্সে বর্তমান মানটা আগে থেকে বসিয়ে দেয়, তাই শুধু ভুলটুকু বদলালেই হয়।

<details>
<summary><b><code>static/style.css</code> দেখুন (পুরো ফাইল)</b></summary>

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

### ১১.৯ README-তে আরো যা আছে

প্রজেক্টের `README.md`-তে আছে সেটআপের ধাপ ([অনুচ্ছেদ ১২](#১২-ভার্চুয়াল-এনভায়রনমেন্ট-ও-অ্যাপ-চালানো)-এ আছে), আর্কিটেকচার ডায়াগ্রাম (অনুচ্ছেদ ১১.২), API-এর উদাহরণ (অনুচ্ছেদ ১২.৪) আর ছয়টা অনুশীলনী ([অনুচ্ছেদ ১৮](#১৮-অনুশীলনী))। এর শেখার লক্ষ্য হলো আপনি যেন এই পুরো প্রবাহটা ব্যাখ্যা করতে পারেন:

```text
HTML ফর্ম → HTTP রিকোয়েস্ট → Flask রাউট → পাইথন ডিকশনারি → Student ক্লাস → Student অবজেক্ট
→ SQLite → Student অবজেক্ট → ডিকশনারি → JSON / HTML
```

---

## ১২. ভার্চুয়াল এনভায়রনমেন্ট ও অ্যাপ চালানো

### ১২.১ ভার্চুয়াল এনভায়রনমেন্ট কেন?

দুজন ক্লায়েন্টের কথা ভাবুন:
- ক্লায়েন্ট A-এর অ্যাপে লাগবে **Flask 3.x**।
- ক্লায়েন্ট B-এর পুরনো অ্যাপে লাগবে **Flask 2.x**।

কম্পিউটারে সাধারণত পাইথন প্যাকেজের একটাই বৈশ্বিক সেট থাকে, তাই দুটো ভার্সন একসাথে ইনস্টল করা যায় না। **ভার্চুয়াল এনভায়রনমেন্ট (Virtual Environment, venv)** হলো **একটা প্রজেক্টের জন্য আলাদা, ব্যক্তিগত প্যাকেজ ফোল্ডার**। প্রতিটি প্রজেক্ট পায় নিজের "দুনিয়া", ভার্সনে কখনো সংঘাত হয় না।

```
আপনার কম্পিউটার
├── project-A/venv  → Flask 3.1
└── project-B/venv  → Flask 2.3
```

### ১২.২ কমান্ডগুলো

| ধাপ | লিনাক্স / ম্যাক | উইন্ডোজ (cmd / PowerShell) |
|---|---|---|
| তৈরি | `python3 -m venv venv` | `python -m venv venv` |
| চালু (Activate) | `source venv/bin/activate` | `venv\Scripts\activate` |
| প্যাকেজ ইনস্টল | `pip install -r requirements.txt` | `pip install -r requirements.txt` |
| অ্যাপ চালানো | `python3 app.py` | `python app.py` |
| অ্যাপ থামানো | `Ctrl + C` | `Ctrl + C` |
| venv থেকে বের হওয়া | `deactivate` | `deactivate` |

- `python3 -m venv venv`-এ `-m venv` পাইথনের বিল্ট-ইন `venv` মডিউল চালায়। শেষের `venv` হলো **ফোল্ডারের নাম**, যা খুশি দিতে পারেন (ক্লাসে: `noor-venv`)।
- চালু করার পর প্রম্পটের শুরুতে ব্র্যাকেটে নামটা দেখায়, যেমন `(noor-venv) $`। এটা দেখেই বোঝেন আপনি ভেতরে আছেন।
- উইন্ডোজের **Git Bash**-এ চালু করুন `source venv/Scripts/activate` দিয়ে।
- **ক্লাসের ভুল:** শিক্ষক প্রথমে লিখেছিলেন `source .venv/bin/activate` (সামনে ডট দিয়ে), আর কাজ করেনি। তিনি যে ফোল্ডার বানিয়েছিলেন তার নাম ছিল `noor-venv`। **পাথে অবশ্যই সেই নামটাই দিতে হবে, যে নামে তৈরি করেছেন।**
- venv ফোল্ডার কখনো গিটে কমিট করবেন না। এটা `.gitignore`-এ যোগ করুন, কারণ `requirements.txt` থেকেই আবার বানানো যায়।

### ১২.৩ চালান

```bash
cd 13.flast-student-management-app
python3 -m venv venv
source venv/bin/activate          # উইন্ডোজ: venv\Scripts\activate
pip install -r requirements.txt
python3 app.py
```

আসল আউটপুট:
```
 * Serving Flask app 'app'
 * Debug mode: on
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on http://127.0.0.1:5000
Press CTRL+C to quit
 * Restarting with stat
 * Debugger is active!
```

ব্রাউজারে খুলুন **http://127.0.0.1:5000**। `127.0.0.1` মানে "এই কম্পিউটারটাই" (একে `localhost`-ও বলে), আর `5000` হলো Flask-এর ডিফল্ট **পোর্ট (Port)**।

### ১২.৪ টার্মিনাল থেকে `curl` দিয়ে API-এর সাথে কথা

`curl` HTTP রিকোয়েস্ট পাঠানোর একটা জনপ্রিয় কমান্ড-লাইন টুল। এক টার্মিনালে অ্যাপ চালু রেখে আরেকটা টার্মিনাল ট্যাব খুলুন। নিচের সব আউটপুট আসল:

```bash
# সব পড়া (শুরুতে খালি)
curl http://127.0.0.1:5000/api/students
```
```json
[]
```

```bash
# API দিয়ে তৈরি
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
স্ট্যাটাস: **201 Created**। (id ২, কারণ এর আগে ফর্ম দিয়ে একজন স্টুডেন্ট যোগ করা হয়েছিল।)

```bash
# দরকারি ফিল্ড নেই
curl -X POST http://127.0.0.1:5000/api/students -H "Content-Type: application/json" -d '{"name": "X"}'
```
```json
{
  "error": "Missing required student information"
}
```
স্ট্যাটাস: **400 Bad Request**।

```bash
# এমন স্টুডেন্ট যে নেই
curl http://127.0.0.1:5000/api/students/99
```
```json
{
  "error": "Student not found"
}
```
স্ট্যাটাস: **404 Not Found**।

| `curl` ফ্ল্যাগ | অর্থ |
|---|---|
| `-X POST` | এই HTTP মেথড ব্যবহার করো (ডিফল্ট GET) |
| `-H "Content-Type: application/json"` | একটা হেডার যোগ করো: "যে বডি পাঠাচ্ছি তা JSON" |
| `-d '...'` | রিকোয়েস্টের বডি (ডাটা) |

খেয়াল করুন, Flask JSON-এর কী-গুলো **বর্ণানুক্রমে সাজিয়ে** দেয় (`age`, `cgpa`, `course`…)। JSON-এ কী-এর ক্রমে কিছু যায় আসে না। `curl`-এর গ্রাফিক্যাল বিকল্প হলো **Postman**।

### ১২.৫ ডেভঅপস কোথায় আসে

ডেভেলপাররা অ্যাপ বানিয়ে হাতে দেন `README.md`, `requirements.txt`, পোর্ট, আর চালু করার নিয়ম। **ডেভঅপস ইঞ্জিনিয়াররা** এগুলো দিয়ে টেস্ট এনভায়রনমেন্ট, **CI/CD পাইপলাইন**, কন্টেইনার আর প্রোডাকশন সার্ভার (Gunicorn + Nginx) সেটআপ করেন। প্রতিটি অ্যাপ নিজে লিখতে হবে না, কিন্তু **কীভাবে চলে তা অবশ্যই বুঝতে হবে**। এজন্যই আমরা একটা অ্যাপ শুরু থেকে শেষ পর্যন্ত বানিয়েছি।

---

## ১৩. CRUD বাস্তবে: একটা রিকোয়েস্টের পুরো যাত্রা

ক্লাসে ব্রাউজার দিয়ে স্টুডেন্ট যোগ করা হয়েছিল (হ্যারিসন, তারপর ফারহান তানভীর), এডিট ও ডিলিটও করা হয়েছিল। প্রতিটি ক্লিকের পেছনে যা ঘটে:

### ১৩.১ Create — "Save Student"

```
১. ব্রাউজার    ফর্ম পূরণ করে "Save Student" ক্লিক
২. HTML        <form action="/students" method="POST"> ফিল্ডগুলো পাঠায়
৩. Flask       @app.route("/students", methods=["POST"]) → create_student()
৪. পাইথন       request.form["age"] হলো "24" (str) → int("24") → 24
৫. অবজেক্ট     student = Student(name=..., age=24, ...)
৬. রিপোজিটরি   StudentRepository.create(student) → INSERT INTO students ... ; commit
৭. ডাটাবেস     SQLite রো-টাকে id দেয় ১, ২, ৩...
৮. রেসপন্স     redirect(url_for("index")) → 302 → ব্রাউজার "/" চায়
৯. পড়া        index() → get_all() → render_template → টেবিলে নতুন স্টুডেন্ট
```

### ১৩.২ Update — "Edit" (হ্যারিসনের বয়স ২৪ → ২৭, CGPA ২.৭ → ৩.৯)

`GET /students/2/edit` আগে-পূরণ-করা ফর্ম দেখায়, তারপর `POST /students/2/edit` → `update_student(2)` → `UPDATE students SET ... WHERE id = 2`। যাচাই করা ফলাফল:

```json
{"age": 27, "cgpa": 3.9, "course": "DevOps", "email": "harrison@example.com", "id": 2, "name": "Harrison", "skills": "Ansible, JavaScript"}
```

### ১৩.৩ Delete — "Delete" ("নূর ভাই, সানডে-মানডে ক্লোজ!")

`POST /students/1/delete` → `delete_student(1)` → `DELETE FROM students WHERE id = 1`। স্টুডেন্ট ১ টেবিল থেকে আর `/api/students` থেকে উধাও।

### ১৩.৪ এই অ্যাপ আপডেট-ডিলিটে PUT আর DELETE না দিয়ে POST দিল কেন?

**HTML ফর্ম শুধু `GET` বা `POST` পাঠাতে পারে।** তাই ব্রাউজারের পেজগুলো এডিট ও ডিলিটে `POST` ব্যবহার করে। আসল REST API সাধারণত দেয়:

```
PUT    /api/students/<id>    ← হালনাগাদ
PATCH  /api/students/<id>    ← আংশিক হালনাগাদ
DELETE /api/students/<id>    ← মুছে ফেলা (README-র অনুশীলনী ৫, সমাধান অনুচ্ছেদ ১৮-এ)
```

এই অ্যাপে `PUT` পাঠালে ফেরত আসে **405 Method Not Allowed** (যাচাই করা), কারণ কোনো রাউট এটা নেয় না।

> **সূক্ষ্ম সংশোধন: POST বনাম PUT বনাম PATCH।** ক্লাসে বলা হয়েছে: "POST পুরো অবজেক্ট রি-রাইট করে, PUT শুধু যেটুকু বদলান সেটুকু আপডেট করে, আর PATCH-ও আছে।" প্রচলিত অর্থ হলো:
> - **POST** → নতুন কিছু **তৈরি** (বা কোনো কাজ চালু)।
> - **PUT** → আগের জিনিসটাকে পুরো নতুন রূপ দিয়ে **প্রতিস্থাপন**।
> - **PATCH** → আগের জিনিসের **আংশিক হালনাগাদ** (শুধু পাঠানো ফিল্ডগুলো)।
>
> শিক্ষকের মূল কথা ঠিক: আপডেটে আদর্শভাবে `POST` নয়, `PUT`/`PATCH` হওয়া উচিত।

### ১৩.৫ ভুল ইনপুট দিলে কী হয়?

বয়সের বক্সে `abc` লিখলে (ব্রাউজারের নম্বর-বক্স এড়িয়ে, যেমন curl দিয়ে, এটা সম্ভব) `int("abc")` তোলে `ValueError`। Flask ফেরত দেয় **500 Internal Server Error** (যাচাই করা)। প্রোডাকশন কোড ইনপুট যাচাই (Validation) করে সহায়ক বার্তাসহ `400` দিত। `try / except`-এর এটা একটা ভালো ব্যবহার।

---

## ১৪. `pdb` দিয়ে ডিবাগিং

### ১৪.১ পুরো ফাইল: `14.pdb-set-trace-debugger.py`

```python
import pdb

def calculate_server_ratio(servers, users):
    # The script stops here!
    pdb.set_trace() 
    
    ratio = servers / users
    return ratio

print(calculate_server_ratio(10, 0))
```

**`pdb`** হলো পাইথনের বিল্ট-ইন **ডিবাগার (Debugger)** (স্ট্যান্ডার্ড লাইব্রেরি থেকে)। `pdb.set_trace()` একটা **ব্রেকপয়েন্ট (Breakpoint)**: প্রোগ্রাম এই লাইনে পৌঁছালে **থেমে যায়** আর একটা `(Pdb)` প্রম্পট দেয়। তখন ভেরিয়েবল দেখতে পারেন, পরের লাইনগুলো একটা একটা করে চালাতে পারেন।

কেন? বড় প্রোগ্রাম বিভ্রান্তিকরভাবে ব্যর্থ হয়। ট্রেসব্যাক জানায় *কোথায়* ক্র্যাশ হলো, কিন্তু সবসময় *কেন* হলো তা নয়। সমস্যার লাইনের ঠিক আগে থামিয়ে চারপাশটা দেখে নেওয়া যায়।

### ১৪.২ আগে ট্রেসব্যাক পড়া

ক্লাসে শিক্ষক প্রথমে একটা বানান ভুল ঢুকিয়েছিলেন (`ratio` বনাম ভুল বানানের একটা নাম), স্ক্রিপ্ট চালিয়ে **ট্রেসব্যাক (Traceback)** পড়েছিলেন, যা পাইথনের এরর রিপোর্ট। এটা পড়ুন **নিচ থেকে ওপরে**: শেষ লাইনে থাকে এররের ধরন ও বার্তা, ওপরের লাইনগুলোতে থাকে ফাইল আর কোন **লাইন নম্বরে** ঘটেছে। তারপর তিনি ঠিক ওই লাইনের আগে ব্রেকপয়েন্ট বসিয়েছিলেন।

### ১৪.৩ একটা আসল সেশন (`10, 0` দিয়ে)

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

ভাগের *আগেই* `users = 0` দেখে ক্র্যাশের কারণ বোঝা যায়। **সমাধান:** শূন্য ছাড়া অন্য সংখ্যা দিন, অথবা আগে `if users == 0:` যাচাই করুন।

### ১৪.৪ `10, 4` দিয়ে সেশন (ক্লাসের ডেমো)

```
(Pdb) l                        ← বর্তমান লাইনের আশেপাশের কোড দেখাও
  5  ->	    pdb.set_trace()
  7  	    ratio = servers / users
  8  	    return ratio
 10  	print(calculate_server_ratio(10, 4))
(Pdb) n                        ← এক লাইন চালাও
-> ratio = servers / users
(Pdb) p ratio
*** NameError: name 'ratio' is not defined     ← এখনো তৈরিই হয়নি: লাইন ৭ চলেনি
(Pdb) n
-> return ratio
(Pdb) c                        ← শেষ পর্যন্ত চালাও
2.5
```

### ১৪.৫ pdb কমান্ড

| কমান্ড | নাম | কী করে |
|---|---|---|
| `n` | next (পরের লাইন) | বর্তমান লাইন চালিয়ে পরের লাইনে থামে |
| `s` | step (ভেতরে ঢোকা) | `n`-এর মতো, কিন্তু ফাংশন কলের **ভেতরে ঢোকে** |
| `c` | continue (চালিয়ে যাও) | পরের ব্রেকপয়েন্ট বা শেষ পর্যন্ত চালায় |
| `p x` | print (দেখাও) | `x`-এর মান দেখায় |
| `l` | list (তালিকা) | বর্তমান লাইনের আশেপাশের কোড দেখায় |
| `q` | quit (বের হও) | প্রোগ্রাম থামিয়ে ডিবাগার থেকে বের হয় |
| `h` | help (সাহায্য) | সব কমান্ডের তালিকা |

ক্লাসের মতো `Ctrl + C` দিয়েও বের হওয়া যায়।

**আধুনিক শর্টকাট:** Python 3.7 থেকে বিল্ট-ইন **`breakpoint()`** ঠিক `import pdb; pdb.set_trace()`-এর কাজই করে, ইমপোর্ট লাগে না। ভিএস কোডেও একটা গ্রাফিক্যাল ডিবাগার আছে (লাইন নম্বরের বামে ক্লিক করলে লাল ফোঁটার ব্রেকপয়েন্ট বসে)। শিক্ষক তবুও `pdb.set_trace()` বেশি পছন্দ করেন।

⚠️ **কোড কমিটের আগে ব্রেকপয়েন্ট মুছে ফেলুন।** ভুলে থেকে যাওয়া `set_trace()` একটা সার্ভার বা CI পাইপলাইনকে আটকে দেবে, কারণ সে এমন ইনপুটের অপেক্ষায় থাকবে যা কখনো আসবে না।

---

## ১৫. ক্লাসে যে প্রশ্নগুলো এসেছিল

**প্রশ্ন: পাথ ছাড়া `import modules` কাজ করল কীভাবে?**
দুটো ফাইল একই ফোল্ডারে, আর পাইথন সবসময় আগে চলমান স্ক্রিপ্টের ফোল্ডারেই খোঁজে (`sys.path`)। দেখুন [অনুচ্ছেদ ৩.৬](#৩৬-পাথ-ছাড়া-মডিউল-পেল-কীভাবে)।

**প্রশ্ন: `for student in students`-এ `student` কি শুধু একটা ভেরিয়েবল?**
হ্যাঁ। এটা একটা **লুপ ভেরিয়েবল**। প্রতি চক্রে এতে থাকে পরের আইটেম: প্রথমে `students[0]`, তারপর `students[1]`, এভাবে। নাম যা খুশি দেওয়া যায়; `student` শুধু পড়তে সুন্দর লাগে।

**প্রশ্ন: আপনি বললেন পড়ার পরেই ফাইল বন্ধ হয়। তাহলে লেখা হলো কীভাবে?**
দুটো আলাদা `with` ব্লক: পড়ে বন্ধ, মেমোরিতে ডাটা বদলানো, তারপর `"w"` মোডে আবার খুলে লেখা ও বন্ধ। দেখুন [অনুচ্ছেদ ৭.৪](#৭৪-ফাইল-কখন-বন্ধ-হয়-ক্লাসের-প্রশ্ন)।

**প্রশ্ন: API-এর ধারণা পরিষ্কার, কিন্তু এই অ্যাপে নির্দিষ্টভাবে কীভাবে কাজ করছে?** *(মুস্তাফিজুর ভাই)*
সেটাই [অনুচ্ছেদ ১১–১৩](#১১-flask-স্টুডেন্ট-ম্যানেজমেন্ট-অ্যাপ-পুরো-কোড-বিশ্লেষণ): প্রতিটি রাউট একটা API এন্ডপয়েন্ট, ব্রাউজারের ফর্ম ডাকে `POST /students`, আর `/api/students` ফেরত দেয় JSON।

**প্রশ্ন: পাইথন অ্যাপ কি সরাসরি Nginx দিয়ে সার্ভ করা যায়?**
সরাসরি না। পাইথন অ্যাপ চলে Gunicorn (Flask/Django) বা Uvicorn (FastAPI)-এর অধীনে, আর Nginx সামনে রিভার্স প্রক্সি হিসেবে বসে।

**প্রশ্ন: অবজেক্ট (OOP) কি আরো বিস্তারিত পড়ানো হবে?**
হ্যাঁ। পুরোদমে OOP পড়াতে ৩–৪টা ক্লাস লাগে, আর প্রজেক্টের কাজে এটা আবার আসবে।

**প্রশ্ন: ডেভঅপস ইন্টারভিউতে কি এত পাইথন লাগে?**
ডেভঅপস রোলের জন্য এই ধারণাগুলো বুঝে মিলিয়ে নিতে পারলেই যথেষ্ট। পাইথন ইঞ্জিনিয়ার রোলে আরো গভীরে যেতে হবে। সব মুখস্থ লাগবে না, কিন্তু বেসিক জিনিসগুলো লাগবেই।

---

## ১৬. কোর্স রিপোর বাগ ও সতর্কতা

| # | ফাইল | সমস্যা | প্রভাব | সমাধান |
|---|---|---|---|---|
| ১ | `11.JSON-Data/app.py` | `else` ভেতরের স্কিল-লুপের ভেতরে | প্রতিটি না-মেলা স্কিলে "No students…" ছাপে, সার্চ সফল হলেও (Linux-এ ২৬ বার) | আগে মিল জমাও, লুপ শেষে প্রিন্ট ([৬.৮](#৬৮-ধাপ-৬--স্কিল-দিয়ে-সার্চ-ইউজার-ইনপুট-ও-কেস-সামলানো)) |
| ২ | `11.JSON-Data/inser-data-in-json.py` | ডুপ্লিকেট যাচাই নেই | প্রতিবার চালালে রোমান আবার যোগ হয় | আগে id যাচাই ([৭.৫](#৭৫-দুবার-চালালে-রোমান-দুবার-আসে)) |
| ৩ | `11.JSON-Data/*.py` | রিলেটিভ পাথ `"students.json"` | ফোল্ডারের ভেতর থেকে না চালালে `FileNotFoundError` | আগে `cd 11.JSON-Data` |
| ৪ | `11.JSON-Data/json-api.py` | `requests` লাগে; হেডিংয়ে "Latest 5" | ইনস্টল না থাকলে `ModuleNotFoundError`; সর্বশেষ নয়, প্রথম ৫টা দেখায় | `pip install requests`; সর্বশেষের জন্য `posts[-5:]` |
| ৫ | `12.modules/modules.py` | এররে `divide()`/`remainder()` স্ট্রিং ফেরত দেয় | ফেরত মানের টাইপ মিশ্র | এক্সেপশন তোলা |
| ৬ | `12.modules/app.py` | রিপোতে খালি | চালানোর কিছু নেই | [৩.৪](#৩৪-ব্যবহার-12modulesapppy)-এর সংস্করণ ব্যবহার করুন |
| ৭ | `13.flast-student-management-app` | `debug=True`; ইনপুট যাচাই নেই | প্রোডাকশনে অনিরাপদ; বয়সে `abc` দিলে 500 | debug বন্ধ; যাচাই করে 400 |
| ৮ | `13.flast-student-management-app` | ফোল্ডারের নামে "flast" বানান ভুল; `students.db` ও `__pycache__/` কমিট করা | নাম বিভ্রান্তিকর; কমিট করা নমুনা DB-তে একটা আসল ইমেইল | নাম ঠিক করুন; `*.db` ও `__pycache__/` `.gitignore`-এ দিন |
| ৯ | `14.pdb-set-trace-debugger.py` | `calculate_server_ratio(10, 0)` ডাকে | `ZeroDivisionError`, **ইচ্ছা করেই**, ডিবাগিং প্র্যাকটিসের জন্য | শূন্য ছাড়া সংখ্যা দিন, বা `users == 0` যাচাই করুন |

---

## ১৭. প্রচলিত ভুল ধারণা ও সংশোধন

| ❌ ভুল ধারণা | ✅ বাস্তবতা |
|---|---|
| "মডিউল পাইথনের বিশেষ কোনো ফিচার।" | মডিউল **শুধু একটা `.py` ফাইল**। |
| "`from x import y` শুধু `y` লোড করে, মেমোরি বাঁচে।" | দুই ক্ষেত্রেই পুরো মডিউল চলে ও লোড হয়। শুধু বদলায় কোন নামগুলো সরাসরি ব্যবহার করা যাবে। |
| "`import modules as math` ভালো অ্যালিয়াস।" | এটা পাইথনের আসল `math` মডিউলকে ঢেকে দেয়। অনন্য অ্যালিয়াস দিন। |
| "JSON আর পাইথনের ডিকশনারি একই জিনিস।" | JSON হলো **টেক্সট**: শুধু ডাবল কোটেশন, `true`/`false`/`null`। পাইথনের dict-এ সিঙ্গেল কোটেশন চলে, আর থাকে `True`/`False`/`None`। |
| "`print(data)` বৈধ JSON দেয়।" | এটা ছাপে একটা **পাইথন dict**। আসল JSON-এর জন্য `json.dumps(data, indent=4)`। |
| "`json.load` আর `json.loads` একই।" | `load` পড়ে **ফাইল**; `loads` পড়ে **স্ট্রিং**। `dump`/`dumps`-ও তাই। |
| "`append()` শুরুতে যোগ করে।" | `append()` সবসময় **শেষে** যোগ করে। |
| "`append`-এর পরেই ফাইল হালনাগাদ হয়ে যায়।" | শুধু মেমোরি বদলায়। আবার ফাইলে লিখতে হবে (`"w"` + `json.dump`)। |
| "`"w"` দিয়ে খুললে ফাইলে যোগ হয়।" | `"w"` আগে ফাইলটা **মুছে ফেলে**। টেক্সট যোগ করতে `"a"`। |
| "`posts[-1]` লুপ চালানোর মতো একটা লিস্ট দেয়।" | এটা দেয় **একটা আইটেম** (dict)। এক আইটেমের লিস্টের জন্য `posts[-1:]`। |
| "`requests` পাইথনের সাথে আসে।" | এটা **থার্ড-পার্টি** প্যাকেজ। `pip install requests`। |
| "JSONPlaceholder শুধু GET দেয়।" | POST/PUT/DELETE নেয়, কিন্তু কিছুই **সেভ করে না**। |
| "ব্রাউজার পাইথনকে জাভাস্ক্রিপ্টে রূপান্তর করে।" | পাইথন চলে **সার্ভারে**। ব্রাউজার পায় শুধু HTML/CSS/JS/JSON। |
| "SQLite একটা সিকোয়েন্সিয়াল ডাটাবেস।" | এটা একটা ফাইলে রাখা **রিলেশনাল** (SQL) ডাটাবেস। |
| "`@staticmethod` মেথডকে protected বানায়।" | এর মানে শুধু মেথডটা **`self` পায় না**। কিছুই protected হয় না। |
| "POST পুরো অবজেক্ট আপডেট করে, PUT একটা ফিল্ড।" | **POST = তৈরি, PUT = প্রতিস্থাপন, PATCH = আংশিক হালনাগাদ।** |
| "Flask-এর ডেভ সার্ভার প্রোডাকশনে চলবে।" | এটা শুধু ডেভেলপমেন্টের জন্য। Nginx-এর পেছনে Gunicorn দিন, `debug` বন্ধ করুন। |
| "ভার্চুয়াল এনভায়রনমেন্ট মানে ভার্চুয়াল মেশিন।" | এটা শুধু এক প্রজেক্টের জন্য **আলাদা একটা পাইথন প্যাকেজ ফোল্ডার**। |

---

## ১৮. অনুশীলনী

এই ছয়টা অনুশীলনী প্রজেক্টের `README.md` থেকে নেওয়া। ৪–৬ নম্বরের সমাধান অ্যাপের সাথে চালিয়ে পরীক্ষা করা হয়েছে।

**অনুশীলনী ১ — `phone` ফিল্ড যোগ করুন।** **চারটা স্তরই** হালনাগাদ করুন: `Student` ক্লাস (`__init__` + `to_dict`), ডাটাবেস টেবিল (`CREATE TABLE`, আর রিপোজিটরির `INSERT`/`UPDATE`/`SELECT`), দুটো HTML ফর্ম, আর API।
*ইঙ্গিত:* `CREATE TABLE IF NOT EXISTS` আগে থেকে থাকা টেবিল বদলায় না। ডেভেলপের সময় `students.db` মুছে দিন, যাতে নতুন করে তৈরি হয়।

**অনুশীলনী ২ — `department` ফিল্ড যোগ করুন।** অনুশীলনী ১-এর মতোই ধাপ। দুবার করলে ধরনটা মাথায় গেঁথে যাবে।

**অনুশীলনী ৩ — নাম দিয়ে সার্চ।** একটা সার্চ বক্স যোগ করুন; যাদের নামে লেখাটা আছে, তাদের দেখান।
*ইঙ্গিত:* SQL-এ `WHERE name LIKE ?` আর মান `f"%{text}%"`, অথবা পাইথনে `get_all()` ফিল্টার করুন দুই পাশে `.lower()` দিয়ে ([৬.৮](#৬৮-ধাপ-৬--স্কিল-দিয়ে-সার্চ-ইউজার-ইনপুট-ও-কেস-সামলানো)-এর মতো)।

**অনুশীলনী ৪ — `GET /api/students/cgpa/3.5`।** যাদের CGPA ≥ সংখ্যাটা, তাদের ফেরত দিন।

<details>
<summary>সমাধান দেখুন</summary>

```python
@app.route("/api/students/cgpa/<float:min_cgpa>", methods=["GET"])
def api_students_by_cgpa(min_cgpa):
    students = StudentRepository.get_all()
    return jsonify([s.to_dict() for s in students if s.cgpa >= min_cgpa])
```

রহিম (৩.৭৫) আর হাসান (৩.২) দিয়ে পরীক্ষা: `/api/students/cgpa/3.5` → `["Rahim"]`।

⚠️ Flask-এর `<float:...>` কনভার্টারে দশমিক বিন্দু লাগবেই: `/cgpa/3` দিলে **404**। লিখুন `/cgpa/3.0`।

</details>

**অনুশীলনী ৫ — `DELETE /api/students/<id>`।**

<details>
<summary>সমাধান দেখুন</summary>

```python
@app.route("/api/students/<int:student_id>", methods=["DELETE"])
def api_delete_student(student_id):
    if StudentRepository.get_by_id(student_id) is None:
        return jsonify({"error": "Student not found"}), 404
    StudentRepository.delete(student_id)
    return jsonify({"message": f"Student {student_id} deleted"}), 200
```

পরীক্ষা: প্রথমবার `DELETE /api/students/2` → `200 {"message": "Student 2 deleted"}`; দ্বিতীয়বার → `404 {"error": "Student not found"}`।

```bash
curl -X DELETE http://127.0.0.1:5000/api/students/2
```

</details>

**অনুশীলনী ৬ — স্কিলগুলো JSON অ্যারে হিসেবে ফেরত দিন।** API-তে `"Python, Linux, Docker"` কে বানান `["Python", "Linux", "Docker"]`।

<details>
<summary>সমাধান দেখুন</summary>

`Student.to_dict()`-এ `"skills"` লাইনটা বদলে দিন:

```python
"skills": [skill.strip() for skill in self.skills.split(",") if skill.strip()],
```

`split(",")` কমায় কমায় কেটে ভাগ করে, `.strip()` স্পেস মোছে, আর `if skill.strip()` খালি টুকরো বাদ দেয়। পরীক্ষা: `"Python, Linux,Docker , "` → `['Python', 'Linux', 'Docker']`।

</details>

**বোনাস:**
- `11.JSON-Data/app.py` এমনভাবে বদলান, যাতে একটা স্কিল কতজন জানে তার **সংখ্যা** দেখায়।
- `json-api.py` বদলে `https://jsonplaceholder.typicode.com/users` থেকে প্রতিটি ইউজারের নাম আর শহর দেখান (ইঙ্গিত: `user["address"]["city"]`, অনুচ্ছেদ ৬.৪-এর মতো নেস্টেড অ্যাক্সেস)।
- Flask অ্যাপের `create_student()`-এর ভেতরে `breakpoint()` বসান, ফর্ম জমা দিন, আর `p request.form` দিয়ে দেখুন কী এলো।

---

## ১৯. ইন্টারভিউ কর্নার

**প্রশ্ন: মডিউল কী? প্যাকেজ কী?**
মডিউল একটা একক `.py` ফাইল। প্যাকেজ হলো মডিউলের একটা ফোল্ডার (প্রথাগতভাবে `__init__.py`-সহ), যেমন আমাদের অ্যাপের `models/`।

**প্রশ্ন: স্ট্যান্ডার্ড লাইব্রেরি বনাম থার্ড-পার্টি প্যাকেজ?**
স্ট্যান্ডার্ড লাইব্রেরি পাইথনের সাথেই আসে (`json`, `os`, `sqlite3`)। থার্ড-পার্টি প্যাকেজ ইনস্টল করতে হয় `pip` দিয়ে (`requests`, `flask`)।

**প্রশ্ন: `if __name__ == "__main__":` কী করে?**
ফাইলটা সরাসরি চালানো হলে তবেই এর নিচের কোড চলে, ইমপোর্ট হলে নয়।

**প্রশ্ন: `json.load`, `json.loads`, `json.dump`, `json.dumps`-এর পার্থক্য?**
`load`/`dump` ফাইল নিয়ে কাজ করে; `loads`/`dumps` স্ট্রিং নিয়ে। `load(s)` JSON পড়ে পাইথনে আনে; `dump(s)` পাইথনকে JSON হিসেবে লেখে।

**প্রশ্ন: `with open(...)` কেন?**
ব্লকের ভেতরে এরর হলেও ফাইল নিজে থেকে বন্ধ হয়ে যায়।

**প্রশ্ন: API কী? REST কী?**
API হলো সেই ইন্টারফেস, যা দিয়ে প্রোগ্রামগুলো একে অপরের সাথে কথা বলে। REST API URL-এর ওপর HTTP মেথড (GET, POST, PUT/PATCH, DELETE) ব্যবহার করে CRUD করে, সাধারণত JSON দিয়ে।

**প্রশ্ন: CRUD-কে HTTP মেথডের সাথে মেলান।**
Create → POST, Read → GET, Update → PUT/PATCH, Delete → DELETE।

**প্রশ্ন: 200, 201, 400, 404, 405 আর 500 মানে কী?**
OK, Created, Bad Request, Not Found, Method Not Allowed, Internal Server Error।

**প্রশ্ন: Flask বনাম Django বনাম FastAPI?**
Flask: হালকা মাইক্রো-ফ্রেমওয়ার্ক। Django: সব-সাথে-আনা ফুল-স্ট্যাক। FastAPI: দ্রুত API বানানোর আধুনিক ফ্রেমওয়ার্ক।

**প্রশ্ন: প্রোডাকশনে Flask-এর ডেভ সার্ভার কেন নয়? তাহলে কী ব্যবহার করবেন?**
এটা ডেভেলপমেন্টের জন্য বানানো, ভারী ট্রাফিকের জন্য নয়, আর debug চালু থাকলে একটা ইন্টারঅ্যাক্টিভ কনসোল খুলে দেয়। প্রোডাকশনে Nginx (রিভার্স প্রক্সি)-এর পেছনে Gunicorn (WSGI), অথবা async অ্যাপে Uvicorn।

**প্রশ্ন: ভার্চুয়াল এনভায়রনমেন্ট কী, কেন লাগে?**
প্রতিটি প্রজেক্টের জন্য আলাদা প্যাকেজ ফোল্ডার, যাতে ভিন্ন প্রজেক্ট সংঘাত ছাড়াই ভিন্ন প্যাকেজ ভার্সন ব্যবহার করতে পারে।

**প্রশ্ন: পাইথনে SQL ইনজেকশন ঠেকাবেন কীভাবে?**
প্যারামিটারাইজড কোয়েরি (`?` প্লেসহোল্ডার আর মানগুলোর একটা টাপল) দিয়ে। কখনো f-স্ট্রিং দিয়ে SQL বানাবেন না।

**প্রশ্ন: পাইথন স্ক্রিপ্ট কীভাবে ডিবাগ করেন?**
ট্রেসব্যাক নিচ থেকে ওপরে পড়ি, তারপর ব্রেকপয়েন্ট বসাই (`breakpoint()` / `pdb.set_trace()`), `n`/`s` দিয়ে এগোই, `p` দিয়ে দেখি, `c` দিয়ে চালিয়ে যাই।

---

## ২০. পরিভাষা (Glossary)

| ইংরেজি টার্ম | বাংলা অর্থ | প্রসঙ্গ / ব্যাখ্যা |
|---|---|---|
| **Alias** | অ্যালিয়াস, ডাকনাম | ইমপোর্ট করা মডিউলের ডাকনাম: `import modules as calc` |
| **API** | এপিআই, অ্যাপ্লিকেশন প্রোগ্রামিং ইন্টারফেস | যেভাবে প্রোগ্রামগুলো একে অপরের সাথে কথা বলে |
| **Backend** | ব্যাকএন্ড | সার্ভারে চলা কোড, যা লজিক ও ডাটা সামলায় |
| **Breakpoint** | ব্রেকপয়েন্ট, থামার বিন্দু | ডিবাগের জন্য প্রোগ্রাম যেখানে থামে |
| **Built-in module** | বিল্ট-ইন মডিউল | পাইথনের স্ট্যান্ডার্ড লাইব্রেরির মডিউল |
| **Client** | ক্লায়েন্ট | যে পাশ রিকোয়েস্ট পাঠায় (সাধারণত ব্রাউজার) |
| **Commit (database)** | কমিট, স্থায়ী সংরক্ষণ | ডাটাবেসে পরিবর্তন স্থায়ীভাবে সেভ করা |
| **Constant** | কনস্ট্যান্ট, ধ্রুবক | যে মান বদলানোর কথা না; রীতি অনুযায়ী বড় হাতে লেখা |
| **CRUD** | তৈরি, পড়া, হালনাগাদ, মোছা | Create, Read, Update, Delete |
| **`curl`** | কার্ল | HTTP রিকোয়েস্ট পাঠানোর কমান্ড-লাইন টুল |
| **Custom module** | কাস্টম মডিউল | নিজের লেখা যে `.py` ফাইল ইমপোর্ট করা হয় |
| **Debugger** | ডিবাগার | চলমান প্রোগ্রাম থামিয়ে পরীক্ষার টুল (`pdb`) |
| **Decorator** | ডেকোরেটর | নিচের ফাংশন পরিবর্তন করে এমন `@something` লাইন (`@app.route`, `@staticmethod`) |
| **Docstring** | ডকস্ট্রিং | `def` বা `class`-এর ঠিক নিচে তিন-কোটেশনের বিবরণ |
| **DRY** | নিজেকে পুনরাবৃত্তি কোরো না | Don't Repeat Yourself |
| **Dunder** | ডান্ডার | ডাবল আন্ডারস্কোরওয়ালা নাম, যেমন `__name__`, `__init__` |
| **Edge case** | এজ কেস, প্রান্তিক অবস্থা | যে অস্বাভাবিক ইনপুট সরল কোড ভেঙে দেয় |
| **Endpoint** | এন্ডপয়েন্ট | API যে নির্দিষ্ট URL + মেথডে সাড়া দেয় (`GET /api/students`) |
| **Entry point** | এন্ট্রি পয়েন্ট, প্রবেশবিন্দু | প্রোগ্রাম যেখান থেকে শুরু হয় (`if __name__ == "__main__":`) |
| **Exception** | এক্সেপশন, ব্যতিক্রম | চলার সময় ওঠা এরর; `try / except` দিয়ে ধরা হয় |
| **Flask** | ফ্লাস্ক | হালকা পাইথন ওয়েব ফ্রেমওয়ার্ক |
| **Frontend** | ফ্রন্টএন্ড | ব্রাউজারে ব্যবহারকারী যা দেখে ও ব্যবহার করে |
| **HTTP method** | HTTP মেথড | রিকোয়েস্টের ধরন: GET, POST, PUT, PATCH, DELETE |
| **Idempotent** | আইডেমপোটেন্ট | দুবার চালালেও ফল একবারের মতোই |
| **Jinja2** | জিঞ্জা২ | Flask-এর টেমপ্লেট ভাষা: `{{ }}` আর `{% %}` |
| **JSON** | জেসন | JavaScript Object Notation: কী-ভ্যালু ডাটার টেক্সট ফরম্যাট |
| **Localhost / 127.0.0.1** | লোকালহোস্ট | এই কম্পিউটারটাই |
| **Model** | মডেল | এক ধরনের ডাটাকে প্রকাশ করে এমন ক্লাস (`Student`) |
| **Module** | মডিউল | ইমপোর্ট করা যায় এমন পাইথন ফাইল |
| **Named import** | নেমড ইমপোর্ট | `from module import name1, name2` |
| **Package** | প্যাকেজ | মডিউলের একটা ফোল্ডার (যেমন `models/`) |
| **Parameterised query** | প্যারামিটারাইজড কোয়েরি | `?` প্লেসহোল্ডারসহ SQL; SQL ইনজেকশন ঠেকায় |
| **`pip`** | পিপ | পাইথনের প্যাকেজ ইনস্টলার |
| **Port** | পোর্ট | নেটওয়ার্ক ট্রাফিকের জন্য কম্পিউটারের নম্বরওয়ালা দরজা (Flask: 5000) |
| **Repository (pattern)** | রিপোজিটরি প্যাটার্ন | একটা মডেলের সব ডাটাবেস-কাজ ধরে রাখা ক্লাস |
| **Request / Response** | রিকোয়েস্ট / রেসপন্স | ক্লায়েন্ট যা চায় / সার্ভার যা উত্তর দেয় |
| **`requirements.txt`** | রিকোয়ারমেন্টস ফাইল | প্রজেক্টের দরকারি প্যাকেজের তালিকা |
| **REST API** | রেস্ট এপিআই | URL-এর ওপর HTTP মেথড দিয়ে CRUD করা API |
| **Reverse proxy** | রিভার্স প্রক্সি | সামনের সার্ভার (Nginx), যা রিকোয়েস্ট অ্যাপে পাঠায় |
| **Route** | রাউট, পথ | Flask-এ একটা ফাংশন যে URL পাথ সামলায় |
| **Separation of concerns** | দায়িত্বের বিভাজন | কোডের প্রতিটি অংশের একটা দায়িত্ব |
| **Server** | সার্ভার | যে পাশ রিকোয়েস্টের উত্তর দেয় |
| **Slice** | স্লাইস, অংশ | লিস্টের একটা অংশ: `posts[1:3]` |
| **SQL injection** | SQL ইনজেকশন | যে আক্রমণে ইনপুট দিয়ে SQL কোয়েরি বদলে ফেলা হয় |
| **SQLite** | এসকিউলাইট | পাইথনে বিল্ট-ইন একক-ফাইলের রিলেশনাল ডাটাবেস (`sqlite3`) |
| **Standard library** | স্ট্যান্ডার্ড লাইব্রেরি | পাইথনের সাথে ইনস্টল হওয়া মডিউলগুলো |
| **Static method** | স্ট্যাটিক মেথড | ক্লাসের ভেতরের যে মেথড `self` পায় না |
| **Status code** | স্ট্যাটাস কোড | রেসপন্সের ফলাফল বোঝানোর সংখ্যা (200, 404, …) |
| **Template** | টেমপ্লেট | Flask যে প্লেসহোল্ডারওয়ালা HTML পূরণ করে |
| **Third-party package** | থার্ড-পার্টি প্যাকেজ | PyPI থেকে `pip` দিয়ে ইনস্টল করা কোড |
| **Traceback** | ট্রেসব্যাক | পাইথনের এরর রিপোর্ট: নিচ থেকে ওপরে পড়ুন |
| **Virtual environment** | ভার্চুয়াল এনভায়রনমেন্ট | এক প্রজেক্টের জন্য আলাদা প্যাকেজের সেট |
| **Web server** | ওয়েব সার্ভার | HTTP রিকোয়েস্ট নিয়ে রেসপন্স ফেরত দেওয়া প্রোগ্রাম |
| **WSGI / ASGI** | ডব্লিউএসজিআই / এএসজিআই | পাইথন অ্যাপ আর ওয়েব সার্ভারের মধ্যকার মানক ইন্টারফেস (sync / async) |

---

## ২১. এক পাতার চিট শিট

```python
# ── মডিউল ─────────────────────────────────────────────────
import modules                      # পুরো ফাইল: modules.add(1, 2)
import modules as calc              # অ্যালিয়াস: calc.add(1, 2)
from modules import add, name       # নেমড:      add(1, 2)
# বিল্ট-ইন: json os sys shutil datetime sqlite3 pdb   থার্ড-পার্টি: pip install requests flask

# ── ফাইল ──────────────────────────────────────────────────
with open("file.json", "r") as f:   # r=পড়া  w=মুছে লেখা  a=শেষে যোগ  x=নতুন তৈরি
    ...                             # এখানে ফাইল নিজে থেকে বন্ধ

# ── JSON ──────────────────────────────────────────────────
import json
data = json.load(f)                 # ফাইল   → পাইথন
data = json.loads(text)             # স্ট্রিং → পাইথন
json.dump(data, f, indent=4)        # পাইথন → ফাইল
text = json.dumps(data, indent=4)   # পাইথন → স্ট্রিং
# JSON true/false/null  ⇄  পাইথন True/False/None ; JSON-এ "ডাবল কোটেশন" লাগবেই

# ── নেস্টেড অ্যাক্সেস, লুপ, ফিল্টার ───────────────────────
data["students"][0]["address"]["city"]            # dict → কী, list → ইনডেক্স
for s in data["students"]:
    if s["cgpa"] > 3.7: print(s["name"])
if user_text.strip().lower() == skill.lower(): ...  # কেস-নিরপেক্ষ তুলনা

# ── আবার ফাইলে লেখা ──────────────────────────────────────
data["students"].append(new_student)              # শুধু মেমোরিতে
with open("students.json", "w") as f:
    json.dump(data, f, indent=4)                  # এবার সেভ হলো

# ── requests দিয়ে API ─────────────────────────────────────
import requests
try:
    r = requests.get(url)
    r.raise_for_status()            # 4xx / 5xx হলে এরর
    posts = r.json()                # list / dict
except Exception as e:
    print(f"Failed: {e}")
posts[:5]  posts[-5:]  posts[1:3]  posts[-1]  # স্লাইস → list ; [-1] → একটা আইটেম

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
    app.run(debug=True)             # শুধু ডেভেলপমেন্টে! প্রোডাকশন: gunicorn + nginx

# ── SQLITE ────────────────────────────────────────────────
conn = sqlite3.connect("students.db"); conn.row_factory = sqlite3.Row
conn.execute("SELECT * FROM students WHERE id = ?", (sid,))   # ? = SQL ইনজেকশন থেকে নিরাপদ
conn.commit(); conn.close()

# ── VENV ও চালানো ─────────────────────────────────────────
# python3 -m venv venv ; source venv/bin/activate   (উইন্ডোজ: venv\Scripts\activate)
# pip install -r requirements.txt ; python3 app.py ; খুলুন http://127.0.0.1:5000
# curl http://127.0.0.1:5000/api/students ; থামাতে Ctrl+C ; deactivate

# ── ডিবাগ ─────────────────────────────────────────────────
import pdb; pdb.set_trace()         # অথবা শুধু: breakpoint()
# n=পরের লাইন  s=ভেতরে ঢোকো  c=চালিয়ে যাও  p x=দেখাও  l=কোড দেখাও  q=বের হও
```

---

*নিজে প্রতিটি ফোল্ডার চালিয়ে প্র্যাকটিস করুন: মডিউল → JSON পড়া → JSON লেখা → API → Flask অ্যাপ → pdb। "দুনিয়ায় কোনো কিছুই কঠিন না, যতক্ষণ আপনি কঠিন মনে না করেন।"*
