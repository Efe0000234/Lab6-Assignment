# Lab 6 Assignment

## Bash Arithmetic, Loops & Functions: Three Useful Tools

**Time:** 90 minutes

**Submit:** fill in `answers.md` and save your **three scripts** undes /scripts in the repo directory,
then commit and push.

**Deadline:** full marks until **12:00**. Pushed between 12:00 and 13:00 → **−20%**.
Pushed after 13:00 and before 23:59 on Tuesday → **−50%**. Nothing accepted after that.

---

## The situation

Your team is building a small collection of Bash tools. One teammate needs a calculator,
another needs a quick summary of numbers entered at the command line, and a third needs
a way to check whether a file exists before working with it.

Today you will write **three independent scripts**, then test them with both ordinary
inputs and situations that beginners often overlook.

**Set up your workspace:**

Save your scripts as:

```text
scripts/calculator.sh
scripts/summarize.sh
scripts/filecheck.sh
```

---

## Ground rules

Everything here can be done using **Labs 1–6**. Focus on the new Lab 6 material:

```text
$(( ))   (( ))   [ ]   [[ ]]   -eq   -lt   -f   -d
if/elif/else/fi   case/esac   while/do/done   for/do/done
$1   $#   "$@"   read -r   functions   local   return
```

You may also use familiar commands such as `echo`, `printf`, `cat`, `wc`, `chmod`,
`mkdir`, and your preferred editor

- Start **every script** with `#!/usr/bin/env bash` and make it executable.
- Use **integer arithmetic** only. For division, Bash discards the fractional part.
- You may choose your exact output wording, but it must clearly communicate the correct result.

> **For every question, write the command(s) you used and the output in `answers.md`.**
> For A1, B1, and C1, the script files serve as your answers
> We are marking how you got there, not just the final answer.

---

## Part A: The calculator

**A1.** Write **`scripts/calculator.sh`**, an interactive calculator. It must:

1. Display a menu repeatedly: **1) Add, 2) Subtract, 3) Multiply, 4) Divide, 5) Exit**.
2. Read the user's menu choice.
3. For choices 1–4, ask for **two integers**, perform the selected operation, and print the result.
4. Implement the four operations as **separate Bash functions** that receive two parameters.
5. Use `case` or `if/elif` to select the correct function, and a `while` loop to return to the menu after a calculation.
6. Exit cleanly when the user selects 5. For an unknown menu choice, print a helpful message and show the menu again.
7. **Never attempt division by zero**: print an error instead, then return to the menu.

Make the script executable

> _Hints: Use a while loop to keep displaying the menu, and `break` to
> exit the loop when the user selects 5.
> Inside each function, `$1` and `$2` refer to the two numbers passed to it.
> For calculations, use `$(( ... ))`._

**A2.** Run your calculator and perform these **two calculations in one session**:

- Add `12` and `8`.
- Divide `17` by `5`.

Show the menu choices, numbers entered, and results. In one sentence, explain why the
second result has no decimal part.

**A3.** Now try the input that might break an unfinished calculator:

- Choose **Divide**, enter `9` and `0`.
- At the next menu, enter an **invalid choice**, such as `9`.
- Then choose **Exit**.

Show what happened. If your script crashes or gets stuck, fix it and run the test again if needed.

---

## Part B: Summarize command-line numbers

**B1.** Write **`scripts/summarize.sh`**, which receives **one or more integers as command-line arguments**. It must:

1. Use a `for` loop to visit **every** argument, no matter how many are supplied.
2. Calculate the **sum** of all arguments using Bash arithmetic.
3. Count how many supplied numbers are **even** (including `0` and negative even numbers).
4. Print the number of arguments, the sum, and the even-number count.
5. If no arguments are supplied, print a usage message and stop without doing calculations.
6. Use a **function** to determine whether a number is even. It should receive the number as a parameter and use a numeric test or arithmetic test.

Make the script executable.

> _Hints: `$#` is the argument count, `for number in "$@"; do ... done` visits every
> argument, `% 2` calculates the remainder, and `return 0` / `return 1` can report a
> successful/unsuccessful test. Assume the supplied arguments are valid integers._

**B2.** Run your script with the following arguments:

```bash
./scripts/summarize.sh 4 7 2 9
```

Show the command and its output.

**B3.** Test **two cases** that might reveal an assumption in your script:

```bash
./scripts/summarize.sh -3 0 8
./scripts/summarize.sh
```

Show both outputs. If either test fails, adjust your script and explain in one sentence
what you changed.

---

## Part C: The file checker (Bonus +20)

**C1.** Write **`scripts/filecheck.sh`**, an interactive tool that keeps asking for a path.
It must:

1. Use a `while` loop to ask the user to enter a file path or type **`quit`**.
2. If the user enters `quit`, print a goodbye message and stop the script.
3. If the path refers to an **existing regular file**, say that it was found and print its number of lines.
4. If the path refers to a **directory**, explain that it is a directory, not a regular file.
5. If the path does not exist, say that it was not found.
6. After any check (except `quit`), ask for a path again.
7. Put the path-checking logic in a **function** that accepts the entered path as its parameter.

Make the script executable. Run it once and show that `quit` exits.

> _Hints: `read -r path`, `[ "$path" = "quit" ]`, `[ -f "$1" ]`, `[ -d "$1" ]`,
> `wc -l < "$1"`. **Quote the path** so filenames containing spaces also work._

**C2.** Create a file containing exactly these three lines, then use your checker to
inspect it:

```text
alpha
beta
gamma
```

You may create it with an editor or with the following command:

```bash
printf 'alpha\nbeta\ngamma\n' > sample.txt
```

Show the command that created the file, your interaction with `filecheck.sh`, and the
reported line count.

**C3.** Test your checker with **two less obvious inputs**:

1. Enter `scripts` (this is a **directory**, not a regular file).
2. Create an empty file named `empty.txt` using `> empty.txt`, then enter `empty.txt`.
3. Finally enter `quit`.

Show all three interactions. If the script treats the directory as a file or rejects the
empty file, fix it and explain briefly what changed.

> _Hint: Test for a regular file with `-f`, not just whether a path exists._

---

## Final checklist

Before pushing, make sure:

- `scripts/calculator.sh`, `scripts/summarize.sh`, and `scripts/filecheck.sh` exist and are executable.
- Your `answers.md` is filled and complete.
- Your answers show both ordinary tests and the requested edge cases.
- You have **committed and pushed** your work before the deadline.
