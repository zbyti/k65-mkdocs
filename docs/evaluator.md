#

Evaluator in **K65** is compile-time executed expression language very similar to **C/C++** expressions.

## Introduction

In every place where the compiler expects a single number, you can invoke the evaluator by simply using square brackets.

```c
data NumberFour {
  [2+2]
}
```

The evaluator can be also invoked freely outside any section body, e.g. to set variables:

```c
[ FOUR = 4 ]
```

which can be later used freely as compiler constants and within further evaluator expressions:

```c
data FourFiveSix {
  FOUR             // value of variable FOUR defined earlier
  [FIVE = FOUR+1]  // defines variable FIVE and returns its value
  [FIVE+1]         // uses recently set FIVE value
}
```

## Operators

Available operators and their precedence levels are similar to operators in **C** and **C++**.

List sorted by precedence from highest to lowest:

* level __2__, associativity: **left-to-right**
    * `++` suffix incremen
    * `--` suffix decrement
    * `()` function call
    * `[]` array subscripting
    * `.` single argument function call
* level __3__, associativity: **right-to-left**
    * `++`  prefix increment
    * `--`  prefix decrement
    * `+`  unary plus
    * `-`  unary minus
    * `!`  logical NOT
    * `~`  bitwise NOT (One's Complement)
* level __5__, associativity: **left-to-right**
    * `*` multiplication
    * `/` division
    * `%` modulo (remainder)
* level __6__, associativity: **left-to-right**
    * `+` addition
    * `-` subtraction
* level __7__, associativity: **left-to-right**
    * `<<`  bitwise left shift
    * `>>`  bitwise right shift
* level __8__, associativity: **left-to-right**
    * `<` less than
    * `<=` less than or equal to
    * `>` greater than
    * `>=` greater than or equal to
    * `?>` select greater value (maximum)
    * `?<` select smaller value (minimum)
* level __9__, associativity: **left-to-right**
    * `==` equal to
    * `!=` not equal to
* level __10__, associativity: **left-to-right**
    * `&` bitwise AND
* level __11__, associativity: **left-to-right**
    * `^` bitwise XOR (exclusive or)
* level __12__, associativity: **left-to-right**
    * `|` bitwise OR (inclusive or)
* level __13__, associativity: **left-to-right**
    * `&&` logical AND
* level __14__, associativity: **left-to-right**
    * `||` logical OR
* level __15__, associativity: **right-to-left**
    * `?:` ternary conditional
* level __16__, associativity: **right-to-left**
    * `= `  direct assignment
    * `+=`  assignment by sum
    * `-=`  assignment by difference
    * `*=`  assignment by product
    * `/=`  assignment by quotient
    * `%=`  assignment by remainder
    * `<<=` assignment by bitwise left shift
    * `>>=` assignment by bitwise right shift
    * `&=`  assignment by bitwise AND
    * `^=`  assignment by bitwise XOR
    * `|=`  assignment by bitwise OR
* level __18__, associativity: **left-to-right**
    * `,` expression list (executes in sequence, returns value of the last)

## Functions

### `acos( x )`

Arcus cosinus

---

### `addbyte( sec, b )`

Add byte b to section sec

---

### `asin( x )`

Arcus sinus

---

### `ceil( x )`

Round up to nearest integer

---

### `clamp( x, min, max )`

Clamp x to range min to max

---

### `color( r, g, b )`

Return palette index for nearest color to color (r,g,b)

---

### `color( x )`

Return palette index for nearest color specified in format 0xRRGGBB

---

### `cos( x )`

Cosinus

---

### `error( err )`

Print error message err ane terminate compilation

---

### `floor( x )`

Round down to nearest integer

---

### `frac( x )`

Get fractional part ( x-floor(x) )

---

### `index( tab, x )`

1-dimensional indexing operator ( same as tab[x] )

---

### `index( tab, x, y )`

2-dimensional indexing operator ( same as tab[x,y] )

---

### `max( a, b )`

Maximum

---

### `min( a, b )`

Minimum

---

### `pow( x, y )`

Power function

---

### `print( msg )`

Print message msg

---

### `rnd( )`

Random value 0 <= x < 1

---

### `round( x )`

Round to nearest integer

---

### `sin( x )`

Sinus

---

### `size( sec )`

Current size of section sec

---

### `sqrt( x )`

Square root