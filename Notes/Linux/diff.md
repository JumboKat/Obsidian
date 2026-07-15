#LinuxCommands 

The **diff** command is used to compare the contents of two files and display the differences between them. 

Take the following code as an example:
```bash
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
- The numbers indicate the line numbers in both files where changes occur. In this case, lines 1 and 2 are different, as well as line 4.
- The letter 'c' means "change." Other possible options are 'a' for "add" and 'd' for "delete".
- Lines starting with '<' are from the first file.
- Lines starting with '>' are from the second file.
- The '---' separates the content of the first file from the second file.
- Line 3 is not shown as there are no differences.
### Unified Format (-u)
The unified format **-u** option provides a more readable output. The example below compares the difference between standard and unified formats. 
```bash
$ cat script_v1.py
def greet(name):
	print("Hello, " + name + "!") 
	
def main(): 
	name = input("Enter your name: ") 
	greet(name) 
	
if __name__ == "__main__":
	main()
	
$ cat script_v2.py
def greet(name):
	print(f"Hello, {name.capitalize()}!")
	
def main(): 
	name = input("Enter your name: ") 
	greet(name)
	print("Thank you for using this script!")
	
if __name__ == "__main__":
	main()
```

Using the command as normal yields the following output:
```bash
$ diff script_v1.py script_v2.py
2c2
<	print("Hello, " + name + "!") 
---
>	print(f"Hello, {name.capitalize()}!") 
6a7
>	print("Thank you for using this script!")
```
- The '6a7' tells us that a new line has been added at line 7 in the new version.

Using the command with -u yields the following output:
```bash
$ diff -u script_v1.py script_v2.py
--- script_v1.py 2023-12-28 10:00:00.000000000 +0000 
+++ script_v2.py 2023-12-28 10:05:00.000000000 +0000 
@@ -1,8 +1,9 @@ 
def greet(name): 
-	print("Hello, " + name + "!") 
+	print(f"Hello, {name.capitalize()}!") 

 def main(): 
 	name = input("Enter your name: ") 
 	greet(name) 
+	print("Thank you for using this script!") 

if __name__ == "__main__":
```
The following is a breakdown of the output:
- The first two lines show the files being compared, with their timestamps
- Lines starting with '-' are from the first file.
- Lines starting with '+' are from the second file.
- Lines without - or + show lines with no changes.
- The "@@ -1,8 +1,9 @@ " indicates that we're seeing lines 1-8 from the first file and 1-9 from the second file.
This format is often preferred as it shows unchanged lines, which provides more context around the changes.
### Ignore Whitespace Changes
Using **diff -w** ignores whitespace (spaces, tabs) changes. If only whitespace changes were made between two files, the command will return no output.
### Comparing Directories
The diff command can also be used to compare entire directories. It shows both differences in files as well as file content between two directories. Using the recursive (**-r**) option allows diff to recursively compare subdirectories as well.

```bash
echo "This is a file in dir1" > dir1/file.txt 
echo "This is a file in dir2" > dir2/file.txt 
echo "This file is unique to dir1" > dir1/unique1.txt 
echo "This file is unique to dir2" > dir2/unique2.txt

diff -r dir1 dir2
```

Provides the output:
```bash
Only in dir1: unique1.txt
Only in dir2: unique2.txt
diff -r dir1/file.txt dir2/file.txt
1c1
< This is a file in dir1
---
> This is a file in dir2
```
### Additional Options
- **-y**: side-by-side comparison.
- **-i**: ignore case differences.
- **-b**: ignore changes in the amount of whitespace.
- **-B**: ignore changes whose lines are all blank.
- **-q**: report only whether files differ, without showing the differences.