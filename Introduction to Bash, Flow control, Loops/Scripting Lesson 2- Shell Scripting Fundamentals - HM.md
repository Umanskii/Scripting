Here are the requested Bash scripts for each task:

---

### **1. Script to Determine the Day of the Week Based on a Number Input**
```bash
#!/bin/bash

echo "Enter a number (1 for Monday, 2 for Tuesday, ..., 7 for Sunday):"
read day

case $day in
    1) echo "Monday" ;;
    2) echo "Tuesday" ;;
    3) echo "Wednesday" ;;
    4) echo "Thursday" ;;
    5) echo "Friday" ;;
    6) echo "Saturday" ;;
    7) echo "Sunday" ;;
    *) echo "Invalid input! Please enter a number between 1 and 7." ;;
esac
```

---

### **2. Script to Print Numbers from 1 to 10 Using a For Loop**
```bash
#!/bin/bash

for i in {1..10}; do
    echo "$i"
done
```

---

### **3. Script to Check if a Directory Exists and Create It if Not**
```bash
#!/bin/bash

DIR="my_directory"

if [ -d "$DIR" ]; then
    echo "Directory '$DIR' already exists."
else
    echo "Creating directory '$DIR'..."
    mkdir "$DIR"
    echo "Directory created successfully."

fi
```

---

### **4. Script to Display an Environment Variable Value**
```bash
#!/bin/bash

echo "Enter the name of the environment variable:"
read var_name

if [ -z "${!var_name}" ]; then
    echo "Variable '$var_name' is not set."
else
    echo "Value of '$var_name': ${!var_name}"
fi
```

---

### **5. Script to Count Down from 10 to 1 Using a While Loop**
```bash
#!/bin/bash

count=10

while [ $count -gt 0 ]; do
    echo "$count"
    ((count--))
done
```

---

### **6. Script Using an Environment Variable**
```bash
#!/bin/bash

export MY_VAR="Hello from environment variable!"
echo "MY_VAR is: $MY_VAR"
```

Run the script:
```bash
source ./script.sh  # Ensures the variable is available in the current session
```

---

### **7. Modify `.bashrc`**
Edit your `~/.bashrc` file and add the following:

```bash
# Alias for listing files in long format
alias ll='ls -al'

# Function to greet the user
greet() {
    echo "Hello, $1! Welcome back!"
}
```

Then, apply changes by running:
```bash
source ~/.bashrc
```

Test the alias:
```bash
ll
```

Test the function:
```bash
greet Alex
```

---

### **8. Script to Modify an Environment Variable**
```bash
#!/bin/bash

export TEST_VAR="Initial Value"
echo "Before modification: $TEST_VAR"

TEST_VAR="Modified Value"
echo "After modification: $TEST_VAR"
```

Run the script:
```bash
./script.sh
```

Since `TEST_VAR` is modified inside the script, it won't persist after the script ends. To make it persistent, use:
```bash
export TEST_VAR="Modified Value"
```
before running the script.

---

### **9. Environment Variable Inheritance Test**
#### **Parent Script (`parent.sh`)**
```bash
#!/bin/bash

export SHARED_VAR="Inherited Variable"
echo "Parent script: SHARED_VAR = $SHARED_VAR"

# Call child script
./child.sh
```

#### **Child Script (`child.sh`)**
```bash
#!/bin/bash

echo "Child script: SHARED_VAR = $SHARED_VAR"
```

Run the test:
```bash
chmod +x parent.sh child.sh
./parent.sh
```

You should see the variable being inherited by the child script.

--- 