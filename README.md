# Parallel R file read with `parallel` and `data.table` packages

There will be times when you will want to read multiple text files in parallel. Data ingest is one of the arduous steps of analysis. For some time now the `parallel` package has existed in R for ... well for parallelizing tasks either on the cores of your computer, or executing jobs on a cluster. The `parallel` package brings together functionality from the `snow` package and the `multicore` package. If you are on a Windows OS, you will not be able to take advantage of the `multicore` or `mc` type functionality. My advise is to switch to using a proper OS ;-).

The `data.table` package is phenominal for handling large datasets in memory and for carrying out analysis on those datasets. It is trully an invaluable tool. One of the many cool things in the `data.table` package is the `fread` function for reading in text files. The `read.csv` function 
