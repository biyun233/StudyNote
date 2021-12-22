# Regex

### Resources

https://www.youtube.com/watch?v=_ssS5iUXHBY&list=PL55RiY5tL51ryV3MhCbH8bLl7O_RZGUUE&index=2

https://regex101.com

### Basic

There are two constructors of regex in JS:

```js
const regex = new RegExp('ll');
const regex = /ll/;
```

Different ways to check a text:

```js
const text = "Hello there!";

// return true or false
regex.test(text);

// return an array with the hit it found
regex.exec(text);
text.match(regex);
// ["ll"]

// return the index where we find this expression
text.search(regex);
```

### Flags

​	`g` == `global` : It doesn't stop after finding one hit, it'll match all occurrences of this pattern

​	`i` == `insensitive` : It doesn't care for the casing



### Symbol

​	1) pipe symbol `|` : means **or** , using with parentheses `()` 

​			e.g.  `(H|h)`   alternative to flag `i`  -> casing insensitive of 'h'

​	2)  question mark `?` :  the part (character or group) immediately prior to the question mark is optional

​			e.g. `H?ello`  both 'Hello' and 'ello' matches the expression

​			e.g. `(He)?llo`  both 'Hello' and 'llo' matches the expression

​	3)  range `[]` : the characters inside are independent, they don't have to be connected

​			e.g. `[tse]`    'testpw1AA' matched

​			**usual use: ** [a-zA-Z0-9]+

​	4)  quantifiers 

- `A?` : zero or one of A
- `A*` : zero or more of A
- `A+` : one or more of A

​	5)  start of string `^` 

​	6)  end of string `$`

​	7)  any single character `.`

​	8)  escape symbol `\` : escape the default meaning

​			e.g.   `\.`  means the character dot `.`   ,   not any single character

### Match full words

​		Everything between `^` and `$` has to be included and included in this order

​				e.g.  `^[a-z]+[0-9]+[A-Z]+!$`  **'testp2445AA!'** matched

​						**'testp2445AA!jie'** not matched



### Grouping

​		example1:  `(ab)\1`   ==>  **abab** matched

​							`\1`  means copy the result in the first capturing group `(ab)`

​							`([a-z]+)@\1`   with **ab@ab** matched



​		example2:  `(?:[a-z]+)@`

​							`?:`  means everything included in the parentheses should not be stored or should not be captured,

​							in this case,  something like `\1` is not useful

​		example3:  `(?=[a-z]+)@`

​							`?=` means positive lookahead.  Treat something as a match if it's followed by something

​							 e.g.   `a(?=c)`  ==> 'ababab**a**c'  the fourth `a ` matched

