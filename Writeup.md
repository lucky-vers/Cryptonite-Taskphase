# Level 0

The command to be run is

```
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

Lets understand the structure of the command first.

- `ssh`, part of the OpenSSH package is the tool used to remotely connect to servers via the CLI.
- `bandit0` is the username we wish to log in with.
- `@bandit.labs.overthewire.org` is the domain name of the server we want to connect to.
- `-p 2220` is a flag specifying the port we want to connect to the server with (in this case 2220).

Once we log in, we're greeted with the following page

```
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit0@bandit.labs.overthewire.org's password:
```

We paste the password we were given (`bandit0`), and are dropped into a shell session.

```
bandit0@bandit:~$
```

# Level 0 → Level 1

The password for Level 1 is stored as plaintext in a file called readme in the home directory. Thus, we use the `cat` shell utility to show it.

```
bandit0@bandit:~$ cat readme
NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL
```

The `cat` command simply outputs the contents of a file to the terminal. We run the same ssh command used for bandit0, but with the username being bandit1 instead.

```
~ $ ssh bandit1@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit1@bandit.labs.overthewire.org's password:
```

We then input the password in (`NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL`), and thus get to Level 1.

# Level 1 → Level 2

Here the password is stored in a file called `-`. As the name is also the indicator for reading from stdin, simply running `cat -` does nothing here.
We have to run a modified version of it.

```
bandit1@bandit:~$ cat ./-
rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi
```

Here, the `./` indicates the current working directory, so `cat` knows its trying to read from a file instead of stdin. We use the password to login to bandit2, and successfully pass the first stage.

# Level 2 → Level 3

Here, the password is stored in a file called `spaces in this filename`.

Trying to run cat normally gives us this

```
bandit2@bandit:~$ cat spaces in this filename
cat: spaces: No such file or directory
cat: in: No such file or directory
cat: this: No such file or directory
cat: filename: No such file or directory
```

`cat` does not realize our arguments are but one. We thus wrap the filename around quotation marks to indicate that it's a single argument.

```
bandit2@bandit:~$ cat "spaces in this filename"
aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG
```

We can also use backslashed spaces `\ ` instead.

```
bandit2@bandit:~$ cat spaces\ in\ this\ filename
aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG
```

We ssh into the next level, enter the password given (`aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG`) into the prompt, and finish the second stage.

# Level 3 → 4

The password is said to be stored in a hidden file in the `inhere` directory. As hidden files are always prefixed with a `.` at the beginning of their filenames, we can the following command.

```
bandit3@bandit:~$ cat inhere/.*
cat: inhere/.: Is a directory
cat: inhere/..: Is a directory
2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe
```

- The `inhere/` indicates that the file whose contents we want to output is in the `inhere` directory.
- The `.*` is a `glob` matcher, which translates to *"cat every file that starts with the character `.` and has an indefinite number of characters after it."* We use it for brevity's sake, as it sidesteps knowing the name of the file inside the `inhere` directory entirely. The only problem is the two error messages output by `cat`, indicating it tried to output the contents of a directory instead of a file.

# Level 4 → 5

The password is stored in the only human-readable file in the inhere directory. We use a new command, `file` here

```
bandit4@bandit:~$ file inhere/*
inhere/-file00: data
inhere/-file01: data
inhere/-file02: data
inhere/-file03: data
inhere/-file04: data
inhere/-file05: data
inhere/-file06: data
inhere/-file07: ASCII text
inhere/-file08: data
inhere/-file09: data
```

The `file` command allows us to determine the file type of a file without directly using `cat` on it. We can see that there's only one file (`-file07`) with the label *ASCII text* attached to it, meaning it only contains human-readable characters. We cat the file and get the password for the next level.

```
bandit4@bandit:~$ cat inhere/-file07
lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR
```

# Level 5 → 6

Here the specifications for the file containing the password are as follows

- human-readable
- 1033 bytes in size
- not executable

We use a new shell utility called `find`, and construct the command as follows

```
find . -type f -size 1033c ! -executable
```

The arguments are as follows

- `.` indicates that we want `find` to search in the current directory.
- `-type f` is to make it so the only results we see are of files `f`. If we wanted to find only directories, we would use `-type d` instead.
- `-size 1033c` filters the results to files that are only 1033 (`1033`) bytes (`c`) large.
- `! -executable` is a negation of the `-executable` argument, i.e. *"only show files that are not (`!`) executable"*

The results of the command are as follows.


```
bandit5@bandit:~$ find . -type f -size 1033c ! -executable
./inhere/maybehere07/.file2
```

We then perform `cat` on the file as usual.

```
bandit5@bandit:~$ cat ./inhere/maybehere07/.file2
P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU
```

The password is revealed to be `P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU`.

# Level 6 → 7

The password of this level has the following properties

- owned by user bandit7
- owned by group bandit6
- 33 bytes in size

The required commmand for this is
```
find / -size 33c -user bandit7 -group bandit6 2>/dev/null
```

- `/` tells find that we want to search the entire filesystem, i.e. the root directory, for the file.
- `-size 33c` means the file's size has to be exactly 33 bytes long.
- `-user bandit7` and `-group bandit6` filter for only those files owned by user bandit7 and group bandit6, respectively.
- `2>/dev/null` removes any error messages that might show up while running the command, e.g. `permission denied` errors. `2` is the indicator for the error stream `stderr`, and `>/dev/null` means we are redirecting it to the null device, i.e. discarding its output.

The result of the command is
```
bandit6@bandit:~$ find / -size 33c -user bandit7 -group bandit6 2>/dev/null
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S
```

Thus, we clear the 6th stage.

# Level 7 → 8

The password for level 8 is stored in the file data.txt next to the word millionth. We take advantage of a new command, `grep` to find the password here. `grep` is essentially a search tool to find text in plaintext files quickly and easily.

```
bandit7@bandit:~$ grep millionth data.txt
millionth       TESKZC0XvTetK0S9xNwm25STk5iWrBvP
```

Here, `millionth` is the word we need to find, and `data.txt` the filename. As we can see, the password turns out to be `TESKZC0XvTetK0S9xNwm25STk5iWrBvP`. The seventh stage has been cleared.

# Level 8 → 9

It is given here that the password for the next level is stored in the file data.txt and is the only line of text that occurs only once.

We use a slightly more complicated *chain* of commands to find the result here.

```
bandit8@bandit:~$ sort data.txt | uniq -c | sort -nr
.
.
.
     10 365RauAVsFlxktPMpoLtIf1uxijU1TfV
     10 2CQ5DQRdtoe9Ft8YpMHqCwQcN1Bk9lCI
     10 1iyGemEgn3qUOOFcAJyGPHOiewqZyp1y
     10 18DyjwhN856SsMx8bNrFSvr6rJxNQKhE
      1 EN632PlfYiZbn3PhVK3XOGSlNInNE00t
```

The logic behind this is as follows

- `sort data.txt` uses the `sort` program to arrange all the lines in the file `data.txt` alphabetically. This has the added effect of grouping together similar strings.
- Unix pipes such as `|` essentially take the standard output of the command on the left side of the pipe, and feed it into the standard input of the command on the right. This is convenient for complex text processing, as no mediator variables need to be assigned here.
- `sort -nr` has two flags in one, `-n` and `-r`. This modifies `sort` to arrange lines by their numeric increasing order instead of alphabetical, and `-r` simply reverses the order of the output from increasing order to decreasing order. This is done for convenience's sake as we won't have to scroll up the terminal to find the one line with a `1` on its left side.

The `uniq -c | sort -nr` commands can also be replaced by a single command, `uniq -u`. The `-u` flag makes it so that uniq only prints unique lines, i.e. lines with only one instance in the text stream.

As we can see, there's only a single line with one occurrence in the file, and it is `EN632PlfYiZbn3PhVK3XOGSlNInNE00t`. We use it as our password for the next level and continue forth.

# Level 9 → 10

The password for the next level is stored here in `data.txt`, inside one of the few human-readable strings. It is also preceded by several `=` characters. The chain of commands to run here is

```
bandit9@bandit:~$ strings data.txt | grep '==='
x]T========== theG)"
========== passwordk^
========== is
========== G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s
```

Here we use a new command called `strings` on `data.txt`. `strings` searches for and prints for any sequences of readable, printable characters larger than 4 characters long in the file given to it. We then pipe the result of this into `grep`. As `grep` is reading from stdin, there's no need to specify a file here. The query `'==='` finds any sequences of text which have at least three instances of `=` inside them. The result, as we can see, is a phrase roughly saying "the password is G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s". Thus, the password comes out to be `G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s`.

# Level 10 → 11

The password for this is stored in `data.txt` like before, but this time it is in base64, a simple data encoding algorithm. A base64 code typically looks like this

```
bandit10@bandit:~$ cat data.txt
VGhlIHBhc3N3b3JkIGlzIDZ6UGV6aUxkUjJSS05kTllGTmI2blZDS3pwaGxYSEJNCg==
```

Here we use another new command, `base64`. It enables us to encode and decode base64 strings.

```
bandit10@bandit:~$ base64 -d data.txt
The password is 6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM
```

We successfully obtain the password as `6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM`.

# Level 11 → 12

It is given that the password for the next level is stored in the file `data.txt`, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions. This is a common cipher, more famously known as *ROT-13*.

```
bandit11@bandit:~$ cat data.txt
Gur cnffjbeq vf WIAOOSFzMjXXBC0KoSKBbJ8puQm5lIEi
```

We use the command `tr` to decode this.

```
bandit11@bandit:~$ tr 'A-Za-z' 'N-ZA-Mn-za-m' < data.txt
The password is JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv
```

Here the conversion can be split into four parts.

| The string | What alphabetical range `tr` interprets the string as                                                      |
|------------|------------------------------------------------------------------------------------------------------------|
| `A-Z`      | From the capital 'A' to capital 'Z'                                                                        |
| `a-z`      | From the small 'a' to small 'z'                                                                            |
| `N-ZA-M`   | Between the capital 'N' and capital 'Z', and subsequently the range between the capital 'A' to capital 'M' |
| `n-za-m`   | Between the small 'n' and small 'z', and subsequently the range between the small 'a' to small 'm'         |

We thus find the password for level 12 as `JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv`.

