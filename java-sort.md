```java
    for (int j=i; j>low && c.compare(dest[j-1], dest[j])>0; j--){
     swap(dest, j, j-1);
    }
```
*java排序*
默认为正序当 j-1(前) j(后) 对应 o1(前) o2(后)

1. 当o1 - o2 > 0 则 说明 前大于后 交换 正序排列 从小到大排列
2. 当o1 - o2 < 0 则 说明 后大于前 不交换 倒序排列 从大到小排列

>总结
    1.o2 > o1 交换 倒序
    2.o2 < o1 交换 正序