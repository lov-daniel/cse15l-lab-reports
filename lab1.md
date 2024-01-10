# Lab 1 (Remote Access and FileSystem)

## cd command examples
***
Running cd with **no arguments**:
```
[user@sahara ~/lecture1/messages]$ cd
[user@sahara ~]$
```
- The working directionary was initially ~/lecture1/messages, but by running the cd command, it steps out of every file in the path. 
- This is not a error.

Running cd with **a path to a directory as an argument**:
```
[user@sahara ~]$ cd lecture1
[user@sahara ~/lecture1]
```
- Running the cd command with a path to a directory as an argument moves into the specified directory and makes it the working directory.
- This is not an error.

Running cd with **a path to a file as an argument**:
```
[user@sahara ~/lecture1]$ cd Hello.java
bash: cd: Hello.java: Not a directory
```
- By running the cd commmand with a file an argument, the command throws an error. Files are not directors so the cd command cannot move into the file.
- This is an error because the cd command can only change directories into a directory.

## ls command examples
***
Running ls with **no arguments**:
```
[user@sahara ~/lecture1]$ ls
Hello.class  Hello.java  messages  README
```
- Running ls prints a list of all the files in the currect directory.
- This is not an error

Running ls with **a path to a directory as an argument**:
```
[user@sahara ~/lecture1]$ ls messages
en-us.txt  es-mx.txt  jp.txt  zh-cn.txt
```
- Using a path to a directory as an argument to the ls command will list all of the files within that specified directory. 
- In this case, the command is viewing the messages directory and it is shown with the four different languages within the messages directory.
- This is not an error.

Running ls with **a path to a file as an argument**:
```
[user@sahara ~/lecture1]$ ls Hello.java
Hello.java
```
- Using a path to a file as an argument to the ls command simply repeats the file name.
- This is not an error.

## cat command examples
***

Running cat with **no argument**:
```
[user@sahara ~/lecture1]$ cat
cd
cd
as
as
cat
cat
^C

[user@sahara ~/lecture1]$ 
```
- Running the cat command with no arguments allows input to be read through the command line. Any input entered will be repeated until CTRL+C is used.
- This is not an error.

Running cat with **a path to a directory as an argument**:
```
[user@sahara ~/lecture1]$ cat messages
cat: messages: Is a directory
```
- Running the cat command with a path to a directory as an argument throws an error. The cat command cannot be run using a path to a directory.
- This is an error because the argument 'messages' is a directory, which cannot be used with the cat command.

Running cat with a **path to a file as an argument**:
```
[user@sahara ~/lecture1/messages]$ cat jp.txt
こんにちは世界。
```
- Running the cat command with a path to a file as an argument prints out the contexts of the file.
- This is not an error.
