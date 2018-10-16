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

## Config

#### Reduce length of prompt string
Source: [SO answer](https://unix.stackexchange.com/a/26950/261707)
```bash
# show last two folders
export PROMPT_DIRTRIM=2
# reset
export PROMPT_DIRTRIM=8
```
