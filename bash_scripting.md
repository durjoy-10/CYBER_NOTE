# Bash Scripting Complete Note

Bash (Bourne Again SHell) is a command language and scripting language
widely used on Unix/Linux systems for task automation and system
administration.

------------------------------------------------------------------------

## 1. Variables

Variables store data to be used later in the script.

### Declaring Variables

``` bash
name="Alice"
count=10
```

-   No spaces around `=` when assigning.
-   Access with `$`:

``` bash
echo "Hello $name"
echo "Count is $count"
```

### Environment Variables

Set globally:

``` bash
export PATH=$PATH:/new/path
```

------------------------------------------------------------------------

## 2. User Input

Read input from the keyboard during script execution.

``` bash
#!/bin/bash
echo -n "Enter your name: "
read username
echo "Welcome, $username!"
```

Flags: - `-p`: prompt inline (`read -p "Enter age: " age`) - `-s`:
silent input (e.g., password)

------------------------------------------------------------------------

## 3. User Arguments

Arguments passed while running the script:

``` bash
./script.sh arg1 arg2
```

Access: - `$0` : script name - `$1`, `$2` : first, second argument -
`$@` : all arguments - `$#` : number of arguments

Example:

``` bash
#!/bin/bash
echo "Script name: $0"
echo "First arg: $1"
echo "Total args: $#"
```

------------------------------------------------------------------------

## 4. Conditions (If/Else)

Conditional checks guide decision-making.

``` bash
if [ $age -ge 18 ]; then
  echo "Adult"
elif [ $age -ge 13 ]; then
  echo "Teen"
else
  echo "Child"
fi
```

Common test operators: - Integers: `-eq`, `-ne`, `-lt`, `-le`, `-gt`,
`-ge` - Strings: `=`, `!=`, `-z` (empty), `-n` (not empty) - Files: `-e`
(exists), `-f` (regular file), `-d` (directory)

------------------------------------------------------------------------

## 5. For Loop

Iterate over lists or ranges.

``` bash
for i in {1..5}
do
  echo "Number $i"
done
```

Iterating over files:

``` bash
for file in *.txt; do
  echo "Found $file"
done
```

------------------------------------------------------------------------

## 6. While Loop

Runs while a condition is true.

``` bash
count=1
while [ $count -le 5 ]
do
  echo "Count $count"
  ((count++))
done
```

Reading a file line-by-line:

``` bash
while read line; do
  echo "$line"
done < myfile.txt
```

------------------------------------------------------------------------

## 7. Functions

Reusable blocks of code.

``` bash
greet() {
  echo "Hello, $1"
}

greet "Alice"
```

-   `$1`, `$2`, ... are parameters.
-   Use `return` for exit status (0 = success).

------------------------------------------------------------------------

## 8. Most Used Utilities

### 8.1 `tee`

Reads from stdin and writes to stdout and files.

``` bash
echo "Log entry" | tee logfile.txt
```

### 8.2 `tr`

Translate or delete characters.

``` bash
echo "hello" | tr 'a-z' 'A-Z'
```

### 8.3 `jq`

Process JSON data.

``` bash
curl -s https://api.github.com | jq '.current_user_url'
```

### 8.4 `grep`

Search text using patterns.

``` bash
grep "error" /var/log/syslog
```

### 8.5 `wget`

Download files from the web.

``` bash
wget https://example.com/file.zip
```

### 8.6 `cut`

Extract specific fields.

``` bash
cut -d':' -f1 /etc/passwd
```

### 8.7 `curl`

Transfer data with URLs.

``` bash
curl -O https://example.com/file.zip
```

------------------------------------------------------------------------

## 9. Redirection Signs

  Symbol   Usage
  -------- ---------------------------------------
  `>`      Redirect stdout to a file (overwrite)
  `>>`     Redirect stdout to a file (append)
  `<`      Redirect file as stdin
  `<<`     Here-document for inline input

Examples:

``` bash
echo "Hello" > file.txt      # overwrite
echo "Again" >> file.txt     # append
cat < file.txt               # input redirection
cat <<EOF
This is
a here-doc example
EOF
```

------------------------------------------------------------------------

## Example: Combined Script

``` bash
#!/bin/bash
# A sample script using variables, input, loops, and conditions

read -p "Enter a number: " num

if [ $num -gt 0 ]; then
    echo "Positive numbers from 1 to $num:"
    for i in $(seq 1 $num); do
        echo $i
    done
else
    echo "Please enter a positive number."
fi
```

## A bash scripting for finding location & other info by ip address
```
#!/usr/bin/env bash
# ipgeo.sh - lookup IP geolocation (argument 1)
# Usage: ./ipgeo.sh <IP-or-hostname>
# Requires: curl (always). jq is optional but recommended.

set -euo pipefail

IP="${1:-}"
if [[ -z "$IP" ]]; then
  echo "Usage: $0 <IP-or-hostname>"
  exit 2
fi

# Primary API: ip-api.com (no API key required for basic usage).
API_URL="http://ip-api.com/json/${IP}?fields=status,message,country,regionName,city,zip,lat,lon,timezone,isp,org,as,query"

# Alternative API (HTTPS, no key for basic fields): ipapi.co/json
ALT_API_URL="https://ipapi.co/${IP}/json/"

echo "Querying ip-api.com for: $IP ..."
resp="$(curl -sS --connect-timeout 5 --max-time 10 "$API_URL")" || {
  echo "Warning: ip-api query failed, will try ipapi.co"
  resp=""
}

# Helper: pretty print using jq if present
if [[ -n "$resp" ]] && command -v jq >/dev/null 2>&1; then
  status="$(printf '%s' "$resp" | jq -r '.status')"
  if [[ "$status" == "success" ]]; then
    printf '\n%s\n' "Result (ip-api.com):"
    printf '%s\n' "$resp" | jq -r '
      "IP: \(.query)",
      "Country: \(.country) (\(.timezone))",
      "Region: \(.regionName)",
      "City: \(.city) \(.zip)",
      "Coordinates: \(.lat),\(.lon)",
      "ISP: \(.isp)",
      "Org: \(.org)",
      "AS: \(.as)"
    '
    exit 0
  else
    msg="$(printf '%s' "$resp" | jq -r '.message // empty')"
    echo "ip-api returned error: $msg"
    resp=""
  fi
fi

# If we reach here, either ip-api failed or jq not available.
# Try to parse with plain shell tools if we have a response string without jq.
if [[ -n "$resp" ]]; then
  # crude parse without jq
  query=$(printf '%s' "$resp" | grep -oP '"query"\s*:\s*"\K[^"]+' || true)
  country=$(printf '%s' "$resp" | grep -oP '"country"\s*:\s*"\K[^"]+' || true)
  region=$(printf '%s' "$resp" | grep -oP '"regionName"\s*:\s*"\K[^"]+' || true)
  city=$(printf '%s' "$resp" | grep -oP '"city"\s*:\s*"\K[^"]+' || true)
  zip=$(printf '%s' "$resp" | grep -oP '"zip"\s*:\s*"\K[^"]+' || true)
  lat=$(printf '%s' "$resp" | grep -oP '"lat"\s*:\s*\K-?[0-9.]+?' || true)
  lon=$(printf '%s' "$resp" | grep -oP '"lon"\s*:\s*\K-?[0-9.]+?' || true)
  isp=$(printf '%s' "$resp" | grep -oP '"isp"\s*:\s*"\K[^"]+' || true)
  org=$(printf '%s' "$resp" | grep -oP '"org"\s*:\s*"\K[^"]+' || true)
  asn=$(printf '%s' "$resp" | grep -oP '"as"\s*:\s*"\K[^"]+' || true)

  if [[ -n "$query" ]]; then
    cat <<EOF

Result (ip-api.com):
IP: $query
Country: ${country:-N/A}
Region: ${region:-N/A}
City: ${city:-N/A} ${zip:+(ZIP: $zip)}
Coordinates: ${lat:-N/A},${lon:-N/A}
ISP: ${isp:-N/A}
Org: ${org:-N/A}
AS: ${asn:-N/A}

EOF
    exit 0
  fi
fi

# Last resort: try ipapi.co (HTTPS)
echo "Querying ipapi.co for: $IP ..."
resp2="$(curl -sS --connect-timeout 5 --max-time 10 "$ALT_API_URL")" || {
  echo "Both lookups failed. Check network or try again later."
  exit 1
}

# Prefer jq if available
if command -v jq >/dev/null 2>&1; then
  # ipapi returns error fields like 'error' or 'reason' sometimes
  if printf '%s' "$resp2" | jq -e '.error? // false' >/dev/null 2>&1; then
    echo "ipapi returned an error:"
    printf '%s\n' "$resp2" | jq -r '.reason // .error'
    exit 1
  fi

  printf '\n%s\n' "Result (ipapi.co):"
  printf '%s\n' "$resp2" | jq -r '
    "IP: \(.ip // "")",
    "Country: \(.country_name // "") (\(.country_code // ""))",
    "Region: \(.region // "")",
    "City: \(.city // "")",
    "Postal: \(.postal // "")",
    "Coordinates: \(.latitude // "") , \(.longitude // "")",
    "Timezone: \(.timezone // "")",
    "Org: \(.org // .asn // "")"
  '
  exit 0
else
  # crude parse for ipapi.co JSON
  ip=$(printf '%s' "$resp2" | grep -oP '"ip"\s*:\s*"\K[^"]+' || true)
  country=$(printf '%s' "$resp2" | grep -oP '"country_name"\s*:\s*"\K[^"]+' || true)
  city=$(printf '%s' "$resp2" | grep -oP '"city"\s*:\s*"\K[^"]+' || true)
  region=$(printf '%s' "$resp2" | grep -oP '"region"\s*:\s*"\K[^"]+' || true)
  lat=$(printf '%s' "$resp2" | grep -oP '"latitude"\s*:\s*\K-?[0-9.]+?' || true)
  lon=$(printf '%s' "$resp2" | grep -oP '"longitude"\s*:\s*\K-?[0-9.]+?' || true)
  org=$(printf '%s' "$resp2" | grep -oP '"org"\s*:\s*"\K[^"]+' || true)

  cat <<EOF

Result (ipapi.co):
IP: ${ip:-N/A}
Country: ${country:-N/A}
Region: ${region:-N/A}
City: ${city:-N/A}
Coordinates: ${lat:-N/A},${lon:-N/A}
Org: ${org:-N/A}

EOF
  exit 0
fi


```

------------------------------------------------------------------------

### Key Tips

-   Start scripts with `#!/bin/bash`.
-   Make executable: `chmod +x script.sh`.
-   Run with `./script.sh`.
