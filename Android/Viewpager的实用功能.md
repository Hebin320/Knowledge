#Viewpager的实用功能
<span style="font-family:‘Times New Roman‘;">&nbsp;&nbsp;&nbsp;&nbsp; Viewpager经常被用在引导图、广告图之中，是一个能够滑动切换视图的一个控件。Viewpager会被用在各种各样的场景之中，随之而来的可能就是给Viewpager带来各种各样的Bug。以下列举几个常见的Bug以及解决方案。</span>

**<h2>1、在ScrollView中使用Viewpager，Viewpager不显示</h2>**

 - 这个问题是在实际开发中，经常会遇到的，如果Viewpager不设置固定高度，在Scrollview中使用的时候，就会出现Viewpager不显示的问题。这种问题的解决方法有三种，
  - 第一种
  <br>
  **设置Viewpager的高度，让Viewpager为固定高度；**
  <br>
  - 第二种
  <br>
  **设置ScrollView的android:fillViewPort="true"**
  <br>
  - 第三种
  <br>
  **重写一个类，继承于Viewpager，然后重新Viewpager的onMeasure，具体如下：**
  <br>
```
public class MyViewPager extends ViewPager {

    public MyViewPager (Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    public MyViewPager (Context context) {
        super(context);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        int height = 0;
        // 下面遍历所有child的高度
        for (int i = 0; i < getChildCount(); i++) {
            View child = getChildAt(i);
            child.measure(widthMeasureSpec,
                    MeasureSpec.makeMeasureSpec(0, MeasureSpec.UNSPECIFIED));
            int h = child.getMeasuredHeight();
            // 采用最大的view的高度
            if (h > height) {
                height = h;
            }
        }
        heightMeasureSpec = MeasureSpec.makeMeasureSpec(height,
                MeasureSpec.EXACTLY);
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
    }

}
```
**<h2>2、在ScrollView中，Viewpager滑动卡顿</h2>**

 - 解决了Viewpager在ScrollView中不显示的问题，可能又会出现Viewpager滑动卡顿的问题；这个问题的解决方法是通过Viewpager的onTouchevent以及OnPageChangeListener来解决的，具体代码如下：
 

```
viewpager.setOnTouchListener(new View.OnTouchListener() {  
  
        @Override  
        public boolean onTouch(View v, MotionEvent event) {  
            v.getParent().requestDisallowInterceptTouchEvent(true);  
            return false;  
        }  
    });  
  
    viewpager.addOnPageChangeListener(new OnPageChangeListener() {  
  
        @Override  
        public void onPageSelected(int arg0) {  
        }  
  
        @Override  
        public void onPageScrolled(int arg0, float arg1, int arg2) {  
          mPager.getParent().requestDisallowInterceptTouchEvent(true);  
        }   
        @Override   
        public void onPageScrollStateChanged(int arg0) {  
        }   
    });  
```

 - 因为屏蔽掉了Viewpager的 onTouchevent，所以在执行Viewpager的点击事件的时候，应该屏蔽掉onTouchevent里面的方法；
 

```
viewpager.getParent().requestDisallowInterceptTouchEvent(true);  
```
**<h2>3、SwipeRefreshLayout与Viewpager的滑动冲突</h2>**

 - SwipeRefreshLayout是Google提供的一个下拉刷新的控件，简单易用且样式也比较好看，所以在很多APP中都会看到，但是当SwipeRefreshLayout与Viewpager一起使用的时候，就会出现Viewpager滑动卡顿的问题，这个问题的解决方法，可以通过以下方式解决：
 

```
 viewpager.setOnTouchListener(new View.OnTouchListener() {
            @Override
            public boolean onTouch(View v, MotionEvent event) {
                switch (event.getAction()) {
                    case MotionEvent.ACTION_MOVE:
                        swipeRefreshLayout.setEnabled(false);
                        break;
                    case MotionEvent.ACTION_UP:
                    case MotionEvent.ACTION_CANCEL:
                        swipeRefreshLayout.setEnabled(true);
                        break;
                }
                return false;
            }
        });
```
**<h2>4、设置Viewpager不能滑动</h2>**

 - 在很多APP中，首页都会用左右切换几个界面的方式，像QQ、微信、淘宝那样；这种功能可以用Viewpager切换Fragment的方式实现，Viewpager的能够左右滑动的，但是不是所有的APP都希望他们的首页是可以左右切换的，特别是在首页的子页面里面包含了能够左右切换的功能的时候，这时候就需要屏蔽掉Viewpager的左右滑动功能；这个问题可以通过重写Viewpager进行解决，具体实现方式如下：
 

```
public class MyViewpager extends Viewpager{

    private boolean isPagingEnabled = true;

    public MyViewpager(Context context) {
        super(context);
    }

    public MyViewpager(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        return this.isPagingEnabled && super.onTouchEvent(event);
    }

    @Override
    public boolean onInterceptTouchEvent(MotionEvent event) {
        return this.isPagingEnabled && super.onInterceptTouchEvent(event);
    }

    public void setPagingEnabled(boolean b) {
        this.isPagingEnabled = b;
    }
}

```

 - 然后在应用Viewpager之后，调用重写Viewpager中的方法，即可实现屏蔽Viewpager的滑动功能；
 

```
viewpager.setPagingEnabled(false);
```
