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

下面进入正题



###Linearlayoutmanager RecyclerView嵌套GridLayoutManager  RecyclerView
布局方式:以一个GridLayoutManager的recyclerview作为Linearlayoutmanager RecyclerView的itemview

####下面主要看看这两个适配器怎么写

#####MainRecyclerAdapter.java

```java
public class MainRecyclerAdapter extends RecyclerView.Adapter<MainRecyclerAdapter.ViewHolder> {
    @Override
    public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.item_view_main, parent, false);
        return new ViewHolder(view);
    }

    @Override
    public void onBindViewHolder(ViewHolder holder, int position) {
        holder.mButton.setText("Item " + position);
        if(holder.mRecyclerView.getAdapter()==null) {
            holder.mRecyclerView.setAdapter(new GridAdapter());
        }else {
            holder.mRecyclerView.getAdapter().notifyDataSetChanged();
        }

    }

    @Override
    public int getItemCount() {
        return 10;
    }

    public static class ViewHolder extends RecyclerView.ViewHolder {
        public final AppCompatButton mButton;
        public final RecyclerView mRecyclerView;

        public ViewHolder(View itemView) {
            super(itemView);
            mButton = (AppCompatButton) itemView.findViewById(R.id.button);
            mRecyclerView = (RecyclerView) itemView.findViewById(R.id.recyclerView);
            RecyclerView.LayoutManager manager = new GridLayoutManager(itemView.getContext(), 4);
            manager.setAutoMeasureEnabled(true);
            mRecyclerView.setLayoutManager(manager);
        }
    }
}
  
```
*** 需要注意的是GridLayoutManager要设置setAutoMeasureEnabled(true)成自适应高度***

*** onBindViewHolder 中需要判断item中recyclerview是否已经设置适配器,重复设置会导致滑动不流畅***
#####GridAdapter.java

```java
public class GridAdapter extends RecyclerView.Adapter<GridAdapter.ViewHolder> {
    @Override
    public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.item_view_grid, parent, false);
        return new ViewHolder(view);
    }

    @Override
    public void onBindViewHolder(ViewHolder holder, int position) {
        holder.mTextView.setText("Grid "+position);
      }

    @Override
    public int getItemCount() {
        return 8;
    }

    public static class ViewHolder extends RecyclerView.ViewHolder {
        public final AppCompatTextView mTextView;

         public ViewHolder(View itemView) {
            super(itemView);
             mTextView= (AppCompatTextView) itemView.findViewById(R.id.textView);
         }
    }
}
  
```
如出现bug，欢迎反馈

