# tpch-dbgen
repo for creating tpc-h datasets using dbgen tool, on apple silicon m1

Download the dbgen tool from [TPC.org](https://www.tpc.org/tpc_documents_current_versions/current_specifications5.asp)
Unzip the file and rename the root folder w/o spaces and special characters. Record the root folder name in .gitignore!

## Modify the dbgen/makefile.suite
```
CC = gcc
...
DATABASE = ORACLE 
MACHINE = LINUX
WORKLOAD = TPCH
CFLAGS = ... -DEOL_HANDLING (Add to get rid of trailing |)
```
These values are required to run make and you can use any of the allowed values. For my usecase, I don't care about the DDL as I use an external program to parse and load  the pipe(|) seperated files.

## Modify dbgen/bm_utils.c & varsub.c
change #include <malloc.h> to #include <sys/malloc.h>

## Run make to build dbgen
make -f makefile.suite -B (-B forces rebuild)
This produces a dbgen file which can be executed as ./dbgen -h

## Run dbgen to produce data
initial dataset with scaling factor(s) = 1
```
mkdir data && cd data
../dbgen -vf -s 1 -b ../dists.dss
```
## Run dbgen to produce changes to table data
Changes are to orders and lineitems only and are produced as a set of deletes
and updates. Produces changes up to 10% of the volume.
```
../dbgen -vf -s 1 -b ../dists.dss -U 1
```

## Verify the trailing pipe is fixed.
head nation.tbl
```
0|ALGERIA|0| haggle. carefully final deposits detect slyly agai
1|ARGENTINA|1|al foxes promise slyly according to the regular accounts. bold requests alon
2|BRAZIL|1|y alongside of the pending deposits. carefully special packages are about the ironic forges. slyly special 
3|CANADA|1|eas hang ironic, silent packages. slyly regular packages are furiously over the tithes. fluffily bold
4|EGYPT|4|y above the carefully unusual theodolites. final dugouts are quickly across the furiously regular d
5|ETHIOPIA|0|ven packages wake quickly. regu
```




