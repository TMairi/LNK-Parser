# LNK-Parser

An experimental BASH script designed to parse out the contents of data structures found in Windows shortcut (LNK) files.

## ABOUT

This is a relatively simple (*experimental*) BASH script which carves out and parses the data structures associated with LNK files. One of the design goals for this script was to ensure that it could work on most default Linux distributions, meaning there is no over-reliance on unique tools.

To this end, the script primarily takes advantage of the `xxd` tool to seek and carve out the LNK data structures. LNK files are (for the most part) well-documented by Microsoft, which can be found [here](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-shllink).

To test the functionality of this script, over 200 LNK file samples were used, which were generated from various Windows Operating Systems ranging from XP to 10. Please be aware that this script may encounter parsing errors if the input LNK file is corrupted or the data structures are missing.

## USAGE
```shell
$  ./lnkparser [-arg/--argument] [LNK FILE] [-o/--output] [OUTPUT FILE]
```

## ARGUMENTS

* `-a`, `--all`:        Display the data from all defined LNK parsers
* `-H`, `--header`:     Verify the header of the LNK file
* `-L`, `--links`:      Display the enabled LinkFlags for the LNK file
* `-l`, `--linkinfo`:   Display the contents of the LinkInfo structure
* `-A`, `--attributes`: Display the attributes of the target file
* `-t`, `--timestamps`: Display the MAC timestamps of the target file
* `-s`, `--shell`:      Display the contents of the shell data (if present)
* `-S`, `--size`:       Display the size (in bytes) of the target file
* `-i`, `--index`:      Display the Icon Index value
* `-w`, `--window`:     Display the Window State of an application launched by the link
* `-k`, `--key`:        Display the keystrokes used to launch the application
* `-e`, `--extra`:      Display the contents of the ExtraData structure (if present)
* `-o`, `--output`:     Write the output to a file (CSV compatible)
* `-v`, `--version`:    Display version number and exit
* `-h`, `--help`:       Display help information and exit
* `-m`, `--manual`:     Display information about LNK files and their structure
        
When using the `-o` parameter, the script will automatically detect whether to output the contents into CSV form by reading the output file extension. Therefore, to output in CSV format, please make sure your output file ends with either `.csv` or `.CSV`.

You can also write the output of multiple arguments to the same output file as this script is set to append data rather than write to a new file.

## EXAMPLES

To check the validity of the LNK headers:
```shell
$  ./lnkparser -H file.lnk
```

To display the target file MAC timestamps ONLY:
```shell
$  ./lnkparser --timestamps file.lnk
```

To display all available data and write the output to a CSV file:
```shell
$  ./lnkparser -a file.lnk -o lnk_data.csv
```

Display brief information about the LNK structure
```shell
$  ./lnkparser -m
```

## CHANGELOG:
* v0.10 (2021-03-26):  Core code written, pseudo-code for parsing functions written
* v0.11 (2021-03-26):  Primary functions implemented and tested, fixed user sanitisation
* v0.12 (2021-03-26):  Added eight new data structure parsers, all tested and successfully parsing with no issues
* v0.14 (2021-03-27):  Added more parsers for the target file paths and fixed a null byte error in the hex parsing
* v0.15 (2021-03-27):  Fixed two broken parsers with better hex-reading commands and made some of the variable names more readable
* v0.16 (2021-03-27):  Implemented input file header checks for each function and link flag checks where appropriate
* v0.20 (2021-03-27):  Implemented parser for extra data and cleaned up several scripts
* v0.21 (2021-03-27):  Implemented shell data parser for OS version and MS-DOS timestamps
* v0.22 (2021-03-27):  Added shell data parser for MFT INDX information where available
* v0.23 (2021-03-28):  Cleaned up the code, added a missing VolumeID parser and fixed an issue when calculating the target size in MiB
* v0.25 (2021-03-28):  Added an output data option with CSV support, tested and working as it should
* v0.30 (2021-03-28):  Several parsing issues fixed, v0.3 of the experimental script now achieves all intended functionality without issue
* v0.31 (2021-04-08):  Hotfix for file deletion output error. Also changed the name of the output file to be more unique
* v0.32 (2021-09-30):  Hotfix for handling LNK files which include spaces in the file name. Reported by NeutralKaon (Thanks!)


## KNOWN ISSUES:

* The BASH script does not handle unicode characters well.
* There are no in-depth parsers for network share-related data.
