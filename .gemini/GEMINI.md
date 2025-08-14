# GEMINI.md - Project Context for Gemini

This file provides context for the Svara Sankhya project to help Gemini understand the project and assist with development.

## Project Overview

Svara Sankhya is a Python command-line tool designed to count *aksharas* (syllables) in Devanagari text. The primary goal is to analyze scriptural and poetic texts, where such counts are often important. The tool can process text from a command-line string, a single file, a directory of files, or a glob pattern. It also provides a detailed explanation of the counting process for each character in the text.

The core logic is based on identifying vowels (*svara*) and consonants (*vyanjana*) in the Devanagari script. Each vowel sound is considered a single *akshara*.

The project consists of a single Python script, `svara_counter.py`, which uses the `argparse` library for command-line argument parsing.

## Building and Running

The project is a single Python script and does not require a build process.

### Running the script

The script can be run from the command line using `python`.

**Show help:**
```sh
python svara_counter.py --help
```

**Run predefined checks:**
```sh
python svara_counter.py --check
```

**Run predefined checks with explanation:**
```sh
python svara_counter.py --check --explain
```

**Process a string:**
```sh
python svara_counter.py -i "some Devanagari text"
```

**Process a single file:**
```sh
python svara_counter.py -p <file_path>
```

**Process a directory:**
```sh
python svara_counter.py -p <directory_path>
```

**Process files using a glob pattern:**
```sh
python svara_counter.py -p '*.txt'
```

## Development Conventions

*   The project uses standard Python 3.
*   Command-line arguments are handled by the `argparse` library.
*   The script is self-contained and has no external dependencies.
*   The code includes docstrings to explain the purpose of each function.
*   The `--explain` flag provides a detailed, human-readable explanation of the counting logic, which is useful for debugging and verification.