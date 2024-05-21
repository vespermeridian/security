# One-Liners and Command References

## Sorting and Searching

*Printing part of a line (the 3rd feild, with : as a delimiter, and output to a file)*
``` 
cat text.txt | awk 'BEGIN {FS=":"};{print $3}' > output 
```

*Counting the occurrences of text in a file (based on the 2nd field)*
```
cut -d ':' -f2 text.txt  | uniq -c
```
*Search for occurrences of a word in all files in a directory and list the file*
```
grep -c <FILENAME> *
```
