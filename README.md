# Parallel R file read with `parallel` and `data.table` packages

## Introduction

There will be times when you will want to read multiple text files in parallel. Data ingest is one of the arduous steps of analysis. For some time now the `parallel` package has existed in R for ... well for parallelizing tasks either on the cores of your computer, or executing jobs on a cluster. The `parallel` package brings together functionality from the `snow` package and the `multicore` package. If you are on a Windows OS, you will not be able to take advantage of the `multicore` or `mc` type functionality in the `parallel` package. My advise is to switch to using a proper OS ;-). If you use R for analysis, I would also strongly recommend that you read the book **Parallel R** *: Data Analysis in the Distributed World By Q. Ethan McCallum, Stephen Weston*. There is a clear difference between using `snow`-type function and `multicore` or `mc`-type functions. The `snow`-type functions are essentially for clusters but can be used for parallel processing on your local machine, whereas the `mc` type function run locally on your machine using the system `fork` command. This is why `mc`-type functions will not run on Windows systems.

The `data.table` package is phenominal for handling large datasets in memory and for carrying out analysis on those datasets. It is trully an invaluable tool. The `read.table` or `read.csv` functions come out of the box with R, and all R programmers have at one time or another used these functions, they are good and convenient but can sometimes be a little on the slow side when text files get large. The `fread` function from the `data.table` package is an equivalent tool to `read.table` but is much faster.

The laptop I am working on has a quad core i7 processor showing 8 threads by hyper-threading and 30GB of memory.

## Preparations

The `mclapply` function in the `parallel` package is the multicore equivalent of often used `lapply` function. Very often you are working locally and simply need to read lots of files on a computer or a laptop. In these cases `mclapply` is your friend. It is convenient and with just a few changes you can make best use of your local computing resources.

First we read in the file names with the `list.files` function. This is another very convenient function it is such a task saver. the data used are the Aquisition data sets from Fannie Mae and there are 49 such file in the folder ...

```
fNames <- list.files("raw/", full.names = TRUE)
```

Then we compare times of `lapply` and `mclapply` functions with `read.csv`, and then look at the effect of using the `fread` function instead:

```
system.time(x1 <- lapply(fNames, read.csv, sep = "|", header = FALSE))
 user      system elapsed 
 209.846   0.851  210.748 
 
system.time(x2 <- mclapply(fNames, read.csv, sep = "|", mc.cores = 8, header = FALSE))
 user      system elapsed 
 388.316   3.353  65.352 
 
system.time(x3 <- mclapply(fNames, fread, sep = "|", mc.cores = 8))
 user     system elapsed 
 85.441   2.148  23.012 
```
The R programmer will know that the time of importance is the elapsed time, notice that the `fread` function doesn't have to be told whether there is a header or not, it is clever enough to work it out for itself. But this isn't all, the other nice thing about using the `data.table` format is that binding the list of tables together to form one large table is much faster than using data frames:

