#LinuxCommands 

The **uniq** command is used for identifying and filtering duplicate lines within text files. The command structure is as follows:
```
uniq [input_file] [output_file]
```
Here, the input file contains the original text, while the output file is where we want to store the unique lines within the original text. Any lines that are an exact duplicate of an earlier line will be removed in the output.
### Optional Commands
#### Count Occurrences (-c)
Using **uniq -c** will print the unique lines within a text file, along with the number of occurrences of that line in the original text file. The number is displayed to the left of each unique line.

For example, running **uniq -c**  on a file containing the following text:
```
Alice,Electronics
Alice,Electronics 
Alice,Electronics 
Bob,Books 
Bob,Books 
Charlie,Clothing
Charlie,Clothing 
David,Home Goods 
Eve,Toys 
Frank,Sports
```
 Will produce the following output:
 ```
 3 Alice,Electronics 
 2 Bob,Books 
 2 Charlie,Clothing 
 1 David,Home Goods 
 1 Eve,Toys 
 1 Frank,Sports
 ```
#### Identify Duplicates (-d)
Using **uniq -d** will print only lines with duplicates within a text file. Using the same example as before, this command will produce the following output:
```
Alice,Electronics 
Bob,Books 
Charlie,Clothing
```
#### Identify Unique Lines (-u)
Using **uniq -u** provides the opposite effect of **uniq -d**, that is, it will only print lines that appear exactly once.
#### Ignore Casing (-i)
Using **uniq -i** will ignore casing when comparing lines to determine if they are unique.
#### Skip the First N Fields
**uniq -f N** will skip the first N fields when comparing lines.
#### Skip the First N Characters
**uniq -s N** will skip the first N characters when comparing lines.