Scripting is a way of executing a set of commands, gathered in a text file.
#### Shebang

A bash script file is a file with a `.sh` extension. The script begins with a shebang, a line starting with `#!` and giving the path of the interpreter used:

```shell-session
#!/bin/bash
```
#### Variables

Here is a simple way to store a variable and how to use it elsewhere:

```shell
# Defining the Interpreter 
#!/bin/bash
echo "Hey, what’s your name?"
read name
echo "Welcome, $name"
```

#### Loops

Here is an example of simple loop:

```shell
# Defining the Interpreter 
#!/bin/bash
for i in {1..10};
do
echo $i
done
```

#### Conditional statements

Here is an example of a simple `if ... else` statement in bash:

```shell
# Defining the Interpreter 
#!/bin/bash
echo "Please enter your name first:"
read name
if [ "$name" = "Stewart" ]; then
        echo "Welcome Stewart! Here is the secret: THM_Script"
else
        echo "Sorry! You are not authorized to access the secret."
fi
```

#### Comments

Comments can be added by starting a line with `#` and are useful to specify what the script does for other developers.

```shell
# Defining the Interpreter
#!/bin/bash

# Asking the user to enter a value.
echo "Please enter your name first:"

# Storing the user input value in a variable.
read name
```
