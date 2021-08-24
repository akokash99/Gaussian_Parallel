# Gaussian_Parallel

## How to compile

Compile as follows:

gcc gauss_par.c -pthread -o p

./p [file name.dat]  [number of threads]



## Project description:
The sequential program provided performs gaussian elimination. To perform gaussian elimination, the program uses a method called gaussian with partial pivoting. For the purposes of this project, I will be parallelizing the gaussian elimination process. This project will not concern itself with solving the equations. That process itself isn’t costly to begin with. To understand how different parallel strategies affect the performance of the program, I decided to implement two different parallel strategies that use the same synchronization technique which utilizes the use of barriers.

### First parallel strategy

For the first parallel strategy, each thread calls the computeGauss function, and handles a size/threadsNum iterations of the bigger for loop. For example, if we have 5 threads, thread 1 will handle i=1,6,11,17...etc. Specifically, in this version, each thread handles a non-contiguous block of rows when selecting the pivot. And a non-contiguous block of rows and columns when factoring the rest of the matrix. 

### Second parallel strategy

For this parallel strategy, each thread calls the computeGauss function and similar to the first strategy, it handles size/threadsNum iterations of the bigger for loop. But in this strategy, each thread iterates contiguous blocks of iterations. For example, if we have 4 threads and 100 iterations(size=100), then thread 1 would iterate from 1 to 25, thread to would iterate from 26-50, thread 3 51-75 and thread 4 would iterate from 76-100. This means that in selecting the rows and factoring the matrix, each thread- during different timestamps- will handle a contiguous block of rows and columns.

### Synchronization strategy

As for synchronization, I used the barrier. At the barrier, each thread waits until all the others arrive. Barriers were used to take care of the dependencies between selecting the pivot and factoring the rest of the matrix. And also the dependencies between factoring the rest of the matrix, and the selection of the pivot in the next timestamp. To make sure that the number of the threads who finished executing the bigger for loop aren’t waited on, I have created two atomic variables reached and left which keep track of such threads. 

## Results

x-axis: Matrix size

y-axis: Time(s)


![image](https://user-images.githubusercontent.com/77176446/130703496-82a1428b-90f3-47b4-845b-bc31785976ba.png)


![image](https://user-images.githubusercontent.com/77176446/130703549-a877e1ac-74bb-4e8e-8f9a-6e8c5fb8ba7c.png)


![image](https://user-images.githubusercontent.com/77176446/130703637-92945f34-d805-4056-9ce0-8b1d91430be8.png)



## Conclusion

Both parallel versions present significant gain in performance when compared to the sequential version. When comparing both versions together, we can see that the first one that allows more opportunities of exploiting spatial locality across different threads performs significantly better.

## Author of sequential version
Original author of the sequential version is unknown. Code was modified by Kay Shen in January 2010.

For the parallel implementation, the following functions have been modified/added:


main()

barrier()

computeGauss()






