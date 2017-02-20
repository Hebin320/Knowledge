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
