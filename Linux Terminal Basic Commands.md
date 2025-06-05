Here are a list of basic commands that we can use inside a Linux terminal.

To output any text provided: `echo string`, which will simply display the given string in the terminal.

To know which user we are logged in as: `whoami`.

To interact with the file system:
- `ls path`: to list element in the `path` folder
- `cd path`: to travel inside the folder structure and access `path`
- `cat path`: to display the content of a given file, located at `path`
- `pwd`: to print current path we are in
- `touch file_name`: create a file with the given name
- `mkdir folder_name`: create a new folder with the given name
-  `cp file_name copied_file_name`: copies a file with provided new name
- `mv current_dest new_dest`: moves file/folder located at the current destination to the new destination
- `rm file_name`: allows to delete a given file
- `file file_name`: determines the type of a file

To search for files, we can use the `find` command. To search a file according to its name, we can do `find -name given_name`. We can as well search for files with a given extension by typing `find -name *.extension`, which will search thanks to the `*` wildcard all files with given `extension`.

To search for specific file content, we can use `grep searched_words file_name`. 

To count the number of lines of a file, we can use `wc -l file_name`.

Operators can be used as well when makig a command:
- `&` allows to run commands in the background of a terminal
- `&&` allows to run multiple commands at once by writing them on a single line and separate them using this operator
- `>` is a redirector, it takes the output of a command and direct it elsewhere. When provided a file_name after, it will save the output content inside the specified file
- `>>` is the same as `>` but appends the output instead of replacing

## Flags and switches

Flags and switches refer to the arguments the commands can take to execute specific actions.

For exemple using `ls -a` will allow to display even hidden files.

The full list of switches/flags available can be found in the man pages of a command, using `man <command_name>`.
