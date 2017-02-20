# LineChart

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
