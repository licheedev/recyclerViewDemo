##简单实现RecyclerView嵌套RecyclerView
Sina微博，Twitter等移动客户端通常列表中带有若干张图片

看上去效果类似于ListView嵌套GridView

事实上处理确实有很多人用ListView嵌套GridView去实现这种效果

滑动控件之间嵌套极易产生滑动事件冲突,高度计算错误等问题

<font color=red>Google最初并不想让滑动再嵌套滑动控件</font>

<br>
所以最靠谱的方式还是更加数据动态生成view

现在借助搜索引擎能够轻易地实现滑动控件之间嵌套

反观ios中滑动控件能够滑动控件之间可以嵌套。


###Linearlayoutmanager RecyclerView嵌套GridLayoutManager  RecyclerView
布局方式:以一个GridLayoutManager的recyclerview作为Linearlayoutmanager RecyclerView的itemview

####需要注意的是GridLayoutManager要设置setAutoMeasureEnabled(true)成自适应高度***

####onBindViewHolder 中需要判断item中recyclerview是否已经设置适配器,重复设置会导致滑动不流畅***
#####GridAdapter.java

如出现bug，欢迎反馈

