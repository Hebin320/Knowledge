#Json的手动解析以及工具解析
<p>
　　Android在网络通信的过程中，经常会遇到数据传输的问题，而较为常见的数据传输的方式则是通过Json进行传输的。Json的解析方式有很多种，可以手动解析Json，也可以利用工具解析Json，常用的工具有Gson、FastJson、Jackson等等，本文主要是应用Gson对Json进行解析，以及手动解析。</p>
 <font color="#1E90FF" size = 6>1. 运用Gson解析Json数组</font>
<p>
　　Gson 是 Google 提供的用来在 Java 对象和 JSON 数据之间进行映射的 Java 类库。可以将一个 JSON 字符串转成一个 Java 对象，或者反过来将一个Java类库转换成Json数据。</p>　

 - <h4>**首先，导入Gson包，在Android Studio的gradle里面添加：**<h4>
 
```
dependencies {
    compile 'com.google.code.gson:gson:2.6.2'
}
```

 - <h4>**服务器返回值如下：**<h4>
 

```
{
    "status": true, 
    "info": "获取成功", 
    "results": {
        "version": "1", 
    }
 }
```
 - <h4>**然后，用GsonFormat生成Bean文件:**<h4>
 

```
public class WelcomePic {

    private boolean status;
    private String info;
    private ResultsBean results;

    public boolean isStatus() {
        return status;
    }

    public void setStatus(boolean status) {
        this.status = status;
    }

    public String getInfo() {
        return info;
    }

    public void setInfo(String info) {
        this.info = info;
    }

    public ResultsBean getResults() {
        return results;
    }

    public void setResults(ResultsBean results) {
        this.results = results;
    }

    public static class ResultsBean {
        private int version;
        
        public int getVersion() {
            return version;
        }

        public void setVersion(int version) {
            this.version = version;
        }
    }
}
```

 - <h4>**应用Gson，将Json数组转成Java类**<h4>
 

```
     Gson gson = new Gson();
     WelcomePic welcomePic = gson.fromJson(response,WelcomePic.class);
```
 <font color="#1E90FF" size = 6>2. 运用Gson将Java类转成Json数组</font>
 

  - <h4>**Java的实体类还是应用上面的类，然后将值赋给实体类，然后用Gson转为Json**<h4>
  

```
        String info= "我是测试内容";
        String version = "1";
        WelcomePic welcomePic = new WelcomePic();
        welcomePic.setStatus(true);
        welcomePic.setInfo(info);
        welcomePic.getResults().setVersion(version);
        Gson gson = new Gson();
        String results = gson.toJson(welcomePic);
```
  <font color="#1E90FF" size = 6>3. 手动解析Json</font> 

<font>
　　很多人觉得现在已经有Gson这些很便捷的工具了，为什么还要去学手动解析Json呢？有句话，很多企业的HR或者技术主管 都特别注重，那就是：知其然，更要知其所以然。在这个快速开发的时代，培训机构给社会送来一批又一批的初级工程师，很多为了适应时代的步伐，都采取了快速开发的方式，使用工具，快速完成公司的任务。这些人往往没有坚实的基础，在一些正规公司，特别是类似BAT之类的大公司的面试中，往往第一轮就会被筛选掉，基础扎实是很多大公司的第一个标准。所以我个人觉得，前期为了工作，为了完成任务，可以采用快速开发，采用工具，但是，当你有自己的时间，就要多学习一些基础，特别是你使用的工具的原理，扎实自己的基础。所以我认为手动解析Json还是很有必要的。</font></br>
　　<font>
　　在手动解析Json之前，先了解一些基础的概念性的东西。首先，Android提供两个类来解析Json，一个是JSONObject，另一个是JSONArray。简单来说，JSONObject是解析对象的，JSONArray是用来解析数组的。在Json数据中，`{ "status": true, }`，被大括号包围的，可以理解为对象，需要用到JSONObject；`{results:[ {"version":"1"} ,{"version":"2"} ]}` 而被中括号包围的，可以理解为数组，需要用到JSONArray；下面以几个例子来熟悉。</font>　

 - <h4>**1、{“num”:96,”name”:”小狗”}； **<h4>
 解析代码：
```
     JSONObject jsonObject = new JSONObject(stringJson);
     int num = jsonObject.getInt("num");
     String name = jsonObject.getString("name");
```
 - <h4>**2、{“list”:[{name”:”mike”},{”name”:”lucy”}]}；  **<h4>
 这种json格式的解析，先遍历出每个字段再放到数组里面：
```
    JSONObject jsonObject = new JSONObject(stringJson);
    JSONArray jsonArray = jsonObject.getJSONArray("list");
   for (int i = 0; i < jsonArray.length(); i++) {
      JSONObject item = jsonArray.getJSONObject(i);
      String name= item.getString("name");
      }
```

　　
