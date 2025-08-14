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

## Installation

This script requires Python 3.6 or higher. If you don't have Python installed, you can download it from the official Python website: [https://www.python.org/downloads/](https://www.python.org/downloads/)

To check your Python version, run the following command:

```sh
python --version
```

## Usage

The script `svara_counter.py` is a command-line tool that can process Devanagari text from a string, a file, a directory, or a glob pattern.

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

## More Examples

Here are some more examples of how to use the script with the files in the `examples` directory.

### 1. Process a single file

```sh
$ python svara_counter.py -p examples/gita_4_7.txt
   32 | gita_4_7.txt
```

### 2. Process a single file with explanation

```sh
$ python svara_counter.py -p examples/gita_4_7.txt --explain
Legend: S=Svara, V=Vyanjana, M=Matra, H=Halant, Sep=Separator, O=Other
        V+M=Vyanjana+Matra, V+H=Vyanjana+Halant
--------------------
File: examples/gita_4_7.txt
Akshara Count: 32

Explanation for: "यदा यदा हि धर्मस्य ग्लानिर्भवति भारत । अभ्युत्थानमधर्मस्य तदात्मानं सृजाम्यहम् ॥..."
Type | Counted | Count | Chars
-----------------------------------
V    | Y       | 1     | य
V+M  | Y       | 2     | दा
Sep  | -       | 2     |  
V    | Y       | 3     | य
V+M  | Y       | 4     | दा
Sep  | -       | 4     |  
V+M  | Y       | 5     | हि
Sep  | -       | 5     |  
V    | Y       | 6     | ध
V+H  | -       | 6     | र्
V    | Y       | 7     | म
V+H  | -       | 7     | स्
V    | Y       | 8     | य
Sep  | -       | 8     |  
V+H  | -       | 8     | ग्
V+M  | Y       | 9     | ला
V+M  | Y       | 10    | नि
V+H  | -       | 10    | र्
V    | Y       | 11    | भ
V    | Y       | 12    | व
V+M  | Y       | 13    | ति
Sep  | -       | 13    |  
V+M  | Y       | 14    | भा
V    | Y       | 15    | र
V    | Y       | 16    | त
Sep  | -       | 16    |  
O    | -       | 16    | ।
Sep  | -       | 16    |  
S    | Y       | 17    | अ
V+H  | -       | 17    | भ्
V+M  | Y       | 18    | यु
V+H  | -       | 18    | त्
V+M  | Y       | 19    | था
V    | Y       | 20    | न
V    | Y       | 21    | म
V    | Y       | 22    | ध
V+H  | -       | 22    | र्
V    | Y       | 23    | म
V+H  | -       | 23    | स्
V    | Y       | 24    | य
Sep  | -       | 24    |  
V    | Y       | 25    | त
V+M  | Y       | 26    | दा
V+H  | -       | 26    | त्
V+M  | Y       | 27    | मा
V+M  | Y       | 28    | नं
Sep  | -       | 28    |  
V+M  | Y       | 29    | सृ
V+M  | Y       | 30    | जा
V+H  | -       | 30    | म्
V    | Y       | 31    | य
V    | Y       | 32    | ह
V+H  | -       | 32    | म्
Sep  | -       | 32    |  
Sep  | -       | 32    | ॥
-----------------------------------
```

### 3. Process a directory

```sh
$ python svara_counter.py -p examples
Processing files in: examples
   32 | gita_2_47.txt
   32 | gita_4_7.txt
    2 | rama.txt
    3 | sundara.txt
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