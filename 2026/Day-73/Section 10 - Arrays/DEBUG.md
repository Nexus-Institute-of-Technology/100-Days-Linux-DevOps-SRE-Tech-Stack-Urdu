## Here are some valuable DEBUGGING methods in Bash Scripting

### Summary Checklist

#### Using Execution Tracing (`-x` / `+x`)
Using the **`-x`** flag (known as **xtrace** or execution tracing) tells Bash to print every single command to stderr right before executing it. It prefixes each traced line with a `+` symbol (defined by the `$PS4` prompt variable) so students can distinguish executed shell code from standard output. 

* **`-x` (Minus = Turn ON):** Enables debugging/tracing.
* **`+x` (Plus = Turn OFF):** Disables debugging/tracing.

| Goal | Command / Code | Notes |
| :--- | :--- | :--- |
| **Show lines as they run (CLI)** | `bash -x script.sh` | Runs entire script with tracing without modifying the file. |
| **Enable tracing via Shebang** | `#!/bin/bash -x` | Enables tracing automatically when executing directly (`./script.sh`). |
| **Enable tracing mid-script** | `set -x` | Turns ON tracing from this line onward. |
| **Disable tracing mid-script** | `set +x` | Turns OFF tracing (useful for hiding messy boilerplate or loops). |
| **Pause line-by-line (Interactive)** | Add `trap 'read -p "Line $LINENO > "' DEBUG` and `set -x` | Pauses execution before every line until you press **Enter**. |
| **Dry Run (Check syntax only)** | `bash -n script.sh` | Checks for syntax errors without executing any commands. |