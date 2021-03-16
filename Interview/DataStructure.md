1、map hash_map unordered_map  
**map**是STL的一个关联容器，它提供一对一的hash,第一个为关键字(key)，每个关键字只能在map中出现一次；第二个为值(value)；map以模板泛型方式实现，可以存储任意类型的数据，包括使用者自定义的数据类型。map内部实现了一个红黑树（红黑树是非严格平衡二叉搜索树，而AVL是严格平衡二叉搜索树），红黑树具有自动排序的功能，因此map内部的所有元素都是有序的，红黑树的每一个节点都代表着map的一个元素。因此，对于map进行的查找，删除，添加等一系列的操作都相当于是对红黑树进行的操作。map中的元素是按照二叉搜索树（又名二叉查找树、二叉排序树，特点就是左子树上所有节点的键值都小于根节点的键值，右子树所有节点的键值都大于根节点的键值）存储的，使用中序遍历可将键值按照从小到大遍历出来。有序性是map结构最大的优点，其元素的有序性在很多应用中都会简化很多的操作,内部实现一个红黑书使得map的很多操作在lgn的时间复杂度下就可以实现，因此效率非常的高。缺点： 空间占用率高，因为map内部实现了红黑树，虽然提高了运行效率，但是因为每一个节点都需要额外保存父节点、孩子节点和红/黑性质，使得每一个节点都占用大量的空间。  
Map主要用于资料一对一映射(one-to-one)的情況，map內部的实现自建一颗**红黑树**，这颗树具有**对数据自动排序**的功能。在map内部所有的数据都是有序的。  
  
**hash_map**基于哈希表，空间换时间，数据存储和查找耗时降低，几乎常数时间，内存消耗增加。使用一个下标范围比较大的数组来存储元素。可以设计一个哈希函数，使得每个元素的关键字都与一个函数值（即数组下标，hash值）相对应，于是用这个数组单元来存储这个元素；也可以简单的理解为，按照关键字为每一个元素“分类”，然后将这个元素存储在相应“类”所对应的地方，称为桶。因为内部实现了哈希表，因此其查找速度非常的快。缺点： 哈希表的建立比较耗费时间。适用于查找问题，unordered_map会更加高效一些。红黑树 VS hash表 , 还是unorder_map占用的内存要高，但是unordered_map执行效率要比map高很多  
需要**解决冲突**：1， 开放定址法 2， 再哈希法 3， 链地址法 4、建立公共溢出区  
开放定址法的增量 d 可以有不同的取法，根据增量可以分为线性探测再散列、二次探测再散列(k^2， -k^2)、伪随机再散列
链地址法好处：不会产生堆积，适合无法确定表长的情况，但是会增加空间消耗（指针需要空间）
hash_map首先分配一大片内存，形成许多桶。利用hash函数，对key进行映射到不同区域（桶）进行保存。其插入过程是：  
得到key  
通过hash函数得到hash值  
得到桶号(一般都为hash值对桶数求模)  
存放key和value在桶内  
  
其取值过程是:  
得到key  
通过hash函数得到hash值  
得到桶号(一般都为hash值对桶数求模)  
比较桶的内部元素是否与key相等，若都不相等，则没有找到  
取出相等的记录的value  
  
hash_map中直接地址用hash函数生成，解决冲突，用比较函数解决   
由此可见，要实现哈希表, 和用户相关的是：**hash函数**和**比较函数**。这两个参数刚好是我们在使用hash_map时需要指定的参数  
没有指定hash函数和比较函数的时候，会有一个缺省的函数。但有的key类型不可以使用默认的函数，比如string  
**速度**：hash_map **查找速度**会比map快，而且查找速度基本和数据数据量大小，属于常数级别;而map的查找速度是log(n)级别。**并不一定常数就比log(n)小**，hash还有**hash函数**的耗时，如果考虑效率，特别是在元素达到一定数量级时，考虑考虑hash_map。但若对内存使用特别严格，希望程序尽可能少消耗内存，那么一定要小心，hash_map可能会让你陷入尴尬，特别是当你的hash_map对象特别多时，你就更无法控制了，而且hash_map的**构造速度**较慢。  
权衡三个因素: 查找速度, 数据量, 内存使用  
  
为什么用红黑树而不是AVL来进行map，已经有了AVL为什么还要map呢？  
While in both algorithms the insert/delete operations are O(log n), in the case of Red-Black tree re-balancing rotation is an O(1) operation while with AVL this is a O(log n) operation, making the Red-Black tree more efficient in this aspect of the re-balancing stage and one of the possible reasons that it is more commonly used.
  
  
2、快排  
从初始序列“6  1  2 7  9  3  4  5 10  8”两端开始“探测”。先从右往左找一个小于6的数，再从左往右找一个大于6的数，然后交换他们。这里可以用两个变量i和j，分别指向序列最左边和最右边。我们为这两个变量起个好听的名字“哨兵i”和“哨兵j”。刚开始的时候让哨兵i指向序列的最左边（即i=1），指向数字6。让哨兵j指向序列的最右边（即j=10），指向数字8。  
其实哨兵j的使命就是要找小于基准数的数，而哨兵i的使命就是要找大于基准数的数，直到i和j碰头为止。每次必须是哨兵j先出发。  
当基准数选择最左边的数字时，那么就应该先从右边开始搜索；当基准数选择最右边的数字时，那么就应该先从左边开始搜索。不论是从小到大排序还是从大到小排序！  
最差时间复杂度和冒泡排序是一样的都是O(N2)，它的平均时间复杂度为O(NlogN)。  
```C++
//快速排序（从小到大）
void quickSort(int left, int right, vector<int>& arr)
{
	if(left >= right)
		return;
	int i, j, base, temp;
	i = left, j = right;
	base = arr[left];  //取最左边的数为基准数
	while (i < j)
	{
		while (arr[j] >= base && i < j)
			j--;
		while (arr[i] <= base && i < j)
			i++;
		if(i < j)
		{
			temp = arr[i];
			arr[i] = arr[j];
			arr[j] = temp;
		}
	}
	//基准数归位
	arr[left] = arr[i];
	arr[i] = base;
	quickSort(left, i - 1, arr);//递归左边
	quickSort(i + 1, right, arr);//递归右边
}
```
  
  
  
部分来源：  
1、https://blog.csdn.net/qq_33216029/article/details/96470266  
2、https://blog.csdn.net/qq_28584889/article/details/88136498  
3、
