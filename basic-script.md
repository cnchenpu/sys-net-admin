---
tags: sysdamin, Linux, Bash
---

# Basic Bash Scipting 

- Beginning with ```#!/bin/bash```

## Variables
- Specical variables:
  1. ``$0`` : The name of the Bash script.
  2. ``$1`` ~ ``$9`` : The first 9 arguments to the Bash script. (As mentioned above.)
  3. `` $#`` : How many arguments were passed to the Bash script.
  4. ``$@`` : All the arguments supplied to the Bash script.
  5. ``$?`` : The exit status of the most recently run process.
  6. ``$$`` : The process ID of the current script.
  7. ``$RANDOM`` - Returns a different random number each time
  8. ``${#VAR}`` : Length of a variable.
  
9. ``VAR=<something>`` : Assign something to the variable VAR.
10. ``VAR='string1 string2'`` : Assign strings to the variable VAR. Single quotes will treat every character literally.
11. ``VAR1="string $VAR2"`` :  Assign string and the contain of the VAR2 to the variable VAR. Double quotes will allow you to do substitution (that is include variables within the setting of the value).
12. ``export VAR`` : Export the variable VAR after assigning values. Make the variable VAR available to child processes.
13. ``VAR=$( command )`` : Save the output of the command into the variable VAR.
14. ```read VAR``` : Read and save input into a variable VAR.
15. ```read -p 'statement' VAR``` : Read and save input into the variable VAR and ``-p`` allows you to specify a prompt.
16. ```read -sp 'statement' VAR``` : Read and save input into the variable VAR and ``-s`` makes the input silent.
17. ```let "VAR = <expression>"``` : Make the variable VAR equal to an expression.
18. ``echo $VAR`` : Print out the variable VAR.

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

1. ```expr item1 operator item2``` : Print out the result of the expression. There are `spaces` between the item and operator.
2. ```expr "item1 operator item2"``` : If we `do put quotes` around the expression then the expression will not be evaluated but printed instead.
3. ```expr <item1>operator<item2>``` : If we `do not put spaces` between the items of the expression then the expression will not be evaluated but printed instead.
4. ```VAR=$( expr expression )``` : Return the result of the expression to the variable VAR.
5. ```VAR=$(( expression ))``` : Return the result of the expression to the variable VAR.
  - operator : +, -, *, /, ++, --, %
6. ```${#VAR}``` : Return the length of the variable VAR.


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

## Conditions test
No. | Operator | Description
---|---|---
1 | ``!`` EXPRESSION | The EXPRESSION is false.
2 | ``-n`` STRING | The length of STRING is greater than zero.
3 | ``-z`` STRING | The lengh of STRING is zero (ie it is empty).
4 | STRING1 ``=`` STRING2 | STRING1 is equal to STRING2
5 | STRING1 ``!=`` STRING2 | STRING1 is not equal to STRING2
6 | INTEGER1 ``-eq`` INTEGER2 | INTEGER1 is numerically equal to INTEGER2
7 | INTEGER1 ``-gt`` INTEGER2 | INTEGER1 is numerically greater than INTEGER2
8 | INTEGER1 ``-lt`` INTEGER2 | INTEGER1 is numerically less than INTEGER2
9 | ``-d`` FILE | FILE exists and is a directory.
10 | ``-e`` FILE | FILE exists.
11 | ``-r`` FILE | FILE exists and the read permission is granted.
12 | ``-s`` FILE | FILE exists and it's size is greater than zero (ie. it is not empty).
13 | ``-w`` FILE | FILE exists and the write permission is granted.
14 | ``-x`` FILE | FILE exists and the execute permission is granted.

- ``=`` is slightly different to ``-eq``. ``[ 001 = 1 ]`` will return ``false (1)`` as ``=`` does a `string comparison` (ie. character for character the same) whereas ``-eq`` does a `numerical comparison` meaning ``[ 001 -eq 1 ]`` will return ``true (0)``.
- When we refer to ``FILE`` above we are actually meaning a path of file. Remember that a path may be absolute or relative and may refer to a file or a directory.

### Lab 3: test
```bash
01  #!/bin/bash
02  test 001 = 1
03  echo $?
04
05  test 001 -eq 1
06  echo $?
07
08  touch myfile
09  test -s myfile
10  echo $?
11
12  ls /etc > myfile
13  test -s myfile
14  echo $?
```

## if - then
```bash
01  if [ <some test> ]
02  then
03      <commands>
04  fi
```

- The ``[ ]`` in ``if`` command is just a reference to the command ``test``.

## Nested if
```bash
01  if [ <test> ]
02  then
03      <commands>
04      if [ <test> ]
05      then
06          <different commands>    
07      fi
08  fi
```

### Lab 4: Nested if
```bash
01    #!/bin/bash
02    # Nested if statements
03    if [ $1 -gt 100 ]
04    then
05       echo Hey that\'s a large number.
06       if (( $1 % 2 == 0 ))
07       then
08          echo And is also an even number.
09       fi
10    fi
```

## if - else
```bash
01  if [ <some test> ]
02  then
03      <commands>
04  else
05      <other commands>
06  fi
```

### Lab 5: if - else
```bash
01    #!/bin/bash
02    # else example
03    if [ $# -eq 1 ]
04    then
05        nl $1
06    else
07        nl /dev/stdin
08    fi
```

## if - elif - else
```bash
01  if [ <some test> ]
02  then
03      <commands>
04  elif [ <some test> ]
05  then
06      <different commands>
07  else
08      <other commands>
09  fi
```

### Lab 6: if - elif - else
```bash
01    #!/bin/bash
02    # elif statements
03    if [ $1 -ge 18 ]
04    then
05       echo You may go to the party.
06    elif [ $2 == 'yes' ]
07    then
08       echo You may go to the party but be back before midnight.
09    else
10       echo You may not go to the party.
11    fi
```

## && (and), || (or)
```bash
01  if [ <test> ] && [ <test> ] || [ <test> ]
02  then
03      <commands>
04  elif [ <test> ]
05  then
06      <different commands>    
07  else
08      <other commands>    
09  fi
```

## case
```bash
01  case <VAR> in
02  <pattern 1>)
03      <commands>
04      ;;
05  <pattern 2>)
06      <other commands>
07      ;;
08  *)
09      <defaults>
10      ;;
11  esac    
```

### Lab 7: case
```bash
01    #!/bin/bash
02    # case example
03    case $1 in
04       start)
05          echo starting
06          ;;
07       stop)
08          echo stoping
09          ;;
10       restart)
11          echo restarting
12          ;;
13       *)
14          echo don\'t know
15          ;;
16       esac
```

### Lab 8: case - check disk usage
```bash
01    #!/bin/bash
02    # case example: check disk useage.
03    space_free=$( df -h | awk '{ print $5 }' | sort -n | tail -n 1 | sed 's/%//' )
04    case $space_free in
05      [1-5]*)
06          echo Plenty of disk space available
07          ;;
08      [6-7]*)
09          echo There could be a problem in the near future
10          ;;
11      8*)
12          echo Maybe we should look at clearing out old files
13          ;;
14      9*)
15          echo We could have a serious problem on our hands soon
16          ;;
17      *)
18          echo Something is not quite right here
19          ;;
20     esac
```

## while loop
```bash
01  while [ <some test> ]
02  do
03      <commands>
04  done
```

### Lab 9: simple while loop
```bash
01    #!/bin/bash
02    # Basic while loop
03    counter=1
04    while [ $counter -le 10 ]
05    do
06      echo $counter
07      ((counter++))
08    done
09    echo All done
```

## until
```bash
01  until [ <some test> ]
02  do
03      <commands>
04  done
```

### Lab 10: until
```bash
01    #!/bin/bash
02    # Basic until loop
03    counter=1
04    until [ $counter -gt 10 ]
05    do
06      echo $counter
07      ((counter++))
08    done
09    echo All done
```

## for-loop
```bash
01  for VAR in {list}
02  do
03      <commands>
04  done
```

### Lab 11: for-loop 1
```bash
01    #!/bin/bash
02    # Basic for loop
03    names='Stan Kyle Cartman'
04    for name in $names
05    do
06      echo $name
07    done
08    echo All done
```

### Lab 11: for-loop 2
```bash
01    #!/bin/bash
02    # Basic range in for loop
03    for value in {1..5}
04    do
05      echo $value
06    done
07    echo All done
```

### Lab 11: for-loop 3
```bash
01    #!/bin/bash
02    # Basic range with steps for loop
03    for value in {10..0..2}
04    do
05      echo $value
06    done
07    echo All done
```

## loop control
- break
- continue

### Lab 12: break in loop
```bash
01  i=0
02  while [ $i -lt 5 ]
03  do
04    echo "Number: $i"
05    ((i++))
06    if [[ "$i" == '2' ]]; then
07      break
08    fi
09  done
10  echo 'All Done'
```

### Lab 13: continue in loop
```bash
01  i=0
02  while [ $i -lt 5 ]
03  do
04    echo "Number: $i"
05    ((i++))
06    if [[ "$i" == '2' ]]; then
07      continue
08    fi
09  done
10  echo 'All Done'
```

## Function
```bash
01  function_name () {
02      <commands>
03  return <value>
}
```

### Lab 14: call the function and passing arguments
```bash
01    #!/bin/bash
02    # Setting a return status for a function
03    print_something () {
04      echo Hello $1
05      return 0
06    }
07    print_something Mars
08    print_something Jupiter
09    echo "The return value: $?"
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
1.    ``.`` (dot) : a single character.
2.    ``?`` : the preceding character matches 0 or 1 times only.
3.    ``*`` : the preceding character matches 0 or more times.
4.    ``+`` : the preceding character matches 1 or more times.
5.    ``{n}`` : the preceding character matches exactly n times.
6.    ``{n,m}`` : the preceding character matches at least n times and not more than m times.
7.    ``{n,}`` : match at least n times.
8.    ``[agd]`` : the character is one of those included within the square brackets.
9.    ``[^agd]`` : the character is not one of those included within the square brackets.
10.    ``[c-f]`` : the dash within the square brackets operates as a range. In this case it means either the letters c, d, e or f.
11.    ``\s`` : anything which is considered whitespace.
12.    ``\S`` : anything which is NOT considered whitespace.
13.    ``\d`` : a digit (ie. 0 ~ 9).
14.    ``\D`` : anything which is NOT a digit.
15.    ``\w`` : anything which is considered a word character.
16.    ``\W`` : anything which is NOT considered a word character.
17.    ``()`` : allows us to group several characters to behave as one.
18.    ``|`` (pipe symbol) : the logical OR operation.
19.    ``^`` : matches the beginning of the line.
20.    ``$`` : matches the end of the line. 
21.    ``\b`` : word boundary - wither the beginning or end of a word.
22.    e.q. email address: ```[a-zA-Z0-9.+-_]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,63}```
23.    e.q. IP address: ```\b(\d{1,3}\.){3}\d{1,3}\b```

### egrep examples:

1. Print every line in myfile.txt containing the string 'mellon'.
   
   ```egrep 'mellon' myfile.txt```

2. Same as above but print a line number before each line.
  
   ```egrep -n 'mellon' myfile.txt```
 
3. Print each file in the directory projectx which contains a number of 8 digits or more.
  
   ```egrep -l '[0-9]{8,}' /files/projectx/*```
    
4. Print every line of myfiles.txt containing an email address.
  
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
# source example file: mysampledata.txt
01  Fred apples 20
02  Susy oranges 5
03  Mark watermellons 12
04  Robert pears 4
05  Terry oranges 9
06  Lisa peaches 7
07  Susy oranges 12
08  Mark grapes 39
09  Anne mangoes 7
10  Greg pineapples 3
11  Oliver rockmellons 2
12  Betty limes 14

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

