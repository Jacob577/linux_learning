## Create simple shell scripts
Shell scripts is one of the fundamental building block to automation within a Linux environment.
- Conditionally execute code (use of: if, test, [], etc.)
- Use Looping constructs (for, etc.) to process file, command line input
- Process script inputs ($1, $2, etc.)
- Processing output of shell commands within a script

### Common flags:
```bash
# Is equal (integers)
-eq
# Is equal (string)
==
# Not equal to (integers)
-ne
# Not equal to (string)
!==

# Less or equal, greater or equal, less than, greater than
-le, -ge, -lt, -gt


###
# Other common deliminators
###
# File exists and is not empty
-s 
# File exists and is not a directory
-f
# directory exists
-d
# File is executable
-x
# File is writable
-w
# File is readable
-r

```

### Basic shell script:
As a Hello world in shell scripting we'll use this:
```bash
#!/bin/bash
echo "Hello world"
```

When running commands in a shell script we'll use ` as a symbol.

### Simple loop
```bash
#!/bin/bash
for i in {1..5}
    do
    echo "Hello from $i"
done
```

### For loop in shell descently advanced
How would you list all users one by one from /etc/passwd?
```bash
#!/bin/bash
i=1
for username in `awk -F: '{print $1}' /etc/passwd`
do 
    echo "username $((i++)) : $username"
done
```

Where -F is a flag for field separation, e.i. we are filtering for the field separator `:`. 

### How would you sequentially print the numbers 1 to 10 each second using a fore while loop? (reverse order)
```bash
#!/bin/bash
count=0
top=10
while [ $count -lt $top ]
do 
echo ""
echo "Numbers left of process: " $(( $top - $count ))
echo ""
sleep 1
((count++))
done
echo "Process "$1" is done"
```

### Simple if statement: Check if it is Sunday
```bash
#!/bin/bash
day=`date | awk '{print $1}'`
if ["$day" == Sun]
    then
    echo "Today is Sunday!!"
    else
    echo "Today is in fact: "$day
fi
```

### Processing output of shell commands within a script
Take in a file name, save date as a variable and input into the file name
```bash
#!/bin/bash
day=date

$day >> $1
```

### Write a simple test
```bash
#!/bin/bash
[ $1 = 0 ]
echo "input was zero" $?
[ $1 = 1 ]
echo "input is one" $?

if [$1 -ne 1 ] && [$1 -ne 2]
    then
    echo "bad number"
fi
```