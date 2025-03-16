### **Manual for Scripting Lesson 4 - Hands-On**  
**Course:** DevOps Engineering  
**Topic:** Scripting Lesson 4  

---

## **Introduction**  
This manual provides guidance on completing two hands-on scripting exercises:  

1. **roman_numerals.sh** – Converts Roman numerals to decimal.  
2. **count_vowels.sh** – Counts the number of vowels in a given word.  

Each section includes a description, example usage, and implementation details.  

---

## **1. Roman Numerals to Decimal Conversion**  

### **Objective**  
Write a shell script (`roman_numerals.sh`) that converts a given Roman numeral into a decimal number.  

### **Constraints**  
- The script takes one argument (a valid Roman numeral).  
- The smallest numeral is `I` (1), and the largest is `MMMCMXCIX` (3999).  

### **Example Usage**  
```sh
$ ./roman_numerals.sh MCMLXXXIV
1984

$ ./roman_numerals.sh MMXXV
2025

$ ./roman_numerals.sh MMMCMXCIX
3999
```

### **Implementation**  
- Iterate through the Roman numeral string from right to left.  
- Assign numeric values:  
  - M = 1000, D = 500, C = 100, L = 50, X = 10, V = 5, I = 1  
- If a smaller numeral appears before a larger one, subtract it (e.g., IV = 4).  
- Otherwise, add its value.  

### **Script: `roman_numerals.sh`**  
```sh
#!/bin/bash

declare -A roman=(
    ["I"]=1 ["V"]=5 ["X"]=10 ["L"]=50 ["C"]=100 ["D"]=500 ["M"]=1000
)

if [[ -z "$1" ]]; then
    echo "Usage: $0 ROMAN_NUMERAL"
    exit 1
fi

input=$(echo "$1" | tr '[:lower:]' '[:upper:]')  # Convert to uppercase
prev=0
total=0

for (( i=${#input}-1; i>=0; i-- )); do
    char="${input:i:1}"
    value=${roman[$char]}

    if [[ -z "$value" ]]; then
        echo "Invalid Roman numeral!"
        exit 1
    fi

    if (( value < prev )); then
        total=$(( total - value ))
    else
        total=$(( total + value ))
    fi

    prev=$value
done

echo $total
```

---

## **2. Count Vowels in a Word**  

### **Objective**  
Write a shell script (`count_vowels.sh`) that counts the number of vowels (a, e, i, o, u) in a given word, case-insensitively.  

### **Example Usage**  
```sh
$ ./count_vowels.sh tentek
2

$ ./count_vowels.sh cat
1

$ ./count_vowels.sh TENtek
2

$ ./count_vowels.sh CAT
1
```

### **Implementation**  
- Accept a single argument (word).  
- Convert it to lowercase for case-insensitive comparison.  
- Use `grep` or `tr` to filter out vowels and count them.  

### **Script: `count_vowels.sh`**  
```sh
#!/bin/bash

if [[ -z "$1" ]]; then
    echo "Usage: $0 WORD"
    exit 1
fi

word=$(echo "$1" | tr '[:upper:]' '[:lower:]')  # Convert to lowercase
vowel_count=$(echo "$word" | grep -o '[aeiou]' | wc -l)

echo $vowel_count
```

---

## **Conclusion**  
These scripts demonstrate basic scripting techniques, such as:  
- Using arrays and loops in **roman_numerals.sh**  
- Pattern matching and text processing in **count_vowels.sh**  

To make the scripts executable, run:  
```sh
chmod +x roman_numerals.sh count_vowels.sh
```

Then execute them as:  
```sh
./roman_numerals.sh MCMLXXXIV  
./count_vowels.sh tentek  
```
