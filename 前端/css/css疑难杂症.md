# 、css那些疑难杂症

### margin失效问题

**1. 外边距合并**

​      现象：发生在两个并列的元素之间

​		![img](https://pic3.zhimg.com/v2-39a61730c6f5c9a359c96f3a9d58499a_b.jpg)

**解决：**

* 只设置其中一个元素的margin值即可（推荐）
* 给每一个元素添加父元素，然后触发BFC规则（不推荐）
* 设置display: inline-block



**BFC:**

BFC：Block Formatting Context，块级格式化上下文，BFC决定了元素对其内容定位，以及当前元素与其他元素之间的关系和相互作用。其目的就是形成一个独立的空间，让空间中的子元素不会影响到这个独立空间之外的布局。这样我们在写页面的时候就可以根据自己的需求，选择一些比较合适的解决方案，主要解决方案如下：

1. 子元素或者父元素的**float**不为**none**
2. 子元素或者父元素的**position**不为**relative**或**static**
3. 父元素的**overflow**为**auto**或**scroll**或**hidden**
4. 父元素的**display**的值为**table-cell**或**inline-block**



**2. 外边距塌陷**

​	现象：个嵌套关系的（父子关系）块元素，当父元素有上外边距或者没有上外边距（margin-top），子元素也有上外边距的时候。两个上外边距会合成一个上外边距，以值相对较大的上外边距值为准

![img](https://pic1.zhimg.com/v2-6ac492e5d34a17790ea8662a9c9323cc_b.jpg)

**解决:**

* 给父元素设置外边框（border）或者内边距（padding）(不建议)
* 触发BFC（推荐）

