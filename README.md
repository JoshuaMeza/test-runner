# Test Runner

Test Runner (`test-runner`) is a command-line tool to test a code file against stdin/stdout test cases written in a single, easy to read and write test cases file (`spec.inout`).

## Installation and usage

Install using the following command:

```txt
npm i -g uadyfmat/test-runner
```

The general API of the CLI is the following:

```txt
Usage: test-runner [options] [dir]
```

Run `test-runner --help` to get a more thorough help message.

`[dir]` expects a [valid coding exercise project structure](#coding-exercise-project-structure).

**Notice that you must separately install the language-specific compiler, runtime or interpreter for your language of choice.**

|Language| Required available commands |
|---|---|
|Java|`javac` and `java`|
|C|`gcc`|
|C++|`g++`|
|Python|`python`|

For each of the commands, you can usually learn if they are available by running `[command] --help` or `[command] --version`. For instance, `gcc --version` should print the version of the C compiler.

## Coding exercise project structure

Test Runner works on a directory like `exercise-name` (the name of the folder could be anything), as shown below. A `Solution` file in a [supported language](#supported-languages) is required, as well as a [valid `spec.inout`](#test-cases-file-specinout) file. A `README.md` is the recommended way to explain what the exercise is about, although is not required by the CLI.

```txt
.
└── exercise-name
    ├── README.md (optional)
    ├── Solution.java
    └── spec.inout
```

## Test cases file (`spec.inout`)

`spec.inout` is the file where test cases are defined. A single test case has the following structure:

```txt
//
The data for the test case is placed between two
double slashes (//). This is provided to the Solution
file as standard input. Multi-line input is supported.
//
$$
The expected output from the Solution file is
placed between double dollar symbols ($$). This
should be the standard output of the Solution.
Multi-line output is supported.
$$
```

The file `spec.inout` can contain as many of these `//in//$$out$$` pairs as required test cases.

## Supported languages

Can be found by running: `test-runner --help`.

Supporting a new **compiled** language requires the following changes:

- Update [compile](./src/shell/compile) and [run](./src/shell/run) to add the required commands.
- Update [clean](./src/shell/clean) to remove the new compilation output.
- Update [config.js](./src/config.js) to add the new supported source language extension.
- Update this README file to add the required commands in the section [Installation and usage](#installation-and-usage).

If the language is **interpreted**, it is only required to update [run](./src/shell/run), [config.js](./src/config.js) and this README.

## Development

### Requirements

- Node.js

When using Windows, use GitBash, not CMD.

### Code guidelines

Two types of JavaScript files are used:

1. 'Class-like', where each file defines a constructor function and methods attached to the constructed object. This is similar to Java's idea of 1 class per file. Here, the constructor function is the first line after imports, then prototype attachments follow, then private functions and finally the export of the constructor.
2. 'Free-form', where the file has a config object, does some imports, or performs or is something else which would not benefit from a constructor function.

Naming conventions:

- JS file type #1: Use `PascalCase` for the file name, equal to the name of the constructor function.
- JS file type #2: Use `camelCase` for the file name, whatever makes sense.

Order of imports:

1. Local imports
2. Node.js imports
3. Third-party imports

### Sample `Solution` and `spec.inout` files

The folder [debugging](./debugging) contains a sample exercise, palindromes, with its `spec.inout` and `Solution` files. For debugging, run test-runner against a specific solution, e.g.:

```bash
./src/cli.js --language py debugging/palindromes
```

Or to use the installed CLI:

```bash
test-runner --language py debugging/palindromes
```