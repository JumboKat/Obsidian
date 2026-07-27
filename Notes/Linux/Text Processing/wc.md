The **wc** (word count) command is used to count lines, words, and or characters in a file.
### Line Counting (-l)
**wc -1** is used to count lines in a file:
```bash
$ wc -l < access.log > output1.txt
$ cat output1.txt
1562
```
- **-l** tells the wc command to count the lines of the file.
- **<** is input redirection; instead of feeding the filename as an argument to the wc command, the shell is the one that opens the file and feeds the contents to the command instead. Normally, the wc command prints the number along with the name of the file provided, but without access to the file's name, the command simply prints the line count.
- **>** is an output redirection to save the result of the wc -l command to a file (the number of lines in the file)
