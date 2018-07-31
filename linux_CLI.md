# Linux Command Line

## Download images 
Reference: [SO question](https://stackoverflow.com/questions/32330737/ubuntu-using-curl-to-download-an-image)

```bash
# use this
curl https://www.python.org/static/apple-touch-icon-144x144-precomposed.png > image.png
# or this
wget https://www.python.org/static/apple-touch-icon-144x144-precomposed.png
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
####

## Jupyter
#### List running notebook
Check if there are notebook running:
```bash
jupyter list notebook
```
If there are and you don't remember their PID (you don't), you can find them here:
```
ps aux
```
