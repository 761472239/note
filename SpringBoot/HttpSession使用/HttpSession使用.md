# HttpSession使用

## 失效时间

在springboot2.0之后，设置session超时的方式修改为在application.yml或application.xml上面添加

```yaml
server:
  port: 8080
  # server.servlet.session.timeout=PT30M 30分钟
  servlet:
    session:
      timeout: PT30M
```

**server.servlet.session.timeout=PT1M**

PT1M 意思是设置session失效的时间是1分钟。

扩展：Duration
通过查看springboot源码发现setTimeouot方法，这里要求传入Duration的实例

```java
public void setTimeout(Duration timeout) {
    this.timeout = timeout;
}
```

```纯文本
而Duration是在Java8中新增的，主要用来计算日期差值，Duration是被final声明的，并且是线程安全的
转换字符串方式，类似于 SimpleDateFormat 的格式化日期方式
Duration 字符串类似数字有正负之分,默认正,负以’\-‘开头,紧接着’P’,下面所有字母都不区分大小写:
‘D’ – 天
‘H’ – 小时
‘M’ – 分钟
‘S’ – 秒
字符’T’是紧跟在时分秒之前的，每个单位都必须由数字开始,且时分秒顺序不能乱,比如:P2DT3M5S,P3D,PT3S
PT3M2S 等于 \-PT\-3M\-2S

最后总结一下Duration最实用的一个功能其实是 between 方法，因为有很多时候我们需要计算两个日期之间的天数或者小时数，用这个就可以很方便的进行操作。
```
