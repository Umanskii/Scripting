## Overview
This document provides a manual for creating and using shell scripts for gathering system information, analyzing disk usage, and performing utility tasks. The scripts included are:

1. `report.sh` - Stores server-related information.
2. `sizecheck.sh` - Checks the size of directories.
3. `diskhog.sh` - Finds the five largest files/directories in a specified location.
4. `split.sh` - Splits a string into words and prints them line by line.
5. `nxn_table.sh` - Generates an N×N table.
6. `ten_dirs.sh` - Creates directories and files with specific content.
7. `validate_ip.sh` - Validates an IP address format.

---


### 1. Here's a Bash script:`report.sh` script that collects server-related information, formats it in a human-readable way, and saves it in a structured report file inside a `reports` directory.

### Explanation:
1. **Creates a `reports` directory** if it doesn’t already exist.
2. **Generates a timestamp** in `YEAR_MONTH_DAY_HOUR_MINUTE_SECOND` format.
3. **Stores system details** in a file named `REPORT_YYYY_MM_DD_HH_MM_SS.txt`.
4. **Includes disk, memory, and CPU usage:**
   - `df -h` for human-readable disk usage.
   - `free -h` for memory usage.
   - `top -b -n1 | grep "%Cpu"` to extract CPU utilization.
5. **Outputs a confirmation message** with the report location.

---
## Implementation:


```bash 

#!/bin/bash

# Create reports directory if it doesn't exist
mkdir -p reports

# Get current date and time in the specified format
TIMESTAMP=$(date '+%Y_%m_%d_%H_%M_%S')
REPORT_FILE="reports/REPORT_${TIMESTAMP}.txt"

# Collect system information
{
    echo "System Report - $(date)"
    echo "========================================="
    echo "Disk Usage:"
    df -h | awk 'NR==1 || /^\/dev/'
    echo ""
    echo "Memory Usage:"
    free -h
    echo ""
    echo "CPU Utilization:"
    top -b -n1 | grep "%Cpu" | awk '{print "CPU Usage: User: "$2"%, System: "$4"%, Idle: "$8"%"}'
} > "$REPORT_FILE"

# Confirm report creation
echo "Report saved to $REPORT_FILE"

```
---

 
### 2. Here's a Bash script `sizecheck.sh` that checks the size of directories passed as arguments. If no arguments are provided, it defaults to the current directory.

### Explanation:
- The `display_size()` function uses `du -sh` to show the human-readable size of a directory.
- If no arguments are provided, it defaults to checking the current directory (`.`).
- If one or more directories are given as arguments, it iterates through them and displays their sizes.
- If an argument is not a valid directory, it prints an error message.

### Example Usage:
```bash
./sizecheck.sh /tmp
# Output: Size of /tmp

./sizecheck.sh /tmp /bin /etc
# Output: Sizes of /tmp, /bin, and /etc

./sizecheck.sh
# Output: Size of the current directory
```
--- 
---
## Implementation:
```bash
#!/bin/bash

# Function to check the size of a directory
display_size() {
    if [ -d "$1" ]; then
        du -sh "$1"
    else
        echo "Error: $1 is not a valid directory"
    fi
}

# If no arguments are given, check the current directory
if [ $# -eq 0 ]; then
    display_size "."
else
    # Iterate over all provided arguments
    for dir in "$@"; do
        display_size "$dir"
    done
fi
```
---



### 3. Here's a Bash script: `diskhog.sh` script that accomplishes your requirements:

### Explanation:

This script:
- Defaults to the current directory if no argument is provided.
- Checks if the provided directory exists.
- Lists the five largest files and directories in the specified directory.
- Includes hidden files and directories.
- Displays sizes in a human-readable format.

## Implementation:
```bash 
#!/bin/bash

# Set directory to provided argument or default to current directory
dir="${1:-.}"

# Check if the given directory exists
if [ ! -d "$dir" ]; then
    echo "Error: Directory '$dir' does not exist." >&2
    exit 1
fi

# List the five largest items (files or directories) directly in the given directory
# -A includes hidden files (but not . and ..)
# -sh prints human-readable sizes
# sort -rh sorts in reverse human-readable order
# head -n 5 gets the top five largest items

du -ah --max-depth=1 "$dir" | sort -rh | head -n 5
```

---


### 4. Here's a Bash script: `split.sh` 

### Description
Splits a string argument into words and prints them line by line.

## Implementation
```bash
#!/bin/bash
echo "$1" | tr ' ' '\n'
```

---

### 5. Here's a Bash script: `nxn_table.sh`

### Description
Generates an N×N table filled with sequential numbers up to N×N.

## Implementation
```bash
#!/bin/bash
N=$1
count=1
for ((i=1; i<=N; i++)); do
    for ((j=1; j<=N; j++)); do
        echo -n "$count "
        ((count++))
    done
    echo ""
done
```

---

### 6. Here's a Bash script: `ten_dirs.sh`

### Description
Creates a directory named `ten`, containing ten subdirectories with four files each, formatted and populated with specified content.

## Implementation
```bash
#!/bin/bash
mkdir -p ten
for i in {01..10}; do
    mkdir -p "ten/dir$i"
    for j in {1..4}; do
        seq -s "\n" $j > "ten/dir$i/file$j.txt"
    done
done
```

---

### 7. Here's a Bash script: `validate_ip.sh`

### Description
Validates whether a given IP address is a correct IPv4 address.

## Implementation
```bash
#!/bin/bash
ip="$1"
if [[ "$ip" =~ ^([0-9]{1,3}\.){3}[0-9]{1,3}$ ]]; then
    IFS='.' read -r -a octets <<< "$ip"
    for octet in "${octets[@]}"; do
        if ((octet < 0 || octet > 255)); then
            echo "Invalid IP address: '$ip'"
            exit 1
        fi
    done
    echo "The provided IP address '$ip' is valid"
else
    echo "Invalid IP address: '$ip'"
fi
```


---

## Conclusion
These scripts provide useful utilities for monitoring system performance and handling common tasks. Ensure they have executable permissions using `chmod +x scriptname.sh` before running them.

For any improvements or extensions, modify the scripts accordingly.



### Usage example:
Make the script executable:
```sh
chmod +x split.sh
```

Run the script with a sentence as an argument:
```sh
./split.sh "Hello world, this is a test"
```

### Expected Output:
```
Hello
world,
this
is
a
test
```
