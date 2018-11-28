# Useful shell commands


## Unzip
### Unzip in parallel
```for i in *zip; do unzip "$i" & done```

### Unzip 10 files in parallel

```find . -name '*.zip' -print0 | xargs -0 -I {} -P 10 unzip {}```

### Unzip 10 files in parallel without recursion into subdirectories

```find . -maxdepth 1 -name '*.zip' -print0 | xargs -0 -I {} -P 10 unzip {}```

### Count number of pdf files in a directory

```find . | grep '.pdf' | wc -l```

## List all unique file extensions in a directory

```find . -type f | sed -rn 's|.*/[^/]+\.([^/.]+)$|\1|p' | sort -u```

## Grep a string in a directory and sub directories

```grep -R 'string' dir/```

or

```find dir/ -type f -exec grep -H 'string' {} +```

## Find and replace all occurrences of a string in a directory and sub directories

```find ./ -type f -exec sed -i -e 's/apple/orange/g' {} \;```

or

```grep -rl 'apples' /dir_to_search_under | xargs sed -i 's/apples/oranges/g'```

## Grep only the first match and stop

```grep -o -a -m 1 -h -r "Pulsanti Operietur" /path/to/dir | head -1```

## Extract the first 2 characters of a string

```${string:position:length}```

## Install an RPM in Ubuntu

Run this command to install alien and other necessary packages:

```sudo apt-get install alien dpkg-dev debhelper build-essential```

To convert a package from rpm to debian format, use this command syntax. The sudo may not be necessary, but we’ll include it just in case.

```sudo alien packagename.rpm```

To install the package, you’ll use the dpkg utility, which is the internal package management tool behind debian and Ubuntu.

```sudo dpkg -i packagename.deb```

The package should now be installed, providing it’s compatible with your system.

## Find a file containing a particular string

```grep -R "string" directory/path/```

-R is for dereference recursive i.e. recursive with following sym-links

## Find directory names with wildcard
Find all directories with a name prefix of "ora10"

```find / -type d -name "ora10*"```

## Find files containing a specific string and move them

```grep -lir 'string' ~/directory/* | xargs mv -t DEST```

## Extract one column of a csv file

```awk -F "\"*,\"*" '{print $2}' textfile.csv```

## Find and replace string in a text file with awk

```awk '{gsub("string1", "string2", $0); print}' file.txt```

## Find and replace string in a text file with sed

```sed -i -e 's/string1/string2/g' file.txt```

## Replace new line characters (\n) using sed

```sed -e ':a' -e 'N' -e '$!ba' -e 's/\n/ /g' file```

1. Create a label via :a
2. Append the current and next line to the pattern space via N
3. If we are before the last line, branch to the created label $!ba ($! means not to do it on the last line as there should be one final newline).
4. Finally the substitution replaces every newline with a space on the pattern space (which is the whole file).

## Replace new line characters (\n) using tr

```tr '\n' ' ' < input_filename```

```tr -d '\n' < input.txt > output.txt```

```tr --delete '\n' < input.txt > output.txt```

## List directories in current path

```ls -d /path/to/folder/*/```

## Convert .pem to .crt 

```openssl x509 -outform der -in your-cert.pem -out your-cert.crt```

## Store the output of a command to a variable

```
$ b=$(pwd)
$ echo $b
/home/user1
or
```$ a=`pwd`
$ echo $a
/home/user1
```
The first way is preferred. Note that there can't be any spaces after the = for this to work.

## Change case of a string 
### To lower case

```
$ string="A FEW WORDS"
$ echo "${string,,}"
a few words
```

### To upper case
```
$ string="a few words"
$ echo "${string^}"
A few words
$ echo "${string^^}"
A FEW WORDS
```

## Delete lines that do not start with a pattern
### Modify in place
```sed -i '/^\(report\|-t\(h\|o\)\)/!d' your_file```

### Keep a backup of original and modify in place

```sed -i'.bak' -e '/^\(report\|-t\(h\|o\)\)/!d' your_file```

## Get a website's response time 

```curl -s -w %{time_total}\\n -o /dev/null http://www.iaditya.com```

## Rename multiple files

Rename all files that begin with 'fgh' to begin with 'jkl':

```rename 's/^fgh/jkl/' fgh*```

or 

```rename fgh jkl fgh*```

## Find and delete files or directories

### Find and delete files
```find . -name ".svn" -exec rm -r "{}" \;```

### Find and delete directories
```find . -name ".svn" -type d -exec rm -r "{}" \;```

### Find and delete empty directories and directories that contain only empty directories
```find . -name ".svn" -type d -empty -delete```

## Grep in a PDF
Install either pdfgrep or pdftotext

```find /path -iname '*.pdf' -exec pdfgrep <REGEX_PATTERN> {} +```

```pdftotext my.pdf - | grep 'pattern'```

## Convert all PDFs in a directory to txt

```
 #!/bin/bash
 FILES=*.pdf
 for f in $FILES
 do
  pdftotext -enc UTF-8 $f 
  echo $f
 done
```

## Count all occurrences of a string in a directory

```cat * | grep -c 'aditya'```

## Search for multiple words or patterns 

### Use single quotes in the pattern

```grep 'pattern*' file1 file2```

### Use extended regular expressions

```egrep 'pattern1|pattern2' *.py```

### Use this syntax on older Unix shells

```grep -e pattern1 -e pattern2 *.pl```

### Search for three words

```grep 'word1\|word2\|word3' /path/to/file```

### Search all text files

```grep 'word*' *.txt```

### Search all python files for 'wordA' or 'wordB' 

```
grep 'wordA*'\''wordB' *.py
grep -E 'word1|word2' *.doc
grep -e string1 -e string2 *.pl
egrep "word1|word2" *.c
```

## Search in multiple PDF files

```find /path -name '*.pdf' -exec sh -c 'pdftotext "{}" - | grep --with-filename --label="{}" --color "your pattern"' \;```

- The "-" is necessary to have pdftotext output to stdout, not to files. 
- The --with-filename and --label= options will put the file name in the output of grep. - The optional --color flag is nice and tells grep to output using colors on the terminal.

## Check size of directory
du -sh <directory_path>

