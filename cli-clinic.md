# Command line tips for developers

Think of the tale of the woodcutters

> Woodcutter A cuts wood all day. Woodcutter B keeps stopping and sitting down. At the end of the day, woodcutter B has three times more wood than woodcutter A. Woodcutter A asks: “How could this happen? You were resting all day!” Woodcutter B says: “I wasn’t resting. I was sharpening my saw.” Take time to sharpen your skills, your tools, and your resources, and you will be more productive.

- Holmes, Chet. The Ultimate Sales Machine (pp. 21-22). Penguin Publishing Group. Kindle Edition.

## Disclaimer / Windows

I only tested this on MacOS using iTerm2, it should mostly all work with Linux.

I don't have Windows, for that start here:

Configure a Linux Development Environment on Windows with WSL and VS
https://pybit.es/articles/installing-wsl-vscode-windows-11/ │

> The Windows Subsystem for Linux lets developers run a GNU/Linux environment — including most command-line tools, utilities, and applications — directly on Windows, unmodified, without the overhead of a traditional virtual machine or dualboot setup.”

## Toggling Vim and terminal processes

CTRL+Z and fg

processes in background are jobs:

$ jobs
[1]    suspended  /usr/local/Cellar/vim/9.0.0000/bin/vim
[2]    suspended  /usr/local/Cellar/vim/9.0.0000/bin/vim todos
[3]  - suspended  /usr/local/Cellar/vim/9.0.0000/bin/vim cli-clinic.md
[4]  + suspended  /usr/local/Cellar/vim/9.0.0000/bin/vim

to get number 3 in the forground:

%3

## Useful Unix commands

find (and xargs)
grep
sed & awk
perl -pe

## Unix tricks

&& combine commands but only if previous one succeeds
cd - go to previous directory (also works with git checkout)
set -e in a shell script -> fails as soon as something goes wrong

repeating stuff:
!!
!int
history

I use most: Ctrl + R == "reverse-i-search"

## Unix stdin and stdout

1 = standard output, so 1> file.out
2 = standard error, so 2> file.err

to send both combined: 2>&1 > file.out_and_err

without giving the number it defaults to standard output, e.g.
pip freeze > requirements.txt

to protect file overwriting:

set -o noclobber

force overwriting file:
pip freeze >| requirements.txt

## Unix pipes

Combine tools, stdout of one command becomes stdin of the next command, e.g.

ls | sort | uniq -c | sort -nr

Vim trick, get output of a command into vim:

some_command.sh | vi -

## Get help

man
pydoc

## Useful developer tools

git
ag (--python)
fzf
pgcli
wget / curl
jq
tmux (have not used)

## Useful shell aliases (and functions)

some of my favorites: https://gist.github.com/bbelderbos/3069c087af95ce756facf1712d501b7d

git -> ~/.gitconfig -> [alias] section

alias everything!

$ alias whatweek
whatweek='date +%U'
$ alias gitign
gitign='curl -s https://raw.githubusercontent.com/github/gitignore/main/Python.gitignore > .gitignore'
$ alias ytidea
ytidea='open https://github.com/pybites/youtube_content/issues'

etc. etc.

## Env variables

add more directories to search for commands: PATH
default editor: EDITOR
run some stuff before entering REPL: PYTHONSTARTUP
set shell prompt: PS1 (for git download this: ~/.git-prompt.sh)

## Python tools

virtual env
ipython
bpython
pipx
black / flake / ruff
pre-commit

## Pybites tools

search
alarm
pybites-tools
pybites-carbon
emojisearcher
color-searcher
git-stats
github-tools
youtube thumbs
biteme and eatlocal

## Read source code

python -m inspect / pys alias: https://gist.github.com/bbelderbos/3069c087af95ce756facf1712d501b7d#file-gistfile1-txt-L90

## Cloud cli tools

heroku toolbelt
deta
github CLI

## SSH

You don't want to type in password when connecting to login and push things to remote servers, enter:

ssh-keygen – OpenSSH authentication key utility

$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/bbelderbos/.ssh/id_rsa): /Users/bbelderbos/.ssh/test_key
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /Users/bbelderbos/.ssh/test_key
Your public key has been saved in /Users/bbelderbos/.ssh/test_key.pub
The key fingerprint is:
SHA256:VlUtVlUAOaqkbQ5Vaog3sRkWZyQk+V7UdfcRM7WvKk4 bbelderbos@Bobs-iMac.local
The key's randomart image is:
+---[RSA 3072]----+
|    .o+o+. .+++O@|
|    ..++. o.o.o.B|
|     + B o.. o o.|
|    . B *..     .|
|     o OS.      .|
|      +.+      . |
|       +  E   .  |
|        ...  .   |
|         ....    |
+----[SHA256]-----+

√ Downloads  $ lt /Users/bbelderbos/.ssh/
...
-rw-------  1 bbelderbos  staff   2.5K May 16 12:42 test_key
-rw-r--r--  1 bbelderbos  staff   580B May 16 12:42 test_key.pub

## Have fun / Q&A

$ cowsay "Let's get productive"
 ______________________
< Let's get productive >
 ----------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
