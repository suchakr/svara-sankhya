# Svara Sankhya

An initiative to count *aksharas* (syllables) in Devanagari script, particularly for scriptural and poetic texts.

## Overview

This project provides a command-line tool for analyzing Devanagari text to count *aksharas*. Many traditional texts are structured hierarchically, with divisions like chapters (*kāṇḍa*), sections (*sarga*), etc. The names and depth of these levels can vary from one text to another and must often be inferred.

The fundamental unit for counting is typically a verse (*shloka*) or a block of text, which may be accompanied by a verse number. The goal is to process these units, count the *aksharas* within them, and provide counts that can be aggregated across the text's hierarchy.

## Akshara Counting Methodology

The core of the counting process is based on identifying the vowels (*svara*) in the text. Each *svara* is counted as a single *akshara*. The process involves:

1.  **Splitting**: The text is logically split into its constituent consonants (*vyanjana*) and vowels (*svara*).
2.  **Counting**: The number of identified *svara* instances is counted.

A key rule is that a pure consonant followed by a vowel sound forms a countable syllable. This includes special cases like the *visarga* (`ः`).

### Examples

Here are a few examples illustrating the counting logic:

**Example 1:**

*   **Text**: `रमा`
*   **Split**: `र्` + `अ` + `म्` + `आ`
*   **Svaras**: `अ`, `आ`
*   **Akshara Count**: 2

**Example 2:**

*   **Text**: `लक्ष्मिनृसिम्हः`
*   **Split**: `ल्`+`अ` + `क्`+`श्`+`म्`+`इ` + `न्`+`ऋ` + `स्`+`इ` + `म्`+`ह्`+`अः`
*   **Svaras**: `अ`, `इ`, `ऋ`, `इ`, `अः`
*   **Akshara Count**: 5
    *Note: In this example, `हः` which is `ह्` + `अः` contributes one to the count, as `अः` is the vowel sound.*

## Usage

The script `svara_counter.py` is a command-line tool that can process Devanagari text from a string, a file, a directory, or a glob pattern.

### Command-Line Options

*   `-h`, `--help`: Show the help message and exit.
*   `-c`, `--check`: Run a check with predefined shlokas.
*   `-i INPUT`, `--input INPUT`: A single Devanagari string to process.
*   `-p PATH`, `--path PATH`: Path to a file, directory, or a glob pattern (e.g., `'*.txt'`). When using a glob pattern, it's recommended to quote it to prevent shell expansion.
*   `--explain`: Show a step-by-step explanation of the counting process.

### Examples

**1. Count aksharas from a string:**

```sh
python3 svara_counter.py -i "यदा यदा हि धर्मस्य"
```

**2. Count aksharas from a single file:**

```sh
python3 svara_counter.py -p my_text_file.txt
```

**3. Count aksharas from all `.txt` files in a directory:**

```sh
python3 svara_counter.py -p 'shlokas/*.txt'
```

**4. Run the predefined checks with a detailed explanation:**

```sh
python3 svara_counter.py --check --explain
```

## Example Output

Here is an example of the output when running the `--explain` flag:

```
Legend: S=Svara, V=Vyanjana, M=Matra, H=Halant, Sep=Separator, O=Other
        V+M=Vyanjana+Matra, V+H=Vyanjana+Halant
--------------------
Running predefined checks...
====================
Source: Bhagavad Gita 4.7
Text: "यदा यदा हि धर्मस्य ग्लानिर्भवति भारत । अभ्युत्थानमधर्मस्य तदात्मानं सृजाम्यहम् ॥"
Akshara Count: 32

Explanation for: "यदा यदा हि धर्मस्य ग्लानिर्भवति भारत । अभ्युत्थानमधर्मस्य तदात्मानं सृजाम्यहम् ॥..."
Type | Counted | Count | Chars
----------------------------------------------------------------------
V    | Y       | 1     | य
V+M  | Y       | 2     | दा
Sep  | -       | 2     |
V    | Y       | 3     | य
V+M  | Y       | 4     | दा
Sep  | -       | 4     |
V+M  | Y       | 5     | हि
... (output truncated for brevity)
```

## Aggregation

The syllable counts from individual verses or text blocks can be summed up to provide totals for any hierarchical level defined in the source text (e.g., total *aksharas* per section, per chapter, etc.).