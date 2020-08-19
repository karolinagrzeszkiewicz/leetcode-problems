# Easy Scala Problems Solved

[Two Sum](https://leetcode.com/problems/two-sum/submissions/)

  - Iterative Solution
  
  ```
  object Solution {
      def twoSum(nums: Array[Int], target: Int): Array[Int] = {

          val count_tracker = scala.collection.mutable.HashMap[Int, Int]()

      for (i <- 0 until nums.length) {
        val differrence = target - nums(i)

        if (count_tracker.contains(differrence)) 
          return Array(count_tracker(differrence), i)
       else
          count_tracker.put(nums(i), i)
      }
          throw new Exception("Two such numbers not found"); 
      }
  }
  ```
  - Recursive Solution
  
  ```
    object Solution {
      def twoSum(nums: Array[Int], target: Int): Array[Int] = {

          def helper(map:Map[Int, Int], index:Int):Array[Int] = {
              val current = nums(index)

              map.get(target - current) match {
                  case None => helper(map + (current -> index), index + 1)
                  case Some(index_new) => Array(index_new, index)
              }
          }
          helper(Map(), 0)
      }
  }
  ```

[Remove Duplicates from Sorted Array]() 

```
object Solution {
    def removeDuplicates(nums: Array[Int]): Int = {
        def check(as: Array[Int], p1: Int, p2: Int, acc: Int): Int = {
         if (p2 >= as.length)  acc
         else if (as(p1) == as(p2)) check(as, p1, p2 + 1, acc)
         else {
            as(p1 + 1) = as(p2)
            check(as, p1 + 1, p2 + 1, acc + 1)
        }  
    }
        if (nums.isEmpty) 0
        else check(nums, 0, 0, 0) + 1
    }
}
```
