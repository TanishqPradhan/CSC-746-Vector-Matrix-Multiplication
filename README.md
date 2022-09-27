# vmmul instructional test harness

This directory contains a benchmark harness for testing different implementations of
vector-matrix multiply (VMM) for varying problem sizes.

The main code is benchmark.cpp, which sets up the problem, iterates over problem
sizes, sets up the vector and matrix, executes the vmmul call, and tests the
result for accuracy by comparing your result against a reference implementation (CBLAS).

Note that cmake needs to be able to find the CBLAS package. For CSC 746 Fall 2022,
this condition is true on Cori@NERSC and on the class VM. It is also true for some
other platforms, but you are on your own if using a platform other than Cori@NERSC
or the class VM.

# Build instructions - general

After downloading, cd into the main source directly, then:

% mkdir build  
% cd build  
% cmake ../  

Before these commands actually work, you may need to make some adjustments to your environment.
Below is information for Cori@NERSC, and for MacOS systems.

# Build peculiarities on Cori@NERSC

When building on Cori, make sure you are on a KNL node when doing the compilation. The
Cori login nodes are *not* KNL nodes, the Cori login nodes have Intel Xeon E5-2698
processors, not the Intel Xeon Phi 7250 (KNL) processors.  The simplest way to do this is
grab an interactive KNL node:  
salloc --nodes 1 --qos interactive --time 01:00:00 --constraint knl --account m3930

Also on Cori:

- cmake version: you need to use cmake/3.14 or higher. By default, Cori's cmake is cmake/3.10.2. 
Please type "module load cmake" from the command line, and the modules infrastructure will make
available to you cmake/3.14.4.

- Programming environment. You need to use the Cray-Gnu compilers for this assignment. To access
them, please type "module swap PrgEnv-intel PrgEnv-gnu" on the command line.

# Build peculiarities for MacOSX platforms:

On Prof. Bethel's laptop, which is an intel-based Macbook Pro running Big Sur, and
with Xcode installed, cmake (version 3.20.1) can find the BLAS package, but then the build fails with
an error about not being able to find cblas.h.

The workaround is to tell cmake where cblas.h lives by using an environment variable:
export CXXFLAGS="-I /Library/Developer/CommandLineTools/SDKs/MacOSX10.15.sdk/System/Library/Frameworks/Accelerate.framework/Versions/A/Frameworks/vecLib.framework/Versions/A/Headers/"
then clean your build directory (rm -rf * inside build) and run cmake again. 

Note you will need to "locate cblas.h" on your machine and replace the path to cblas.h
in the CXXFLAGS line above with the path on your specific machine.

# Adding your code

For timing:

You will need to modify the benchmark.cpp code to add timing instrumentation, to 
report FLOPs executed, and so forth.


For vector-matrix multiplication:

There are stub routines inside dgemv-basic.cpp and dgemv-openmp.cpp where you can
add your code for doing basic and OpenMP-parallel vector-matrix multiply, respectively.

For the OpenMP parallel code, note that you specify concurrency at runtime using
the OMP_NUM_THREADS environment variable. While it is possible to set the number of
concurrent OpenMP threads at compile time, it is generally considered better practice to
specify the number of OpenMP threads via the OMP_NUM_THREADS environment variable.


# Running the codes

Some sample job scripts are provided as part of the harness. In principle, you should be able to use
them to launch batch jobs that run your code. You will probably need to make some adjustments
to these scripts for your particular testing workflow.

These sample job scripts have some reference values and tips for managing OpenMP-related
environment variables that are relevant to HW3 and Cori@NERSC.


# Results
```
Basic Vector-Matrix Multiplication:
Description:	Basic implementation of matrix-vector multiply.

Working on problem size N=1024 
 Elapsed time is : 0.00449559 
Working on problem size N=2048 
 Elapsed time is : 0.0180013 
Working on problem size N=4096 
 Elapsed time is : 0.0719027 
Working on problem size N=8192 
 Elapsed time is : 0.287621 
Working on problem size N=16384 
 Elapsed time is : 1.15421 

CBLAS Vector-Matrix Multiplication:
Description:	Reference dgemv.

Working on problem size N=1024 
 Elapsed time is : 0.00522704 
Working on problem size N=2048 
 Elapsed time is : 0.00332148 
Working on problem size N=4096 
 Elapsed time is : 0.013366 
Working on problem size N=8192 
 Elapsed time is : 0.0543675 
Working on problem size N=16384 
 Elapsed time is : 0.216577 


OpenMP Vector-Matrix Multiplication
Static Thread Scheduling
T = 1
Description:	OpenMP dgemv.

Working on problem size N=1024 
 Elapsed time is : 0.0084445 
Working on problem size N=2048 
 Elapsed time is : 0.0260397 
Working on problem size N=4096 
 Elapsed time is : 0.0891222 
Working on problem size N=8192 
 Elapsed time is : 0.322143 
Working on problem size N=16384 
 Elapsed time is : 1.22279

T = 2  
Description:	OpenMP dgemv.

Working on problem size N=1024 
 Elapsed time is : 0.00495479 
Working on problem size N=2048 
 Elapsed time is : 0.0135341 
Working on problem size N=4096 
 Elapsed time is : 0.045388 
Working on problem size N=8192 
 Elapsed time is : 0.162727 
Working on problem size N=16384 
 Elapsed time is : 0.614793 

T = 4
Description:	OpenMP dgemv.

Working on problem size N=1024 
 Elapsed time is : 0.00378109 
Working on problem size N=2048 
 Elapsed time is : 0.00681303 
Working on problem size N=4096 
 Elapsed time is : 0.0228169 
Working on problem size N=8192 
 Elapsed time is : 0.0814659 
Working on problem size N=16384 
 Elapsed time is : 0.307819 

T = 8
Description:	OpenMP dgemv.

Working on problem size N=1024 
 Elapsed time is : 0.00371544 
Working on problem size N=2048 
 Elapsed time is : 0.00356864 
Working on problem size N=4096 
 Elapsed time is : 0.0115675 
Working on problem size N=8192 
 Elapsed time is : 0.0409575 
Working on problem size N=16384 
 Elapsed time is : 0.15461 

T = 16
Description:	OpenMP dgemv.

Working on problem size N=1024 
 Elapsed time is : 0.00509413 
Working on problem size N=2048 
 Elapsed time is : 0.00191315 
Working on problem size N=4096 
 Elapsed time is : 0.0058977 
Working on problem size N=8192 
 Elapsed time is : 0.0207311 
Working on problem size N=16384 
 Elapsed time is : 0.0773457 

T = 32
Description:	OpenMP dgemv.

Working on problem size N=1024 
 Elapsed time is : 0.00877347 
Working on problem size N=2048 
 Elapsed time is : 0.00431386 
Working on problem size N=4096 
 Elapsed time is : 0.0031877 
Working on problem size N=8192 
 Elapsed time is : 0.0106654 
Working on problem size N=16384 
 Elapsed time is : 0.0395025 

T = 64
Description:	OpenMP dgemv.

Working on problem size N=1024 
 Elapsed time is : 0.0149637 
Working on problem size N=2048 
 Elapsed time is : 0.00592867 
Working on problem size N=4096 
 Elapsed time is : 0.0118562 
Working on problem size N=8192 
 Elapsed time is : 0.0218847 
Working on problem size N=16384 
 Elapsed time is : 0.0201267 


Dynamic Thread Scheduling:

T = 1
Description:	OpenMP dgemv.

Working on problem size N=1024 
 Elapsed time is : 0.00850337 
Working on problem size N=2048 
 Elapsed time is : 0.0258818 
Working on problem size N=4096 
 Elapsed time is : 0.0890545 
Working on problem size N=8192 
 Elapsed time is : 0.322221 
Working on problem size N=16384 
 Elapsed time is : 1.22339 

T = 2
Description:	OpenMP dgemv.

Working on problem size N=1024 
 Elapsed time is : 0.0049495 
Working on problem size N=2048 
 Elapsed time is : 0.013486 
Working on problem size N=4096 
 Elapsed time is : 0.0453626 
Working on problem size N=8192 
 Elapsed time is : 0.163246 
Working on problem size N=16384 
 Elapsed time is : 0.614773 
srun: Job 63057749 step creation temporarily disabled, retrying (Requested nodes are busy)
srun: Step created for job 63057749

T = 4
Description:	OpenMP dgemv.

Working on problem size N=1024 
 Elapsed time is : 0.00387519 
Working on problem size N=2048 
 Elapsed time is : 0.00678587 
Working on problem size N=4096 
 Elapsed time is : 0.0228293 
Working on problem size N=8192 
 Elapsed time is : 0.0815869 
Working on problem size N=16384 
 Elapsed time is : 0.307404 

T = 8
Description:	OpenMP dgemv.

Working on problem size N=1024 
 Elapsed time is : 0.00355334 
Working on problem size N=2048 
 Elapsed time is : 0.00354701 
Working on problem size N=4096 
 Elapsed time is : 0.0115305 
Working on problem size N=8192 
 Elapsed time is : 0.0409486 
Working on problem size N=16384 
 Elapsed time is : 0.154315 

T = 16
Description:	OpenMP dgemv.

Working on problem size N=1024 
 Elapsed time is : 0.00491259 
Working on problem size N=2048 
 Elapsed time is : 0.00196851 
Working on problem size N=4096 
 Elapsed time is : 0.00597139 
Working on problem size N=8192 
 Elapsed time is : 0.0206897 
Working on problem size N=16384 
 Elapsed time is : 0.077548 

T = 32
Description:	OpenMP dgemv.

Working on problem size N=1024 
 Elapsed time is : 0.00894032 
Working on problem size N=2048 
 Elapsed time is : 0.00422705 
Working on problem size N=4096 
 Elapsed time is : 0.00326015 
Working on problem size N=8192 
 Elapsed time is : 0.0105747 
Working on problem size N=16384 
 Elapsed time is : 0.0395554 

T = 64
Description:	OpenMP dgemv.

Working on problem size N=1024 
 Elapsed time is : 0.0147919 
Working on problem size N=2048 
 Elapsed time is : 0.00585922 
Working on problem size N=4096 
 Elapsed time is : 0.0115717 
Working on problem size N=8192 
 Elapsed time is : 0.0216075 
Working on problem size N=16384 
 Elapsed time is : 0.0201811 
```