# Basic Bash Scipting 

- Beginning with ```#!/bin/bash```
- Specical variables:
  - $0 : The name of the Bash script.
  - $1 ~ $9 : The first 9 arguments to the Bash script. (As mentioned above.)
  - $# : How many arguments were passed to the Bash script.
  - $@ : All the arguments supplied to the Bash script.
  - $? : The exit status of the most recently run process.
  - $$ : The process ID of the current script.
  - ${#VAR} : Length of a variable.
  
- VAR=$(`command`) : Save the output of the command into a variable VAR
- ```read VAR``` : Read and save input into a variable VAR
- ```let "VAR = <expression>"```
- ```expr "item1 operator item2"```
- ```VAR=$( expr expression )```
- ```VAR=$(( expression ))```
  - operator : +, -, *, /, ++, --, %

## Re-direct: <, >
```bash
# Re-direct output and error to test.log
# 1: stdout
# 2: stderr
$ test > test.log 2>&1
```

## pipe: |

## Conditions
### if
```bash
if [ <test> ] && [ <test> ] || [ <test> ]
then
    <commands>
elif [ <test> ]
then
    <different commands>    
else
    <other commands>    
fi
```

### case
```bash
case <VAR> in
<pattern 1>)
    <commands>
    ;;
<pattern 2>)
    <other commands>
    ;;
*)
    <defaults>
    ;;
esac    
```

### test operations
- ! exp : false expression
- -n string : The length of string is greater than zero
- -z string : the length of string is zero (empty) 
- string1 = string2 : strings are equal
- string1 != string2 : strings are not equal
- integer1 -eq integer2 : integers are equal
- integer1 -gt integer2 : integer1 is greater than integer2
- integer1 -lt integer2 : integer1 is less than integer2
- -d DIR : directory DIR exists
- -e file : file exists


## Loop
### while
```bash
while [ <some test> ]
do
    <commands>
done
```

### until
```bash
until [ <some test> ]
do
    <commands>
done
```

### for-loop
```bash
for VAR in {list}
do
    <commands>
done
```

### loop control
- break
- continue


## Function
```bash
function_name () {
    <commands>
    return <value>
}

# call the function and passing arguments
function_name VAR
```


## Filters
### head [-number of lines to print] [file]
View the first n lines of data.

### tail [-number of lines to print] [path]
View the last n lines of data.

### sort [-options] [path]
Organise the data into order.

### nl [-options] [path]
Print line numbers before data.

### wc [-options] [path]
Print a count of lines, words and characters.

### cut [options] [file]
Cut the data into fields and only display the specified fields.
```bash
# -f : select only these fields
# -d : field delimiter
$ cut -f 1,2 -d ' ' test.log
```

### uniq [file]
Report or omit repeated lines.

### tac [file]
Print files in reverse of ```cat```.

## sed <expression> [file]
Do a search and replace on the data.
```bash
# e.q.: s/search/replace/g
# s : substitute
# g : global (optional)
$ sed 's/Warning/Error/g' test.log
```


## egrep [option] '<pattern>' [file-path]

### option : basic usage
- -i : Ignore case.
- -v : Return all lines which don't match the pattern.
- -w : Select only matches that from whole words.
- -c : Conunt of matching lines.
- -l : Print the file name which contains a match.
- -n : Print the line number before each line that matches.
- -r : Read all files in given directory and sub-directories recursively.

### pattern: Reqular Expression
-    . (dot) : a single character.
-    ? : the preceding character matches 0 or 1 times only.
-    * : the preceding character matches 0 or more times.
-    + : the preceding character matches 1 or more times.
-    {n} : the preceding character matches exactly n times.
-    {n,m} : the preceding character matches at least n times and not more than m times.
-    {n,} : match at least x times.
-    [agd] : the character is one of those included within the square brackets.
-    [^agd] : the character is not one of those included within the square brackets.
-    [c-f] : the dash within the square brackets operates as a range. In this case it means either the letters c, d, e or f.
-    \s : anything which is considered whitespace.
-    \S : anything which is NOT considered whitespace.
-    \d : a digit (ie. 0 ~ 9).
-    \D : anything which is NOT a digit.
-    \w : anything which is considered a word character.
-    \W : anything which is NOT considered a word character.
-    () : allows us to group several characters to behave as one.
-    | (pipe symbol) : the logical OR operation.
-    ^ : matches the beginning of the line.
-    $ : matches the end of the line. 
-    \b : word boundary - wither the beginning or end of a word.
-    e.q. credit card numbers: ```\d{4}[-, ]?\d{4}[-, ]?\d{4}[-, ]\d{4}```
-    e.q. email address: ```[a-zA-Z0-9.+-_]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,63}```
-    e.q. IP address: ```\b(\d{1,3}\.){3}\d{1,3}\b```


- ```egrep 'mellon' myfile.txt```
  
  Print every line in myfile.txt containing the string 'mellon'.

- ```egrep -n 'mellon' myfile.txt```

    Same as above but print a line number before each line.

- ```egrep -l '[0-9]{8,}' /files/projectx/*```
    
    Print each file in the directory projectx which contains a number of 8 digits or more.

- ```egrep '[a-z0-9.+-_]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,63}' myfile.txt```
    
    Print every line of myfiles.txt containing an email address.



## awk '{actions}' file-to-process
```bash
# source example file
$ cat mysampledata.txt
Fred apples 20
Susy oranges 5
Mark watermellons 12
Robert pears 4
Terry oranges 9
Lisa peaches 7
Susy oranges 12
Mark grapes 39
Anne mangoes 7
Greg pineapples 3
Oliver rockmellons 2
Betty limes 14

# print colume 2
$ awk '{print $2}' mysampledata.txt

# print folmated lines and if the value colume 3 > 10
$ awk '$3 > 10 {print $1 " - " $2 " : " $3}' mysampledata.txt
```
