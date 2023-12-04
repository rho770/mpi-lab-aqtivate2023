# AQTIVATE Workshop 2023: MPI Lab 1

## Introduction

In this lab, you will gain familiarity with MPI program structure, and point-to-point communication by working with venerable programs such as "Hello, World", calculation of $\pi$, the game of life, and parallel search.

### Goals

- Get familiar with MPI program structure and point-to-point communication by writing a few simple MPI programs.
- Explore MPI performance

### Duration

1.5 hours

### Preparation

In preparation for this lab, read the "General Instructions for the MPI Labs".

## Exercise 1: Run "Hello, World"

Compile and run the "Hello, World" program found in the lecture. Make sure you understand how each processors prints its rank as well as the total number of processors in the communicator ``MPI_COMM_WORLD``.

### Source Code(s)

- Hello, World: Serial C and Fortran ([hello_mpi.c](hello_mpi.c) and [hello_mpi.f90](hello_mpi.f90)) 

## Exercise 2: Bandwidth and latency between nodes

Use ``MPI_Wtime`` to compute latency and bandwidth with the bandwidth and latency codes listed above.

For this exercise you should compare different setups where (a) both MPI ranks are on the same node, e.g.

```
salloc -p shared --nodes=1 --ntasks-per-node=2 -t 0:01:00 --account=<account> --reservation=<name-of-reservation>
srun -n 2 ./mpi_latency.x
```

or on separate nodes, e.g.

```
salloc -p main --nodes=2 --ntasks-per-node=1 -t 0:01:00 --account=<account> --reservation=<name-of-reservation>
srun -n 2 ./mpi_latency.x
```

Compare the different results and reason about the observed values.

### Source Codes

- MPI Latency: C and Fortran ([mpi_latency.c](mpi_latency.c) and [mpi_latency.f90](mpi_latency.f90))
- MPI Bandwidth : C and Fortran ([mpi_bandwidth.c](mpi_bandwidth.c) and [mpi_bandwidth.f90](mpi_bandwidth.f90))
- MPI Bandwidth Non-Blocking: C and Fortran ([mpi_bandwidth-nonblock.c](mpi_bandwidth-nonblock.c) and [mpi_bandwidth-nonblock.f90](mpi_bandwidth-nonblock.f90))

## Exercise 3: Find $\pi$ using P2P communication (master/worker)

The given program calculates $\pi$ using an integral approximation. Take the serial version of the program and modify it to run in parallel.

First familiarize yourself with the way the serial program works. How does it calculate $\pi$?

Hint: look at the program comments. How does the precision of the calculation depend on DARTS and ROUNDS, the number of approximation steps?

Hint: edit DARTS to have various input values from 10 to 10000. What do you think will happen to the precision with which we calculate $\pi$ when we split up the work among the nodes?

Now parallelize the serial program. Use only the following six basic MPI calls: MPI_Init, MPI_Finalize, MPI_Comm_rank, MPI_Comm_size, MPI_Send, MPI_Recv

Hint: As the number of darts and rounds is hard coded then all workers already know it, but each worker should calculate how many are in its share of the DARTS so it does its share of the work. When done, each worker sends its partial sum back to the master, which receives them and calculates the final sum.

### Source Code

- Calculation of $\pi$: Serial C and Fortran ([pi_serial.c](pi_serial.c) and [pi_serial.f90](pi_serial.f90))

## Exercise 4: Use P2P communication for doing "Parallel Search"

In this exercise, you learn about the heart of MPI: point-to-point message-passing routines in both their blocking and non-blocking forms as well as the various modes of communication.

Your task is to parallelize the "Parallel Search" problem. In the parallel search problem, the program should find all occurrences of a certain integer, which will be called the target. It should then write the target value, the indices and the number of occurrences to an output file. In addition, the program should read both the target value and all the array elements from an input file.

Hint: One issue that comes up when parallelizing a serial code is handling I/O. As you can imagine, having multiple processes writing to the same file at the same time can produce useless results. A simple solution is to have each process write to an output file named with its rank. Output to these separate files removes the problem. Here is how to do that in C and Fortran:

The C version is quite straightforward.

```
sprintf(outfilename, "found.data_%d", myrank);
outfile = fopen(outfilename,"w") ;
```

The Fortran version is similar, but working with strings is not something normally done in Fortran.

```
write(rankchar, '(i4.4)') myrank
outfilename="found.data_" // rankchar
open(unit=11, file=outfilename)
```

### Source Code

- Parallel Search: Serial C and Fortran ([parallel_search-serial.c](parallel_search-serial.c) 
  and [parallel_search-serial.f90](parallel_search-serial.f90)),
  input file ([b.data](b.data)), and output file ([reference.found.data](reference.found.data))

## Acknowledgment

The examples in this lab are provided for educational purposes by [National Center for Supercomputing Applications](http://www.ncsa.illinois.edu/), (in particular their [Cyberinfrastructure Tutor](http://www.citutor.org/)), [Lawrence Livermore National Laboratory](https://computing.llnl.gov/) and [Argonne National Laboratory](http://www.mcs.anl.gov/). Much of the LLNL MPI materials comes from the [Cornell Theory Center](http://www.cac.cornell.edu/).  We would like to thank them for allowing us to develop the material for machines at PDC.  You might find other useful educational materials at these sites.
