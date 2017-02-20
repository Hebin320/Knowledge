#T-MVP的使用
</br>
用了MVP也有一段时间了，最为深的体会就是，写代码的时候，整个思维很清晰，修改需求也变得较为简单，还有一个体会就是，复用性变强了。一开始看着网上的教程，就在项目中用MVP，写了几个界面，发现增加类的量不是一般的多，类一多就会觉得，一点也不简洁；于是，便有了T-MVP。

实现的效果图：

![这里写图片描述](http://img.blog.csdn.net/20160727185315933)

项目结构，大概是这样的：

![这里写图片描述](http://img.blog.csdn.net/20160727185409022)


1、用泛型实现MVP的大瘦身
--------------

MVP给人的第一感觉就是要写很多类，很多接口；如果一下基础接口用泛型写，则可以免去重复写接口的繁琐。

 - 几个基础接口
 
```
public interface EasyOnListener<T> {

    void getSuccess(T T);

    void getFail(T T);
}
```

```
public interface IEasyBiz<T> {
    void  getData(Context context, T T,EasyOnListener onListener);
}

```

```
public interface IEasyView<T> {

    T getData();

    void getSuccess(int type, T T);

    void getFaild(int type, T T);

    void showLoading();

    void hideLoading();

    void noConnet();

    void isConnect();

}

```

2、T-MVP的项目使用
------------

Demo中用到的数据接口来自聚合数据提供的免费接口；数据结构大概是这样的：

![这里写图片描述](http://img.blog.csdn.net/20160727190507240)

用Android Studio的GsonFormat可以将Json快速生成实体类；

 - 有了接口、实体类，接下来就是请求接口，得到数据，Demo中用到网络请求是Volley，解析Json用的是Gson，代码如下：
 
 

```
public class DataBiz implements IEasyBiz {

    @Override
    public void getData(Context context, Object T, final EasyOnListener onListener) {

        MyJsonObjectRequest json = new MyJsonObjectRequest(BaseMethod.dataUrl, null, new                                    Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                try {
                    if ("200".equals(response.getString("resultcode"))) {
                        Gson gson = new Gson();
                        DataBean dataBean = gson.fromJson(response.toString(), DataBean.class);
                        onListener.getSuccess(dataBean);
                    } else {
                        onListener.getFail(1);
                    }
                } catch (JSONException e) {
                    e.printStackTrace();
                }
            }
        }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                onListener.getFail(2);
            }
        });
        VolleyController.getInstance(context).addToRequestQueue(json);
    }
}
```

 - 请求网络得到的数据交给Presenter，用泛型的好处在于Model层得到数据交给Presenter层的时候，可以用泛型代替所有的实体类，从而不用每一个界面都写接口，Presenter层的代码如下：
 

```
public class DataPresenter {

    private IEasyView iEasyView;
    private DataBiz dataBiz;
    public final static int NOCONNECT = 1;
    public final static int GETFAILED = 2;
    public final static int GETSUCCESS = 1;

    public DataPresenter(IEasyView iEasyView) {
        this.iEasyView = iEasyView;
        this.dataBiz = new DataBiz();
    }

    public void getList(Context context) {

        if (!NetUtils.isNetworkConnected(context)) {
            iEasyView.getFaild(NOCONNECT,null);
            iEasyView.hideLoading();
        } else {
            iEasyView.isConnect();
            iEasyView.showLoading();
            dataBiz.getData(context, iEasyView.getData(), new EasyOnListener() {
                @Override
                public void getSuccess(Object T) {
                    iEasyView.hideLoading();
                    iEasyView.getSuccess(GETSUCCESS, T);
                }
                @Override
                public void getFail(Object T) {
                    iEasyView.hideLoading();
                    int type = (int) T;
                    switch (type){
                        case 1:
                            iEasyView.getFaild(GETFAILED,null);
                            break;
                        case 2:
                            iEasyView.getFaild(NOCONNECT,null);
                            break;
                    }
                }
            });
        }
    }
}
```

 - Presenter层将数据交给View层，让View层做视图加载
 

```
public class SimpleRecyclerView extends AppCompatActivity implements IEasyView {

    @Bind(R.id.rv_simple)
    RecyclerView rvSimple;
    @Bind(R.id.ll_noconnect)
    LinearLayout llNoconnect;
    @Bind(R.id.ll_loading)
    LinearLayout llLoading;
    @Bind(R.id.tv_change)
    TextView tvChange;

    private int count = 0;
    private DataPresenter presenter;
    private SimpleAdapter adapter;
    private List<DataBean.ResultBean.FutureBean> mList = new ArrayList<>();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_simple_recycler_view);
        ButterKnife.bind(this);
        setList(0);
        presenter = new DataPresenter(this);
        presenter.getList(this);
    }

    private void setList(int type) {
        if (type == 0) {
            LinearLayoutManager manager = new LinearLayoutManager(this);
            manager.setOrientation(LinearLayoutManager.VERTICAL);
            rvSimple.setLayoutManager(manager);
        } else if (type == 1) {
            GridLayoutManager gridLayoutManager = new GridLayoutManager(this, 2);
            rvSimple.setLayoutManager(gridLayoutManager);
        }
    }

    @Override
    public Object getData() {
        return null;
    }

    @Override
    public void getSuccess(int type, Object T) {
        switch (type) {
            case GETSUCCESS:
                DataBean dataBean = (DataBean) T;
                List<DataBean.ResultBean.FutureBean> list = dataBean.getResult().getFuture();
                mList.addAll(list);
                if (adapter == null) {
                    adapter = new SimpleAdapter(this, mList);
                    rvSimple.setAdapter(adapter);
                } else {
                    adapter.notifyDataSetChanged();
                }
                break;
        }
    }

    @Override
    public void getFaild(int type, Object T) {
        switch (type) {
            case NOCONNECT:
                noConnet();
                break;
            case GETFAILED:
                ToastUtil.showToast(this, "请求数据失败");
                break;
        }
    }

    @Override
    public void showLoading() {
        llLoading.setVisibility(View.VISIBLE);
    }

    @Override
    public void hideLoading() {
        llLoading.setVisibility(View.GONE);
    }

    @Override
    public void noConnet() {
        llNoconnect.setVisibility(View.VISIBLE);
        ToastUtil.showErro(this);
    }

    @Override
    public void isConnect() {
        llNoconnect.setVisibility(View.GONE);
    }

    @OnClick({R.id.tv_change, R.id.ll_noconnect})
    public void onClick(View view) {
        switch (view.getId()) {
            case R.id.tv_change:
                count++;
                setList(count % 2);
                adapter.notifyDataSetChanged();
                break;
            case R.id.ll_noconnect:
                presenter = new DataPresenter(this);
                presenter.getList(this);
                break;
        }
    }
}
```
## 总结 ##
MVP的好处之一就是复用性很高，像获取用户信息这样一个网络请求，在app中肯定有很多处会用到，如果用这种写法，就可以避免每次用到都做网络请求，当然，这只是好处之一。用泛型，能够更加灵活地处理各种情况，也可以减少相当多的类。
 

