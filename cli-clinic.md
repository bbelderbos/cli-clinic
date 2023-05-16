# Command line tips for developers

Think of the tale of the woodcutters

> Woodcutter A cuts wood all day. Woodcutter B keeps stopping and sitting down. At the end of the day, woodcutter B has three times more wood than woodcutter A. Woodcutter A asks: “How could this happen? You were resting all day!” Woodcutter B says: “I wasn’t resting. I was sharpening my saw.” Take time to sharpen your skills, your tools, and your resources, and you will be more productive.

- Holmes, Chet. The Ultimate Sales Machine (pp. 21-22). Penguin Publishing Group. Kindle Edition.

## Disclaimer / Windows

I only tested this on MacOS using iTerm2, it should mostly all work with Linux.

I don't have Windows, for that start here:

[Configure a Linux Development Environment on Windows with WSL and VS](https://pybit.es/articles/installing-wsl-vscode-windows-11/)

> The Windows Subsystem for Linux lets developers run a GNU/Linux environment — including most command-line tools, utilities, and applications — directly on Windows, unmodified, without the overhead of a traditional virtual machine or dualboot setup.”

## Make it interactive!

Please ask me to do stuff from the CLI in this session, way more interesting ...

## Toggling Vim and terminal processes

I see me use Vim a lot, if you use an IDE you can skip this. To "hide" Vim (push it to background) I use CTRL+Z and then `fg` to bring it to the foregrond.

When you pushed multiple processes to the background you can list them with the `jobs` command:

```
$ jobs
[1]    suspended  /usr/local/Cellar/vim/9.0.0000/bin/vim
[2]    suspended  /usr/local/Cellar/vim/9.0.0000/bin/vim todos
[3]  - suspended  /usr/local/Cellar/vim/9.0.0000/bin/vim cli-clinic.md
[4]  + suspended  /usr/local/Cellar/vim/9.0.0000/bin/vim
```

The Vim session for this doc is `[3]` so to get that back to the foreground I can do `%3`, `fg` would bring back the first one (top of the stack so to say).

## Useful Unix commands

Finding things:

```
$ find . -name '*.log'
./runner_logs_1682351096.log
./runner_logs_1682166796.log
./07/messages.log
./runner_logs_1682499432.log
./runner_logs_1682316770.log
./runner_logs_1682351099.log
./runner_logs_1682168138.log
$ find . -name '111'
./111
$ find . -name '111' -type f
$ find . -name '111' -type d
./111
$ alias bigfiles
bigfiles='find . -size +1000 -type f -ls | sort -k 7,7 -n'
```

Filtering things

```
$ grep ^## cli-clinic.md
## Disclaimer / Windows
## Make it interactive!
## Toggling Vim and terminal processes
## Useful Unix commands
...
```

Advanced grep -> `ag`

```
$ cd ~/code/bitesofpy && ag -l car_brands
newbies/24/bite-extended.html
newbies/24/tuples.py
newbies/24/tuples_template.py
newbies/24/test_tuples.py
newbies/22/for_loops_template.py
newbies/22/bite-extended.html
newbies/22/for_loops.py
newbies/22/test_for_loops.py
newbies/25/calling_functions_template.py
newbies/25/calling_functions.py
newbies/25/test_calling_functions.py
```

Sed

```
$ git remote get-url origin
git@github.com:pybites/bitesofpy.git
$ git remote get-url origin|sed 's,git@github.com:,https://github.com/,'
https://github.com/pybites/bitesofpy.git
```

Awk

```
$ pip list 2>/dev/null | tail -n +3|cut -d"\t" -f1
cut: bad delimiter
$ pip list 2>/dev/null | tail -n +3|awk '{print $1}'|head
aiohttp
aiosignal
anyio
async-timeout
attrs
backports.entry-points-selectable
bcrypt
beautifulsoup4
biopython
bs4
```

Perl for nice regex engine

```
$ echo smb://servername.domain.com/sharename | perl -pe 's/.*\/(.*)$/\1/g'
sharename
```

## Unix tricks

`&&` - combine commands but only if previous one succeeds
`cd -` go to previous directory (also works with git checkout)
`set -e` in a shell script -> fails as soon as something goes wrong

Repeating stuff: `!!`, `!int` or use `history` command

Mostly I use `Ctrl + R` though which lets you do a "reverse-i-search" (use type ahead on your command history)

For zsh I configured unlimited history, see [here](https://unix.stackexchange.com/questions/273861/unlimited-history-in-zsh)

## Unix stdin and stdout

`1` = standard output, so 1> file.out
`2` = standard error, so 2> file.err

To send both combined: 2>&1 > file.out_and_err

Without giving the number it defaults to standard output, e.g.

```
pip freeze > requirements.txt
```

To protect file overwriting: `set -o noclobber`

Force overwriting file: `pip freeze >| requirements.txt`

## Unix pipes

Combine tools, stdout of one command becomes stdin of the next command, e.g.

```
ls | sort | uniq -c | sort -nr
```

Vim trick, get output of a command into vim: `some_command.sh | vi -`

Look at some things used in duration.sh script (you will appreciate Python more for scripting)

## Get help

man
pydoc

## Useful developer tools

git
ag (--python)
fzf (hooked up in Vim)
pgcli
wget / curl (or py requests)
jq
tmux (have not used)

## Useful shell aliases (and functions)

Some of my favorites: https://gist.github.com/bbelderbos/3069c087af95ce756facf1712d501b7d

git -> `~/.gitconfig` -> [alias] section

Alias everything!

```
$ alias whatweek
whatweek='date +%U'
$ alias gitign
gitign='curl -s https://raw.githubusercontent.com/github/gitignore/main/Python.gitignore > .gitignore'
$ alias ytidea
ytidea='open https://github.com/pybites/youtube_content/issues'
```

etc. etc.

## Env variables

Add more directories to search for commands: `PATH`

Default editor: `EDITOR`

Run some stuff before entering REPL: `PYTHONSTARTUP`

Set shell prompt: `PS1` (for git download this: ~/.git-prompt.sh) -- for zsh I used [this](https://www.themoderncoder.com/add-git-branch-information-to-your-zsh-prompt/) to get git info in my prompt.

## Python tools

Virtual envs:

```
$ alias pvenv
pvenv='python3 -m venv venv && source venv/bin/activate'
$ alias ae
ae='source venv/bin/activate'
```
ipython (pipx)

```
$ pipx install ipython
  installed package ipython 8.13.2, installed using Python 3.10.6
  These apps are now globally available
    - ipython
    - ipython3
done! ✨ 🌟 ✨
(venv) √ bin  $ which ipython
/Users/bbelderbos/.local/bin/ipython
(venv) √ bin  $ ipython
/Users/bbelderbos/.local/pipx/venvs/ipython/lib/python3.10/site-packages/IPython/core/interactiveshell.py:889: UserWarning: Attempting to work in a virtualenv. If you encounter problems, please install IPython inside the virtualenv.
  warn(
Python 3.10.6 (main, Aug 11 2022, 13:49:25) [Clang 13.1.6 (clang-1316.0.21.2.5)]
Type 'copyright', 'credits' or 'license' for more information
IPython 8.13.2 -- An enhanced Interactive Python. Type '?' for help.

In [1]: import re

In [2]: re.search??
Signature: re.search(pattern, string, flags=0)
Source:
def search(pattern, string, flags=0):
    """Scan through string looking for a match to the pattern, returning
    a Match object, or None if no match was found."""
    return _compile(pattern, flags).search(string)
File:      /usr/local/Cellar/python@3.10/3.10.6_1/Frameworks/Python.framework/Versions/3.10/lib/python3.10/re.py
Type:      function
```

I also pipx installed `bpython` and linters like `black`, `flake8` and `ruff` (but run latter mostly through pre-commit).

## Pybites tools

Some command tools we have developed and use:

- pybites-search: search our content from the command line
- pybites-alarm: play an alarm after a time interval
- pybites-tools: mix of useful tools, e.g. upload something to s3 bucket, worldclock
- pybites-carbon: make a code carbon image
- emojisearcher: quickly copy emojis to the clipboard
- color-searcher: get color hex codes
- git-stats: get stats from a github repo
- github-tools: copy over issues to new PDM repo
- youtube-thumbs: YouTube video thumbs automation
- platform CLI wrapper tools: eatlocal + biteme

For development, Typer is my goto library to build CLI apps.

Pybites learning path: https://codechalleng.es/bites/paths/typer

## Read source code

- python -m inspect / `pys` alias: https://gist.github.com/bbelderbos/3069c087af95ce756facf1712d501b7d#file-gistfile1-txt-L90
- IPython, object??
- not cli, but worth including: grep.app

## Cloud cli tools

Each provider tends to have their command line interface tool:

- Heroku toolbelt
- Deta (trying very soon)
- Github CLI (not tried yet)
- TODO: Pulumi AI / natural language in CLIs ...

## SSH

You don't want to type in password when connecting to login and push things to remote servers, enter: `ssh-keygen (OpenSSH authentication key utility)`

```
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
```

Now upload the .pub key to to your server's `authorized_keys` and you can ssh in without username + password.

## Have fun / Q&A

```
$ cowsay "Let's get productive"
 ______________________
< Let's get productive >
 ----------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```
