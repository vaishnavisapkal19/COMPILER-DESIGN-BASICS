# COMPILER-DESIGN-BASICS

COMPANY NAME:- CODTECH IT SOLUTIONS

NAME:- SAPKAL VAISHNAVI GANESH 

INTERN ID:- 

DOMAIN:- C++

DURATION:- 8 WEEKS

MENTOR:- NEELA SANTOSH

DISCRIPTION:- 1. Purpose & High‑Level Overview
This program reads a line of input—a mathematical expression like

Copy
Edit
(3 + 4.5)* -2 / (1 - 0.5)
—and evaluates it as a double. It uses a recursive‑descent parser, a top‑down parsing technique where each function implements one grammar rule 
Wikipedia
+15
GeeksforGeeks
+15
Reddit
+15
.

The grammar it implements (informally expressed) is:

javascript
Copy
Edit
Expression → Term { ('+' | '-') Term }
Term       → Factor { ('*' | '/') Factor }
Factor     → ('+' | '-') Factor | '(' Expression ')' | Number
Number     → sequence of digits with optional '.'
The Parser class maintains the input string s and a position index i, using methods like peek(), skipWhitespace(), parseExpression(), parseTerm(), parseFactor(), and parseNumber().

2. Parsing Functions & Precedence
parseExpression()
Calls parseTerm() to evaluate the left operand.

In a loop, checks peek() for '+' or '-'; if found, advances i, parses the next term as right operand, and updates the accumulator (val += rhs or val -= rhs).

parseTerm()
Starts by calling parseFactor().

Then loops while peek() is '*' or '/'; processes the operator, re‑parses the next factor, and performs multiplication or division on the accumulator.

parseFactor()
Handles the "power‑up" portion of grammar:

If a leading '+' or '-' is seen, it consumes it, recursively calls parseFactor(), then applies the sign.

If '(', consumes it, recursively calls parseExpression(), then expects and consumes ')', throwing an error if missing.

Otherwise delegates to parseNumber().

This separation ensures correct precedence: * and / bind tighter than + and -, and unary +/- apply even tighter, as is standard 
GitHub
GitHub
.

3. Numbers and Whitespace
parseNumber() skips leading whitespace, then scans digits and at most one . using a while‑loop, ensuring it's a valid numeric literal. It throws a runtime_error if no number is found at the current position. Valid substrings are converted with std::stod, and the parser index i is advanced.

peek() first calls skipWhitespace() to ignore spaces and tabs, then returns s[i] or '\0' if at end-of-string.

4. Main Loop & Error Handling
In main(), the program:

Prompts "Enter expression…".

Reads lines until EOF (e.g. Ctrl+D).

Ignores blank lines.

Constructs a Parser, calls parseExpression(), then checks if peek() returns '\0'; if not, throws "Unexpected char" error.

Prints either = <result> or Error: <message>.

This approach allows the parser to report syntax errors like mismatched parentheses, extra trailing characters, or invalid tokens, using clear position‑aware messages.

5. Design Notes & Grammar Handling
Because it handles grammar non‑left‑recursively, the parser is predictive—no backtracking is required—and runs in essentially linear time relative to input length 
GeeksforGeeks
GeeksforGeeks
+2
GeeksforGeeks
+2
Wikipedia
+2
.

As discussed in programming discussions, “[if you’re parsing a language that includes arithmetic expressions…] it’s easier to build them into your recursive‑descent grammar instead” rather than use mixed algorithms like shunting‑yard separately 
Reddit
+5
Reddit
+5
Reddit
+5
.

6. Strengths & Possible Extensions
Strengths:
Simple, modular functions mirror grammar rules directly.

Handles unary operators, nested parentheses, decimal numbers.

Provides detailed error messages with character positions.

Readable and easy to modify—ideal for students learning about parsing.

Extensions you might add:
Support exponentiation (^) or other operators/functions (sin, log).

Better error recovery or clearer messages (e.g. report character and context).

Tokenization (lexing) as a separate step improving performance or readability.

Constructing an abstract syntax tree (AST) rather than evaluating on the fly.

Support variables and assignment (for a scripting-style interpreter).

7. Summary
In ~400 lines of C++, this code shows a clean, working recursive‑descent parser implementing a fully functional calculator with correct operator precedence and unary handling. Its architecture—parseExpression → parseTerm → parseFactor → parseNumber—reflects the standard pattern used in many textbook and real-world language‑parser implementations. With robust error messages and support for whitespace and decimal syntax, it’s a solid foundation, easy to build on further
