# MODULE 02/07 – Practice Lab: Pattern Matching Files Using Shell Expansions
> **Hands-on Practice Lab – Bash Mein Pattern Matching (Globbing) (Roman Urdu)**

---

# 🎯 Lab Ka Maqsad

Is practice lab mein aap seekhenge:

- Bash shell mein Pattern Matching (Globbing) ka istemal.
- Wildcard characters ki madad se files ko match karna.
- File names ko prefixes aur patterns ki bunyaad par search karna.
- Mukhtalif length ke file names ko match karna.
- Bash Shell Expansions ka introduction hasil karna.
- Aane wale topics jaise Tilde Expansion, Brace Expansion, Variable Expansion aur Command Substitution ke liye tayyar hona.

---

# 📖 Introduction

**Bash Shell** command line ko process karte waqt kai qisam ki **Shell Expansions** perform karta hai jo commands ko likhna aur files ke saath kaam karna bohot aasaan bana deti hain.

Common Shell Expansions ye hain:

- Pattern Matching (Globbing)
- Tilde Expansion
- Brace Expansion
- Variable Expansion
- Command Substitution

Is practice lab mein hum sirf **Pattern Matching (Globbing)** ko seekhenge.

Pattern Matching ki madad se aap wildcard characters use karke multiple files ko match kar sakte hain, bina har filename manually type kiye.

---

# Pattern Matching Kya Hai?

Pattern Matching Bash Shell ka ek feature hai jo wildcard characters ki madad se automatically file names ko match karta hai.

Har file ka naam likhne ki zarurat nahi hoti.

Misal ke taur par:

```bash
ls *.txt
```

Ye tamam `.txt` files ko display karega.

---

# Common Wildcards

| Wildcard | Matlab |
|----------|---------|
| `*` | Zero ya us se zyada characters ko match karta hai |
| `?` | Sirf ek character ko match karta hai |
| `[abc]` | Brackets ke andar diye gaye characters mein se kisi ek ko match karta hai |
| `[a-z]` | Ek range ke kisi bhi character ko match karta hai |

---

# 🔬 Lab 1 – Practice Directory Create Karein

`glob` naam ki directory create karein.

```bash
mkdir glob
```

Us directory ke andar jayein.

```bash
cd glob
```

Apni location verify karein.

```bash
pwd
```

---

# 🔬 Lab 2 – Practice Files Create Karein

`touch` command ki madad se multiple files create karein.

```bash
touch able alpha baker bravo cast charlie delta dog easy echo
```

Files verify karein.

```bash
ls
```

Expected Output:

```text
able
alpha
baker
bravo
cast
charlie
delta
dog
easy
echo
```

---

# 🔬 Lab 3 – Tamam Files Display Karein

Current directory ki tamam files dekhein.

```bash
ls
```

Ye command sari available files display karegi.

---

# 🔬 Lab 4 – Sirf "a" Se Shuru Hone Wali Files Dhoondein

Agar aap sirf woh files dekhna chahte hain jo **a** se shuru hoti hain.

Command:

```bash
ls a*
```

Expected Output:

```text
able
alpha
```

### Explanation

| Pattern | Matlab |
|----------|---------|
| `a*` | Har woh filename jo **a** se start hota hai |

---

# 🔬 Lab 5 – Jin Files Mein "a" Mojood Ho Unhein Dhoondein

Agar aap har woh filename dekhna chahte hain jis mein kahin bhi **a** ho.

Command:

```bash
ls *a*
```

Expected Output:

```text
able
alpha
baker
bravo
cast
charlie
delta
easy
```

### Explanation

| Pattern | Matlab |
|----------|---------|
| `*a*` | Har woh filename jis mein kahin bhi **a** mojood ho |

---

# 🔬 Lab 6 – "a" Ya "c" Se Shuru Hone Wali Files Dhoondein

Agar aap sirf woh files dekhna chahte hain jo:

- **a** se start hoti hon.
- Ya **c** se start hoti hon.

Command:

```bash
ls [ac]*
```

Expected Output:

```text
able
alpha
cast
charlie
```

### Explanation

| Pattern | Matlab |
|----------|---------|
| `[ac]*` | Har woh filename jo **a** ya **c** se shuru hota hai |

---

# 🔬 Lab 7 – Sirf 4 Characters Wali Files Dhoondein

Agar aap sirf un file names ko dekhna chahte hain jin ki length **4 characters** ho.

Command:

```bash
ls ????
```

Expected Output:

```text
able
cast
easy
echo
```

### Explanation

Har `?` sirf **ek character** ko represent karta hai.

```text
???? = Total 4 Characters
```

---

# 🔬 Lab 8 – Sirf 5 Characters Wali Files Dhoondein

Agar aap sirf un file names ko dekhna chahte hain jin ki length **5 characters** ho.

Command:

```bash
ls ?????
```

Expected Output:

```text
alpha
baker
bravo
delta
```

### Explanation

```text
????? = Total 5 Characters
```

---

# Wildcards Ko Samjhein

## Asterisk (`*`)

`*` match karta hai:

- Zero characters
- Ek character
- Ya bohot saare characters

Examples:

```bash
ls *.txt
```

Tamam text files.

```bash
ls a*
```

Har file jo **a** se shuru hoti hai.

---

## Question Mark (`?`)

`?` sirf **ek character** ko match karta hai.

Examples:

```bash
ls ?
```

Sirf ek-character wale file names.

```bash
ls ????
```

Sirf 4-character wale file names.

---

## Square Brackets (`[]`)

Brackets ke andar diye gaye characters mein se sirf ek character ko match karta hai.

Example:

```bash
ls [ab]*
```

Ye sirf un file names ko match karega jo:

- **a**
- Ya **b**

se shuru hote hain.

---

# Practice Examples

| Command | Result |
|----------|--------|
| `ls *` | Tamam files display kare |
| `ls a*` | Sirf **a** se shuru hone wali files |
| `ls *a*` | Jin file names mein **a** mojood ho |
| `ls [ac]*` | **a** ya **c** se shuru hone wali files |
| `ls ????` | Sirf 4-character wale file names |
| `ls ?????` | Sirf 5-character wale file names |

---

# Directory Structure

```text
glob
├── able
├── alpha
├── baker
├── bravo
├── cast
├── charlie
├── delta
├── dog
├── easy
└── echo
```

---

# 🧪 Practice Exercises

---

## Exercise 1

Practice directory create karein.

```bash
mkdir glob
cd glob
```

---

## Exercise 2

Ye files create karein.

```bash
touch apple alpha bat ball cat cake dog eagle
```

---

## Exercise 3

Sirf **a** se shuru hone wali files display karein.

```bash
ls a*
```

---

## Exercise 4

Jin file names mein **a** ho unhein display karein.

```bash
ls *a*
```

---

## Exercise 5

Sirf **b** ya **c** se shuru hone wali files display karein.

```bash
ls [bc]*
```

---

## Exercise 6

Sirf 4-character wale file names display karein.

```bash
ls ????
```

---

## Exercise 7

Sirf 5-character wale file names display karein.

```bash
ls ?????
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1

Koi file pattern ko match nahi karti.

Command:

```bash
ls z*
```

Output:

```text
ls: cannot access 'z*': No such file or directory
```

Matlab koi bhi file **z** se start nahi hoti.

---

### Scenario 2

Aap ne zyada `?` use kar diye.

Example:

```bash
ls ???????
```

Sirf 7-character wale file names match honge.

---

### Scenario 3

Multiple starting letters match karne hain.

Command:

```bash
ls [abc]*
```

Ye sirf un file names ko match karega jo:

- a
- b
- c

se shuru hote hain.

---

### Scenario 4

Tamam files display karni hain.

Command:

```bash
ls *
```

`*` wildcard tamam file names ko match karta hai.

---

# 📌 Quick Revision

| Pattern | Matlab |
|----------|---------|
| `*` | Zero ya us se zyada characters |
| `?` | Sirf ek character |
| `[ab]` | **a** ya **b** mein se koi ek |
| `a*` | **a** se shuru hone wali files |
| `*a*` | Jin mein **a** mojood ho |
| `????` | Sirf 4-character wale file names |
| `?????` | Sirf 5-character wale file names |

---

# 📖 Key Takeaways

- Pattern Matching Bash Shell perform karta hai.
- `*` zero ya us se zyada characters ko match karta hai.
- `?` sirf ek character ko match karta hai.
- `[]` diye gaye characters mein se kisi ek ko match karta hai.
- Pattern Matching multiple files ke saath kaam karna bohot aasaan bana deti hai.
- Globbing Linux System Administrator ki roz marrah ki bohot important skill hai.

---

# 💡 Yaad Rakhein

> **Pattern Matching ko ek Search Filter ki tarah samjhein.**
>
> - `*` ka matlab hai **"Jo bhi ho, sab match karo."**
> - `?` ka matlab hai **"Sirf ek character match karo."**
> - `[abc]` ka matlab hai **"Sirf in teen characters mein se kisi ek ko match karo."**
>
> **Golden Rules Yaad Rakhein:**
>
> ```text
> *      = Bohot saare characters
> ?      = Sirf ek character
> [abc]  = List mein se ek character
> ```
>
> **Agar aap Pattern Matching achhi tarah seekh lein to Linux mein files ke saath kaam karna bohot tez aur aasaan ho jata hai.**