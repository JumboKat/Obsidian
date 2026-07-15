#LinuxCommands 

The **diff** command is used to compare the contents of two files and display the differences between them. 

Take the following code as an example:
```
$ cat file1.txt 
This is version 1 of the file.
It contains some initial content.
This line is the same in both files.
This is the fourth line.

$ cat file2.txt 
This is version 2 of the file.
It contains updated content.
This line is the same in both files.
This is a modified fourth line.

$ diff file1.txt file2.txt
1,2c1,2
< This is version 1 of the file.
< It contains some initial content.
---
> This is version 2 of the file.
> It contains updated content.
4c4
< This is the fourth line.
---
> This is a modified fourth line.
```
The breakdown of the output is as follows:
- The numbers indicate the line numbers in both files where changes occur 
- The letter 'c' means "change."