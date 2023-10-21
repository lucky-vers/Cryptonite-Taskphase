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

We paste the password we were given **(bandit0)**, and are dropped into a shell session.

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

We then input the password in **(NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL)**, and thus get to Level 1.

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

We ssh into the next level, enter the password given **(aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG)** into the prompt, and finish the second stage.

# Level 3 → 4

The password is said to be stored in a hidden file in the `inhere` directory. As hidden files are always prefixed with a `.` at the beginning of their filenames, we can the following command.

```
bandit3@bandit:~$ cat inhere/.*
cat: inhere/.: Is a directory
cat: inhere/..: Is a directory
2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe
```

- The `inhere/` indicates that the file whose contents we want to output is in the `inhere` directory.
- The `.*` is a `glob` matcher, which translates to *cat every file that starts with the character `.` and has an indefinite number of characters after it.* We use it for brevity's sake, as it sidesteps knowing the name of the file inside the `inhere` directory entirely. The only problem is the two error messages output by `cat`, indicating it tried to output the contents of a directory instead of a file.

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

