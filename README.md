# Parallel Quicksort
A hybrid quicksort using rayon in rust

Functional and Parallel Programming 2020-T1
By: 6280632 - Gorramuth P.
Submitted to: Ajarn Rachata Ausavarungnirun 

This project attempts to optimize the parallelization of the quicksort algorithm. In this case, during the partitioning
phase of the algorithm, the process can be parallelized through the use of spawning threads to parallelly process each array
slices recursively. Conventionally, quicksort algorithms tend to employ randomized pivot, but after initial testings, the
cost of randomization almost doubles the execution time at all ranges of values with worse outcome as input becomes larger.
In light of the constraint, I have elected to use a fixed middle pivot for partitioning for the parallel segment and
the rightmost pivot for the normal segment to add elements of randomness. Randomness may also cause the straggler problem where
the element in each array are unequal, finishing at uneven time. The fixed scheme reduces this behavior.

The utilization of parallelization have led to a significant improvement of array elements over the 8000 range, however this comes with
a trade-off of lower efficient in smaller arrays due to the overhead incurred by spawning and joining threads. As such, the insertion sort
and the non-parallel version of the quicksort is integrated into the parallel quicksort forming an optimized hybrid quicksort. Testing has
revealed that the insertion sort is most effective up until the 50 element ranges, where the normal quicksort will take over until the 500th element,
and finally, the parallel sort will deal with the larger element onwards. How this works is that, given a very large array, the parallel portion of the sort
will work in parallel to simultaneously process the slices until they are sufficiently reduced enough to be managed by the normal sorts which require less overhead.

The current benchmark are as follows:

    --- Enhanced parallel sort
    50 numbers ~ 13.60us
    100 numbers ~ 30.70us
    6000 numbers ~ 2.33ms
    10000 numbers ~ 2.92ms
    500000 numbers ~ 159.93ms
    3 million ~ 856.16ms
    50 million numbers ~18s
    100 million numbers ~ 37.28s
    300 million numbers ~130.19s
    
    --- Normal Quicksort
    50 numbers ~ 12.80us
    100 numbers ~ 28.80us
    6000 numbers ~ 5.24ms
    10000 numbers ~ 6.72ms
    500000 numbers ~ 565.54ms
    3 million ~ 3.34s
    50 million numbers ~115s
    100 million numbers ~ 152.25s
    300 miilion numbers ~ 534.75s
    
    --- Rust default sort
    50 numbers ~ 21.20us
    100 numbers ~ 31.00us
    6000 numbers ~ 2.46ms
    10000 numbers ~ 3.92ms
    500000 numbers ~ 299.56ms
    3 million ~ 1.78s
    50 million numbers ~40s
    100 million numbers ~ 79.14s
    300 million numbers ~ 312.92s
   
As you can see, as the elements in the array becomes increasingly larger, the parallel quicksort becomes increasingly more efficient compared to its counterparts. The optimization has made it a close competitor for both sorts at all strata, with the increasing effectiness on scale.
