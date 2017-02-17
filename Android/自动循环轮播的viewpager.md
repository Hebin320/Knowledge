<p>
　　Viewpager是Android官方提供的一个控件，它的作用是可以实现几个视图的滑动切换，切换的子项可以的view，也可以是Fragment。但是官方提供的Viewpager是不允许无限切换的，而且也没有提供自动切换的接口。所以，如果想要实现自动循环切换视图，就只能自己来重写；以下提供一种实现方式。</p>
　　

 **1. 首先，用Viewpager先实现简单的加载广告图**
 

 - 新建一个Activity，并在xml中添加Viewpager：
 

```
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context="com.hebin.viewpager.MainActivity">
    
    <android.support.v4.view.ViewPager
        android:id="@+id/vp_main"
        android:layout_width="match_parent"
        android:layout_height="200dp"
        />
</LinearLayout>
```

 - 为Viewpager添加Adapter，adapter继承的是FragmentPagerAdapter:
 

```
public class VpAdapter extends FragmentPagerAdapter {

    private ArrayList<Fragment> list;

    public VpAdapter(FragmentManager fm, ArrayList<Fragment> list) {
        super(fm);
        this.list = list;
    }

    @Override
    public int getCount() {
        return list.size();
    }

    @Override
    public Fragment getItem(int arg0) {
        return list.get(arg0);
    }

    @Override
    public void destroyItem(ViewGroup container, int position, Object object) {
        super.destroyItem(container, position, object);
    }


    @Override
    public int getItemPosition(Object object) {
        return PagerAdapter.POSITION_NONE;
    }

    @Override
    public long getItemId(int position) {
        return super.getItemId(position);
    }
}
```

 - 新建一个Fragment，并在xml中，添加一个ImageView：
 

```
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context="com.hebin.viewpager.ImgFragment">
  
    <ImageView
        android:id="@+id/iv_main"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:scaleType="fitCenter"
        />
</LinearLayout>

```

 - 在Activity中，创建一个Fragment集合，然后根据这个Fragment集合创建Adapter，并设置给Viewpager：
 

```
public class MainActivity extends FragmentActivity {

    ViewPager vpMain;
    private int[] img = {R.mipmap.ic_vp_01,R.mipmap.ic_vp_02,R.mipmap.ic_vp_03};

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        vpMain = (ViewPager) findViewById(R.id.vp_main);
        ArrayList<Fragment> list = new ArrayList<>();
        for (int i = 0; i <img.length ; i++) {
            ImgFragment imgFragment = ImgFragment.newInstance(img[i]);
            list.add(imgFragment);
        }
        VpAdapter adapter = new VpAdapter(getSupportFragmentManager(),list);
        vpMain.setAdapter(adapter);

    }
}
```

 - Fragment接收到Activity传过来的值，并将这个值（图片路径）设置给Imageview，加载出图片，从而实现了用Viewpager加载广告图的功能。
 实现效果如下： </br>
 ![这里写图片描述](http://img.blog.csdn.net/20170217135504982?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSGViaW4zMjAzMjA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
</br>
</br>
 **2.为Viewpager添加指示圆点**
<br>
 - 在刚刚创建的Activity的xml里面添加多一个LinearLayout：
 

```
 <LinearLayout
        android:id="@+id/ll_point"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignBottom="@+id/vp_main"
        android:layout_marginBottom="15dp"
        android:layout_centerHorizontal="true"
        android:orientation="horizontal"
        />
```

 - 然后新增一个类，命名为VpListener.class，该类主要实现将小圆点实例化为ImageView然后动态添加到LinearLayout，并且实现，Viewpager滑动切换的时候，改变小圆点的颜色。
 

```
public class VpListener implements ViewPager.OnPageChangeListener {

    ImageView[] imageViews;
    ViewPager viewPager;

    public VpListener(ImageView[] imageViews, ViewPager viewPager) {
        this.imageViews = imageViews;
        this.viewPager = viewPager;
    }


    @Override
    public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {

    }

    @Override
    public void onPageSelected(int position) {
        for (int i = 0; i < imageViews.length; i++) {
            if (i==position){
                imageViews[i].setBackgroundResource(R.drawable.color_circle);
            }else {
                imageViews[i].setBackgroundResource(R.drawable.gray_circle);
            }
        }
    }

    @Override
    public void onPageScrollStateChanged(int state) {
    }


    public static ImageView[] getImageViews(Context context, LinearLayout linearLayout, List list) {
        if (list.size() == 1) {
            return null;
        } else {
            linearLayout.removeAllViews();
            ImageView[] imageViews = new ImageView[list.size()];
            for (int i = 0; i < list.size(); i++) {
                ImageView img = new ImageView(context);
                int tb = (int) context.getResources().getDimension(R.dimen.margin_5);
                LinearLayout.LayoutParams layoutParams = new LinearLayout.LayoutParams(tb, tb);
                layoutParams.setMargins(5, 0, 5, 0);
                img.setLayoutParams(layoutParams);
                imageViews[i] = img;
                if (i == 0) {
                    imageViews[i].setBackgroundResource(R.drawable.color_circle);
                } else {
                    imageViews[i].setBackgroundResource(R.drawable.gray_circle);
                }
                linearLayout.addView(imageViews[i]);
            }
            return imageViews;
        }
    }
}
```

 - 其中两个小圆点的代码如下：
 

```
//橙色圆点
<layer-list xmlns:android="http://schemas.android.com/apk/res/android" >

    <item>
        <shape android:shape="oval" >
            <solid android:color="#ff672c" />
        </shape>
    </item>

</layer-list>
//灰色圆点
<layer-list xmlns:android="http://schemas.android.com/apk/res/android" >

    <item>
        <shape android:shape="oval" >
            <solid android:color="#686b6d" />
        </shape>
    </item>

</layer-list>
```

 - 最后在Activity中，添加多一句代码，即可为Viewpager加上随之滑动的小圆点：
 

```
vpMain.addOnPageChangeListener(new VpListener(VpListener.getImageViews(this,llPoint,list),vpMain));
```

 - 最终实现效果如下：</br>
 ![这里写图片描述](http://img.blog.csdn.net/20170217151046866?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSGViaW4zMjAzMjA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 **3.  实现Viewpager的无限切换**
 </br>
 

 - 首先，在Activity中，在给List添加Fragment的时候，应该增加多一步操作，那就是，在List的第一项之前添加多最后一个Fragment，并且，在List的最后一项后面添加多第一个Fragment，修改后如下图：
 </br>
![这里写图片描述](http://img.blog.csdn.net/20170217150658221?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSGViaW4zMjAzMjA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 

 - 然后，在VpListener.class中，也进行修改:
 

```
public class VpListener implements ViewPager.OnPageChangeListener {

    private ImageView[] imageViews;
    private ViewPager viewPager;
    private int mPagePoistion;
    private int mCount;

    public VpListener(ImageView[] imageViews, ViewPager viewPager) {
        this.imageViews = imageViews;
        this.viewPager = viewPager;
        if (imageViews.length == 3) {
            viewPager.addOnPageChangeListener(null);
            mCount = imageViews.length - 2;
        } else if (imageViews.length > 3) {
            mCount = imageViews.length - 2;
        } else {
            mCount = 1;
        }
    }


    @Override
    public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {

    }

    @Override
    public void onPageSelected(int position) {
        if (mCount > 1) {
            if (position < 1) {//若到实际首位时把poistion设值为感官末尾
                mPagePoistion = mCount;
                setPoint(mCount);
            } else if (position > mCount) {//若到实际末位时把poistion设值为感官首尾
                mPagePoistion = 1;
                setPoint(1);
            } else {
                mPagePoistion = position;
                setPoint(position);
            }
        }
    }
    private void setPoint(int states) {
        if (imageViews != null ) {
            for (int i = 1; i < mCount + 1; i++) {
                if (i == states) {
                    imageViews[i].setImageResource(R.drawable.color_circle);
                } else {
                    imageViews[i].setImageResource(R.drawable.gray_circle);
                }
            }
        }
    }

    @Override
    public void onPageScrollStateChanged(int state) {
        if (ViewPager.SCROLL_STATE_IDLE == state) {
            viewPager.setCurrentItem(mPagePoistion, false);//第二个参数必须为false，不然会有个跳转动画
        }
    }


    public static ImageView[] getImageViews(Context context, LinearLayout linearLayout, List list) {
        if (list.size() == 1) {
            return null;
        } else {
            linearLayout.removeAllViews();
            ImageView[] imageViews = new ImageView[list.size()];
            for (int i = 0; i < list.size(); i++) {
                ImageView img = new ImageView(context);
                int tb = (int) context.getResources().getDimension(R.dimen.margin_5);
                LinearLayout.LayoutParams layoutParams = new LinearLayout.LayoutParams(tb, tb);
                layoutParams.setMargins(5, 0, 5, 0);
                img.setLayoutParams(layoutParams);
                imageViews[i] = img;
                if (i == 0) {
                    imageViews[i].setBackgroundResource(R.color.transparent);
                } else if (i == 1) {
                    imageViews[i].setBackgroundResource(R.drawable.color_circle);
                } else if (i == list.size() - 1) {
                    imageViews[i].setBackgroundResource(R.color.transparent);
                } else {
                    imageViews[i].setBackgroundResource(R.drawable.gray_circle);
                }
                if (list.size()==3){
                    for (int j = 0; j < list.size(); j++) {
                        imageViews[i].setBackgroundResource(R.color.transparent);
                    }
                }
                linearLayout.addView(imageViews[i]);
            }
            return imageViews;
        }
    }
}
```

 - 在Activity中添加代码，设置Viewpager的第一个加载页面为列表的第二项，因为列表第一项，是额外增加的最后一个Fragment。
 

```
vpMain.setCurrentItem(1);
```

 - 最后实现的效果图如下：</br>
 ![这里写图片描述](http://img.blog.csdn.net/20170217162151006?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSGViaW4zMjAzMjA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 **4. 给Viewpager添加自动切换**
 

 - 用一个Handler来控制Viewpager的自动切换，在VpListener里面添加如下代码：
 

```
private int PHOTO_CHANGE_TIME = 1000;//定时变量
    private int index = 1;
    private Handler mHandler = new Handler() {

        public void handleMessage(Message msg) {
            if (index > imageViews.length - 2) {
                index = 1;
            }
            viewPager.setCurrentItem(index);
            index++;
            mHandler.sendEmptyMessageDelayed(1, PHOTO_CHANGE_TIME);
        }
    };

    public VpListener(ImageView[] imageViews, ViewPager viewPager,int PHOTO_CHANGE_TIME ) {
        this.imageViews = imageViews;
        this.viewPager = viewPager;
        this.PHOTO_CHANGE_TIME = PHOTO_CHANGE_TIME;
        if (imageViews.length == 3) {
            viewPager.addOnPageChangeListener(null);
            mCount = imageViews.length - 2;
        } else if (imageViews.length > 3) {
            mCount = imageViews.length - 2;
        } else {
            mCount = 1;
        }
        mHandler.sendEmptyMessageAtTime(1, PHOTO_CHANGE_TIME);
    }

```


 - 最后在Activity中，添加多一个切换时间变量:
 

```
vpMain.addOnPageChangeListener(new VpListener(VpListener.getImageViews(this,llPoint,list),vpMain,800));
```

 - 最终实现效果如下图：</br>
 ![这里写图片描述](http://img.blog.csdn.net/20170217165317365?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSGViaW4zMjAzMjA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
