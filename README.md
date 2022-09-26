# tccl IDE

## What is this ?
I made a programming language called TCCL and I wrote an interpreter for it in Python, but I thout that editing this as plaintext file was a bit annoying, so I made my own IDE.<br>
(The interpreter is not on GitHub yet, I have to make a few final adjustements before relasing it)

# TCCL language

## Base syntax
The syntax is pretty simple: *`<function name>`* `[...arguments]`.<br>
But, that's not only it, you can also send the return value of the function into a variable like so: *`<function name>`* `[...arguments]` **`>>`** *`<variable name>`*.<br>
You can also make a line conditionnal with **`if`** *`a`* ___`comparator`___ *`b`* placed at the end of the line, the comparator can be any of these: `==`, `!=`, `<`, `<=`, `>`, `>=`, `is`, `!is`. I am pretty sure you understand what most of them are, but what are `is`, and `!is` ? Well, they are used to check the type of a value (this is not very useful because in most situations variables are staticly typed but there is an `any` wildcard that can mess things up a little sometimes).<br>
But how to create variables ? Pretty straight forward, **`define`** ___`type`___ *`<variable name>`* `[initial value]`. The default types are: `string`, `int`, `boolean`, `byte`, `void`.<br>
You can set the value of a variable after declaring it with **`set`** *`<variable name>`* *`<value>`*.

I'll detail *functions*, *if statements*, *loops* and more in the next parts.

## Hello, world !
By default, TCCL deos not have any printing fuction, we have to *include* it with the `#include` directive: `#include <stdlib>` the `<` `>` are shortcuts for `stdlib.ch++`.
The `stdlib` librairy provides a handlful of functions, but I will only use two of them for this example: `print` and `println`.<br>
The program needs an entry point, and for that we need to create a `main` *function*, the declaration syntax is again, pretty simple: **`fn`** ___`type`___ *`<function name>`*; in this case it would be `fn void main`, and the function body has to be closed with the `end` keyword.
```
#include <stdlib>

fn void main
    // some code here
end
```
> Everything following `//` on a line will be ignored 

This is how the code looks so far, but we still need to display some text, `print` and `println` both  do that, but `println` adds a newline automatically after the text, where `print` does not. 
They both take any number of arguments and they display all of them separated by spaces.
```
#include <stdlib>

fn void main
    println "Hello, world !"
end
```
This is the TCCL *Hello, world !* program.

A few more words about functions, they can take parameters. To declare parameters it is as simple as *`<parameter name>`*`:`___`type`___, but you have to add spaces between each parameter, and you cannot add any space between the the colon and the parameter name or type.
Here's a simple example for this:
```
#include <stdlib>

fn int sum_of a:int b:int
    define int o
    add a b >> o
    return o
end

fn void main
    define int a 5
    define int b 10
    define int c

    sum_of a b >> c

    println c
end
```
> In the `main` *function*, the `c` variable is not assigned a default value, and by default, `int`s are given a value of 0.

In this example, a `sum_of` *function* is created, and it retuns the sum of the two given `int`s. In the main *function*, three numbers are created, `a`, `b` and `c` with a default value of `5`, `10` and `0`. `sum_of`is called with `a` and `b` as the argument and the return value is redirected into `c`, which now holds `5+10`, or `15`, lastly, this value is shown into the console.

## Turing completeness
Conditional execution is very important, so I tried not to make this part of the language too complicated, you can have one line execute conditionally, but most of the time you would want multiple lines to be without having to repeat the condtition every time, that's where the `do` keyword becomes useful, by defualt it does not allow much, in fact, its only purpose is to create a new scope like this:
```
do
    // some code in a separate scope
end
```
> All variables inside a scope are destroyed when it is closed. 

But, where the `do` keyword is really useful is when combined with an `if` like so:
```
do if a == 1
    // some stuff to do if the 'a' variable is equal to 1
end
```

`stdlib` comes with a lot of other useful functions such as `input` and `convert`. `input` waits for the user to type in something and returns a `string`. `convert` converts (when possible) a value of a type to another and returns the converted value, it should be used like so: **`convert`** *`<value>`* ___`type`___.

With all of this it is possible to make a very simple calculator program:
```
#include <stdlib>

fn void main

    define string tmp
    define string operator
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
At first glance, this might seem very complicated, but do not worry; it is not and I will explain it:<br> 
The first lines can be ignored, as well as the last one, they are just required for making the rest work.<br>
Now, the 5 next line define a few fariables, the first one is used for storing the user input that will then be processed accordingly, the next one will store the operator that will determine what to do with the two numbers, speaking of which, the next two one are the converted numbers and the last one, the output value.<br>
The `loop` keyword creates a, well... you've guessed it, loop and it has to be closed with an `end` keyword.<br>
Then, the user is prompted with `'A: '`, and the typed value is stored in `tmp`, the value is checked against `"exit"`, and if they are equal, it will `break` out of the loop and exit. If it did not break, the value is converted to an `int` and then put into `a`.<br>
The user is then prompted with `'Operator: '`, and the given value is place dinto the `operator` variable.<br>
After that, a `'B: '` prompt is shown, and the answer is then converted into an `int` and placed into `a`.<br>
The next part applied an oberation betwen `a` and `b` according to which operator was selected, and the value is placed into `q`.<br>
Lastly, this value is shown.