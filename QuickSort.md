# Different flavors of Quick Sort (YSC3248 Midterm Report)

##### Alaukik Nath Pant 


In this report, we examine four different ways of implementing Quicksort
in Scala, exploiting some concurrency features of the language.

### 1. Simple Quick Sort

In our implementation of Simple Quick Sort, we use the traditional divide and
conquer approach. The key to our implementation involves using the partition method that
picks the last element of a given sub-array as the pivot and places this element at its correct position in sorted array such that all elements greater than this element are placed 
in higher indices(first sub array) while elements smaller than or equal to this element (second sub array) are placed at lower indices. This process is done in linear time. Then we recursively use the same partition method in the two afformentioned sub-arrays.

**Worst Case:** The worst case time complexity of quicksort is O(n^2) and occurs when the pivot in the partition process is always the greatest or smallest element. In our implementation, this happens when the array is sorted in descending or ascending order.

**Best Case:** The best case complexity of quicksort is O(n* Log(n)) and occurs when the partition always picks the middle element. In its best case scenario, Quick Sort can be the best sorting algorithm.
![Simple Quick Sort](https://www.techiedelight.com/wp-content/uploads/Quicksort.png)

Fig 1.0 - Simple Quick Sort visualised [[1]](https://www.techiedelight.com/quicksort/)

### 2. Parallel QuickSort

We can infer from the divide and conquer approach of quicksort that it can exploit concurrency. In particular, the sub-arrays before and after the pivot can be partitioned concurrently. Each time we apply the partition method on a sub-array, we assign a new thread to do this. The threads are synchronised such that no two threads partition the same sub-array. Additionally, if ```a``` is a sub-array of ```b```, then the ```partition()``` method is invoked for```a``` only after this method has been invoked for ```b```.  In this way, our implementation, part of which can be seen below, partitions the subarrays concurrently in log(P) iterations.


```scala
  def sort_helper[T: Ordering](arr: Array[T], lo: Int, hi: Int): Unit = {
    if (lo < hi) {
      val p_idx = partition(arr, lo, hi)
      val p1 = new Sorter[T](arr, lo, p_idx - 1)             \\ Concurrent operation 1 described above
      val p2 = new Sorter[T](arr, p_idx + 1, hi)             \\ Concurrent operation 2 described above

      p1.start()
      p2.start()

      p1.join()
      p2.join()

    }
  }

    class Sorter[T: Ordering](arr: Array[T], lo: Int, hi: Int) extends Thread {
      override def run() = {
        // Get my thread id
        val id = Thread.currentThread().getId
        sort_helper(arr, lo, hi)
      }
    }
```

### 3. Implementation of a simple thread pool and its applications to Quicksort

The disadvantage of aforementioned Parallel Quicksort algorithm is that new threads are started whenever a new concurrent computation needs to be executed. Hence, it can actually be slower than the Simple QuickSort. For instance, for an Array of size 5000, ```ParallelSort time= 1,877 ms``` and ```SimpleSort time= 104 ms```. 

Hence, we implement a ```thread pool``` that does not start threads whenever a new concurrent computation needs to be executed. Instead, "it has a fixed array (the pool) of constantly running threads that “pick up” tasks from the queue and execute them". [[2]](https://ilyasergey.net/YSC3248/week-07-midterm-project.html) Each of these threads are called a ```Worker``` and each worker thread is waiting for something to be added to the task queue. A worker thread starts executing a task by dequeuing it from the ```taskQueue```.
Our ```taskQueue``` is a simple sequential (non-concurrent) queue and a Worker thread needs to acquire a ```ReentrantLock()``` to dequeue from this queue. This means that if two threads cannot get access to the queue at the same time, one of them will have to wait to get access to the lock. This ensures thread-safety. For the same reasons, the ```workInProgress``` array, an array of flags indicating which threads are currently active, is also a sequential and non-atomic array. 

Additionally, the ```async()``` method takes a task from the user and adds it to the queue so that a worker thread can execute it at some point.
Finally the ```startAndWait()``` method is implemented such that the caller will be synchronised with all concurrent tasks that will be executed by the thread pool. 

We use a thread pool composed of 4 worker threads to implement a ```PooledQuickSort```. We experimented with different sizes for the thread pool for different array sizes and found 4 worker threads to be optimal for pooled sorting.

For example, for an array size of 5000, we found the following results.

| No of Worker Threads | Average time  to sort array of size 5000|
| ------------- | ------------- |
| 3            | 157       |
| 4            | 155       |
| 10           | 168       |
| 50           | 177       |

**Performance:** Although better than the Parallel Quicksort, for an Array of size 5000, ```PooledSort time= 155 ms```, which is worse than SimpleSort's time of ```104 ms```.


### 4. Hybrid QuickSort

We will now experiment with using a combination of single-threaded and parallel sorting to get the best of both worlds. Our implementation is ver similar to Pooled Quicksort, but when we arrive at a stage where we are partitioning arrays recursively, our method for sorting/partitioning sub-arrays changes. In particular, we refer to the following peice of code:

```scala
pool.async(_ => {
          val leng = p_idx - 1 - lo
          if (leng < x) {                    ```note that x is the variable we vary here
            simpleQuickSortSubArray(arr, lo, p_idx -1)
          } else {
            sort_helper(arr, lo, p_idx - 1)
        }
        })

pool.async(_ => {
          val leng = hi - p_idx + 1

          if (leng < x) {                    ```note that x is the variable we vary here
            simpleQuickSortSubArray(arr, p_idx + 1, hi)
          } else {
            sort_helper(arr, p_idx + 1, hi)
        }
        })
```

The attempt here is to perform a Simple QuickSort using 1 worker thread for sub-arrays of sizes smaller than ```x```. We try to find an ```x``` where the cost of a new worker thread "picking" up a parallelizable task is greater than its benefit.


For an array of size 100000, we vary x and run ```HybridQuickSortTests``` five times each and get the following data:

|x|Average Time tp sort (ms)|
| -------------| -------------|
|1000 | 316 |
|1500 | 349 |
|1900 | 280 ms|
|2000 | 226 ms|
|2200 | 287 ms|
|5000 | 314 ms|
|10000 | 322 ms|

From the given data, we infer that the value of variable ```x``` should be approximately 2000. In other words, we use a fresh and available Worker thread from our pool for partitioning sub-arrays recursively up until the point where the length of our sub-arrays are greater than or equal to 2000. For sub-arrays of size less than 2000, we just use a single thread to perform a Simple QuickSort. This suggests that when ```x``` approximately equals 2000, the net benefit of parallelising the two recursive calls in Quicksort to our Worker Threads becomes negative and a simple QuickSort is better.


### Benchmarking

Using this knowledge, we benchmark the sorting algorithms defined above against Scala's native sort method and see that our ```hybrid quicksort``` beats it every time.


|Sorting Method|Array size 100000 |Array size 1000000 |Array size 10000000| Array size 20000000|
| -------------| ------------- | ------------- | ------------- | ------------- |
|ScalaSort | 191 ms| 765 ms | 4,120 ms| 8,538 ms|
|SimpleSort| 167 ms| 922 ms| 6,133 ms| 12,668 ms|
|PooledSort| 387 ms|  1,371 ms|  14,151 ms| 30,347 ms|
|HybridSort| 56 ms| 255 ms|  3,343 ms| 6,491 ms|



