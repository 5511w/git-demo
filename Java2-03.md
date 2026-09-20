## 排序
### 冒泡排序

核心思想：
1. 相邻的元素两两比较，大的放右边，小的放左边
2. 第一轮比较完毕后，最大值就已经确定，第二轮可以少循环一次，后面以此类推
3. 如果数组中n个数据，总共需要比较n-1轮

```java
 //1.定义数组
        int[] arr = {2,3,4,5,1};

        //2.利用冒泡排序将数组中的数据变成1 2 3 4 5
        for (int i = 0; i < arr.length-1; i++) {
            for (int j = 0; j < arr.length-1-i; j++) {
                if (arr[j] > arr[j+1]){
                    int temp = arr[j];
                    arr[j] = arr[j+1];
                    arr[j+1] = temp;
                }
            }
        }
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
```

## 选择排序
1. 从0索引开始，跟后面的元素一一比较
2. 小的放前面，大的放后面
3. 第一次循环结束后，最小的数据已经确定了
4. 第二次循环从1索引开始，跟后面的元素一一比较

```java
int[] arr = {2,3,4,5,1};
        for (int i = 0; i < arr.length-1; i++) {
            for (int j = i+1; j < arr.length; j++) {
                if (arr[i] > arr[j]){
                    int temp = arr[i];
                    arr[i] = arr[j];
                    arr[j] = temp;
                }
            }
        }
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
```

## 插入排序

将0索引的元素到n索引的元素看做是有序的，把n+1索引的元素到最后一个当成是无序的

遍历无序的数据，将遍历到的元素插入有序序列中适当的位置，如遇到相同数据，插在后面。

n的范围：0~最大索引

```java
int[] array = {5, 3, 8, 4, 2, 7, 1, 10, 6};

        //1.找到无序的哪一组数组是从哪个索引开始的
        int startIndex = -1;
        for (int i = 0; i < array.length; i++) {
            if (array[i] > array[i + 1]){
                startIndex = i + 1;
                break;
            }
        }

        //2.遍历从startIndex开始到最后一个元素，依次得到无序的那一组数据中的每一个元素
        for (int i = startIndex; i < array.length; i++) {
            //问题：如何把遍历到的数据，插入到前面有序的这一组

            //记录当前要插入数据的索引
            int index = i;
            while (index > 0 && array[index] < array[index - 1]){
                //交换
                int temp = array[index];
                array[index] = array[index - 1];
                array[index - 1] = temp;
                index--;
            }
        }
        for (int i : array) {
            System.out.print(i + " ");
        }
```

## 递归

递归一定要有出口，否则会出现内存溢出

作用：把一个复杂的问题层层转换为一个与原问题相似的规模较小的问题来求解

递归策略只需少量的程序就可描述出解题的过程所需要的多次重复计算

两个核心：

找出口：什么时候不再调用方法

找规律：如何将大问题转换为小问题

```java
public static void main(String[] args) {
    //需求：求1~100的和
    //大问题转化为小问题
    //1~100等于100加1~99
    //1~99等于99加1~98
    //...

    getsum(100);
}
public static int getsum(int num){
        if (num == 1){
            return 1;
        }
        //num不是1
        return num + getsum(num - 1);
    }
```

##  快速排序
第一轮：以0索引数字为基准数，确定基准数在数组中正确的位置，

比基准数小的全部在左边，比基准数大的全部在右边

以此类推，递归实现

```java
public static void main(String[] args) {
    int[] arr = {5, 2, 8, 6, 1, 9, 3, 7, 4};

    quickSort(arr, 0, arr.length - 1);
    for (int i : arr) {
        System.out.print(i);
    }
}
//参数一：我们要排序的数组
//参数二：起始索引
//参数三：结束索引

public static void quickSort(int[] arr, int i, int j) {
        //定义两个变量记录要查找的范围
        int start = i;
        int end = j;

        //出口
        if (start >= end) {
            return;
        }

        //定义一个基准数
        int base = arr[i];
        //利用循环找到要交换的数字
        while (start != end) {
            //利用end从后向前找比基准数小的数字，找到后停下
            while (true) {
                if (start <= end || arr[end] >= base){
                    break;
                }
            }
            end--;
            //利用start从前向后找比基准数大的数字，找到后停下
            while (true) {
                if (start <= end && arr[start] <= base) {
                    break;
                }
            }
            start++;
            //如果找到了就交换这两个数字
                int temp = arr[start];
                arr[start] = arr[end];
                arr[end] = temp;

        }
        //当start等于end时，说明找到了基准数正确的位置
        //基准数归位
        //就是拿着这个范围中的第一个数字，跟start指向的元素进行交换
        int temp = arr[i];
        arr[i] = arr[start];
        arr[start] = temp;

        //确定基准数左边和右边的范围，递归实现快速排序
        quickSort(arr, i, start - 1);
        quickSort(arr, start + 1, j);
    }
```