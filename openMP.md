# OpenMP

## deal with the openMP

- as a user i can access directly through
  - environment variables
- as application i can access the openMP through

  - compiler directives : through pramgas
  - directly with the runtime Library : calling the functions of openMP

**operating system is responsible for execute the commends of openMP**

## information about openMP

- openMP uses the shared memory architecture
- each thread have stack memory used to save t's private data
- no need for data transfer though the data transfer is transparent
  - When it is said that data transfer through OpenMP is transparent to the programmer, it means that the programmer does not need to explicitly write code to manage data movement between different threads. OpenMP provides a set of directives and functions that allow the programmer to specify parallel regions of code, and the system manages the data transfer between the different threads in the background. This transparency is achieved through the use of shared memory. All threads in an OpenMP program share the same memory address space, so data stored in one thread's memory can be accessed by another thread without the need for explicit data transfer. The OpenMP system takes care of ensuring that the shared data is properly synchronized to prevent conflicts and data inconsistencies.In other words, the programmer can write the code as if it were running in a single-threaded environment, and the OpenMP system takes care of distributing the work across multiple threads and managing the necessary data transfers. This can make it much easier to write parallel code, since the programmer does not need to worry about the low-level details of thread management and data synchronization.

## fork-join model

The basic idea of the fork-join model is that a program begins with a single thread of execution (the "master" thread), which then forks into multiple threads to perform parallel work, and finally joins back together at the end to combine the results.

Here is a more detailed description of the fork-join execution model in OpenMP:

Sequential region: The program starts with a single thread of execution (the master thread) that executes a sequential region of code.

Fork: At some point in the code, the master thread encounters an OpenMP parallel region directive. This directive tells the OpenMP system to create a team of threads, where each thread will execute a copy of the code enclosed in the parallel region.

Parallel execution: After the parallel region directive is encountered, the master thread creates a team of threads and distributes the work among them. Each thread executes its own copy of the code enclosed in the parallel region, potentially operating on different parts of the input data.

Join: Once all the threads have completed their work, they synchronize and join back together into a single thread of execution (the master thread) at the end of the parallel region.

Parallelism continues: The master thread can then continue executing sequentially, or encounter another parallel region directive and repeat the fork-join process to create another team of threads.

Here's an example of using OpenMP pragmas in C to parallelize a simple loop that calculates the sum of an array:

## Examples

1. example 1

```c
#include <stdio.h>
#include <omp.h>

int main()
{
int i;

    #pragma omp parallel
    {
        int thread_num = omp_get_thread_num();
        int num_threads = omp_get_num_threads();

        printf("Hello from thread %d of %d\n", thread_num, num_threads);

        // do some parallel work here...

    }

    return 0;

}
/**

output
Hello from thread 0 of 4
Hello from thread 3 of 4
Hello from thread 2 of 4
Hello from thread 1 of 4


the actual output order may vary depending on how the threads are scheduled by the system.

**/
```

In this example, we use the #pragma omp parallel directive to create a parallel region that will be executed by multiple threads. Inside the parallel region, we use the omp_get_thread_num() function to retrieve the thread number for each thread, and the omp_get_num_threads() function to retrieve the total number of threads in the parallel region. We then use printf() function to print out the thread number and total number of threads for each thread.

When we run this program, the printf() statements will be executed by multiple threads in parallel, and each thread will print out its own thread number and the total number of threads in the parallel region.

the number of threads determined by **OMP_NUM_THREADS** environment variable we can assign it when run we run the program by typing in the terminal **OMP_NUM_THREADS=4 ./a.out** or it by default set by th **OS**

2. example 2

```c
#include <stdio.h>
#include <omp.h>

#define N 100000000

int main()
{
    int i, sum = 0;
    int a[N];

    // initialize the array
    for (i = 0; i < N; i++) {
        a[i] = i + 1;
    }

    #pragma omp parallel for reduction(+:sum)
    for (i = 0; i < N; i++) {
        sum += a[i];
    }

    printf("Sum = %d\n", sum);

    return 0;
}
```

**Explanation :**
In this example, we use the #pragma omp parallel for directive to parallelize the loop that calculates the sum of the array. The reduction(+:sum) clause tells OpenMP to perform a reduction on the sum variable, meaning that each thread will have its own private copy of sum, and the final result will be computed by combining these partial sums using addition.

When we run this program, the loop will be executed in parallel by multiple threads, with each thread processing a different subset of the array. The reduction clause ensures that the final result is correctly computed by summing the partial results computed by each thread.

## using ifdef

```c
#include <stdio.h>

#define N 100000000

int main()
{
    int i, sum = 0;
    int a[N];

    // initialize the array
    for (i = 0; i < N; i++) {
        a[i] = i + 1;
    }

    #ifdef _OPENMP
    #pragma omp parallel for reduction(+:sum)
    #endif
    for (i = 0; i < N; i++) {
        sum += a[i];
    }

    printf("Sum = %d\n", sum);

    return 0;
}
```

in order to compile and run this program with OpenMP support, you will need to include the appropriate compiler flags.
For example, to compile with GCC, you can use the -fopenmp flag:

```python
"""
to use GCC compiler :
    gcc -fopenmp example.c -o example
to use intel c++ compiler :
    icc -qopenmp my_program.cpp -o my_program
"""

```

## Data scooping

Here are the different data scoping clauses that can be used in OpenMP:

shared: The shared clause indicates that a variable is shared among all threads in a parallel region. This means that all threads can access and modify the variable. By default, variables declared outside a parallel region are shared.

- private: The private clause indicates that a variable is private to each thread in a parallel region. This means that each thread has its own copy of the variable that is not visible to other threads. The variable is initialized to its original value before the parallel region is executed.

- firstprivate: The firstprivate clause is similar to private, but the variable is initialized with its original value before the parallel region is executed, and its final value is copied back to the original variable after the parallel region is executed.

- lastprivate: The lastprivate clause is similar to firstprivate, but the final value of the variable is copied back to the original variable after the parallel region is executed only for the last iteration of the loop or the last section in a sections directive.

- reduction: The reduction clause specifies a reduction operation on a variable in a parallel region. The variable is private to each thread, and the final result is computed by combining the private values from all threads using the specified reduction operation.

## Variable masking

Variable masking occurs in OpenMP when a variable is declared both inside and outside a parallel region, but with different scoping. This can lead to unexpected behavior, as the variable may have a different value inside the parallel region than it does outside. This is because each thread creates its own copy of the variable, and the value of the copy depends on the scoping of the variable.

Here's an example of variable masking in OpenMP:

```c
#include <stdio.h>
#include <omp.h>

int main()
{
    int x = 0;

    #pragma omp parallel private(x)
    {
        x = omp_get_thread_num();
        printf("Thread %d: x = %d\n", omp_get_thread_num(), x);
    }

    printf("Outside parallel region: x = %d\n", x);

    return 0;
}


/**
Output :
Thread 1: x = 1
Thread 0: x = 0
Outside parallel region: x = 0
***/
```

In this example, we declare the variable x outside the parallel region with a global scope, and then declare it again inside the parallel region with a private scope. The private clause specifies that each thread should have its own copy of x.

Inside the parallel region, each thread sets its copy of x to its thread ID using the omp_get_thread_num() function, and then prints out the value of x for that thread.

However, when we print out the value of x outside the parallel region, we may see unexpected results due to variable masking. The value of x outside the parallel region is still 0, because the x variable that was modified inside the parallel region is different from the x variable declared outside.

In this case, the values of x inside the parallel region are 1 and 0, but the value of x outside the parallel region is still 0, because the modifications made to the private copies of x inside the parallel region did not affect the global copy of x declared outside.

On OpenMP, all data in a parallel region is shared by default. This includes global data, such as global static variables and C++ class variables. However, there are a few exceptions to this rule:

- Variables of parallel loops are private by default, unless otherwise specified using a workshare construct.
- Local (stack) variables within a parallel region are private by default.
- Local data within enclosed function blocks are also private by default, unless they are explicitly declared static.

## Stack size problem

The stack size refers to the amount of memory allocated for storing function call frames and local variables in a program. When running multithreaded programs with OpenMP, it is possible to encounter stack overflow errors if the default stack size is not sufficient for the program's needs. This can occur if the program has deep function call chains or large arrays declared as local variables.

To avoid these errors, it may be necessary to increase the stack size allocated to each thread. This can be done by setting the appropriate environment variable before running the program. The exact method for setting the environment variable will depend on the operating system being used.

```python
"""
setenv OMP_STACKSIZE 100M
"""
```

or

we can use the HEAD instead

## Race condition

is happen when we try to edit shared variable in the parallel block

## default

we can set the default(shared | private | none )
none mean we should explicitly mak the variable shared or private
