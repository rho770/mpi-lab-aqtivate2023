# PDC Summer School 2022: MPI Lab 2

## Introduction

In this lab you will get more familiar with MPI I/O and MPI performance measurements.

### Goals

Get experience in MPI I/O as well as MPI performance.

### Duration

3 hours

### Source Codes

- MPI I/O. Serial hello world in C and Fortran ([hello_mpi.c](hello_mpi.c) and [hello_mpi.f90](hello_mpi.f90))
- MPI Derived types and I/O. Serial STL file reader in C and Fortran ([mpi_derived_types.c](mpi_derived_types.c) and [mpi_derived_types.f90](mpi_derived_types.f90)
- MPI Latency: C and Fortran ([mpi_latency.c](mpi_latency.c) and [mpi_latency.f90](mpi_latency.f90))
- MPI Bandwidth : C and Fortran ([mpi_bandwidth.c](mpi_bandwidth.c) and [mpi_bandwidth.f90](mpi_bandwidth.f90))
- MPI Bandwidth Non-Blocking: C and Fortran ([mpi_bandwidth-nonblock.c](mpi_bandwidth-nonblock.c) and [mpi_bandwidth-nonblock.f90](mpi_bandwidth-nonblock.f90))

### Preparation

In preparation for this lab, read the "General Instructions for the MPI Labs".

## Exercise 1: MPI I/O

MPI I/O is used so that results can be written to the same file in parallel. Take the serial hello world programs and modify them so that instead of writing the output to screen the output is written to a file using MPI I/O.

The simplest solution is likely to be for you to create a character buffer, and then use the ``MPI_File_write_at`` function.

## Exercise 2: MPI I/O and derived types

Take the serial stl reader and modify it such that the stl file is read (and written) in parallel using collective MPI I/O. Use derived types such that the file can be read/written with a maximum of 3 I/O operations per read and write.

The simplest solution is likely to create a derived type for each triangle, and then use the ``MPI_File_XXXX_at_all`` function. A correct solution will have the same MD5 hash for both stl models (input and output), unless the order of the triangles has been changed.

```
md5sum out.stl data/sphere.stl
822aba6dc20cc0421f92ad50df95464c  out.stl
822aba6dc20cc0421f92ad50df95464c  data/sphere.stl
```

## Exercise 3: Bandwidth and latency between nodes

Use ``mpi_wtime`` to compute latency and bandwidth with the bandwidth and latency codes listed above.

For this exercise you should compare different setups where (a) both MPI ranks are on the same node, e.g.

```
salloc -p shared --nodes=1 --cpus-per-task=2 -t 0:30:00 -A edu22.summer --reservation=<name-of-reservation>
mpirun -n 2 ./mpi_latency.x
```

or on separate nodes, e.g.

```
salloc -p main --nodes=2 --cpus-per-task=2 -t 0:30:00 -A edu22.summer
mpirun -n 2 ./mpi_latency.x
```

Compare the different results and reason about the observed values.

## Acknowledgment

The examples in this lab are provided for educational purposes by [National Center for Supercomputing Applications](http://www.ncsa.illinois.edu/), (in particular their [Cyberinfrastructure Tutor](http://www.citutor.org/)), [Lawrence Livermore National Laboratory](https://computing.llnl.gov/) and [Argonne National Laboratory](http://www.mcs.anl.gov/). Much of the LLNL MPI materials comes from the [Cornell Theory Center](http://www.cac.cornell.edu/).  We would like to thank them for allowing us to develop the material for machines at PDC.  You might find other useful educational materials at these sites.
