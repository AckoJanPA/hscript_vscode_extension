# HScript Documentation Summary (Updated)

This document summarizes the HScript language, incorporating new information regarding command syntax, graphical operations, and specialized compiler-handled commands. HScript is a scripting language with capabilities for both console-based operations and graphical programming.

## 1\. Data Types

HScript defines four primary data types:

  * **NUM (Number):** Used for mathematical operations and storing numerical values.
      * *Examples:* `15`, `12`
  * **STR (String):** Used for text manipulation, storing textual data, and handling input/output operations.
      * *Examples:* `"Hello"`, `"World"`
  * **MEM (Memory):** Represents a temporary memory access token for setting and loading data that does not persist after a reboot.
      * *Examples:* `@0`, `@1`
  * **DRV (Drive):** Represents a drive access token for setting and loading files that are stored persistently, similar to a hard drive.
      * *Examples:* `$0`, `$1`

## 2\. Commands (Console/Main Functions)

These commands are used for general programming and console-based interactions:

  * **PRINT** `(1: str/int/mem)`: Prints the provided input to the console.
  * **LD1** `(1: str/int/mem)`: Loads the input value into a designated register or memory location referred to as "0".
  * **LD2** `(1: mem)`: Loads the input memory value into a designated register or memory location referred to as "1".
  * **ADD** `(1: mem)`: Performs addition of the values in "0" and "1", storing the result in the specified input memory location.
  * **SUB** `(1: mem)`: Performs subtraction of "1" from "0", storing the result in the specified input memory location.
  * **MUL** `(1: mem)`: Performs multiplication of "0" and "1", storing the result in the specified input memory location.
  * **DIV** `(1: mem)`: Performs division of "0" by "1", storing the result in the specified input memory location.
  * **SET** `(1: mem 2: str/int/mem)`: Stores the value from the second input (string, integer, or memory) into the memory cell specified by the first input.
  * **JOIN** `(1: mem)`: Concatenates the values in "0" and "1", storing the combined result back into "0".
  * **JMP** `(1: int/mem)`: Jumps to the command line number or memory address specified by the input.
  * **JEQ** `(1: int/mem)`: Jumps to the command line number or memory address if the value in "0" is equal to "2".
  * **JNEQ** `(1: int/mem)`: Jumps to the command line number or memory address if the value in "0" is not equal to "2".
  * **INP** `(1: mem 2: str/mem)`: Prompts the user for input using the provided string or memory value as a prompt, and stores the user's response into the specified memory cell.
  * **TYPE** `(1: mem 2: str/mem)`: Changes the data type of the specified memory cell to NUM, STR, or DRV.
  * **CMNT** `(1: str)`: Used for comments within the script.
  * **EXEC** `(1: str)`: Executes a string containing other HScript commands.

## 3\. Commands (Graphical Mode)

These commands are specifically designed for graphical operations:

  * **G-INIT**: Initializes the Graphical Mode (GM).
  * **G-TITLE** `(1: str/mem)`: Sets the title of the graphical window.
  * **G-RESET** `(Color)`: Fills the entire screen with a default color (`0`) or a user-specified color. Color is a `str/mem` with a single hexadecimal character (e.g., `"0"` up to `"f"`).
  * **G-TICK**: Renders all pending changes to the screen, allowing for visual updates.
  * **G-RENDER** `(1: Img Data)`: Renders an image using `Img Data` at the X position loaded by `LD1` (into "0") and the Y position loaded by `LD2` (into "1").
  * **G-MOUSE** `(1: mem 2: mem)`: Stores the current X and Y coordinates of the mouse into the specified memory locations (e.g., `mem[1] = mouseX; mem[2] = mouseY`).

## 4\. File System / Drive Operations

HScript supports basic file operations with its DRV data type:

  * **SAVE** `(1: DRV 2:istr/mem)`: Stores data from an input string or memory location into a virtual disk at the E\&S address.
  * **LOAD** `(1: DRV 2:mem)`: Loads data from a virtual disk at the E\&S address into a specified memory address.

## 5\. Sample Program: Simple Adder

This example demonstrates how to create a program that takes two numbers from the user, adds them, and prints the result:

```hscript
INP @0 "number 1:"
TYPE @0 "NUM"
INP @1 "number 2:"
TYPE @1 "NUM"
LD1 @0
LD2 @1
ADD @2
PRINT @2
```

## 6\. Image Data Format

The `Img Data` format is described as `str/mem - styled "{sizex} | {sizey} | img data"` and `"3|3|fffffffff"`. This structure is intended to create a "3x3 square" that is colored white.

## 7\. Drives:

  * **$0**: Mouse Sprite
  * **$1** to **$256**: ASCII characters (corresponding to ASCII codes 1 through 256)

## 8\. HS++ Specific Commands (Compiler-Handled)

These commands are high-level and are processed by the HS++ compiler into standard HScript commands.

  * **TEXT** `(1: int 2: int 3: str)`: Displays a string at the given X and Y coordinates. Compiles into an `EXEC` command containing multiple `FONT` commands.
  * **FONT** `(1: int 2: int 3: str)`: Renders a single character at the given X and Y coordinates by loading its sprite from a drive (e.g., `$72` for 'H').
