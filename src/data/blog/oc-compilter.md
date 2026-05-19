---
author: Manmeet Singh
pubDatetime: 2016-01-28T00:00:00Z
# modDatetime: 2024-01-01T00:00:00:00Z  # uncomment if you edited the post after publishing
title: Compiler Design - OC
slug: oc-compiler
featured: false
draft: false
tags:
  - general
  - compilers
  - computerscience
description: Compiler design CMPS 104A at UCSC Fall 2015

---
*All source code can be found [HERE](https://github.com/manmeet3/OC-compiler)*

### SYNOPSIS
`oc [-ly] [-@ flag ...] [-D string] program.oc`

### OPTIONS
* **-@ flags**: Call `set_debugflags`, and use `DEBUGF` and `DEBUGSTMT` for debugging.
* **-D string**: Pass this option and its argument to `cpp`. Use `-D__OCLIB_OH__` to suppress inclusion of the code from `oclib.oh` when testing a program.
* **-l**: Debug `yylex()` with `yy_flex_debug = 1`.
* **-y**: Debug `yyparse()` with `yydebug = 1`.

### OUTPUT FILES
| Output Type | Filename |
| :--- | :--- |
| String set | `prog_name.str` |
| Scanned tokens | `prog_name.tok` |
| Abstract syntax tree | `prog_name.ast` |
| Symbol table | `prog_name.sym` |
| Interm. language | `prog_name.oil` |

---

## The 5 Stages of Compilation

1. **I. Preprocessor and token generation**
2. **II. Lexical Analyzer (using flex)**
3. **III. LALR(1) Parser (using bison)**
4. **IV. Symbols and Type Checking**
5. **V. Intermediate Language Emission**

---

## I. Preprocessor and String Set Generation
The first part of the compiler consisted of writing a main program for the language **oc**. Included in it was a **string set ADT** used to create unique string sets based on the output from the C preprocessor.

The purpose of the string set is to keep track of strings uniquely. For example, if "abc" is entered multiple times, it appears only once in the table. This allows the compiler to compare pointers instead of using `strcmp(3)`.

## II. Lexical Analyzer
The second part augmented the string table into a scanner written in **flex**. Tokens are identified based on regex in the `scanner.l` file.

* **Special symbols:** `[] ( ) [ ] { } ; , . = == != < <= > >= + - * / % !`
* **Reserved words:** `void bool char int string struct if else while return false true null ord chr new`
* **Identifiers:** Sequences of letters, digits, and underscores (cannot start with a digit).
* **Constants:** Supports Integer (decimal only), Char, and String constants.

## III. LALR Parser
This stage involved writing an LALR parser using **bison**.

* **AST Construction:** Operators become parents of their operands (children). Children can be leaf nodes (identifiers/constants) or other expression nodes.
*