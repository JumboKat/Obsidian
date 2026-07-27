#LinuxCommands 

The **join** command allows you to merge data from multiple files.

Below is an example:
```bash
$ cat employees.txt
1001 John Engineering 
1002 Sarah Marketing 
1003 Mike Sales 
1004 Emily HR 
1005 David Finance

$ cat salaries.txt
1001 75000 
1002 65000 
1003 70000 
1004 60000 
1005 80000

$ join employees.txt salaries.txt
1001 John Engineering 75000 
1002 Sarah Marketing 65000 
1003 Mike Sales 70000 
1004 Emily HR 60000 
1005 David Finance 80000
```
Here, the join command looks at the first field (representing employee ID) in both files. When matching IDs were found, it combined the lines from both files. The output shows the employee ID, then all fields from employees.txt, then all fields from salaries.txt.
### Formatting Output (-o)
We can change the output to rearrange the order of the fields or only include specific fields using **-o**.

```bash
$ join -o 1.2,1.3,2.2,1.1 employees.txt salaries.txt
John Engineering 75000 1001 
Sarah Marketing 65000 1002 
Mike Sales 70000 1003 
Emily HR 60000 1004 
David Finance 80000 1005
```
The following is a breakdown of the command:
- -o stands for "output format"
- 1.2 refers to the second field form the first file (names)
- 1.3 refers to the third field from the first file (department)
- 2.2 refers to the second field from the second file (salary)
- 1.1 refers to the first field in the first file (employee ID)
### Unmatched Records (-a)
Lines in one file may not appear in the other file. Normally, these lines will not manifest in the output. We can add these lines to the output using **-a**. 
```bash
$ echo "1006 Alex IT" >> employees.txt
$ join -a 1 employees.txt salaries.txt
1001 John Engineering 75000 
1002 Sarah Marketing 65000 
1003 Mike Sales 70000 
1004 Emily HR 60000 
1005 David Finance 80000 
1006 Alex IT
```
Here the '-a 1' option tells the join command to include unpairable lines from the first file.
### Joining on Different Fields
We can join based on a different field, which can be useful when files are organized differently, or if you want to merge based on another common attribute.
```bash
$ cat << EOF > departments.txt
Engineering ENG
Marketing MKT
Sales SLS
HR HRS
Finance FIN
IT ITS
EOF
```

```bash
$ join -1 3 -2 1 employees.txt departments.txt
Engineering 1001 John ENG 
Marketing 1002 Sarah MKT 
Sales 1003 Mike SLS 
HR 1004 Emily HRS 
Finance 1005 David FIN 
IT 1006 Alex ITS
```
Here, the '-1 3' tells the join command to use the third field in the first file (employees.txt) as the join field, and '-2 1' tells the command to use the first field from the second file (departments.txt) as the join field.