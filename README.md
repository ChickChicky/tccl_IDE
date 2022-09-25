# tccl IDE

## What is this ?
I made a programming language called TCCL and I wrote an interpreter for it in Python, but I thout that editing this as plaintext file was a bit annoying, so I made my own IDE.<br>
(The interpreter is not on GitHub yet, I have to make a few final adjustements before relasing it)

# TCCL language

## Base syntax
The syntax is pretty simple: *`<function name>`* `[...arguments]`.<br>
But, that's not only it, you can also send the return value of the function into a variable like so: *`<function name>`* `[...arguments]` **`>>`** *`<variable name>`*.<br>
You can also make a line contionnal with **`if`** *`a`* ___`comparator`___ *`b`* placed at the end of the line, the comparator cna be any of these: `==`, `!=`, `<`, `<=`, `>`, `>=`, `is`, `!is`. I am pretty sure you understand what most of them are, but what is `is`, and `!is` ? Well, it can be used to check the type of a value (this is not very useful because in most situations because variables are staticly typed but thre is an `any` wildcard that can mess things up a little sometimes).<br>
But how to create variables ? Pretty straight forward, **`define`** ___`type`___ *`<variable name>`* `[initial value]`. The default types are: `string`, `int`, `boolean`, `byte`, `void`.<br>
You can set the value of a variable after declaring it with **`set`** *`<variable name>`* *`<value>`*.

I'll detail *functions*, *if statements*, *loops* and more in the next parts.

## Hello, world !
By default, TCCL deos not have any printing fuction, we have to *include* it with the `#include` directive: `#include <stdlib>` the `<` `>` are shortcuts for `stdlib.ch++`.
The `stdlib` librairy provides a handlful of functions, but only two them are really useful for this example: `print` and `println`.<br>
The program needs an entry point, and for that we need to create a `main` *function*, the declaration syntax is again, pretty simple: **`fn`** ___`type`___ *`<function name>`*; in this case it would be `fn void main`, and the function body has to be closed with the `end` keyword.
```
#include <stdlib>

fn void main
    // a bit empty...
end
```
This is how the program looks like so far, and also, averything after a `//` is considered as comment.<br>
But we still have to display some text, `print` and `println` both show text, but `println` adds a newline automatically after the text, where `print` does not. 
They both take any number of arguments and they display all of them separated by spaces.
```
#include <stdlib>

fn void main
    println "Hello, world !"
end
```
This is the TCCL *Hello, world !* program.

## Turing completeness
Conditional execution is very important, so I tried not to make this part of the language too complicated, you can have one line execute conditionally, but most of the time you would want multiple lines to be without having to repeat the condtition every time, that's where the `do` keyword becomes useful, by defualt it does not allow much, in fact, its only purpose is to create a new scope like this:
```
do
    // some code in a separate scope
end
```
All variables inside a scope are destroyed when exiting out of it. But, where the `do` keyword is really useful is when combined with an `if` like so:
```
do if a == 1
    // some stuff to do if the 'a' variable is equal to 1
end
```
Speaking of variables, for now, I've shown how to create and modify variables, but I did not show how to use functions to do that; when a function's return type is the same as a varaible's type, it is possible to redirect the output value into this variable. A simple example for that would be:
```
define int a 10
define int b 5
define int c

add a b >> c

println c
```
> In these examples, I reduce down the porgram length to prevent pollution from other stuff, but it assumes that all the code is inside the `main` function and `stdlib` is included.

Here, 3 variables are declared, `a`, `b` and `c` that are all `int`s, `a` and `b` are intialized with `10` and `5`, by default `int`s are intialized with `0`, so is `c`.<br>
Then, the `add` function is called with `a` (`10`) and `b` (`5`), and the retun value (`15`) is redirected into `c`, meaning that it now has the value `15`.<br>
So, when printing `c`, *15* should be shown.

`stdlib` comes with a lot of other useful functions such as `input` and `convert`. `input` waits for the user to type in something and returns a `string`. `convert` converts (when possible) a value of a type to another and returns the converted value, it should be used like so: **`convert`** *`<value>`* ___`type`___.

With all of this it is possible to make a very simple calculator program:
```
#include <stdlib>

fn void main

    define string tmp
    define string oprator
    define int a
    define int b
    define int q

    println "Type 'exit' to exit"

    loop

        input "A: " >> tmp
        break if tmp == "exit"
        convert tmp int >> a

        input "Operator: " >> operator

        input "B: " >> tmp
        convert tmp int >> b

        add a b >> q if operator == "+"
        sub a b >> q if operator == "-"
        mul a b >> q if operator == "*"
        div a b >> q if operator == "/"
        mod a b >> q if operator == "%"

        print "> "
        println q
        println "" 

    end

end
```
The user is prompted for an A value, on operator and a B value, the operator can be any of 5 basic operations: *+-\*/%*.