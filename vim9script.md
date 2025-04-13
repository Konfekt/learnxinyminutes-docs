---
name: Vim9Script
filename: learnvim9script.vim
contributors:
  - ["HiPhish", "http://hiphish.github.io/"]
  - ["Yegappan Lakshmanan", "https://github.com/yegappan"]
  - ["OpenAI", "https://openai.com"]
---

# Vim9Script in Y Minutes

Vim9Script is a modernized scripting language introduced in Vim 9.0. It improves performance, readability, and structure over legacy Vimscript. This guide is modeled after the Learn X in Y Minutes format and assumes familiarity with Vim and basic programming concepts.

> To enable Vim9Script in a file, start with:
```vim
vim9script
```

---

## Table of Contents

1. [Primitive Types and Operators](#primitive-types-and-operators)
2. [Variables and Collections](#variables-and-collections)
3. [Control Flow](#control-flow)
4. [Functions](#functions)
5. [Classes](#classes)
6. [Modules and Imports](#modules-and-imports)
7. [File I/O](#file-io)
8. [Exceptions](#exceptions)
9. [Advanced Features](#advanced-features)
10. [Testing](#testing)
11. [External Commands](#external-commands)
12. [JSON and Regex](#json-and-regex)

---

## Primitive Types and Operators

```vim
# Numbers
var i: number = 42
var f: float = 3.14

# Booleans
var done: bool = true

# Strings
var s: string = 'hello'

# Arithmetic
var sum = 1 + 2
var div = 10 / 3
var mod = 10 % 3
var pow = float2nr(pow(2, 3))

# Comparison
echo 1 == 1      # true
echo 2 != 3      # true
echo 2 > 1       # true
echo 2 >= 2      # true
echo 1 < 2       # true
echo 2 <= 2      # true

# Logical
echo true && false  # false
echo true || false  # true
echo !true          # false

# Ternary
echo true ? 'yes' : 'no'
```

---

## Variables and Collections

```vim
# Variable declaration
var name: string = 'Vim'
var count = 10  # type inferred

# Constants
const pi = 3.1415

# Lists
var l: list<number> = [1, 2, 3]
echo l[0]
l->add(4)
l->remove(1)

# Tuples
var t: tuple<number> = (1, 2, 3)
echo t[1]

# Dictionaries
var d: dict<number> = {a: 1, b: 2}
echo d.a
d.c = 3

# Sets (via dict keys)
var s = {1: true, 2: true}
```

---

## Control Flow

```vim
# If / Else
if count > 5
  echo 'big'
elseif count == 5
  echo 'medium'
else
  echo 'small'
endif

# For loop
for i in range(3)
  echo i
endfor

# While loop
var i = 0
while i < 3
  echo i
  i += 1
endwhile

# Loop control
for x in [1, 2, 3]
  if x == 2
    continue
  endif
  if x == 3
    break
  endif
  echo x
endfor
```

---

## Functions

```vim
# Basic function
def Add(x: number, y: number): number
  return x + y
enddef

echo Add(3, 4)

# Default arguments
def Power(base: number, exp: number = 2): number
  return float2nr(pow(base, exp))
enddef

# Variable arguments
def Sum(...args: list<number>): number
  return reduce(args, (x, y) => x + y)
enddef

# Closures
def MakeAdder(x: number): func
  def Adder(y: number): number
    return x + y
  enddef
  return funcref('Adder')
enddef

var add5 = MakeAdder(5)
echo add5(3)  # => 8
```

---

## Classes

```vim
class Point
  var x: number
  var y: number

  def new(x: number, y: number)
    this.x = x
    this.y = y
  enddef

  def Move(dx: number, dy: number)
    this.x += dx
    this.y += dy
  enddef

  def ToString(): string
    return $"({this.x}, {this.y})"
  enddef
endclass

var p = Point.new(1, 2)
p.Move(3, 4)
echo p.ToString()  # => (4, 6)
```

---

## Modules and Imports

```vim
# Source another script file
source mylib.vim

# Runtime path
runtime plugin/myplugin.vim

# Define script-local functions
def s:Helper()
  echo 'internal'
enddef
```

---

## File I/O

```vim
# Write
writefile(['line1', 'line2'], 'file.txt')

# Append
writefile(['line3'], 'file.txt', 'a')

# Read
var lines = readfile('file.txt')
echo lines
```

---

## Exceptions

```vim
try
  var lines = readfile('nofile.txt')
catch /E484:/
  echo 'File not found'
finally
  echo 'Done'
endtry

# Throw
throw 'MyError'
```

---

## Advanced Features

```vim
# Lambda
var square = (x) => x * x
echo square(4)

# Partial
def Log(level: string, msg: string)
  echo $"[{level}] {msg}"
enddef

var Warn = function('Log', ['WARN'])
Warn('Disk low')

# Decorator-like
def LogWrap(f: func): func
  def wrapper(...args: list<any>): any
    echo 'Calling'
    var result = call(f, args)
    echo 'Done'
    return result
  enddef
  return funcref('wrapper')
enddef
```

---

## Testing

```vim
v:errors = []

assert_equal(4, 2 + 2)
assert_notequal(1, 2)
assert_true(1 < 2)
assert_false(2 < 1)
assert_match('\d\+', 'abc123')
assert_notmatch('\d\+', 'abc')

if len(v:errors) == 0
  echo 'All tests passed'
else
  echo 'Test failures:'
  echo v:errors
endif
```

---

## External Commands

```vim
# Run a shell command and capture output
var result = system('ls')
echo result

# Run and split into lines
var lines = systemlist('ls')
for line in lines
  echo line
endfor

# Check exit status
echo v:shell_error
```

---

## JSON and Regex

```vim
# JSON encode/decode
var data = {'name': 'Vim', 'version': 9}
var json = json_encode(data)
echo json

var parsed = json_decode(json)
echo parsed.name

# Regex match
var s = 'abc123'
if s =~ '\d\+'
  echo 'Contains digits'
endif

# Replace
var new = substitute('foo bar', 'bar', 'baz', '')
echo new
```

---

## Tips and Idioms

```vim
# Source guard (plugin pattern)
if exists('g:loaded_myplugin')
  finish
endif
var g:loaded_myplugin = true

# Default value
var greeting = get(g:, 'myplugin_greeting', 'Hello')

# Script-local function
def s:Internal()
  echo 'Only accessible in this script'
enddef

# Command definition
command! Hello echo 'Hello Vim9'

# Autocommand
augroup AutoReload
  autocmd!
  autocmd BufWritePost $MYVIMRC source $MYVIMRC
augroup END
```

---

## Resources

- `:help vim9`
- `:help vim9script`
- `:help vim9-class`
- [Vim9 Script Reference](https://vimhelp.org/vim9.txt.html)
- [Vim9 Class Reference](https://vimhelp.org/vim9class.txt.html)
- [Vim9 for Python Developers](https://github.com/yegappan/Vim9ScriptForPythonDevelopers)
