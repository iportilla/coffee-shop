# ☕ Coffee Shop Regulars — Warm-Up Solution

Notebook: [`Warmup_Coffee_Shop_Solution.ipynb`](Warmup_Coffee_Shop_Solution.ipynb)

This notebook practices **reading and writing files** in Python. It answers two questions for the manager of Bean There Café:

1. Which of **today's visitors** are on the frequent customer list?
2. How many times did each **frequent customer** visit **last week**?

---

## How to run it

1. Open `Warmup_Coffee_Shop_Solution.ipynb` in Jupyter or VS Code.
2. Run the cells **from top to bottom**. The setup cell has to run first, because it creates the data files that the other cells read.
3. The output shows up under each cell, and a new file called `visit_report.txt` appears in the folder.

> ⚠️ The setup cell opens files in `"w"` (write) mode, which **erases** anything already in `frequent_customers.txt` and `visits_last_week.txt`.

---

## The big picture

```mermaid
flowchart LR
    S["Step 0<br/>Setup cell"] -->|writes| F[("frequent_customers.txt")]
    S -->|writes| V[("visits_last_week.txt")]
    F -->|read| P1["Part 1<br/>Check today's visitors"]
    F -->|read| P2["Part 2<br/>Count last week's visits"]
    V -->|read| P2
    P2 -->|writes| R[("visit_report.txt")]
    R -->|read back| OUT["Printed report"]
```

---

## The files

| File | Created by | What's inside |
|---|---|---|
| `frequent_customers.txt` | Setup cell | One name per line (`Ana`, `Marcus`, ...) |
| `visits_last_week.txt` | Setup cell | One visit per line: `day,name` (e.g. `Monday,Ana`) |
| `visit_report.txt` | Part 2 | The final visit report |

---

## Part 1: Is this visitor a regular?

We read the frequent customer names into a list, then check each of today's visitors against it.

```mermaid
flowchart TD
    A["Open frequent_customers.txt"] --> B["For each line:<br/>strip the \n and add the name to the list"]
    B --> C{"For each name<br/>in todays_visitors"}
    C --> D{"Is the name in<br/>the frequent list?"}
    D -- Yes --> E["Print: is a frequent customer! ⭐"]
    D -- No --> G["Print: is not on the list"]
```

**Why `.strip()`?** Every line read from a file ends with `\n`, so `"Ana\n"` is not equal to `"Ana"`. Calling `.strip()` removes that newline.

---

## Part 2: The weekly visit report

We use a **dictionary** to count visits, with each name as a key and that customer's count as the value.

```mermaid
flowchart TD
    A["Create visit_counts dict<br/>with every frequent customer set to 0"] --> B["Open visits_last_week.txt"]
    B --> C["For each line:<br/>split on the comma → day, customer"]
    C --> D{"Is the customer<br/>in visit_counts?"}
    D -- Yes --> E["Add 1 to their count"]
    D -- No --> C
    E --> C
    C -- "no more lines" --> F["Write the report to visit_report.txt"]
    F --> G["Open the report and print it"]
```

**Why start everyone at 0?** So a customer who never came in (like Lena) still shows up in the report as `Lena: 0` instead of being left out.

Expected output:

```
Frequent Customer Visits - Last Week
------------------------------------
Ana: 4
Marcus: 3
Priya: 3
Diego: 1
Lena: 0
------------------------------------
Total visits: 11
```

---

## File modes cheat sheet

```mermaid
flowchart TD
    Q{"What do you need?"} -- "Read a file that exists" --> R["'r': read"]
    Q -- "Make a new file / replace one" --> W["'w': write ⚠️ erases old contents"]
    Q -- "Add to the end of a file" --> A["'a': append"]
```

Tip: always use `with open(...) as file:`. It closes the file for you automatically, even if something goes wrong.
