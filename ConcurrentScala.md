## Concurrent Algorithms in Scala

# [Peterson Lock in a tree](https://www.seas.upenn.edu/~alur/Spring09/hw1.pdf)

````scala
class TreeLock(numThreads: Int) extends Lock {

  final val DEPTH: Int = (Math.log(numThreads) / Math.log(2)).ceil.toInt

  // assuming numThreads = 5, arrSize would equal 2^4 - 1 = 15
  final val SIZE: Int = math.pow(2.0, DEPTH).toInt

  val lock_arr = new Array[PetersonNode](SIZE)

  for (x <- 0 until SIZE) {
    lock_arr(x) = new PetersonNode
  }

  def encodeToBinary(n: Int): String = {
    var res = n.toBinaryString
    while (this.DEPTH > res.length) {
      res = '0' + res
    }
    res
  }


  override def lock(): Unit = {
    def getInitIndex(binaryEnc:String) = {
      var index = 0
      for (i <- 0 until binaryEnc.length - 1) {
        if (binaryEnc(i).equals('1')) {
          index = index * 2 + 2
        } else {
          index = index * 2 + 1
        }
      }
      index
    }
    val threadID = ThreadID.get
    val lockPath = encodeToBinary(threadID)
    var index: Int = getInitIndex(lockPath)

    for (curLock <- lockPath.reverse) {
      val curLockId = if (curLock.equals('0')) 0 else 1
      this.lock_arr(index).lock(curLockId )
      index = (index - 1) / 2
    }
  }


  override def unlock(): Unit = {
    val threadID = ThreadID.get
    var index = 0
    val lockPath = encodeToBinary(threadID)

    for (curLock <- lockPath) {
      val curLockId = if (curLock.equals('0')) 0 else 1
      this.lock_arr(index).unlock(curLockId)
      if (curLockId == 0) index = index * 2 + 1 else index = index * 2 + 2
    }
  }
  
  ````
  
  
  To solve this question, we first calculate the depth of a tree required for ```n``` threads trying to access locks, which is given by ```log_2(n)```. 
  We then create an array representation of the tree of locks. The size of the array is going to be within the bounds of ```2^Depth```. Note that this array can 
  be used to represent a perfect Binary Tree, in which all the internal nodes have two children and all leaf nodes are at the same level.
  
  After creating an array representation of the tree, we we work on doing a binary encoding of the leaf nodes as shown in the following image.
  
![alt text][logo]

[logo]: https://www.researchgate.net/profile/Zoltan_Kasa/publication/45900964/figure/fig1/AS:306083185872912@1449987326220/Encoding-of-binary-trees-for-n-4.png "Logo Title Text 2"

