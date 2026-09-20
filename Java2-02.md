# 查找
## 基本查找
从0索引开始挨个往后查找

```java
//需求：定义一个方法利用基本查找，查询某个元素是否存在
        //{42，113，55，123，34，67，85}
public static void main(String[] args) {
        int[] arr = {42, 113, 55, 123, 34, 67, 85};
        int number = 55;
        BasicSearch(arr, number);
    }

    public static boolean BasicSearch(int[] arr, int number) {
        for (int i = 0; i < arr.length ; i++) {
            if (arr[i] == number) {
                return true;
            }
        }
        return false;
    }
```

## 二分查找//折半查找
每次排除一半的查找数据

前提：数组必须是有序的

如果数据是乱的，先排序再用二分查找得到的索引没有实际意义，只能确定当前数字在数组中存在，因为排序之后数字的位置就可能发生变化

```java
public static void main(String[] args) {
        int[] arr = {1, 3, 5, 7, 9, 11, 13, 15, 17, 19};
        int target = 7;
        int index = binarySearch(arr, target);

        //需求：定义一个方法利用二分查找，查询某个元素在数组中的索引
    }

    public static int binarySearch(int[] arr, int target) {
        //定义两个变量，记录查找的范围
        int min = 0;
        int max = arr.length - 1;

        while (true) {
            if (min > max) {
                return -1;
            }

            //计算中间索引
            int mid = (min + max) / 2;

            if (arr[mid] == target) {
                return mid;
            } else if (arr[mid] > target) {
                max = mid - 1;
            } else {
                min = mid + 1;
            }
        }
    }
```

* 二分查找-插值查找

mid = min + (key - arr[min]) * (max - min) / (arr[max] - arr[min])

要求数组里的数据分布要比较均匀

对于表长较大，而关键字分布又比较均匀的查找表来说，插值查找算法的平均性能比折半查找要好的多。反之，数组中如果分布非常不均匀，那么插值查找未必是很合适的选择。

斐波那契查找

斐波那契数：1, 1, 2, 3, 5, 8, 13, 21, 34, 55, ...（从第三个数开始，每个数都是前两个数的和）

基本思想：也是二分查找的一种提升算法，通过运用黄金比例的概念在数列中选择查找点进行查找，提高查找效率。同样地，斐波那契查找也属于一种有序查找算法。

## 分块查找

分块的原则：
1. 前一块中的最大数据，小于后一块中的所有数据（块内无序，块间有序）
2. 块数数量一般等于数字的个数开根号。比如：16个数字一般分为4块左右

核心思路：先确定要查找的元素在那一块，然后在块内挨个查找

实现步骤：
1. 创建数组blockArr存放每一个块对象的信息
2. 先查找blockArr确定要查找的数据属于那一块
3. 在单独遍历这一块数据

```java
public static void main(String[] args) {
    int[] arr = {16,5,9,12,21,18,
                32,23,37,26,45,34,
                50,48,61,52,73,66};

    Block block1 = new Block(21,0,5);
    Block block2 = new Block(45,6,11);
    Block block3 = new Block(73,12,17);

    Block[] blockArr = {block1,block2,block3};
    int find = 30;

    //调用一个方法
    int index = searchBlock(blockArr, find,arr);

    System.out.println(index);
}

private static int searchBlock(Block[] blockArr, int find, int[] arr) {
        //1.确定要查找的数据属于那一块
        int blockIndex = searchBlock(blockArr, find);
        if (blockIndex == -1) {
            return -1;
        }
        //2.在确定的块中遍历查找
        int startIndex = blockArr[blockIndex].getStartindex();
        int endIndex = blockArr[blockIndex].getEndindex();

        //3.遍历查找
        for (int i = startIndex; i <= endIndex; i++) {
            if (arr[i] == find) {
                return i;
            }
        }
        return -1;
    }

    //定义一个方法用来确定
    public static int searchBlock(Block[] blockArr, int find) {
//    Block block1 = new Block(21,0,5); ----0
//    Block block2 = new Block(45,6,11); ----1
//    Block block3 = new Block(73,12,17); ----2

        //从0索引开始遍历blockArr，如果find小于max，那么表示find是在这一块中
        for (int i = 0; i < blockArr.length; i++) {
            if (find <= blockArr[i].getMax()) {
                return i;
            }
        }
        return -1;
    }


@Data
@AllArgsConstructor
@NoArgsConstructor
class Block{
    private int max;
    private int startindex;
    private int endindex;
}
```

