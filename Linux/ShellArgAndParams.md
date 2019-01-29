 ## `$*`(space deliminated args as one string) and `$@`(array of strings for each arg)
 `> ./test.sh one two "three four"`
This does not include $0
The script:

```sh
#!/bin/bash
for a in "$*"; do 
    echo $a; #result: one two three four
done

for a in $*; do
    echo $a; #result: one \n two \n three \n four \n
done

for a in "$@"; do # "" applies to each element of array
    echo $a; #result: one \n two \n three four \n
done

for a in $@; do # expand twice, first time in loop, second time space
    echo $a; #result: one \n two \n three \n four \n 
done              
```
---
## `$#` check number of args
This does not include #0
```sh
if [ $# -lt 2 ]; then
    echo $1
fi
```
---
`shift [n]` will shift args to the left(not affecting `$0`) by n(`$#, $@, $*` changes correspondingly)
```sh
while [ $# -ne 0 ]; do
    echo $1
done
```
---
Bash arg over 9th: `${19}, ${10}, ${100}`, etc

set will set $1, $2, ...
```sh
set `data`
echo "$3.$2.$6 um $4"
```


