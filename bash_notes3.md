# BASH

## Contents

1. [Basic Script](#1-basic-script)
2. [Variables and Expansions](#2-variables-and-expansions)
3. [Bash Processing](#3-bash-processing)
4. [User Input](#4-user-input)
5. [Logic](#5-logic)
6. [Processing and Reading Files](#6-processing-and-reading-files)
7. [Arrays](#7-arrays)
8. [Debugging](#8-debugging)
9. [Cron](#9-cron)
10. [Remote Servers](#10-remote-servers)

## 1 Basic Script

```bash
#!/bin/bash

# Author: Myself
# Date Created: 01 JAN 2023
# Date Modified: 01 JAN 2023

# Description:
# Creates a backup in ~/bash_course folder of all files in the home directory.

# Usage:
# Backup_script

tar -cvf ~/bash_course/my_backup_"$(date +%d-%n-%y_%H-$S)".tar ~/* 2>/dev/null
exit 0
```

## 2 Variables and Expansions
Variables allow you to store data under names in the script. Shell expansions are very powerfull 
that allow us to retrieve data, process command output, and perform calculations.

There are three types of shell parameters.
- Variable.
- Positional parameters.
- Special parameters.

When declaring variables do not have white space around the = or a command not found error will 
occur.

Shell variable come in two catagories.
- Bourne Shell Variable.
- Bash Shell Variables.

PATH is a shell variable.

The HOME variable is the absolute path to the users home directory.
```bash
echo $HOME
```

The USER variable contains the username of the current user.
```bash
echo $USER
```

The HOSTNAME and HOSTTYPE tell them about the computer they are using.
```bash
echo $HOSTNAME

echo $HOSTTYPE
```

The PS1 variable controls the prompt string.
```bash
PS1="$: "

# to change the prompt to the original do the following.

source ~/.bashrc
```

Shell variables are in all UPPERCASE.

Command substitution allows you to save the output from commands to variables.

Command substitution allows you to use the output from one command inside another command.

Arithmetic expansion does not calculate decimals only whole numbers.

To do calculations with decimal numbers you need to use bc basic calculator.

In basic calculator the scale variable is used to control how many decimal places are given 
in the output of the command.

to enter basic calculator you can enter bc into the command line.

To exit bc you just type quit.

Tilde expansion ~ refers to the current users directory.
```bash
echo ~

echo ~user1
```
The current working directory is always stored in a variable pwd.
```bash
echo $PWD
```
The last directory you were in is stored in a variable called OLDPWD.
```bash
echo $OLDPWD
```
Shorthand for the PWD variable.
```bash
echo ~+
```
Shorthand for the OLDPWD variable.
```bash
echo ~-
```
Brace expansion is performed using curly braces.
```bash
echo {a,19,z,barry,42}

#a 19 z barry 42

echo {jan,feb,mar,apr,may,jun}

#jan feb mar apr may jun
```
There must be no spaces inside the expansion or the expansion will be interpeted as a string.
```bash
echo {jan,feb,mar,apr,jun}
```
use brace expansion to print a range of numbers.
```bash
echo {1..10}

1 2 3 4 5 6 7 8 9 10
```
Use brace command to print a range of letters.
```bash
echo {a..z}

#a b c d e f g h i j k l m n o p q r s t u v w x y z
```
Use brace command to create folders.
```bash
echo month{1..12}
```
Add a leading zero so all the folder names are the same length.
```bash
mkdir month{01..12}
```
Create files within each folder.
```bash
touch month{01..12}/day{01..31}.txt
```

Arithmetic expansion.
```bash
#!/bin/bash


# arithmetic expansion can only deal with whole numbers.

x=4
y=2
echo $(( $x + $y ))
echo $(( $x - $y ))
echo $(( $x * $y ))
echo $(( $x / $y ))
echo $(( $x ** $y ))
echo $(( $x % $y ))

# to work with decimal numbers you need to use bc or basic calculator.
```

Basic calculator.
```bash
#!/bin/bash

echo "5/2" | bc

echo "scale=2; 5/2" | bc

echo "scale=10; 5/2" | bc
```

expansion examples
```bash
#!/bin/bash

name="CiarAN"

echo ${name}

# change first letter to lowercase.

echo ${name,}

# change entire name to lowercase.

echo ${name,,}

name="ciaran"

# change first letter to uppercase.

echo ${name^}

# convert the entire string to uppercase.

echo ${name^^}

# get the number of letters in the name.

echo ${#name}

# get a slice of a variable.
# will be in format of {parameter:offset:length}

my_var="this_is_a_long_variable"

echo ${my_var:3:5}

# can use negative offset but there must be a space before the -

echo ${my_var: -10:16}

# print variable from an offset to the end.

echo ${my_var:3}
```

## 3 bash Processing

Bash reads through a script line by line and goes through a five step process to intercept 
the command line.

STEPS
1. Tokenisation. A sequence of characters that is considered as a single unit by the shell.
  Uses meta characters to break up the command line.
  |
  &
  ;
  ()
  <>
  space, tab, newline.

  The difference between words and metacharacters is that words never contain unquoted 
  metacharacters. Operators always contain unquoted metacharacters.

2. Command identification. Simple commands are just a bunch of individual words, and each simple
   command is terminated by a control operator such as a newline or a comma. Compound commands 
   on the other hand provide bash with its programming constructs such as if statements, for loops,
   while loops, etc.

3. Expansion.

4. Quote removal. Quotes are added to control how the command is interpreted, so this step will 
  simply remove all those supportive quotes.

5. redirection.

After the above steps have completed bash will execute the command line that is left over.

Bash uses three different types of quoting. 

Quoting is the removal of meaning.

Three types of quoting.
- 1. Backslash (\):
     Removes special meaning from next character.
- 2. Single Quotes (''):
     Removes special meaning from all characters inside.
- 3. Double Quotes (""):
     Removes special meaning from all ecxcapt dollar signs ($) and
     backticks (`)

A character being Quoted or escaped is the same thing.

Example of escaping a special character.
```bash
echo john & jane

# above will throw an error.

echo john \& jane

# filepath

filepath=C:\User1\home\Documents\my_dir

echo filepath -> C:user1homeDocumentsmy_dir

# use \ to escape.

filepath=C:\\user1\\home\\Documents\\my_dir

echo filepath -> C:\user1\home\Documents\my_dir

# use quotes to enclose string.

filepath='C:\User1\home\Documents\my_dir'

# single quotes cannot contain a single quote even if preceeded with a backslash.

filepath="C:\\$USER\Documents"

# double quotes do not escape the meaning of dollar $ signs and backticks `.
```

There are four stages to shell expansion.
- Stage 1; Brace Expansion.
- Stage 2:
  * Parameter Expansion.
  * Arithmetic Expansion.
  * Command Substitution.
  * Tilde Expansion.
- Stage 3: Word Splitting.
- Stage 4: globbing.

Example.
```bash
x=10

echo {1..$x}

# above will throw an error as the brace expansion happens before parameter expansion.
```
Word splitting is a process to split the result of some unquoted expansions into seperate words.

Word splitting is only performed on the result of unquoted:
- Parameter expansions.
- Command substitutions.
- Arithmetic expansions.

The characters used to split the words are governed by the IFS (internal Field Seperator) variable.
- Space.
- tab.
- New line.

See the IFS characters.
```bash
echo "${IFS}"

echo "${IFS}"
```
Example.
```bash
numbers="1 2 3 4 5"

touch "$numbers"

# will make files '1 2 3 4 5'

touch $numbers

# will make files
# 1 
# 2
# 3
# 4
# 5
```
Change the IFS.
```bash
IFS=","
```
If you want the output of an expansion to be considered as one word wrap it in double quotes "".


Globbing is only carried out on words.

Globbing patterns are words that contain unquoted Special Pattern Characters: * ? [

The * character is a wildcard character.
```bash
*.txt

# finds all files ending with .txt

file*.txt

# finds all files beggining with 'file' and ends with .txt
```
The ? character is a placeholder for a single character.
```bash
file???.txt

# will match file123.txt, file456.txt, fileABC.txt but not file12.txt or fileAB.txt
```
The [] will only match the characters inside them.
```bash
file[ab].txt

# will match filea.txt, fileb.txt.

file[a-g].txt

# will match file followed by any letter a-g then .txt.

rm ~/Downloads/*.jpg

# will delete all .jpg files in Downloads directory.
```


Removal of Quotes \ '' ""

uring Quote removal, the shell removes all unquoted backslashes, single quote characters, 
and double quote characters that did not result from a shell expansion.

Example.
```bash
echo `\$HOME`

# will output \$HOME

echo \$HOME

# will output  
```
Redirection using  the < and > operators.
```bash
cat < my_file.txt
```
Redirect the stderr.
```bash
cd /root 2> error.txt
```
Redirect the stdout and the stderr to the /dev/null.
```bash
cd /root &> /dev/null
```
Append to file.

    >>

    2>>

    &>>

## 5 Logic

Some logical operators are ";","&","&&","||".

List operators are types of control operators that enable us to create lists
of commands that operate in different ways.

The & operator will send a process into the background and allow other commands to
run before the background one has completed.
```bash
echo 123 & echo 456
```
The ; operator wiil wait for one command to finish before starting the next one.
```bash
echo 123 ; echo 456
```
The && operator makes it so that the second command only runs if the first one was
successful.
```bash
ls non_existant_directory && echo "Success"
```
The || operator makes it so that the second command only runs if the first one 
failed.
```bash
ls non_existant_directory || echo "Something went wrong"
```
Example.
```bash
commandA && commandB || commandC

# if commandA is successful run commandB else run commandC
# The above is an example of a ternary operator.
```

### TEST COMMANDS

A test command can be used to test if a command can be used to compare different
pieces of information.  test command produces a 0 or 1.
```bash
[ 2 -eq 2 ] ; echo $?

# produces a 0 output meaning true.
```
Some comparisson operators -gt -lt -geq -leq. these operators can only be used
to compare integers not decimals.

Comparing strings using = or !=.
```bash
a=hello
b=goodbye
[[ $a = $b ]] ; echo $?
```
Check if string is not empty.
```bash
[[ -z $c ]] ; echo $?
```
Check if a string is empty.
```bash
[[ -n $c ]] ; echo $?
```
File test operators compare files.

test if file exists.
```bash
[[ -e today.txt ]] ; echo $?
```
Check if is file.
```bash
[[ -f today.txt ]] ; $?
```
Check if is directory.
```bash
[[ -d today.txt ]] ; echo $?
```
Check if a file exists and has executable permisions.
```bash
[[ -x script ]] ; echo $?
```
Other test operators -r, -w, -nt.

### IF ELSE

An if staement starts with "if" and ends with "fi".

Use || && to comnine staements.

```bash
#!/bin/bash


if [ 1 -eq 1 ]; then
    echo "one is equal to one"
elif [ 2 -eq 2 ]; then
    echo "two is equal to two"
else
    echo "Test failed"
fi

```

```bash
#!/bin/bash


a=$(cat file1.txt)
b=$(cat file2.txt)
c=$(cat file3.txt)


if [ $a = $b ] && [ $a = $c ]; then
    rm file2.txt file3.txt
else
    echo "Files do not match"
fi

```
### CASE STAEMENTS
```bash
#!/bin/bash

read -p "Please enter a number: " number
case "$number" in
    [0-9]) echo "You have entered a single digit number";;
    [0-9][0-9]) echo "You have entered a two digit number";;
    [0-9][0-9][0-9]) echo "You have entered a three digit number";;
    *) echo "Invalid input. No match";;
esac

```

## 6 Processing and Reading Files


* Example of a while loop.
```bash
read -p "Enter your number: " num

while []; do
    echo $num
    num=$(( $num-1 ))
done
```
getopts command.
```bash
getopts "f:c:"

# the above could be used to convert between farenheit and celcius.
# above allows you to input f or c.
# the : after the f and c allows you to enter a number after it.
```
### getopts command.
```bash
#!/bin/bash


while getopts "f:c:" opt; do
case "$opt" in
    c) result=$(echo "scale=2; ($OPTARG * (9 / 5) + 32)" | bc);;
    f) result=$(echo "scale=2; ($OPTARG - 32) * (5/9)" | bc);;
    \?);;
esac
done

echo "$result"
```

### Optargs
```bash
#!/bin/bash


# you can enter both options for minutes and seconds.

total_seconds=""

while getopts "m:s:" opt; do
    case "$opt" in
        m) total_seconds=$(( $total_seconds + $OPTARG * 60 )) ;;
        s) total_seconds=$(( $total_seconds + $OPTARG )) ;;
    esac
done

while [ $total_seconds -gt 0 ]; do
    echo "$total_seconds"
    total_seconds=$(( $total_seconds - 1 ))
    sleep 1s
done

echo "Time is up"
```
### Read while loop.
```bash
#!/bin/bash


while read line; do
    echo $line
done < "$1"


# following reads input from ls $HOME which uses process substitution
# to read it in as a file.
while read line; do
    echo $line
done < <(ls $HOME)

```
### 
```bash
#!/bin/bash



read -p "Enter your number: " num


while [ $num -gt 10 ]; do
    echo $num
    num=$(( $num-1 ))
done
```

## 7 Arrays

### Indexed Arrays

Declare an indexed array.
```bash
numbers=(1 2 3 4 5)
```

Capture the output of a command in an indexed array.
```bash
current_users=(`cut -f 1 "users.txt"`)
```

Echo the first element of an array.
```bash
echo numbers
```

Get a value at a certain index of an indexed array.
```bash
echo ${numbers[2]}
```

Echo out the entire indexed array.
```bash
echo ${numbers[@]}
```

Apply an offset to an indexed array expansion to start from element one.
```bash
echo ${numbers[@]:1}
```

Apply an offset and a length to an array expansion to start from a certain
element and expand to a certain length.
```bash
echo ${numbers[@]:1:2}
```

Add an element to an indexed array.
```bash
numbers+=(5)
```

Delete an element from an indexed array at a certain index.
```bash
unset numbers[2]
```

When an element is removed at an index that index is removed aswell.
To see the indexes do following.
```bash
numbers=(1 2 3 4 5)

unset numbers[2]

echo ${!numbers[@]}

# 0 1 3 4
```

Get the length of a an array.
```bash
echo ${#numbers[@]}
```

Change an element in an array.
```bash
numbers[0]=a
```

Use the readarray command to read file contents into an array.
```bash
readarray days < days.txt
```

See the invisible characters in an array.
```bash
echo ${days[@]@Q}
```

Use readarray but remove invisible characters.
```bash
readarray -t days < days.txt
```

Run ls command into a readarray.
```bash
readarray files <(ls ~/my_dir/array)
```

Iterate over an array with a for loop.
```bash
for element in my_array; do
    echo $element
done
```

Use readarray in a for loop.
```bash
readarray -t files < files.txt

for file in "${files[@]}"; do
    touch "$file"
done
```

Use if statement inside a for loop.
```bash
readarray -t files < files.txt

for file in "${files[@]}"; do
    if [ -f "$file" ]; then
        echo "$file already exists"
    else
        touch "$file"
        echo "$file was created"
    fi
done
```

Use for loop to read webpages.
```bash
readarray -t urls < urls.txt

for url in "${urls[@]}"; do
    webname=$(echo $url | cut -d "." -f 2)
    curl --head "$url" > "$webname".txt
done
```

### Asociative Arrays

Declare an associative array.
```bash
declare -A newmap
```

Add key value pairs to an associative array.
```bash
newmap[name]="david"
newmap[address]="33 some street, some city, somewhere"
newmap[postcode]="wxyz"
newmap[telephone]="555-1234"
```

Echo value in an associative array.
```bash
echo ${newmap[name]}
```

Echo keys from an associative array.
```bash
echo "keys: ${!newmap[@]}"
```

Echo values from an associative array.
```bash
echo "Values : ${newmap[*]}"
```

Echo length of an associative array.
```bash
echo ${#newmap[@]}
```

Append to an associative array.
```bash
newmap+=(["owner"]="true")
```

Append to an associative array.
```bash
newmap[has dog]="false"
```

Remove a key value pair from an associative array.
```bash
unset newmap["owner"]
```

Remove all key value pairs from an associative array.
```bash
declare -A newmap=()
```

Remove all key value pairs from an associative array.
```bash
unset newmap
```

Declare a read only associative array.
```bash
declare -r -A imutablemap=(
["name"]="john"
["address"]="33 some street, some city, somewhere else"
["phone"]="555-9876"
)
```

Check if key is present in an associative array.
```bash
if [[ -n "${newmap[address]}" ]]
then
echo "Element exists"
else
echo "Element not present"
fi
```