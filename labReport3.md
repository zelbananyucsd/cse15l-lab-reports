# Lab Report 3

## Command: `find`

The command I will be further exploring is `find`, which performs multiple operations aimed at finding files and directories through different methods based on command-line options. All command-line options where gathered from the Linux/UNIX manual page, which can be found at this URL (https://man7.org/linux/man-pages/man1/find.1.html) or the following hyperlink:

[find(1) - Linux manual page](https://man7.org/linux/man-pages/man1/find.1.html)



Four useful command-line options for `find` will be explored more in-depth below.


## 1. -path 

The -path command-line option allows the user to find files based on patterns in their paths. This is useful because it helps locate files within the entire directory system, and indicates their relative paths based on the current working directory. 

**Example 1:**
The below example matches all paths in the working directory that end with "51.txt". It uses the `*` wildcard which basically means to match any and all patterns that go before the "51.txt". Since the command-line option is -path, it returns all paths that include the specified characters. This is useful in the biomed subdirectory because it finds the paths for a specific research project, #51. 

``` 
-bash-4.2$ find biomed -path *51.txt
biomed/gb-2001-2-12-research0051.txt
biomed/gb-2002-3-9-research0051.txt
biomed/gb-2003-4-8-r51.txt
```

**Example 2:**
This below example matches all paths in the working directory that include "13". Since this uses two wildcards, it means that it should match any and all patterns that go before or after "13", or in more clear terms, returns any paths that simply include "13" anywhere in the path. This is useful for the subdirectory 911report since it helps filter and find the files for a specific chapter, in this case chapter 13. 
``` 
-bash-4.2$ find 911report 911report -path "*13*"
911report/chapter-13.1.txt
911report/chapter-13.2.txt
911report/chapter-13.3.txt
911report/chapter-13.4.txt
911report/chapter-13.5.txt
911report/chapter-13.1.txt
911report/chapter-13.2.txt
911report/chapter-13.3.txt
911report/chapter-13.4.txt
911report/chapter-13.5.txt
``` 

## 2. -size
The -size command-line option allows the user to find files based on their sizes. The operators + and - indicate whether it should find files that are greater than (+) or less than (-) the specified size. It is also possible to use "=" which looks for file with that exact size. Multiple letters can be used to indicate different size classifications: 'b' is bytes, 'k' is kilobytes, 'M' is megabytes, 'G' is gigabytes and 'T' is terabytes. This is useful to be able to filter through the directory and find files of specific sizes, which can be useful in certain circumstances such as finding large files to delete for memory space or find trivially-small files that may not be needed anymore. 

**Example 1:**
This below example finds files in the biomed subdirectory that are smaller than 10 kilobytes. This is useful when searching for files that are small. 
``` 
-bash-4.2$ find biomed -size -10k 
biomed/1471-2334-3-13.txt
biomed/1471-2490-3-2.txt
``` 

**Example 2:**
This below example finds files in the government subdirectory that are greater than 200 kilobytes. This is useful when searching for files that are large and include important and dense data, or for backup/memory purposes. 
``` 
-bash-4.2$ find government -size +200k
government/About_LSC/commission_report.txt
government/Env_Prot_Agen/bill.txt
government/Gen_Account_Office/GovernmentAuditingStandards_yb2002ed.txt
government/Gen_Account_Office/Statements_Feb28-1997_volume.txt
government/Gen_Account_Office/d01591sp.txt
``` 

## 3. -type 
The -type command-line option allows the user to find files based on their type. The different types are denoted by different letters, including: "f" for regular file, "d" for directory, "b" for block special, "c" for character special, and more. This is useful when differentiating between files and directories, or filtering all files into the type the user is looking for. This is useful in larger directories where there are multiple types of files, and the user is attempting to look for specific types. It is also useful not just for inclusive searching but for exclusive searching as well, which is allowed by combining -not and -type as seen in example 2 below. 

**Example 1:**
This below examples searches for all regular files in 911report, which are files with some sort of data/text. 
``` 
-bash-4.2$ find 911report -type f
911report/chapter-1.txt
911report/chapter-10.txt
911report/chapter-11.txt
911report/chapter-12.txt
911report/chapter-13.1.txt
911report/chapter-13.2.txt
911report/chapter-13.3.txt
911report/chapter-13.4.txt
911report/chapter-13.5.txt
911report/chapter-2.txt
911report/chapter-3.txt
911report/chapter-5.txt
911report/chapter-6.txt
911report/chapter-7.txt
911report/chapter-8.txt
911report/chapter-9.txt
911report/preface.txt
``` 
**Example 2:**
This below examples looks through the main technical directory and finds all files that are not considered regular files, which includes mainly directories. This is a useful combination (-not -type) that helps narrow down the find results based on specific type criteria. 
``` 
-bash-4.2$ find technical -not -type f        
technical
technical/911report
technical/biomed
technical/government
technical/government/About_LSC
technical/government/Alcohol_Problems
technical/government/Env_Prot_Agen
technical/government/Gen_Account_Office
technical/government/Media
technical/government/Post_Rate_Comm
technical/plos
``` 

## 3. -mtime and -ctime
The -mtime and -ctime command-line options allow the user to find files based on their modification time (-mtime) and creation time (-ctime). These also use the operators "+" and "-" as indicators of greater than or less than a number of days. The number that follows the operator is multiplied by 24 to indicate the number of hours ago. For example 1 is 24 hours, 2 is 48 hours, etc. This would help sort through a big database of files and find files created either recently or long ago. This can help find files that are relatively old and can possibly be deleted to free up memory if they are no longer of use. On the other hand, it can help track activity on which files have been recently modified. 

**Example 1:**
This example below finds files of type regular file (using -type f) that have been modified in the past 2 days. This is achieved through -mtime and the "-" before 2 means "less than". Thus, this finds any files that have been modified in the past 48 hours and that exist inside 911 report. 

``` 
-bash-4.2$ find 911report -type f -mtime -2
911report/chapter-1.txt
911report/chapter-10.txt
911report/chapter-11.txt
911report/chapter-12.txt
911report/chapter-13.1.txt
911report/chapter-13.2.txt
911report/chapter-13.3.txt
911report/chapter-13.4.txt
911report/chapter-13.5.txt
911report/chapter-2.txt
911report/chapter-3.txt
911report/chapter-5.txt
911report/chapter-6.txt
911report/chapter-7.txt
911report/chapter-8.txt
911report/chapter-9.txt
911report/preface.txt
``` 
**Example 2:**
This below example finds files that have been created (using -ctime) int the past 3 days ("-" indicates less than, and 3 means 72 hours). This uses the relative path for Env_Prot_Agen inside the current working directory and is useful in revealing newly-created files. 
``` 
-bash-4.2$ find government/Env_Prot_Agen -ctime -3
government/Env_Prot_Agen
government/Env_Prot_Agen/1-3_meth_901.txt
government/Env_Prot_Agen/atx1-6.txt
government/Env_Prot_Agen/bill.txt
government/Env_Prot_Agen/ctf1-6.txt
government/Env_Prot_Agen/ctf7-10.txt
government/Env_Prot_Agen/ctm4-10.txt
government/Env_Prot_Agen/final.txt
government/Env_Prot_Agen/jeffordslieberm.txt
government/Env_Prot_Agen/multi102902.txt
government/Env_Prot_Agen/nov1.txt
government/Env_Prot_Agen/ro_clear_skies_book.txt
government/Env_Prot_Agen/section-by-section_summary.txt
government/Env_Prot_Agen/tech_adden.txt
government/Env_Prot_Agen/tech_sectiong.txt
``` 
