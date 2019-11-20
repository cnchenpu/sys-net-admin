# Basic Bash Scipting 

- Beginning with ```#!/bin/bash```

## Variables
- Specical variables:
  - $0 : The name of the Bash script.
  - $1 ~ $9 : The first 9 arguments to the Bash script. (As mentioned above.)
  - $# : How many arguments were passed to the Bash script.
  - $@ : All the arguments supplied to the Bash script.
  - $? : The exit status of the most recently run process.
  - $$ : The process ID of the current script.
  - $RANDOM - Returns a different random number each time
  - ${#VAR} : Length of a variable.
  
- ``VAR=<something>`` : Assign something to the variable VAR.
- ``VAR='string1 string2'`` : Assign strings to the variable VAR. Single quotes will treat every character literally.
- ``VAR1="string $VAR2"`` :  Assign string and the contain of the VAR2 to the variable VAR. Double quotes will allow you to do substitution (that is include variables within the setting of the value).
- ``export VAR`` : Export the variable VAR after assigning values. Make the variable VAR available to child processes.
- ``VAR=$( command )`` : Save the output of the command into the variable VAR.
- ```read VAR``` : Read and save input into a variable VAR.
- ```read -p 'statement' VAR``` : Read and save input into the variable VAR and ``-p`` allows you to specify a prompt.
- ```read -sp 'statement' VAR``` : Read and save input into the variable VAR and ``-s`` makes the input silent.
- ```let "VAR = <expression>"``` : Make the variable VAR equal to an expression.
- ``echo $VAR`` : Print out the variable VAR.

### Lab 1: Variable and reading from input 
```bash
01    #!/bin/bash
02    # Ask the user for their name
03    echo Hello, who am I talking to?
04    read varname
05    echo It\'s nice to meet you $varname
06
07    # Ask the user for login details
08    read -p 'Username: ' uservar
09    read -sp 'Password: ' passvar
10    echo
11    echo Thank you $uservar, your login password is $passvar.
12
13    # More variables
14    echo What 3 cars do you like?
15    read car1 car2 car3     # Test to input more than 3 cars
16    echo Your first car was: $car1
17    echo Your second car was: $car2
18    echo Your third car was: $car3
19
20    # Use variable for command
21    sampledir=/etc
22    ls $sampledir
23    myvar=$( ls /etc | wc -l )
24    echo There are $myvar entries in the directory /etc
25    
26    # The difference between single and double quotes for assigning variables
27    myvar='Hello World'
28    echo $myvar
29    newvar1='More $myvar'
30    echo $newvar1
31    newvar2="More $myvar"    
32    echo $newvar2
```

## Arithmetics

- ```expr item1 operator item2``` : Print out the result of the expression. There are `spaces` between the item and operator.
- ```expr "item1 operator item2"``` : If we `do put quotes` around the expression then the expression will not be evaluated but printed instead.
- ```expr <item1>operator<item2>``` : If we `do not put spaces` between the items of the expression then the expression will not be evaluated but printed instead.
- ```VAR=$( expr expression )``` : Return the result of the expression to the variable VAR.
- ```VAR=$(( expression ))``` : Return the result of the expression to the variable VAR.
  - operator : +, -, *, /, ++, --, %
- ```${#VAR}``` : Return the length of the variable VAR.


### Lab 2: Arithmetic
```bash
01    #!/bin/bash
02    # Basic arithmetic using expr
03    expr 5 + 4
04    expr "5 + 4"
05    expr 5+4  
06    expr 5 \* $1
07    expr 11 % 2
08    a=$( expr 10 - 3 )
09    echo $a # 7
10
11    # Basic arithmetic using double parentheses
12    a=$(( 4 + 5 ))
13    echo $a # 9
14    a=$((3+5))
15    echo $a # 8
16    b=$(( a + 3 ))
17    echo $b # 11
18    b=$(( $a + 4 ))
19    echo $b # 12
20    (( b++ ))
21    echo $b # 13
22    (( b += 3 ))
23    echo $b # 16
24    a=$(( 4 * 5 ))
25    echo $a # 20
26
27    # Show the length of a variable.
28    a='Hello World'
29    echo ${#a} # 11
30    b=4953
31    echo ${#b} # 4
```

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

 
- Print every line in myfile.txt containing the string 'mellon'.
  
  ```egrep 'mellon' myfile.txt```

- Same as above but print a line number before each line.
  
  ```egrep -n 'mellon' myfile.txt```
 
- Print each file in the directory projectx which contains a number of 8 digits or more.
  
  ```egrep -l '[0-9]{8,}' /files/projectx/*```
    
- Print every line of myfiles.txt containing an email address.
  
  ```egrep '[a-z0-9.+-_]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,63}' myfile.txt```
     


## find /path -name <file name> -exec <command> "{}" \;
```bash
# find and delete 100+ MB files
$ find /path -size +100MB -exec rm -r "{}" \;

# find and delete *.exe files
$ find /path -name "*.exe" -exec rm -r "{}" \;

# find and change php files authority
$ find /path -name "*.php" -exec chmod 644 "{}" \; 

# find and change all folders authority
$ find /path -type d -exec chmod 755 "{}" \;
```


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
## awk -F "delimiter" '{actions}' file-to-process
```bash
# list all user account
$ awk -F ":" '{print $1}' /etc/passwd
```


## Script Debug

- [ShellCheck](https://www.shellcheck.net/)
    finds bugs in your shell scripts. 

- Run script in debug mode:
```
$ export PS4=' $BASH_SOURCE:$LINENO: ${function}'
$ bash -x script-name.sh
```

