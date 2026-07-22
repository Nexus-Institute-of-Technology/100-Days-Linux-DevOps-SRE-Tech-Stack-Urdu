# Module 12: Bash Arithmetic Operations — Addition and Subtraction

## Lesson Overview

In this lesson, we begin learning how to perform arithmetic operations in Bash.

Bash can perform calculations such as:

- Addition
- Subtraction
- Multiplication
- Division
- Powers
- Remainders

This lesson focuses on **addition and subtraction**.

We will also build a practical script that:

1. Accepts two file names as command-line arguments.
2. Finds the size of each file in bytes.
3. Adds the two file sizes.
4. Displays the total size.

---

## Learning Objectives

By the end of this lesson, students will be able to:

- Explain how Bash performs arithmetic.
- Use arithmetic expansion with `$(( ... ))`.
- Add and subtract numbers.
- Store calculation results in variables.
- Use command-line arguments such as `$1` and `$2`.
- Use command substitution with `$( ... )`.
- Obtain file sizes in bytes.
- Validate script arguments.
- Build a practical file-size calculator.

---

## Arithmetic Sections in This Module

This arithmetic module contains four main sections:

1. Addition and subtraction
2. Multiplication and division
3. Powers and remainders
4. Building a Celsius-to-Fahrenheit calculator

This lesson covers the first section.

---

# 1. Arithmetic Expansion in Bash

Bash arithmetic expressions are normally placed inside:

```bash
$(( arithmetic_expression ))
```

This is called **arithmetic expansion**.

Example:

```bash
echo $((10 + 5))
```

Output:

```text
15
```

Another example:

```bash
echo $((10 - 3))
```

Output:

```text
7
```

---

## Arithmetic Expansion Syntax

```bash
$((number1 operator number2))
```

Example:

```bash
$((8 + 2))
```

| Part | Meaning |
|---|---|
| `$` | Requests the calculated result |
| `((` | Begins the arithmetic expression |
| `8 + 2` | Calculation |
| `))` | Ends the arithmetic expression |

---

# 2. Addition

The addition operator is:

```text
+
```

Example:

```bash
echo $((20 + 10))
```

Output:

```text
30
```

---

## Addition with Variables

```bash
num1=20
num2=10

total=$((num1 + num2))

echo "$total"
```

Output:

```text
30
```

Inside arithmetic expansion, the variable names can be written without `$`:

```bash
total=$((num1 + num2))
```

This also works:

```bash
total=$(($num1 + $num2))
```

However, the cleaner Bash style is:

```bash
total=$((num1 + num2))
```

---

# 3. Subtraction

The subtraction operator is:

```text
-
```

Example:

```bash
echo $((10 - 3))
```

Output:

```text
7
```

---

## Subtraction with Variables

```bash
num1=20
num2=8

difference=$((num1 - num2))

echo "$difference"
```

Output:

```text
12
```

---

# 4. Store a Calculation in a Variable

The result of a calculation can be stored in a variable.

Example:

```bash
first_number=25
second_number=15

total=$((first_number + second_number))
```

Display the result:

```bash
echo "Total: $total"
```

Output:

```text
Total: 40
```

---

# 5. Command Substitution Review

Command substitution runs a command and stores its output.

Syntax:

```bash
variable=$(command)
```

Example:

```bash
current_user=$(whoami)
```

Display it:

```bash
echo "$current_user"
```

Another example:

```bash
current_date=$(date)
```

In this lesson, command substitution will be used to capture file sizes.

---

# 6. Command-Line Arguments Review

When running a script, values may be passed after the script name.

Example:

```bash
./addition.sh /etc/passwd /etc/group
```

The arguments are stored as:

| Variable | Value |
|---|---|
| `$0` | Script name |
| `$1` | First argument |
| `$2` | Second argument |
| `$#` | Number of arguments |

For this command:

```bash
./addition.sh /etc/passwd /etc/group
```

the values are:

```text
$0 = ./addition.sh
$1 = /etc/passwd
$2 = /etc/group
$# = 2
```

---

# 7. Find a File Size in Bytes

The GNU `du` command can display a file's apparent size in bytes:

```bash
du -b /etc/passwd
```

Example output:

```text
1872    /etc/passwd
```

The output contains two fields:

1. File size
2. File name

For the arithmetic operation, we need only the first field.

---

# 8. Extract the First Field with `cut`

Use:

```bash
du -b /etc/passwd | cut -f1
```

Example output:

```text
1872
```

Explanation:

| Command | Purpose |
|---|---|
| `du -b /etc/passwd` | Displays the size and file name |
| `|` | Sends the output to the next command |
| `cut -f1` | Selects the first tab-separated field |

The result can be stored in a variable:

```bash
file_size=$(du -b /etc/passwd | cut -f1)
```

Display it:

```bash
echo "$file_size"
```

---

## Alternative Using `stat`

A direct way to get the file size is:

```bash
stat -c %s /etc/passwd
```

This returns only the size in bytes.

Example:

```bash
file_size=$(stat -c %s /etc/passwd)
```

Both methods are useful. The lesson script uses `du -b` and `cut` to demonstrate pipes and field extraction.

---

# 9. Build the Basic Addition Script

Create the script:

```bash
vi addition.sh
```

Press `i` to enter insert mode.

Add:

```bash
#!/bin/bash

fs1=$(du -b "$1" | cut -f1)
fs2=$(du -b "$2" | cut -f1)

echo "File $1 has $fs1 bytes."
echo "File $2 has $fs2 bytes."

total=$((fs1 + fs2))

echo "Total number of bytes: $total"
```

Save and exit:

1. Press `Esc`.
2. Type `:wq`.
3. Press `Enter`.

---

# 10. Explanation of the Script

## Shebang

```bash
#!/bin/bash
```

Tells Linux to use Bash to execute the script.

---

## First File Size

```bash
fs1=$(du -b "$1" | cut -f1)
```

This line:

1. Reads the first command-line argument using `$1`.
2. Runs `du -b` against that file.
3. Uses `cut -f1` to keep only the size.
4. Stores the result in `fs1`.

If `$1` is `/etc/passwd`, Bash effectively runs:

```bash
du -b /etc/passwd | cut -f1
```

---

## Second File Size

```bash
fs2=$(du -b "$2" | cut -f1)
```

This repeats the process for the second file.

---

## Display the File Sizes

```bash
echo "File $1 has $fs1 bytes."
echo "File $2 has $fs2 bytes."
```

Example:

```text
File /etc/passwd has 1872 bytes.
File /etc/group has 892 bytes.
```

---

## Add the File Sizes

```bash
total=$((fs1 + fs2))
```

This adds the values stored in `fs1` and `fs2`.

Example:

```text
1872 + 892 = 2764
```

The result is stored in:

```text
total
```

---

## Display the Total

```bash
echo "Total number of bytes: $total"
```

Example output:

```text
Total number of bytes: 2764
```

---

# 11. Make the Script Executable

```bash
chmod +x addition.sh
```

Check the permissions:

```bash
ls -l addition.sh
```

Example:

```text
-rwxr-xr-x. 1 student student 230 Jul 18 10:00 addition.sh
```

---

# 12. Run the Script

```bash
./addition.sh /etc/passwd /etc/group
```

Example output:

```text
File /etc/passwd has 1872 bytes.
File /etc/group has 892 bytes.
Total number of bytes: 2764
```

The exact sizes may be different on your system.

---

# 13. Verify the Calculation Manually

Check the first file:

```bash
du -b /etc/passwd
```

Check the second file:

```bash
du -b /etc/group
```

Add the numbers manually with Bash:

```bash
echo $((1872 + 892))
```

Output:

```text
2764
```

---

# 14. Improved Script with Argument Validation

The basic script assumes that the user provides exactly two file names.

A better script checks the number of arguments.

```bash
#!/bin/bash

if [[ $# -ne 2 ]]; then
    echo "Usage: $0 FILE1 FILE2"
    exit 1
fi

fs1=$(du -b "$1" | cut -f1)
fs2=$(du -b "$2" | cut -f1)

echo "File $1 has $fs1 bytes."
echo "File $2 has $fs2 bytes."

total=$((fs1 + fs2))

echo "Total number of bytes: $total"
```

---

## Explanation of the Argument Check

```bash
if [[ $# -ne 2 ]]; then
```

| Part | Meaning |
|---|---|
| `$#` | Number of command-line arguments |
| `-ne` | Not equal to |
| `2` | Required number of arguments |

The condition means:

> If the number of arguments is not equal to two, display an error.

---

## Usage Message

```bash
echo "Usage: $0 FILE1 FILE2"
```

`$0` contains the script name.

Example:

```text
Usage: ./addition.sh FILE1 FILE2
```

---

# 15. Validate That Both Files Exist

Use the `-f` test:

```bash
[[ -f "$1" ]]
```

This checks whether `$1` exists and is a regular file.

Complete validation:

```bash
if [[ ! -f "$1" ]]; then
    echo "Error: File '$1' does not exist."
    exit 1
fi

if [[ ! -f "$2" ]]; then
    echo "Error: File '$2' does not exist."
    exit 1
fi
```

The `!` means **not**.

Therefore:

```bash
[[ ! -f "$1" ]]
```

means:

> The first file does not exist or is not a regular file.

---

# 16. Complete Validated Solution

```bash
#!/bin/bash

# Require exactly two arguments
if [[ $# -ne 2 ]]; then
    echo "Usage: $0 FILE1 FILE2"
    exit 1
fi

# Check the first file
if [[ ! -f "$1" ]]; then
    echo "Error: File '$1' does not exist or is not a regular file."
    exit 1
fi

# Check the second file
if [[ ! -f "$2" ]]; then
    echo "Error: File '$2' does not exist or is not a regular file."
    exit 1
fi

# Capture file sizes in bytes
fs1=$(du -b "$1" | cut -f1)
fs2=$(du -b "$2" | cut -f1)

# Display individual file sizes
echo "File 1: $1"
echo "Size 1: $fs1 bytes"
echo
echo "File 2: $2"
echo "Size 2: $fs2 bytes"

# Add the two sizes
total=$((fs1 + fs2))

echo
echo "Total size: $total bytes"
```

---

# 17. Example Successful Execution

```bash
./addition.sh /etc/passwd /etc/group
```

Example output:

```text
File 1: /etc/passwd
Size 1: 1872 bytes

File 2: /etc/group
Size 2: 892 bytes

Total size: 2764 bytes
```

---

# 18. Example Missing Arguments

Run:

```bash
./addition.sh
```

Output:

```text
Usage: ./addition.sh FILE1 FILE2
```

Run with only one argument:

```bash
./addition.sh /etc/passwd
```

Output:

```text
Usage: ./addition.sh FILE1 FILE2
```

---

# 19. Example Invalid File

```bash
./addition.sh /etc/passwd /does/not/exist
```

Output:

```text
Error: File '/does/not/exist' does not exist or is not a regular file.
```

---

# 20. Subtraction Practice

Create:

```bash
vi subtraction.sh
```

Add:

```bash
#!/bin/bash

number1=20
number2=8

difference=$((number1 - number2))

echo "$number1 - $number2 = $difference"
```

Make it executable:

```bash
chmod +x subtraction.sh
```

Run it:

```bash
./subtraction.sh
```

Output:

```text
20 - 8 = 12
```

---

# 21. Subtract Two Command-Line Arguments

Create a script named:

```text
subtract_args.sh
```

Add:

```bash
#!/bin/bash

if [[ $# -ne 2 ]]; then
    echo "Usage: $0 NUMBER1 NUMBER2"
    exit 1
fi

result=$(($1 - $2))

echo "$1 - $2 = $result"
```

Run:

```bash
chmod +x subtract_args.sh
./subtract_args.sh 10 3
```

Output:

```text
10 - 3 = 7
```

---

# 22. Validate Numeric Arguments

The previous script accepts any text. Improve it with validation:

```bash
#!/bin/bash

if [[ $# -ne 2 ]]; then
    echo "Usage: $0 NUMBER1 NUMBER2"
    exit 1
fi

if [[ ! "$1" =~ ^-?[0-9]+$ ]]; then
    echo "Error: '$1' is not a valid integer."
    exit 1
fi

if [[ ! "$2" =~ ^-?[0-9]+$ ]]; then
    echo "Error: '$2' is not a valid integer."
    exit 1
fi

result=$(($1 - $2))

echo "$1 - $2 = $result"
```

---

## Numeric Pattern Explanation

```text
^-?[0-9]+$
```

| Part | Meaning |
|---|---|
| `^` | Start of input |
| `-?` | Optional negative sign |
| `[0-9]+` | One or more digits |
| `$` | End of input |

Valid examples:

```text
10
0
-5
250
```

Invalid examples:

```text
ten
5.5
12abc
```

---

# 23. Interactive Addition Script

Use `read` to ask the user for two numbers.

```bash
#!/bin/bash

read -r -p "Enter the first number: " num1
read -r -p "Enter the second number: " num2

total=$((num1 + num2))

echo "$num1 + $num2 = $total"
```

Example interaction:

```text
Enter the first number: 25
Enter the second number: 15
25 + 15 = 40
```

---

# 24. Interactive Subtraction Script

```bash
#!/bin/bash

read -r -p "Enter the first number: " num1
read -r -p "Enter the second number: " num2

difference=$((num1 - num2))

echo "$num1 - $num2 = $difference"
```

---

# 25. Combined Addition and Subtraction Script

```bash
#!/bin/bash

read -r -p "Enter the first number: " num1
read -r -p "Enter the second number: " num2

addition=$((num1 + num2))
subtraction=$((num1 - num2))

echo
echo "$num1 + $num2 = $addition"
echo "$num1 - $num2 = $subtraction"
```

---

# 26. Increment and Decrement Operators

Bash supports increment and decrement operations.

## Increment

```bash
number=5
((number++))

echo "$number"
```

Output:

```text
6
```

Another form:

```bash
((number += 1))
```

---

## Decrement

```bash
number=5
((number--))

echo "$number"
```

Output:

```text
4
```

Another form:

```bash
((number -= 1))
```

---

# 27. Addition Assignment

```bash
total=10

((total += 5))

echo "$total"
```

Output:

```text
15
```

This is equivalent to:

```bash
total=$((total + 5))
```

---

# 28. Subtraction Assignment

```bash
total=10

((total -= 3))

echo "$total"
```

Output:

```text
7
```

This is equivalent to:

```bash
total=$((total - 3))
```

---

# 29. Arithmetic Expansion Versus Arithmetic Command

These two forms are related but serve slightly different purposes.

## Arithmetic Expansion

```bash
result=$((10 + 5))
```

This returns a value that can be stored or displayed.

```bash
echo $((10 + 5))
```

---

## Arithmetic Command

```bash
((result = 10 + 5))
```

This performs arithmetic directly.

Display the result:

```bash
echo "$result"
```

For beginners, arithmetic expansion is often easier to understand:

```bash
result=$((10 + 5))
```

---

# 30. Integer Arithmetic

Bash performs integer arithmetic by default.

Example:

```bash
echo $((5 / 2))
```

Output:

```text
2
```

Bash does not display:

```text
2.5
```

because the decimal part is removed.

Decimal arithmetic requires another tool such as `bc` or `awk`, which will be covered separately.

---

# 31. Common Mistakes

## Mistake 1: Using One Set of Parentheses

Incorrect:

```bash
total=$(num1 + num2)
```

This attempts command substitution.

Correct:

```bash
total=$((num1 + num2))
```

---

## Mistake 2: Adding Text Values

```bash
num1="hello"
num2=5

echo $((num1 + num2))
```

Bash arithmetic expects integer values.

Validate user input before calculating.

---

## Mistake 3: Forgetting `$` Before Arithmetic Expansion

Incorrect:

```bash
total=((num1 + num2))
```

Correct:

```bash
total=$((num1 + num2))
```

Alternatively, use the arithmetic command:

```bash
((total = num1 + num2))
```

---

## Mistake 4: Forgetting to Quote File Arguments

Less safe:

```bash
du -b $1
```

Preferred:

```bash
du -b "$1"
```

Quoting protects file names containing spaces.

---

## Mistake 5: Not Checking the Number of Arguments

Without validation, `$1` or `$2` may be empty.

Use:

```bash
if [[ $# -ne 2 ]]; then
    echo "Usage: $0 FILE1 FILE2"
    exit 1
fi
```

---

## Mistake 6: Confusing Command Substitution and Arithmetic Expansion

Command substitution:

```bash
file_size=$(du -b "$1" | cut -f1)
```

Arithmetic expansion:

```bash
total=$((fs1 + fs2))
```

They both begin with `$(`, but arithmetic expansion uses two opening and two closing parentheses.

---

# 32. Practice Lab 1: Add Two Numbers

Create `add.sh`:

```bash
#!/bin/bash

num1=15
num2=7

total=$((num1 + num2))

echo "$num1 + $num2 = $total"
```

Expected output:

```text
15 + 7 = 22
```

---

# 33. Practice Lab 2: Subtract Two Numbers

Create `subtract.sh`:

```bash
#!/bin/bash

num1=15
num2=7

difference=$((num1 - num2))

echo "$num1 - $num2 = $difference"
```

Expected output:

```text
15 - 7 = 8
```

---

# 34. Practice Lab 3: Ask the User for Numbers

Create `arithmetic.sh`:

```bash
#!/bin/bash

read -r -p "Enter the first integer: " num1
read -r -p "Enter the second integer: " num2

sum=$((num1 + num2))
difference=$((num1 - num2))

echo
echo "Addition result: $sum"
echo "Subtraction result: $difference"
```

---

# 35. Student Challenge 1: File Size Addition

Create `file_total.sh`.

Requirements:

1. Accept two file names as arguments.
2. Verify that two arguments were provided.
3. Verify that both files exist.
4. Display each file size in bytes.
5. Add the file sizes.
6. Display the total.

---

## Solution

```bash
#!/bin/bash

if [[ $# -ne 2 ]]; then
    echo "Usage: $0 FILE1 FILE2"
    exit 1
fi

if [[ ! -f "$1" ]]; then
    echo "Error: '$1' is not a regular file."
    exit 1
fi

if [[ ! -f "$2" ]]; then
    echo "Error: '$2' is not a regular file."
    exit 1
fi

size1=$(stat -c %s "$1")
size2=$(stat -c %s "$2")

total=$((size1 + size2))

echo "File 1: $1"
echo "Size 1: $size1 bytes"
echo
echo "File 2: $2"
echo "Size 2: $size2 bytes"
echo
echo "Total: $total bytes"
```

---

# 36. Student Challenge 2: Remaining Disk Calculation

Suppose a disk has a total capacity and some space is already used.

Create `remaining.sh`:

```bash
#!/bin/bash

read -r -p "Enter total disk size in GB: " total
read -r -p "Enter used disk space in GB: " used

remaining=$((total - used))

echo "Remaining disk space: $remaining GB"
```

Example:

```text
Enter total disk size in GB: 100
Enter used disk space in GB: 35
Remaining disk space: 65 GB
```

---

# 37. Final Challenge: Arithmetic Menu

Create a script named:

```text
basic_calculator.sh
```

The script must:

1. Ask for two integers.
2. Ask the user to choose addition or subtraction.
3. Validate the numbers.
4. Perform the selected operation.
5. Display the full expression and result.

---

## Final Challenge Solution

```bash
#!/bin/bash

echo "=============================="
echo "     Bash Basic Calculator"
echo "=============================="
echo "1. Addition"
echo "2. Subtraction"
echo

read -r -p "Enter your choice: " choice
read -r -p "Enter the first integer: " num1
read -r -p "Enter the second integer: " num2

if [[ ! "$num1" =~ ^-?[0-9]+$ ]]; then
    echo "Error: '$num1' is not a valid integer."
    exit 1
fi

if [[ ! "$num2" =~ ^-?[0-9]+$ ]]; then
    echo "Error: '$num2' is not a valid integer."
    exit 1
fi

case "$choice" in
    1)
        result=$((num1 + num2))
        echo "$num1 + $num2 = $result"
        ;;

    2)
        result=$((num1 - num2))
        echo "$num1 - $num2 = $result"
        ;;

    *)
        echo "Error: Invalid selection."
        exit 1
        ;;
esac
```

---

## Example Addition

```text
==============================
     Bash Basic Calculator
==============================
1. Addition
2. Subtraction

Enter your choice: 1
Enter the first integer: 25
Enter the second integer: 15
25 + 15 = 40
```

---

## Example Subtraction

```text
Enter your choice: 2
Enter the first integer: 25
Enter the second integer: 15
25 - 15 = 10
```

---

# 38. Knowledge Check

1. What is arithmetic expansion?
2. What syntax is used for arithmetic expansion?
3. What operator performs addition?
4. What operator performs subtraction?
5. What does `$1` represent?
6. What does `$2` represent?
7. What does `$#` represent?
8. What does command substitution do?
9. Why is `cut -f1` used after `du -b`?
10. What does `total=$((fs1 + fs2))` do?
11. What does `-ne` mean in a Bash numeric test?
12. What does `-f` test?
13. Does Bash arithmetic display decimal results by default?
14. What is the difference between `$( ... )` and `$(( ... ))`?

---

# 39. Knowledge Check Answers

1. It calculates an arithmetic expression and returns the result.
2. `$(( expression ))`
3. `+`
4. `-`
5. The first command-line argument.
6. The second command-line argument.
7. The total number of command-line arguments.
8. It runs a command and captures its output.
9. It extracts only the file-size field.
10. It adds `fs1` and `fs2` and stores the result in `total`.
11. Not equal to.
12. Whether a path is a regular file.
13. No; Bash performs integer arithmetic by default.
14. `$( ... )` captures command output; `$(( ... ))` calculates arithmetic.

---

# 40. Command Reference

| Task | Syntax |
|---|---|
| Add numbers | `echo $((10 + 5))` |
| Subtract numbers | `echo $((10 - 3))` |
| Store addition result | `total=$((num1 + num2))` |
| Store subtraction result | `difference=$((num1 - num2))` |
| Increment | `((number++))` |
| Decrement | `((number--))` |
| Add to existing value | `((total += 5))` |
| Subtract from value | `((total -= 3))` |
| First script argument | `$1` |
| Second script argument | `$2` |
| Argument count | `$#` |
| Capture command output | `value=$(command)` |
| Get file size with `du` | `du -b "$file" \| cut -f1` |
| Get file size with `stat` | `stat -c %s "$file"` |

---

# 41. Lab Completion Checklist

- [ ] I understand arithmetic expansion.
- [ ] I can add two integers.
- [ ] I can subtract two integers.
- [ ] I can store a result in a variable.
- [ ] I understand `$1`, `$2`, and `$#`.
- [ ] I can use command substitution.
- [ ] I can obtain a file size in bytes.
- [ ] I can validate the number of arguments.
- [ ] I can verify that a file exists.
- [ ] I completed the calculator challenge.

---

# Summary

Bash arithmetic expressions are placed inside:

```bash
$(( expression ))
```

Addition example:

```bash
total=$((num1 + num2))
```

Subtraction example:

```bash
difference=$((num1 - num2))
```

Command substitution captures command output:

```bash
size=$(du -b "$1" | cut -f1)
```

Command-line arguments are accessed with:

```bash
$1
$2
```

A practical file-size addition script can calculate:

```bash
total=$((fs1 + fs2))
```

Important points:

- Bash arithmetic uses integers by default.
- Use `$(( ... ))` for calculations.
- Use `$( ... )` for command substitution.
- Quote file names and variables.
- Validate command-line arguments.
- Check that files exist before processing them.

The next lesson will cover multiplication and division in Bash.