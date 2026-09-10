#LinuxCommands

**tr**, or the translate command, is used for various tasks involving text. The syntax is as follows:
```
tr [OPTION]... SET1 [SET2]
```
The tr command reads an input text and transforms it based on the given option(s), then writes the result to standard output (stdout). For example, the following command, where "greeting.txt" contains the text "hello, world": 
```
cat greeting.txt | tr 'a-z' 'A-Z'
```
Will print "HELLO, WORLD" to the terminal. The output of greeting.txt is piped to the tr command, which translates each character in the first set ('a-z') to the corresponding character in the second set ('A-Z').

This command does not modify the original file; you would have to redirect the output to another text file to save the transformed text.
### Deleting Characters (-d)
Using **tr -d** will delete all characters from a given set and write the result.

As an example, the following command, where "punctuated.txt" contains the text "Hello World, How are you?":
```
cat puncuated.txt | tr -d '[:punct:]'
```
Will print "Hello World How are you" to the terminal. '[:punct:]'  is a character class that represents all punctuation characters. Character classes are predefined sets of characters that make it easier to specify groups of characters.
### Translating Multiple Characters
tr can also be used for simple encryption and decryption. 

The following is an example command, where "encoded.txt" contains the text "Uijt jt b tfdsfu nfttbhf.":
```
cat encoded.txt | tr 'b-zaB-ZA' 'a-zA-Z'
```
Which will print "This is a secret message." To the terminal. Here, the first set represents a shifted alphabet (covering both lowercase and uppercase letters), while the second is a regular alphabet. This creates a mapping where each letter in the text is shifted back by one letter in the alphabet (b in first set → a in second set, c in first set → b in second set, etc.). This example shows a type of encryption called a **Caesar cipher**.
### Compliment Set (-c)
The compliment option (-c) specifies all characters not contained within the following set.

This is demonstrated in the example below, where "log_entry.txt" contains "User123 logged in at 09:45 AM on 2023-08-15":
```
cat log_entry.txt | tr -cd '[:digit:]'
```
Which prints "123094520230815." With the set '[:digit:]' specifying all digits (0-9), together -cd '[:digit:]' means "delete all characters that are not digits."
### Squeezing Repeats (-s)
The squeeze option (-s) can be used to clean up data with unnecessary repetition by "squeezing" repeated characters into a single occurrance.

The following is an example:
```
echo "This is a test with extra spaces." > ~/project/spaced.txt
cat spaced.txt | tr -s ' '
```
Which gives the output: "This is a test with extra spaces." The **-s** option squeezes the repeats of the ' ' character together.
### Truncate (-t)
The **-t** option, followed by two strings, truncates string1 to the length of string2.