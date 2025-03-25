# Crunch Tool: Syntax, Parameters & Pattern Usage

## 1. Basic Syntax
```bash
crunch <min-length> <max-length> [charset] [options] -o <output-file>
```
### Explanation:
- `<min-length>`: Minimum password length (e.g., 4)
- `<max-length>`: Maximum password length (e.g., 6)
- `[charset]`: Optional custom characters (e.g., abc123)
- `[options]`: Flags like `-t`, `-o`, etc.
- `-o <file>`: Saves output to a file (e.g., `-o wordlist.txt`)

## 2. Key Parameters
| Parameter | Description | Example |
|-----------|-------------|---------|
| `-t` | Uses a pattern with placeholders | `-t pass%%` → `pass00`, `pass01` |
| `-s` | Starts generation from a specific word | `-s apple` → Skips words before "apple" |
| `-e` | Stops generation at a specific word | `-e banana` → Stops after "banana" |
| `-b` | Splits output into smaller files by size | `-b 10MB` → Creates 10MB chunks |
| `-f` | Uses predefined charsets | `-f /usr/share/crunch/charset.lst numeric` |
| `-d` | Limits consecutive duplicate chars | `-d 2` → Allows `aa` but not `aaa` |
| `-o` | Saves output to a file | `-o passwords.txt` |
| `-z` | Compresses output files | `-z gzip` → Creates `.gz` files |

## 3. Pattern Symbols (`-t`)
Crunch uses placeholders to define patterns:

| Symbol | Meaning | Example Pattern | Output Samples |
|--------|---------|----------------|---------------|
| `@` | Lowercase letters | `-t a@b` | `aab`, `acb`, `adb` |
| `,` | Uppercase letters | `-t A,B` | `AA`, `AB`, `AC` |
| `%` | Numbers (0-9) | `-t pass%%` | `pass00`, `pass01` |
| `^` | Special symbols | `-t p@ss^` | `p@ss!`, `p@ss#` |

## 4. Practical Examples

### Example 1: Generate 4-Digit PINs
```bash
crunch 4 4 0123456789 -o pins.txt
```
**Output:** `0000`, `0001`, ..., `9999`

### Example 2: Pattern-Based Passwords
```bash
crunch 5 5 -t admin% -o admin_pass.txt
```
**Output:** `admin0`, `admin1`, ..., `admin9`

### Example 3: Limit Duplicate Characters
```bash
crunch 5 5 abc -d 2 -o no_repeats.txt
```
**Output:** `aabac` (valid), `aaabc` (invalid, has `aaa`)

### Example 4: Use Predefined Charset
```bash
crunch 4 4 -f /usr/share/crunch/charset.lst numeric -o numbers.txt
```
**Output:** Uses `0-9` only.

### Example 5: Start/Stop at Specific Words
```bash
crunch 4 4 abc -s aaba -e abbb -o range.txt
```
**Output:** Generates from `aaba` to `abbb`.

## 5. Summary Cheat Sheet

| Command | Action |
|---------|--------|
| `crunch 4 4 0123 -o pins.txt` | 4-digit PINs (0-3) |
| `crunch 6 6 -t p@ss%%` | Pattern: `p@ss00-p@ss99` |
| `crunch 5 5 abc -d 2` | Limits 2+ repeats |
| `crunch 4 4 -s aaaa -e abcd` | Custom range |

