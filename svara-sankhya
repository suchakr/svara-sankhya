import argparse
import sys
import os
import glob

# Devanagari Unicode ranges
Svara = {
    'अ', 'आ', 'इ', 'ई', 'उ', 'ऊ', 'ऋ', 'ॠ', 'ऌ', 'ॡ', 'ए', 'ऐ', 'ओ', 'औ', 'अं', 'अः'
}

Vyanjana = {
    'क', 'ख', 'ग', 'घ', 'ङ', 'च', 'छ', 'ज', 'झ', 'ञ', 'ट', 'ठ', 'ड', 'ढ', 'ण',
    'त', 'थ', 'द', 'ध', 'न', 'प', 'फ', 'ब', 'भ', 'म', 'य', 'र', 'ल', 'व',
    'श', 'ष', 'स', 'ह', 'ळ', 'क्ष', 'ज्ञ'
}

Matra = {
    'ा', 'ि', 'ी', 'ु', 'ू', 'ृ', 'ॄ', 'ॢ', 'ॣ', 'े', 'ै', 'ो', 'ौ', 'ं', 'ः'
}

Halant = '्'
Avagraha = 'ऽ'

def count_aksharas(text):
    """Counts the number of aksharas (syllables) in a Devanagari string."""
    if not text:
        return 0

    count = 0
    text = text.strip()
    
    i = 0
    while i < len(text):
        char = text[i]

        if char in Svara:
            count += 1
            i += 1
            continue

        if char in Vyanjana:
            if i + 1 < len(text):
                next_char = text[i+1]
                if next_char in Matra:
                    count += 1
                    i += 2
                    continue
                elif next_char == Halant:
                    i += 2
                    continue
            
            count += 1
            i += 1
        else:
            i += 1
            
    return count

def explain_aksharas(text):
    """Generates a step-by-step explanation of the counting process."""
    if not text:
        print("No text to explain.")
        return

    type_map = {
        "Svara": "S",
        "Vyanjana": "V",
        "Vyanjana+Matra": "V+M",
        "Vyanjana+Halant": "V+H",
        "Matra": "M",
        "Halant": "H",
        "Separator": "Sep",
        "Other": "O"
    }

    text = text.strip()
    rows = []
    count = 0
    i = 0

    while i < len(text):
        char = text[i]
        char_type_full = "Other"
        counted = "-"
        increment = 0
        current_chars = char

        if char in Svara:
            char_type_full = "Svara"
            counted = "Y"
            count += 1
            increment = 1
        elif char in Vyanjana:
            char_type_full = "Vyanjana"
            if i + 1 < len(text):
                next_char = text[i+1]
                if next_char in Matra:
                    counted = "Y"
                    count += 1
                    char_type_full = "Vyanjana+Matra"
                    current_chars = f"{char}{next_char}"
                    rows.append((type_map[char_type_full], counted, count, current_chars))
                    i += 2
                    continue
                elif next_char == Halant:
                    counted = "-"
                    char_type_full = "Vyanjana+Halant"
                    current_chars = f"{char}{next_char}"
                    rows.append((type_map[char_type_full], counted, count, current_chars))
                    i += 2
                    continue
            
            counted = "Y"
            count += 1
            increment = 1
        elif char in Matra:
            char_type_full = "Matra"
            counted = "-"
        elif char in Halant:
            char_type_full = "Halant"
            counted = "-"
        elif char.isspace() or char == '|' or char == '॥':
            char_type_full = "Separator"
            counted = "-"
        
        rows.append((type_map[char_type_full], counted, count, current_chars))
        i += increment if increment else 1

    print(f'\nExplanation for: "{text[:100]}..."') # Print snippet
    headers = ["Type", "Counted", "Count", "Chars"]
    # Calculate column widths dynamically for all but the last column
    try:
        # Don't include the "Chars" column in width calculation
        cols_for_width_calc = [h for h in zip(*([headers[:-1]] + [r[:-1] for r in rows]))]
        col_widths = [max(len(str(item)) for item in col) for col in cols_for_width_calc]
    except ValueError: # Handles empty rows
        col_widths = [len(h) for h in headers[:-1]]

    # Add a fixed width for the Chars column, or don't specify one
    header_line = " | ".join(f"{h:<{w}}" for h, w in zip(headers[:-1], col_widths)) + " | " + headers[-1]
    print(header_line)
    print("-" * (len(header_line) + 5)) # A bit of padding

    for row in rows:
        # Don't pad the last column
        formatted_row = " | ".join(f"{str(item):<{w}}" for item, w in zip(row[:-1], col_widths))
        print(f"{formatted_row} | {row[-1]}")
    print("-" * (len(header_line) + 5))

def get_predefined_data():
    """Returns a list of tuples with sample shlokas and their source."""
    return [
        ("यदा यदा हि धर्मस्य ग्लानिर्भवति भारत । अभ्युत्थानमधर्मस्य तदात्मानं सृजाम्यहम् ॥", "Bhagavad Gita 4.7"),
        ("कर्मण्येवाधिकारस्ते मा फलेषु कदाचन । मा कर्मफलहेतुर्भूर्मा ते सङ्गोऽस्त्वकर्मणि ॥", "Bhagavad Gita 2.47"),
        ("रमा", "Example from README"),
        ("सुन्दर", "User example"),
    ]

def process_input_string(text, source="Input", explain=False):
    """Processes a single string of text and prints the count and optional explanation."""
    count = count_aksharas(text)
    print(f'Source: {source}')
    print(f'Text: "{text}"')
    print(f'Akshara Count: {count}')
    
    if explain:
        explain_aksharas(text)
    
    print('-' * 20)

def process_directory(dir_path):
    """Processes all files in a given directory."""
    print(f"Processing files in: {dir_path}")
    try:
        files = sorted([f for f in os.listdir(dir_path) if os.path.isfile(os.path.join(dir_path, f))])
        if not files:
            print("No files found in the directory.")
            return

        for filename in files:
            file_path = os.path.join(dir_path, filename)
            try:
                with open(file_path, 'r', encoding='utf-8') as f:
                    content = f.read()
                count = count_aksharas(content)
                print(f"{count:5d} | {filename}")
            except Exception as e:
                print(f"Could not process file {filename}: {e}")
    except FileNotFoundError:
        print(f"Error: Directory not found at '{dir_path}'", file=sys.stderr)
        sys.exit(1)

def process_file(file_path, explain=False):
    """Processes a single file."""
    try:
        with open(file_path, 'r', encoding='utf-8') as f:
            content = f.read()
        count = count_aksharas(content)
        
        if not explain:
            print(f"{count:5d} | {os.path.basename(file_path)}")
        else:
            print(f"File: {file_path}")
            print(f"Akshara Count: {count}")
            explain_aksharas(content)

    except FileNotFoundError:
        print(f"Error: File not found at '{file_path}'", file=sys.stderr)
        sys.exit(1)

def main():
    parser = argparse.ArgumentParser(
        description="A script to count aksharas (syllables) in Devanagari text.",
        formatter_class=argparse.RawTextHelpFormatter
    )
    group = parser.add_mutually_exclusive_group(required=True)
    group.add_argument(
        '-c', '--check',
        action='store_true',
        help="Run a check with predefined shlokas."
    )
    group.add_argument(
        '-i', '--input',
        type=str,
        help="A single Devanagari string to process."
    )
    group.add_argument(
        '-p', '--path',
        type=str,
        help="Path to a file, directory, or a glob pattern (e.g., '*.txt').\n             When using a glob pattern, it\'s recommended to quote it to prevent shell expansion."
    )
    parser.add_argument(
        '--explain',
        action='store_true',
        help="Show a step-by-step explanation of the counting process. \n(Ignored in directory mode)."
    )

    args = parser.parse_args()

    if args.explain:
        print("Legend: S=Svara, V=Vyanjana, M=Matra, H=Halant, Sep=Separator, O=Other")
        print("        V+M=Vyanjana+Matra, V+H=Vyanjana+Halant")
        print("-" * 20)

    if args.check:
        print("Running predefined checks...")
        print("=" * 20)
        for text, source in get_predefined_data():
            process_input_string(text, source, explain=args.explain)
    elif args.input:
        process_input_string(args.input, explain=args.explain)
    elif args.path:
        if os.path.isdir(args.path):
            if args.explain:
                print("Note: --explain is ignored when processing a directory.")
            process_directory(args.path)
        else:
            # Expand glob pattern. This will also handle single files.
            paths = sorted(glob.glob(args.path))
            if not paths:
                print(f"Error: No files found matching path '{args.path}'", file=sys.stderr)
                sys.exit(1)

            for path in paths:
                if os.path.isfile(path):
                    process_file(path, explain=args.explain)
                # Silently ignore directories that might be matched by the glob



if __name__ == "__main__":
    main()
