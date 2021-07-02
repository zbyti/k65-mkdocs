#

## Comments

**K65**  uses **C**-like comments.

```c
// this is a line comment

/* this is a
   block comment */
```

## Variable declaration

Variables in **K65** are names given to chosen memory addresses.
Examples of variable declaration:

```c
var foo=0x80            // declares 'foo' at address 0x80
var foo2                // declares 'foo2' at address 0x81 (next after previous var)
var foo3, foo4          // multiple declarations per line allowed
var bar[10], bar2[10]   // [] specifies variable size in bytes (address increment for next var)
var bar3 ?              // adding '?' at the end of var declaration makes compiler print var addresses
```

## Constant declaration

The best way of defining constants is using the **evaluator**. Constants defined this way can be changed at any moment during compilation. Constants can be any value of floating point type. When used within **6502** instruction, they are converted to single byte by rounding to nearest integer and `AND`-ing with `0xFF` (this way negative values are represented in U2 form).

Examples:

```c
[                       // square brace starts evaluator expression
  MY_CONSTANT = 5,      // define constants
  SOME_NUMBER = 0x13
]                       // end of evaluator expression
```

## Bank selection

While default bank can be chosen in file list for each file, a file can span across multiple banks. Active bank can be changed at any point in the file.

Example:

```c
bank my_bank            // from now on all code and data will go to 'my_bank'
```

## Data block definition

Data blocks are defined using data keyword. Defining data block at the same time defines a label to its first element, so the block is accessibl using simple indexing, like `MyData,x`. Datablocks can have optional **alignment** or **no-page-crossing** restrictions enabled.

Examples:

```c
data MyData {
  align 16              // align to 16 byte boundary
  1 2 3 4 5 6 7 8       // data bytes folow
}

data MyData2 {
  align 256 + 8         // align to 8 bytes after page boundary (lower address byte will be 0x08)
  1 2 3 4 5 6 7 8       // data bytes folow
}

data MyData3 {
  nocross               // the data will fit completely inside single page, but can be at any offset within it
  1 2 3 4 5 6 7 8       // data bytes folow
}

data MyData {
  address 0x5000        // fixed memory address
  1 2 3 4 5 6 7 8       // data bytes folow
}

data sprite {
  align 256
  // image <file> <x0> <y0> <byte> <repeat> - gather bits from image
  //  <file>    - file name without ".bmp" extension
  //  <x0> <y0> - first pixel to scan
  //  <byte>    - scanning mode for each single byte starting with MSB (count+direction)
  //  <repeat>  - scanning mode for consecutive bytes (count+direction)

  image sprites  0 0 8> 16v   // start at pixel (0,0), each byte is 8 bits to the right, repeat 16 times going up
  image sprites 10 0 8> 16v   // do the same from (10,0)
  image sprites 20 0 8> 16v   // and again starting at (20,0)
}

data fonts {
    address 0x5000
    image "data/font" 0 0  8> 8v tiles 8 0 31
    image "data/font" 0 8  8> 8v tiles 8 0 31
    image "data/font" 0 16 8> 8v tiles 8 0 31
    image "data/font" 0 24 8> 8v tiles 8 0 31
}

data table {
  address 0x2000
  binary "table.bin"
}

data SineX {
  align 256
  0
  for x=0..213 eval [ (sin(x/212*pi*2)*.499+.499)*130 ]
}

data SineY {
  align 256
  0
  for x=0..255 eval [ (sin(x/256*pi*2)*.499+.499)*180+1 ]
}
```

## Compile-time Evaluator

The **K65** compiler has embedded evaluator that always executes at compile-time. The evaluator can perform arbitrary math operations which results are inlined in the final code as immediates. The evaluator can also set and use global compiler constants.

The evaluator is explained in detail on [K65 Evaluator](../evaluator) page.

## Code sections

Executable code in K65 compiler is specified in sections. There currently supported types of code sections are:

### `main`

This function being program entry point.

```c
main {
  a=0        // set accumulator to 0
  {} always  // loop forever
}
```

### `func`

User defined function, that can be called from the code.

```c
func inc_x {
  x++        // increments X register and returns
}            // RTS is added automatically
```

### `naked`

Just like `func`, but no `RTS` is added automatically at the end.

```c
naked inc_x_twice {
  x++        // increments X register
  goto inc_x // jump to previously defined inc_x (saves stack)
}            // no RTS here; make sure function never reaches here
```

### `inline`

User defined `macro` that is inlined in the code when used.

```c
inline inc_y {
  y++
}
```

Functions and inlines are used simply by specifying their names, which places `JSR` opcode or inlines the code. Function and inline calls do not pass any parameters. Any potential parameter and return value handling must be handled explicitly by the programmer using either registers, stack or predetermined memory locations.

```c
func test {
  inc_x      // this will use JSR instruction to call 'inc_x' defined earlier
  inc_y      // this will inline the 'inc_y' inline - note that it will not add any overhead compared to simple 'y++'
}
```

## Far calls

Functions can also be called with far prefix to mark that the target function is in another bank, than the current one. The bankswitching mechanism used is defined by the linker. Inlines do not use far prefix, because the code is simply inlined.

**NOTE**: if far call is used within inline make sure to use such inline with care - inlining such inline in another bank will copy the inline code directly, which will usually result in improper bankswitching and program crash.

```c
func test2_in_different_bank {
  far inc_x
}
```

---

*This page is still under construction - much of the infrmation is still to be filled*