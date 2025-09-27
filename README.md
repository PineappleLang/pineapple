Hi, my name is Robert, I'm the creator of the Pineapple programming language, thank you for your time, and attention.
I made Pineapple not to particularly compete with any specific programming language, but to create something I wanted. This is the programming language I wanted to daily drive, so I made it.

There are 3 official 'implementations' of the Pineapple programming language that I'm working on:
 - the Iwa, a runtime environment for running Pineapple applications, uncompiled; written in C, and is the recommended daily driver for commercial, or hobbyist projects
 - the EliJ compiler, and work environment; written in C mostly, and made for those who desire performance from a compiled executable; this is far more of a community based project, where people who happen to like Pineapple help me build it's compilation tooling, as I'm just an idiot with a keyboard, and an internet connection
 - the PyPapple interpreter; the first implementation of the Pineapple programming language; will always be supported, because I want to

The official technical documentation is [here](pineapple-lang.github.io/pineapple), which is currently in-development.

To get started with Pineapple, it's recommended to either:
 - if you already have Python installed, then install PyPapple as a pip package using [Python 3.10+](https://www.python.org/downloads/). Either to your system (as it as no dependencies other than Python), or within a venv if you want.
 - install the Iwa runtime environment, which will add Iwa to your path (release currently unavailable)


Create a file:
`test.papple`
```
out('I am the walrus')
```

and feed it to PyPapple:
`pypapple test.papple`

or Iwa
`iwa test.papple`


I do not recommend yet playing with the EliJ compiler if you are learning. It's still very early in it's development.


### Here's everything you need to get started with Pineapple:

*every statement is separated by either a newline, or a semi-colon (`;`)*
*all blocks of statements are wrapped in brackets (functions, and class bodies)*
**

```pineapple
~ comments are anything preceeded by a graves
~{
    multi-line comments preceeded by a graves, and then wrapped with brackets
}
```

Variables are duck typed:
```pineapple
x = 10
greeting = 'hello'
```

But can be made strict:
```pineapple
x:int = 10
greeting:str = 'hello'
```

Here's the basic data types:
```pineapple
x:int = 10
pi:float = 3.141
greeting:str = 'hello'
alive:bool = true
some_potential = none
fruits:list = [ 'apple', 'cherry', 'mango' ]
student_gpas:dict = [ 'Alice' : 3.2,
                      'Jason' : 2.9, ]
```


When declaring functions, you can specify the 'return variable' afterward the parameters, which will be the return value automatically. If no return variable is specified, it will return `none`.
```pineapple
fnc add(x, y) sum {
    sum = x + y ~ automatically returned once statements ends
}

x = add(5,5)
out(x)
```
Outputs: `10`


How to make a class:
```pineapple
obj Cat {
	name:String ~ variables are declared as class-scope
	
	fnc init(name) _ {
		_.name = name ~ use `_` as to reference class scope
	}
}
```

You got your while loops:
```pineapple
while true {
    out('To infinity and beyond!')
}
```

And your for loops:
```pineapple
for i in items {
    out(i)
}
```

Throw some conditionals in there:
```pineapple
if x is 10 {
	out('x is 10')
} elif x is 5 {
	out('x is 5')
} else {
	out('x is something else')
}
```


Try/catch statements, nothing out of the ordinary here:
```pineapple
try {
	x = some_thing()
} catch e {
	out(e)
}
```


You can get a little fancy with it if you want with some inline statements:
```
~ Loop Assignment
x = for num in numbers if num % 2 is 0

~ Conditional Assignment
x = x if some_thing else none

~ Inquiry Assignment (ignores runtime errors) |
x = some_thing() ~ fallback to none by default
x = some_thing() ? 0 ~ fallback to 0
```


### Now let's talk some of the nuances
Any reference, assignment, call, or instantiation

`_` is the reference symbol for "owner" of the declaration, if used in scope of script, the script in-memory as a module is the owner.

In a function, you can reach to the outer module's namespace with the `_`
```pineapple
wallet = 0
payout = 500

fnc pay() {
    _.wallet += payout
}
```

You cannot put a function in a function, here's a decorator written in a Pythonish style that would produce an error:
```pineapple
fnc log_decorator(func) result {
    fnc wrapper(*args, **kwargs) result {
        out('Calling {func.name}')
        result = func(*args, **kwargs)
        out('{func.name} finished')
    }
    result = wrapper()
}

@log_decorator
fnc say_hello(name){
    out('Hello, {name}!')
}

say_hello('Alice')
```

here's how you'd implement a decarator in Pineapple
```
fnc log_decorator(func, *args, **kwargs) result{
    out('Calling {func.name}')
    result = func(*args, **kwargs)
    out('{func.name} finished')
    result = func()
}

@log_decorator
fnc say_hello(name){
    out('Hello, {name}')
}

say_hello('alice')
```