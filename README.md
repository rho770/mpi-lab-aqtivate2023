# AQTIVATE Training 2023: General Instructions for the MPI Labs

## Where to run

The exercises will be run on PDC's supercomputer [Dardel](https://www.pdc.kth.se/hpc-services/computing-systems/dardel-1.1043529):

```
dardel.pdc.kth.se
```

## More about the environment on Dardel

Software, which is not available by default, needs to be loaded as a [module](https://www.pdc.kth.se/support/documents/basics/quickstartdardel.html#the-lmod-module-system) at login. Use ``ml avail`` to get a list of available modules. Many software modules become available after loading the latest PDC module using ``ml PDC``.

To setup the development environment see: [Compilers and Libraries on Dardel](https://www.pdc.kth.se/support/documents/software_development/development_dardel.html).

The home directories on Dardel are provided through a Lustre service. See the page [Data Management](https://www.pdc.kth.se/support/documents/data_management/data_management.html) for more information.

To use the Dardel compute nodes you have to use the resource manager as described here: [How to Run jobs](https://www.pdc.kth.se/support/documents/run_jobs/job_scheduling.html).

## Compiling MPI programs on Dardel

The following shell commands show a simple example on how to compile an MPI program using the GNU compilers:

```
ml PDC
cc my_prog.c
CC my_prog.cpp
ftn my_prog.f90
```

## Running MPI programs

First it is necessary to book a node for interactive use:

```
salloc -p shared --nodes=1 --ntasks-per-node=32 -t 0:05:00 --account=edu23.aqti --reservation=<name-of-reservation>
```

Then use the ``srun`` command is used to launch an MPI application:

```
srun -n 32 ./example.x
```

In this example we will start 32 MPI tasks (there are 128 cores per node on all Dardel compute nodes).

## MPI Exercises

- MPI Lab 1: [Program Structure and Point-to-Point Communication in MPI](lab1/README.md)
- MPI Lab 2: [MPI I/O and MPI performance](lab2/README.md)
