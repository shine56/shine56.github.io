---
title: RecyclerView的使用
date: 2019-02-08 21:46:21
edited:
tags: Android入门
categories: Android
---
## 准备工作

### 添加依赖
> implementation "androidx.recyclerview:recyclerview:1.1.0"

### 定义适配器
这个控件使用方法和ListView差不多，都需要先自定义一个布局和一个适配类型，但是适配器的构造不一样，下面是适配器的代码，解析在注释。

<!--more-->

```java
//适配器继承与RecyclerView.Adapter，泛型类为适配器的一个内部类
public class photo2Adapter extends RecyclerView.Adapter<photo2Adapter.ViewHolder> {
    private List<photo1> mphoto2List;

    static class ViewHolder extends RecyclerView.ViewHolder{  //内部类获取控件
        TextView textView;
        ImageView imageView;
        public ViewHolder(View view){
            super(view);
            textView = (TextView) view.findViewById(R.id.name);
            imageView = (ImageView) view.findViewById((R.id.photo1_Image));
        }
    }

    public photo2Adapter(List<photo1> photo2List){  //构造方法
        mphoto2List = photo2List;
    }

    //创造ViewHolder实例
    public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType){
        View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.photo1,
                parent,false);      //加载布局
        ViewHolder holder = new ViewHolder(view);
        return holder;
    }

    //给数据赋值
    public void onBindViewHolder(ViewHolder holder, int position){
        photo1 photo2 = mphoto2List.get(position);
        holder.imageView.setImageResource(photo2.getID());
        holder.textView.setText(photo2.getname());
    }

    //返回子项长度
    public int getItemCount(){
        return mphoto2List.size();
    }
}
```
## 使用

### 基本使用
```java
RecyclerView recyclerView = (RecyclerView) findViewById(R.id.recycler_view);  //获取控件对象
LinearLayoutManager layoutManager = new LinearLayoutManager(this);            //指定控件布局方式，将其设置到控件对象中
recyclerView.setLayoutManager(layoutManager);
photo2Adapter adapter = new photo2Adapter(photo1List);
recyclerView.setAdapter(adapter);
```

### 水平滚动
如何改成水平滚动呢，只需在传送数据那块代码中添加一句代码即可
```java 
LinearLayoutManager layoutManager = new LinearLayoutManager(this);
layoutManager.setOrientation(LinearLayoutManager.HORIZONTAL);//******添加水平滚动代码
```

### 点击事件
在适配器中添加代码，看注释：
```java
View photoView;//*******添加代码定义最外层布局实例
TextView textView;
ImageView imageView;
public ViewHolder(View view){
	super(view);
	photoView = view;//*******添加代码保存最外层实例
	textView = (TextView) view.findViewById(R.id.name);
	imageView = (ImageView) view.findViewById((R.id.photo1_Image));
}
....
 final ViewHolder holder = new ViewHolder(view);
//*************************************为最外层注册事件
holder.photoView.setOnClickListener(new View.OnClickListener() {
	@Override
	public void onClick(View v) {
		int position = holder.getAdapterPosition();
		photo1 photo2 = mphoto2List.get(position);
		Toast.makeText(v.getContext(),"你点击的是："+
				photo2.getname(),Toast.LENGTH_LONG).show();
	}
});
//*******************************为图片注册事件
holder.imageView.setOnClickListener(new View.OnClickListener() {
	@Override
	public void onClick(View v) {
		int position = holder.getAdapterPosition();
		photo1 photo2 = mphoto2List.get(position);
		Toast.makeText(v.getContext(),"你点击的是："+
				photo2.getname(),Toast.LENGTH_LONG).show();
	}
});
//****************************************************************
return holder;
```
## 进阶

### 适配器外的点击事件
在适配器里边的点击事件，直接做一个监听就行了，在外边设置监听，就要写多几行代码（长按事件类同）

* 先在适配器里边加入代码：

```java
private OnitemClick onitemClick;   //定义点击事件接口

   //定义设置点击事件监听的方法
   public void setOnitemClickLintener (OnitemClick onitemClick) {
       this.onitemClick = onitemClick;
   }
   //定义一个点击事件的接口
   public interface OnitemClick {
       void onItemClick(int position);
   }
 
 public void onBindViewHolder(final ViewHolder holder, final int position){
 	final note_list note_list= mList.get(position);
holder.textView.setOnClickListener(new View.OnClickListener() {
            @Override
            //实现点击事件接口
            public void onClick(View v) {
                onitemClick.onItemClick(position);
            }
        });
 }

```
* 在外边加入代码：

```java
adapter.setOnitemClickLintener(new MyAdapter.OnitemClick() {
    @Override
    public void onItemClick(int position) {
       //这里写逻辑
    }
});
```

### 设置item的间距
* 先新建一个java文件

```java
public class SpacesItemDecoration extends RecyclerView.ItemDecoration {
    private int space;

    public SpacesItemDecoration(int space) {
        this.space = space;
    }

    @Override
    public void getItemOffsets(Rect outRect, View view,
                               RecyclerView parent, RecyclerView.State state) {
        //设置item之间的间距（上下左右）
        outRect.left = space;
        outRect.right = space;
        outRect.bottom = space;
        //设置item与parent的间距
        if (parent.getChildLayoutPosition(view) == 0)
            outRect.top = space;
    }
}
```
* 在Activity添加代码

```java
int space = 50; //间距
recyclerView.addItemDecoration(new SpacesItemDecoration(space));
```

### item的添加与删除

#### 添加
在Adapter添加代码

```java
//  添加数据
public void addData(int position, note_list note_list) {
//      在list中指定位置添加对象：note_list
	mList.add(position,note_list);
	//添加动画
	notifyItemInserted(position);
}
```
在外面的Activity调用
```java
//指定位置为0，添加对象note_list
adapter.addData(0,note_list);
```
#### 删除
在Adapter添加代码
```java
//删除子项
public void removeData(int position) {
	mList.remove(position);
	//删除动画
	notifyItemRemoved(position);
	//整体刷新
	notifyDataSetChanged();
}
```
在外面的Activity调用
```java
adapter.removeData(position);
```


