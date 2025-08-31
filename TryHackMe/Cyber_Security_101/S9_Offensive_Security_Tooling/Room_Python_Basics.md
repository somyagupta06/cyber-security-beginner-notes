# Python Basics Notes

# 1. Printing Text
We use the `print()` function to display text on the screen.

Example:
```
    # This is a comment
    print("Hello World")
```
- Line starting with `#` is a **comment** (ignored by computer).
- `print()` is used to show output.
- Text (string) should be inside `" "` or `' '`.
# 2. Mathematical Operators
Python works like a calculator.

| Operation       | Symbol | Example         | Output |
|-----------------|--------|----------------|--------|
| Addition        | +      | 1 + 1          | 2      |
| Subtraction     | -      | 5 - 1          | 4      |
| Multiplication  | *      | 10 * 10        | 100    |
| Division        | /      | 10 / 2         | 5      |
| Modulus         | %      | 10 % 3         | 1      |
| Exponent        | **     | 5 ** 2         | 25     |
# 3. Comparison Operators
Used to compare two values. They return **True or False**.

| Operator | Meaning                  | Example   | Result  |
|----------|--------------------------|-----------|---------|
| >        | Greater than             | 5 > 3     | True    |
| <        | Less than                | 2 < 5     | True    |
| ==       | Equal to                 | 3 == 3    | True    |
| !=       | Not Equal to             | 4 != 5    | True    |
| >=       | Greater or Equal         | 5 >= 5    | True    |
| <=       | Less or Equal            | 4 <= 5    | True    |
# 4. Variables
Variables store data with a name.

Example:
```
    food = "ice cream"
    money = 2000
```
- `food` stores text (string).
- `money` stores number (integer).

We can update variables:
```
    age = 30
    age = age + 1   # now age = 31
```
# 5. Data Types
Different kinds of data in Python:

- String  → Text (e.g., "hello")
- Integer → Whole numbers (e.g., 10)
- Float   → Decimal numbers (e.g., 3.14)
- Boolean → True or False
- List    → Collection of multiple items (e.g., [1, 2, 3])
# 6. Logical & Boolean Operators
Used in conditions (if statements).

Logical operators:
    if x > 5   # greater than
    if x < 5   # less than
    if x == 5  # equal to
    if x != 5  # not equal

Boolean operators:
- AND → Both conditions must be true
- OR  → At least one condition is true
- NOT → Opposite of condition

Example:
```
    a = 1
    if a == 1 or a > 10:
        print("a is either 1 or above 10")

    name = "bob"
    hungry = True
    if name == "bob" and hungry:
        print("bob is hungry")
```
# 7. If Statements
Used for decision making.

Example:
```
    age = 16
    if age < 17:
        print("You are NOT old enough to drive")
    else:
        print("You are old enough to drive")
```
- `if` starts condition.
- `else` runs if condition is False.
- `:` colon ends the condition line.
- Indentation (spaces before code) is very important.
# 8. Loops
Loops repeat code.

## While Loop
Runs until condition is False.
```
    i = 1
    while i <= 10:
        print(i)
        i = i + 1
```
## For Loop
Runs through a list or range.
```
    websites = ["facebook.com", "google.com", "amazon.com"]
    for site in websites:
        print(site)
```
    # range example
```
    for i in range(5):
        print(i)   # prints 0,1,2,3,4
```
# 9. Functions
Reusable block of code.

Example:
```
    def sayHello(name):
        print("Hello " + name + "!")

    sayHello("Ben")   # Output → Hello Ben!
```
- `def` keyword starts a function.
- Functions can return values:
```
    def calcCost(item):
        if item == "sweets":
            return 3.99
        elif item == "oranges":
            return 1.99
        else:
            return 0.99
```
# 10. File Handling
Read and write files.

## Reading:
```
    f = open("file.txt", "r")
    print(f.read())
    f.close()
```
## Writing/Appending:
```
    f = open("file1.txt", "a")  # append
    f.write("More text")
    f.close()

    f = open("file2.txt", "w")  # write new file
    f.write("Hello World")
    f.close()
```
# 11. Libraries
Libraries = extra functions written by others.

Example:
```
    import datetime
    current_time = datetime.datetime.now()
    print(current_time)
```

Popular libraries:
- requests  → HTTP requests
- scapy     → network packets
- pwntools  → exploit development

To install external library:
```
    pip install scapy
```
