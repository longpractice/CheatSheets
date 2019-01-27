## Shell Script Variables

Assignment: `ich=yang` (no space around "=", stored always as str)

Command Substitution: `datum=$(date +%Y_%m_%d)`

Expand Var Value: `mkdir backup_${datum}_daily` "{}" needed when having none-space char behind

Undefine Variable: `unset datum`

`set -u` treat undef var as error, `set -x` show executed commands(debug purpose)

Readonly: `ich=yang; readonly ich; #ich=jim # error!! #unset ich #error!!`

---

Int arithmetic: 

* `var1=100; var2=200; var3=$(expr $var1 + $var2)` spaces needed between + sign
* `((z=5+5))`, `y=$((5 +5))`, `w=$(( y + z ))` spaces does not matter within (())
* `let z=w+20` must no space
  
Floats(bc command): `varA=0.1; varB=0.2; gesamt=$(echo "$varA+$varB" | bc)`

---
cut column of each line: `cut -c1,5,7-12,14- gedicht.txt`

cut with deliminator and field `cut -d\; -f1 datei.txt` (first field seperated by ";")

paste line by line: `paste namen.txt nummer.txt`

`tr '[a-z]' '[A-Z]' gedict.txt` capitalize all

`tr -d ' ','\n' < gedicht.txt` "-d" means delete, here delete all line break and space

`tr -s ' ' ';' < gedicht.txt` "-s" means replace continuously, this will replace all groups of space to semicolon

`${#s}` length of string s,

`${var#pattern}`remove left minimum pattern match,

`${var##pattern}`remove left maximum pattern match,

`${var%pattern}`remove right minimum patter match,

`${var%%pattern}`remove right maximum pattern match

note that pattern is wildcard(*, ?, [])

substring: `${var:start:len}`

---
Single quotes escape all special chars like backtick, backslack, dollar, brackets, star

Bouble quotes escape all(including star) except dollar(var expanded), backslash(so the script itself is line-break, no effect on the string), backtick(command substitution)

---
## Array
assign array: `array=(null eins zwei);` or `array=([2]=zwei drei)`(starting from index 2)

element assign `array[2]=two`

get element: `${array[2]}`

all elements deliminated by space `${array[*]}`

elements count in array `${#array[*]}`

`unset array` undef whole array `unset array[1]` erase a certain element(elements behind shifted backward after erasing)

copy array: `array_copy=(${array_source[*]})

sub array: `${array[*]:1:2}` (specify start and lenght)

---

## Export or typeset -x

none-exported var is not visible in subshell unless directly mensioned in command substitution, eg: `a=hello; h=$(echo $a; a=${a} god; echo $a); echo a`. Here h will be `hello god`. The last `echo` will still be "hello"(a visible in subshell in this case but modification of a does not affect parent shell here)

`export var` or `typeset -x var` will transitively make var visible in subshell

Modification on an exported var a certan subshell is only visible to its own offsprings

Modification on an exported var will never affect the ancester shell

`. ./script1.sh` (extra dot at start) will run script1.sh in current shell, not subshell, therefore var will be in same scope(useful for read conf)

Environment variables are always-exported variables

`export` without args will show all exported vars including ENVs

---
## Shell Variables

`$0`, the command called for this shell(shell name with called path)

`$0, $1, ...`, the 1st, 2nd arguments

`$$` current proc number

`$?` last command return value

`$!` last command proc number

`$*` all command line args

`$#` number of command line args


