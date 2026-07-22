# Bash Practice Lab: Display a Calendar for a Given Year

## Challenge

Create a Bash script named `cal.sh` that:

- Prompts the user to enter a year.
- Stores the year in a variable.
- Displays the complete calendar for that year.
- Uses the `cal` command.

---

## Learning Objectives

By the end of this lab, students will be able to:

- Create and execute a Bash script.
- Use `read` to collect input.
- Store input in a variable.
- Use a variable with another Linux command.
- Add basic input validation.
- Understand simple `if` statements and exit codes.

---

## Estimated Time

**15–20 minutes**

---

## Prerequisites

You need:

- Access to a Linux terminal.
- A text editor such as `vi`, `vim`, or `nano`.
- The `cal` command installed.

Check whether `cal` is available:

```bash
command -v cal
```

A successful result may look like:

```text
/usr/bin/cal
```

Test the command:

```bash
cal
```

This displays the current month's calendar.

To display an entire year, run:

```bash
cal 2026
```

---

# Part 1: Create the Basic Script

## Step 1: Go to Your Home Directory

```bash
cd ~
```

Confirm your current location:

```bash
pwd
```

---

## Step 2: Create `cal.sh`

```bash
vi cal.sh
```

Press `i` to enter insert mode.

---

## Step 3: Add the Shebang

Enter:

```bash
#!/bin/bash
```

### Explanation

The first line is called the **shebang**. It tells Linux to use Bash to execute the script.

---

## Step 4: Ask the User for a Year

Add:

```bash
read -r -p "Enter a year: " year
```

### Explanation

| Part | Purpose |
|---|---|
| `read` | Reads input from the keyboard |
| `-r` | Reads the input without treating backslashes specially |
| `-p` | Displays a prompt |
| `"Enter a year: "` | Message shown to the user |
| `year` | Variable that stores the input |

If the user enters:

```text
2026
```

the value is stored in the variable named `year`.

Use `$year` to access the stored value.

---

## Step 5: Test the Variable

Add:

```bash
echo "You entered: $year"
```

The script should now be:

```bash
#!/bin/bash

read -r -p "Enter a year: " year

echo "You entered: $year"
```

Save and exit from `vi`:

1. Press `Esc`.
2. Type `:wq`.
3. Press `Enter`.

---

## Step 6: Make the Script Executable

```bash
chmod +x cal.sh
```

Check the permissions:

```bash
ls -l cal.sh
```

The permissions should include `x`, for example:

```text
-rwxr-xr-x. 1 student student 81 Jul 17 10:00 cal.sh
```

---

## Step 7: Run the Script

```bash
./cal.sh
```

Example:

```text
Enter a year: 2026
You entered: 2026
```

This proves that `read` stored the input successfully.

---

# Part 2: Display the Calendar

Edit the script:

```bash
vi cal.sh
```

Replace its contents with:

```bash
#!/bin/bash

read -r -p "Enter a year: " year

cal "$year"
```

Save and run it:

```bash
./cal.sh
```

Example:

```text
Enter a year: 2026
```

The script will display all twelve months of 2026.

---

# Basic Solution

```bash
#!/bin/bash

read -r -p "Enter a year: " year

cal "$year"
```

---

# Explanation of the Basic Solution

## `#!/bin/bash`

```bash
#!/bin/bash
```

Tells Linux to execute the file using Bash.

## `read`

```bash
read -r -p "Enter a year: " year
```

Displays a prompt and stores the user's answer in the variable `year`.

## `cal "$year"`

```bash
cal "$year"
```

Passes the value stored in `year` to the `cal` command.

If the user entered `2026`, Bash effectively runs:

```bash
cal 2026
```

The quotation marks around `"$year"` are good Bash practice. They help prevent unwanted word splitting and pathname expansion.

---

# Improved Student-Friendly Solution

```bash
#!/bin/bash

echo "================================="
echo "       Year Calendar Tool"
echo "================================="

read -r -p "Enter a year: " year

echo
echo "Displaying the calendar for $year..."
echo

cal "$year"
```

Example interaction:

```text
=================================
       Year Calendar Tool
=================================
Enter a year: 2026

Displaying the calendar for 2026...
```

The complete calendar then appears below the message.

---

# Part 3: Add Input Validation

The basic script does not prevent invalid input such as:

```text
hello
```

or an empty value.

We can add two checks.

---

## Check 1: Prevent Empty Input

```bash
if [[ -z "$year" ]]; then
    echo "Error: You did not enter a year."
    exit 1
fi
```

### Explanation

- `if` begins a condition.
- `[[ ... ]]` performs a Bash test.
- `-z` checks whether a value is empty.
- `exit 1` stops the script and reports an error.
- `fi` ends the condition.

---

## Check 2: Require Four Digits

```bash
if [[ ! "$year" =~ ^[0-9]{4}$ ]]; then
    echo "Error: Enter a four-digit year, such as 2026."
    exit 1
fi
```

### Pattern Explanation

```text
^[0-9]{4}$
```

| Part | Meaning |
|---|---|
| `^` | Beginning of the input |
| `[0-9]` | Any digit from 0 through 9 |
| `{4}` | Exactly four digits |
| `$` | End of the input |

The `!` means **not**.

The condition therefore means:

> If the input is not exactly four digits, display an error.

Valid examples:

```text
1999
2026
2050
```

Invalid examples:

```text
26
hello
2026abc
20 26
```

---

# Complete Validated Solution

```bash
#!/bin/bash

echo "================================="
echo "       Year Calendar Tool"
echo "================================="

read -r -p "Enter a four-digit year: " year

# Check whether the user entered anything
if [[ -z "$year" ]]; then
    echo "Error: You did not enter a year."
    exit 1
fi

# Check whether the input contains exactly four digits
if [[ ! "$year" =~ ^[0-9]{4}$ ]]; then
    echo "Error: Enter a valid four-digit year, such as 2026."
    exit 1
fi

echo
echo "Displaying the calendar for $year..."
echo

cal "$year"
```

---

# Line-by-Line Review

```bash
#!/bin/bash
```

Uses Bash to execute the script.

```bash
read -r -p "Enter a four-digit year: " year
```

Collects input and stores it in `year`.

```bash
if [[ -z "$year" ]]; then
```

Checks whether the user entered nothing.

```bash
exit 1
```

Stops the script with an error status.

```bash
if [[ ! "$year" =~ ^[0-9]{4}$ ]]; then
```

Checks that the input contains exactly four digits.

```bash
cal "$year"
```

Displays the full calendar for the selected year.

---

# Testing the Script

## Test 1: Valid Year

Run:

```bash
./cal.sh
```

Input:

```text
2026
```

Expected result: the complete 2026 calendar appears.

---

## Test 2: Empty Input

Press `Enter` without typing a year.

Expected output:

```text
Error: You did not enter a year.
```

---

## Test 3: Letters

Input:

```text
hello
```

Expected output:

```text
Error: Enter a valid four-digit year, such as 2026.
```

---

## Test 4: Two-Digit Year

Input:

```text
26
```

Expected output:

```text
Error: Enter a valid four-digit year, such as 2026.
```

---

## Test 5: Letters and Numbers

Input:

```text
2026abc
```

Expected output:

```text
Error: Enter a valid four-digit year, such as 2026.
```

---

# Understanding Variables

The variable is named:

```bash
year
```

The `read` command assigns a value to it:

```bash
read -r -p "Enter a year: " year
```

Do not use `$` when assigning or naming the variable:

```bash
year
```

Use `$` when retrieving its value:

```bash
$year
```

Example:

```bash
echo "Selected year: $year"
```

---

# Understanding Exit Codes

Linux commands and scripts return an exit code.

| Exit code | Meaning |
|---:|---|
| `0` | Success |
| Non-zero | Error |

The command:

```bash
exit 1
```

stops the script and returns an error code.

After the script finishes, check its exit code:

```bash
echo $?
```

---

# Student Practice Tasks

## Task 1: Current Month

Create `current_cal.sh`:

```bash
#!/bin/bash

cal
```

Make it executable and run it.

---

## Task 2: Selected Year

Create a script that asks for a year and displays its full calendar.

---

## Task 3: Add a Heading

Modify the output so it displays:

```text
Calendar Report
Selected year: 2026
```

before the calendar.

---

## Task 4: Ask for a Month and Year

Create `month_cal.sh`:

```bash
#!/bin/bash

read -r -p "Enter the month number: " month
read -r -p "Enter the year: " year

cal "$month" "$year"
```

Example:

```text
Enter the month number: 7
Enter the year: 2026
```

This displays July 2026.

---

# Final Challenge: Calendar Menu

Create `calendar_tool.sh` with three options:

```text
1. Display the current month
2. Display a specific year
3. Display a specific month and year
```

## Final Challenge Solution

```bash
#!/bin/bash

echo "============================="
echo "        Calendar Tool"
echo "============================="
echo "1. Display the current month"
echo "2. Display a specific year"
echo "3. Display a specific month and year"
echo

read -r -p "Enter your choice: " choice

case "$choice" in
    1)
        cal
        ;;

    2)
        read -r -p "Enter a four-digit year: " year

        if [[ ! "$year" =~ ^[0-9]{4}$ ]]; then
            echo "Error: Invalid year."
            exit 1
        fi

        cal "$year"
        ;;

    3)
        read -r -p "Enter the month number (1-12): " month
        read -r -p "Enter a four-digit year: " year

        if [[ ! "$month" =~ ^[0-9]+$ ]] || (( month < 1 || month > 12 )); then
            echo "Error: The month must be between 1 and 12."
            exit 1
        fi

        if [[ ! "$year" =~ ^[0-9]{4}$ ]]; then
            echo "Error: Invalid year."
            exit 1
        fi

        cal "$month" "$year"
        ;;

    *)
        echo "Error: Invalid menu selection."
        exit 1
        ;;
esac
```

---

# Troubleshooting

## Permission Denied

Error:

```text
bash: ./cal.sh: Permission denied
```

Solution:

```bash
chmod +x cal.sh
./cal.sh
```

## Command Not Found

Error:

```text
cal: command not found
```

Check:

```bash
command -v cal
```

If no path is shown, ask the instructor or system administrator to install the package that provides `cal`.

## Script Not Found

Run the script with:

```bash
./cal.sh
```

The `./` means that the script is in the current directory.

## Bad Interpreter

Confirm that the first line is exactly:

```bash
#!/bin/bash
```

---

# Knowledge Check

1. What does the shebang do?
2. What does `read` do?
3. What does the `-p` option do?
4. Where is the entered year stored?
5. How do you access the value of `year`?
6. What does `cal "$year"` do?
7. What does `-z "$year"` check?
8. What does `exit 1` do?
9. Why do we use quotation marks around `"$year"`?
10. What does `chmod +x cal.sh` do?

---

# Knowledge Check Answers

1. It tells Linux to execute the script using Bash.
2. It collects input from the keyboard.
3. It displays a prompt.
4. In the variable named `year`.
5. Use `$year`.
6. It displays the full calendar for the selected year.
7. It checks whether the variable is empty.
8. It stops the script and returns an error status.
9. They prevent unwanted word splitting and pathname expansion.
10. It adds execute permission.

---

# Lab Completion Checklist

- [ ] I created `cal.sh`.
- [ ] I added the Bash shebang.
- [ ] I used `read` to collect a year.
- [ ] I stored the input in a variable.
- [ ] I used `$year` to retrieve the value.
- [ ] I used `cal "$year"`.
- [ ] I made the script executable.
- [ ] I tested valid input.
- [ ] I tested invalid input.
- [ ] I completed the final challenge.

---

# Instructor Validation

```bash
ls -l cal.sh
cat cal.sh
./cal.sh
```

A correct script should:

1. Be named `cal.sh`.
2. Use a valid Bash shebang.
3. Ask the user for a year.
4. Store the response in a variable.
5. Use the variable with `cal`.
6. Display the selected year's calendar.
7. Handle invalid input when validation is included.

---

# Cleanup

```bash
rm -i cal.sh current_cal.sh month_cal.sh calendar_tool.sh
```