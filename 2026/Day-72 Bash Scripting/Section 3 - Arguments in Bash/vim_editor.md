Here is how to install **Vim** and configure it so line numbers appear automatically every time you open any file.

---

## Step 1: Install Vim

Open your terminal and run the command that matches your Linux distribution:

```bash
dnf install -y vim
```
## Step 2: Enable Line Numbers Permanently

Vim uses a configuration file in your home directory called `.vimrc`. Adding the `set number` directive to this file turns on line numbers globally across all files and scripts you open.

Run this single command to create/append the setting into your user's `.vimrc`:

```bash
echo "set number" >> ~/.vimrc
```
## Step 3: Verify the Setup
Open any file or script with Vim:

```Bash
vim test.sh
```
**NOTE: You will see line numbers down the left margin automatically.**

## System Admin Tips!
Pro-Tips for Line Numbers in Vim
If you ever want to toggle line numbers on or off while inside Vim (without editing .vimrc):
1. Turn numbers ON: Type :set number (or :set nu) and press Enter.
2. Turn numbers OFF: Type :set nonumber (or :set nonu) and press Enter.
3. Relative Line Numbers (useful for Vim navigation commands): Type :set relativenumber.
