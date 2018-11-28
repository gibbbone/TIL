# Linux Command Line

## Images
#### Download  
Reference: [SO question](https://stackoverflow.com/questions/32330737/ubuntu-using-curl-to-download-an-image)

```bash
# use this
curl https://www.python.org/static/apple-touch-icon-144x144-precomposed.png > image.png
# or this
wget https://www.python.org/static/apple-touch-icon-144x144-precomposed.png
```
#### Convert pdf to eps
```bash
convert default_multirank_ns.png eps2:default_multirank_ns.eps
convert corr_gdp_multi.pdf eps2:corr_gdp_multi.eps
```

## History 

#### Pipe history to file
Create a txt file with all history commands without lines and a title with today date (for backup).
```bash
history |  awk 'sub("$", "\r")' >> "cl_full_"`date +%Y%m%d`".txt"
```
If you want to remove the line numbers:
```bash
history | cut -c 8- >> file.txt
```

#### Search history
```bash
history | grep command
```

#### Infinite history with autorefresh on reload
Add this to .bashrc:
```bash
export HISTCONTROL=ignoredups:erasedups  # no duplicate entries
export HISTSIZE=100000                   # big big history
export HISTFILESIZE=100000               # big big history
shopt -s histappend                      # append to history, don't overwrite it

# Save and reload the history after each command finishes
export PROMPT_COMMAND="history -a; history -c; history -r; $PROMPT_COMMAND"
```
Taken from [here](https://unix.stackexchange.com/a/48113/261707) (SO answer). 
## Jupyter
#### List running notebook
Check if there are notebook running:
```bash
jupyter list notebook
```
If there are and you don't remember their PID (you don't), you can find them here:
```bash
ps aux
```

## Latex 
#### Convert doc to tex via pandoc
```bash
pandoc file_name.docx -f docx -t latex -s -o file_name.tex
```


## File management

#### Download a full folder from TFP server
``` bash
wget -m -np -nH --cut-dirs=1 --content-on-error -erobots=off https://url.of.the.folder/
```
#### Copy content of a folder inside a remote folder
From [this SO answer](https://unix.stackexchange.com/a/232995/261707):
``` bash
scp ~/local_dir/* user@host.com:/var/www/html/target_dir
``` 
> The -r option means "recursively", so you must write it when you're trying to transfer an entire directory or several directories. 


## Config

#### Reduce length of prompt string
Source: [SO answer](https://unix.stackexchange.com/a/26950/261707)
```bash
# show last two folders
export PROMPT_DIRTRIM=2
# reset
export PROMPT_DIRTRIM=0
```

## Looping
Check [here](http://swcarpentry.github.io/shell-novice/05-loop/index.html). 
```bash
for string in one two three
do
  for number in 25 30 37 40
    do
      echo "$string"_"$number"
    done
done
```
Notes:
- list are white-space separated series of strings
- to use string with spaces included quotes are necessary
- thes same goes when referencing it in the loop (see the echo command)

## Scripting

##### Basics
1. Edit a .sh file
1. Run the file with `$bash file.sh` 
1. The script will accept each word added after the filename when run as positional argument. Refer to it with $position ($1 for the first, $2 for the second and so on)

#### Run script inside script
Taken from [here](https://stackoverflow.com/a/12363366/6332373) (SO answer): run two script sitting in the same foldr.

First script:
```bash
#!/bin/bash
echo "+++++++++ This script is about to run another script. +++++++"
bash ./script_b.sh
echo "+++++++++ This script has just run another script. +++++++"
```
Second script:
```bash
#!/bin/bash
echo "What do the priest says before killing his terminal?"
echo "Bash to bash, AST to AST."
```
Outcome:
```bash
+++++++++ This script is about to run another script. +++++++
What do the priest says before killing his terminal?
Bash to bash, AST to AST.
+++++++++ This script has just run another script. +++++++
```



