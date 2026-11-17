Lab 6 — Answers

Name: Efe Deniz Işıkören
Student ID: [20241701066]

Part A — The calculator
A1. scripts/calculator.sh (full script):
#!/usr/bin/env bash

add() {
    echo "Result: $(($1 + $2))"
}

subtract() {
    echo "Result: $(($1 - $2))"
}

multiply() {
    echo "Result: $(($1 * $2))"
}

divide() {
    if [ "$2" -eq 0 ]; then
        echo "Error: Cannot divide by zero."
    else
        echo "Result: $(($1 / $2))"
    fi
}

while true
do
    echo "----- Calculator -----"
    echo "1) Add"
    echo "2) Subtract"
    echo "3) Multiply"
    echo "4) Divide"
    echo "5) Exit"

    read -r choice

    case $choice in
        1|2|3|4)
            echo "Enter first integer:"
            read -r num1
            echo "Enter second integer:"
            read -r num2

            case $choice in
                1) add "$num1" "$num2" ;;
                2) subtract "$num1" "$num2" ;;
                3) multiply "$num1" "$num2" ;;
                4) divide "$num1" "$num2" ;;
            esac
            ;;
        5)
            echo "Goodbye!"
            break
            ;;
        *)
            echo "Invalid choice. Please try again."
            ;;
    esac
done

Commands used to create and make the script executable:

nano scripts/calculator.sh
chmod +x scripts/calculator.sh
A2. Addition and division in one session (inputs and outputs), plus a one-sentence explanation of integer division:
$ ./scripts/calculator.sh
----- Calculator -----
1) Add
2) Subtract
3) Multiply
4) Divide
5) Exit
1
Enter first integer:
12
Enter second integer:
8
Result: 20
----- Calculator -----
1) Add
2) Subtract
3) Multiply
4) Divide
5) Exit
4
Enter first integer:
17
Enter second integer:
5
Result: 3
----- Calculator -----
1) Add
2) Subtract
3) Multiply
4) Divide
5) Exit
5
Goodbye!
Explanation: Bash uses integer division, so the decimal part of 17 / 5 is discarded and the result is 3.

A3. Division by zero, invalid choice, and exit (inputs and outputs; describe any fix if needed):
$ ./scripts/calculator.sh
----- Calculator -----
1) Add
2) Subtract
3) Multiply
4) Divide
5) Exit
4
Enter first integer:
9
Enter second integer:
0
Error: Cannot divide by zero.
----- Calculator -----
1) Add
2) Subtract
3) Multiply
4) Divide
5) Exit
9
Invalid choice. Please try again.
----- Calculator -----
1) Add
2) Subtract
3) Multiply
4) Divide
5) Exit
5
Goodbye!
Notes/fix (if needed): No fix was needed. The script handled division by zero and an invalid menu choice without crashing and returned to the menu.

Part B — Summarize command-line numbers

B1. scripts/summarize.sh (full script):
#!/usr/bin/env bash

is_even() {
    if [ $(($1 % 2)) -eq 0 ]; then
        return 0
    else
        return 1
    fi
}

if [ "$#" -eq 0 ]; then
    echo "Usage: ./scripts/summarize.sh number1 number2 ..."
    exit 1
fi

sum=0
even_count=0

for number in "$@"
do
    sum=$((sum + number))

    if is_even "$number"; then
        even_count=$((even_count + 1))
    fi
done

echo "Number of arguments: $#"
echo "Sum: $sum"
echo "Even numbers: $even_count"
Commands used to create and make the script executable:
nano scripts/summarize.sh
chmod +x scripts/summarize.sh

B2. Run with 4 7 2 9 (command and output):
$ ./scripts/summarize.sh 4 7 2 9
Number of arguments: 4
Sum: 22
Even numbers: 2

B3. Run with -3 0 8 and with no arguments (commands and outputs; describe any fix if needed):
$ ./scripts/summarize.sh -3 0 8
Number of arguments: 3
Sum: 5
Even numbers: 2

$ ./scripts/summarize.sh
Usage: ./scripts/summarize.sh number1 number2 ...
Notes/fix (if needed): No fix was needed. The script counted both 0 and 8 as even numbers and printed a usage message when no arguments were provided.

Part C — The file checker (Bonus +20)

C1. scripts/filecheck.sh
#!/usr/bin/env bash

check_path() {
    if [ -f "$1" ]; then
        echo "File found: $1"
        lines=$(wc -l < "$1")
        echo "Number of lines: $lines"
    elif [ -d "$1" ]; then
        echo "This is a directory, not a regular file."
    else
        echo "File not found: $1"
    fi
}

while true
do
    echo "Enter a file path (or type quit):"
    read -r path

    if [ "$path" = "quit" ]; then
        echo "Goodbye!"
        break
    fi

    check_path "$path"
done
Commands used to create, make executable, and run the script:
nano scripts/filecheck.sh
chmod +x scripts/filecheck.sh
./scripts/filecheck.sh
Enter a file path (or type quit):
sample.txt
File found: sample.txt
Number of lines: 3
Enter a file path (or type quit):
scripts
This is a directory, not a regular file.
Enter a file path (or type quit):
empty.txt
File found: empty.txt
Number of lines: 0
Enter a file path (or type quit):
quit
Goodbye!

C2. Create sample.txt with three lines, then check it (commands, interaction, and output):
$ printf 'alpha\nbeta\ngamma\n' > sample.txt
$ ./scripts/filecheck.sh
Enter a file path (or type quit):
sample.txt
File found: sample.txt
Number of lines: 3

C3. Check the scripts directory, an empty empty.txt, and quit (commands and interaction; describe any fix if needed):

The following output continued in the same filecheck.sh session used for C2.
$ > empty.txt

Enter a file path (or type quit):
scripts
This is a directory, not a regular file.
Enter a file path (or type quit):
empty.txt
File found: empty.txt
Number of lines: 0
Enter a file path (or type quit):
quit
Goodbye!

Notes/fix (if needed): No fix was needed. The script correctly identified the directory, recognized the empty file as a regular file with zero lines, and exited when quit was entered.
