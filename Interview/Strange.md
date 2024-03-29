1、对于上亿大数据量的很大的数据如何进行排序后获取前最大的100个数：  
外排序。内存一次装不下,要拆分成小的文件一个一个来处理，最终在合并成一个排好序的大文件。哈希分成小文件,小文件可以放进内存里排序了，可以用快速排序，归并排序，堆排序等等.小文件内部排好序之后，就要把这些内部有序的小文件，合并成一个大的文件，可以用二叉堆来做多路合并的操作，每个小文件是一路，合并后的大文件仍然有序。  
首先遍历1000个文件，每个文件里面取第一个数字，组成 (数字, 文件号) 这样的组合加入到堆里（假设是从小到大排序，用小顶堆），遍历完后堆里有1000个 (数字，文件号) 这样的元素然后不断从堆顶拿元素出来，每拿出一个元素，把它的文件号读取出来，然后去对应的文件里，加一个元素进入堆，直到那个文件被读取完。拿出来的元素当然追加到最终结果的文件里。按照上面的操作，直到堆被取空了，此时最终结果文件里的全部数字就是有序的了。  
  
从1亿个整数里找出100个最大的数：  
先对这批海量数据预处理，在O(N)的时间内用Hash表进行拆分映射  
将需要排序的数据切分为多个样本数大致相等的区间，例如：1-100，101-300… 这里要考虑IO次数和硬件资源问题，例如可将小数据文件数设定为1G（要预留内存给执行时的程序使用）  
读取每个小文件的前100个数字，建立最大值堆。（这里采用堆排序将空间复杂度讲得很低，要排序1亿个数，但一次性只需读取100个数字，或者设置其他基数，不需要1次性读完所有数据，降低对内存要求）  
依次读取余下的数，与最大值堆作比较，维持最大值堆。可以每次读取的数量为一个磁盘页面，将每个页面的数据依次进堆比较，这样节省IO时间   
将堆进行排序，即可得到100个有序最大值。  
最后对各个数据区间内的排序结果文件进行处理，最终每个区间得到一个排序结果的文件，将各个区间的排序结果合并。  


2、亿级别数据(同型且有重复)，统计其中出现次数最多的前N个数据：  
如果去重后数据可以放入内存，我们可以为数据建立字典，比如通过map，hashmap，trie，然后直接进行统计即可。当然在更新每条数据的出现次数的时候，我们可以利用一个堆来维护出现次数最多的前N个数据，当然这样导致维护次数增加，不如完全统计后在求前N大效率高。  
如果数据无法放入内存。一方面我们可以考虑上面的字典方法能否被改进以适应这种情形，可以做的改变就是将字典存放到硬盘上，而不是内存。  
更好的方法，就是可以采用分布式计算，基本上就是map-reduce过程，首先可以根据数据值或者把数据hash(md5)后的值，将数据按照范围划分到不同的机子，最好可以让数据划分后可以一次读入内存，这样不同的机子负责处理各种的数值范围，实际上就是map。得到结果后，各个机子只需拿出各自的出现次数最多的前N个数据，然后汇总，选出所有的数据中出现次数最多的前N个数据，这实际上就是reduce过程。  
不能将数据随便均分到不同机子上，而是要根据hash后的值将它们映射到不同的机子上处理，让不同的机器处理一个数值范围。而先映射后排序的方法会消耗大量的IO，效率不会很高。而上面的分布式方法，也可以用于单机版本，也就是将总的数据根据值的范围，划分成多个不同的子文件，然后逐个处理。处理完毕之后再对这些单词的及其出现频率进行一个归并。实际上就可以利用一个外排序的归并过程。
  
3、有最多1000万条不同的整型数据存在于硬盘的文件中（数据不超过最大值），如何在1M内存的情况下对其进行尽可能快的排序。  
(1)一个简单的思路是读1000万条1次，对第i个25万条数据进行排序，并将排好的结果存成外部文件i（这里可以用常见的内部排序，如快排），最后我们生成了40个排好序的外部文件，然后对这40个文件进行归并排序输出成1个文件。  
(2)位图法：更好的思路是位向量排序，我们可以申请一个1千万长度的位向量bit[10000000]，所有位设置为0，顺序读取待排序文件，每读入一个数i，便将bit[i]置为1。当所有数据读入完成，便对 bit做从头到尾的遍历，如果bit[i]=1，则输出i到文件，当遍历完成，文件则已排好序。这个思路和桶排序一样，算法的关键是位向量。对于不支持bit数据结构的语言我们可以自己实现位操作，int是32位的，所以我们需要[1千万/32+1]个int类型存储，大约1.2M。  
  
  
4、N！的第一个非零位在第几位？N/5+1  但是要考虑到25这样有两个5或者更多5的数字  
  
  
  

  
  
  
  
  
  
部分内容来源：
原文链接：https://blog.csdn.net/qq_39521554/article/details/79546854
