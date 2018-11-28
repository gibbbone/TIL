# R programming language 

## Command line
Check [here](https://swcarpentry.github.io/r-novice-inflammation/05-cmdline/). From terminal run:
```bash
Rscript file.r arg1 arg2 arg3
```

(Positional) Arguments will be captured by using
```r
 args <- commandArgs(trailingOnly = TRUE)
 first_arg <- args[1] #and so on
```

## Cat
Cat can pipe to python commands!

