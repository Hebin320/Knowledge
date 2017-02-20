# LineChart

**项目在GitHub上的地址：**

[https://github.com/Hebin320/LineChart](https://github.com/Hebin320/LineChart)

layout布局代码

```
   <HorizontalScrollView
        android:layout_width="match_parent"
        android:layout_height="300dp"
        android:scrollbars="none">

        <RelativeLayout
            android:id="@+id/rl_linechart"
            android:layout_width="wrap_content"
            android:layout_height="300dp" />
    </HorizontalScrollView>
```

自定义View代码：

```
/**
 * 自定义view，用canvas画出两条曲线、X轴坐标等等
 */

public class LineChartView extends View {

    /**
     * 第一条曲线的数字
     */
    private List<Integer> lineOneScore = new ArrayList<>();
    /**
     * 第二条曲线的数字
     */
    private List<Integer> lineTwoScore = new ArrayList<>();
    /**
     * x轴的文字
     */
    private List<String> xLineData = new ArrayList<>();
    /**
     * Y轴的数值
     */
    private List<Integer> yLineData = new ArrayList<>();
    /**
     * 坐标轴、坐标轴字体、线条的画笔
     */
    private Paint datePaint;
    /**
     * 第一条折线的画笔
     */
    private Paint lineOnePaint;
    /**
     * 第二条折线的画笔
     */
    private Paint lineTwoPaint;
    /**
     * 坐标轴、坐标轴字体、线条的颜色
     */
    private int dateColor = Color.parseColor("#636363");
    /**
     * 第一条折线的画笔
     */
    private int lineOneColor = Color.parseColor("#000000");
    /**
     * 第二条折线的画笔
     */
    private int lineTwoColor = Color.parseColor("#f9a13f");
    /**
     * 第一条折线的中间圆点
     */
    private Bitmap lineOneBitmap;
    /**
     * 第二条折线的中间圆点
     */
    private Bitmap lineTwoBitmap;


    private float tenDP;
    private Path path;


    public LineChartView(Context context, List<Integer> lineOneScore, List<Integer> lineTwoScore, List<String> xLineData, List<Integer> yLineData) {
        super(context);
        init(lineOneScore, lineTwoScore, xLineData, yLineData);
    }

    public void init(List<Integer> lineOneScore, List<Integer> lineTwoScore, List<String> xLineData, List<Integer> yLineData) {
        if (lineOneScore.size() == 0 || lineTwoScore.size() == 0 || xLineData.size() == 0 || yLineData.size() == 0)
            return;
        this.xLineData = xLineData;
        this.yLineData = yLineData;
        this.lineOneScore = lineOneScore;
        this.lineTwoScore = lineTwoScore;

        Resources resources = getResources();
        tenDP = resources.getDimension(R.dimen.magin_10);

        datePaint = new Paint();
        datePaint.setStrokeWidth(tenDP * 0.05f);
        datePaint.setTextSize(tenDP * 1.2f);
        datePaint.setColor(dateColor);

        lineOnePaint = new Paint();
        lineOnePaint.setStrokeWidth(tenDP * 0.1f);
        lineOnePaint.setTextSize(tenDP * 1.2f);
        lineOnePaint.setColor(lineOneColor);

        lineTwoPaint = new Paint();
        lineTwoPaint.setStrokeWidth(tenDP * 0.1f);
        lineTwoPaint.setTextSize(tenDP * 1.2f);
        lineTwoPaint.setColor(lineTwoColor);


        path = new Path();
        lineOneBitmap = BitmapFactory.decodeResource(getResources(),
                R.mipmap.ic_point_gray);
        lineTwoBitmap = BitmapFactory.decodeResource(getResources(),
                R.mipmap.ic_point_yellow);
        setLayoutParams(new ViewGroup.LayoutParams((int) (this.xLineData.size() * 6f * tenDP + 3.5f * tenDP), ViewGroup.LayoutParams.MATCH_PARENT));

    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        canvas.setDrawFilter(new PaintFlagsDrawFilter(0, Paint.ANTI_ALIAS_FLAG | Paint.FILTER_BITMAP_FLAG));
        drawLineOne(canvas);
        drawLineTwo(canvas);
        drawDate(canvas);
        drawLine(canvas);
    }


    /**
     * 绘制时间
     *
     * @param c
     */
    public void drawDate(Canvas c) {

        for (int i = 0; i < xLineData.size(); i++) {
            c.drawText(xLineData.get(i), 6f * i * tenDP + 2.5f * tenDP, getHeight() - tenDP * 0.5f, datePaint);
        }
    }


    /**
     * 绘制线
     *
     * @param c
     */
    public void drawLine(Canvas c) {

        for (int i = 0; i < xLineData.size(); i++) {
            for (int k = 0; k < 7; k++) {
                c.drawLine(0, getHeight() - ((getHeight() - tenDP * 8.5f) * k) / 5 - tenDP * 2.0f, 2f * tenDP * xLineData.size() * 3.5f + 1.5f * tenDP,
                        getHeight() - ((getHeight() - tenDP * 8.5f) * k) / 5 - tenDP * 2.0f, datePaint);
            }
        }
    }

    /**
     * 绘制第一条折线
     */
    public void drawLineOne(Canvas c) {
        float x1, x2, y1, y2;
        for (int i = 0; i < lineOneScore.size() - 1; i++) {
            x1 = 6f * i * tenDP + tenDP * 4;
            y1 = getHeight() - lineOneScore.get(i) * (getHeight() - tenDP * 8.5f) / Collections.max(yLineData) - tenDP * 2.0f;
            x2 = 6f * (i + 1) * tenDP + +tenDP * 4f;
            y2 = getHeight() - lineOneScore.get(i + 1) * (getHeight() - tenDP * 8.5f) / Collections.max(yLineData) - tenDP * 2.0f;
            c.drawLine(x1, y1, x2, y2, lineOnePaint);
            path.lineTo(x1, y1);
            c.drawBitmap(lineOneBitmap,
                    x1 - lineOneBitmap.getWidth() / 2,
                    y1 - lineOneBitmap.getHeight() / 2, null);
            path.lineTo(x2, y2);
            path.lineTo(x2, getHeight());
            path.lineTo(0, getHeight());
            path.close();
            c.drawBitmap(lineOneBitmap,
                    x2 - lineOneBitmap.getWidth() / 2,
                    y2 - lineOneBitmap.getHeight() / 2, null);
        }
    }

    /**
     * 绘制第二条折线
     */
    public void drawLineTwo(Canvas c) {
        float x1, x2, y1, y2;
        for (int i = 0; i < lineTwoScore.size() - 1; i++) {
            x1 = 6f * i * tenDP + tenDP * 4;
            y1 = getHeight() - lineTwoScore.get(i) * (getHeight() - tenDP * 8.5f) / Collections.max(yLineData) - tenDP * 2.0f;
            x2 = 6f * (i + 1) * tenDP + +tenDP * 4f;
            y2 = getHeight() - lineTwoScore.get(i + 1) * (getHeight() - tenDP * 8.5f) / Collections.max(yLineData) - tenDP * 2.0f;
            c.drawLine(x1, y1, x2, y2, lineTwoPaint);
            path.lineTo(x1, y1);
            c.drawBitmap(lineTwoBitmap,
                    x1 - lineTwoBitmap.getWidth() / 2,
                    y1 - lineTwoBitmap.getHeight() / 2, null);
            path.lineTo(x2, y2);
            path.lineTo(x2, getHeight());
            path.lineTo(0, getHeight());
            path.close();
            c.drawBitmap(lineTwoBitmap,
                    x2 - lineTwoBitmap.getWidth() / 2,
                    y2 - lineTwoBitmap.getHeight() / 2, null);
        }
    }


}

```
activity代码，模拟一些数据，将数据给view，然后addview

```
  private void setView() {
        List<String> xLineDate = new ArrayList<>();
        List<Integer> yLineDate = new ArrayList<>();
        List<Integer> lineOneDate = new ArrayList<>();
        List<Integer> lineTwoDate = new ArrayList<>();
        int[] oneInt = {24,35,43,16,24,37,56};
        int[] twoInt = {64,15,23,46,54,47,26};
        int[] yInt = {10,20,30,40,50,60,70};
        String[] xString = {"周一","周二","周三","周四","周五","周六","周日"};
        for (int i = 0; i <oneInt.length ; i++) {
            xLineDate.add(xString[i]);
            yLineDate.add(yInt[i]);
            lineOneDate.add(oneInt[i]);
            lineTwoDate.add(twoInt[i]);
        }
        rlLinechart.addView(new LineChartView(this,lineOneDate,lineTwoDate,xLineDate,yLineDate));
    }
```

![image](https://github.com/Hebin320/ImageSave/blob/master/img/linechart.png)
