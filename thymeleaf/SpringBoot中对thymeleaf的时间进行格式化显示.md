## 场景

对thymeleaf中时间进行格式化显示。

## 实现

```html
<div class="form-group col-md-4 ml_10"><label>创建时间:</label>
    <input type= "text"th:value="${detailsVO==null || detailsVO.gmtCreat ==null ?'':#dates.format(detailsVO.gmtCreat, 'yyyy-MM-dd HH:mm:ss')}" readonly="readonly" class="form-control"/>
</div>
```



```html
<div class="layui-inline">
    <input type="text" class="layui-input" id="test1"
           th:value="${#dates.format(currentProcessVo.processEndTime,'yyyy-MM-dd HH:mm:ss')}" lay-key="1">
</div>
```



其他处理日期的方式见官方文档：

https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#dates

举例：


```js
/*
 * ======================================================================
 * See javadoc API for class org.thymeleaf.expression.Dates
 * ======================================================================
 */
 
/*
 * Format date with the standard locale format
 * Also works with arrays, lists or sets
 */
${#dates.format(date)}
${#dates.arrayFormat(datesArray)}
${#dates.listFormat(datesList)}
${#dates.setFormat(datesSet)}
 
/*
 * Format date with the ISO8601 format
 * Also works with arrays, lists or sets
 */
${#dates.formatISO(date)}
${#dates.arrayFormatISO(datesArray)}
${#dates.listFormatISO(datesList)}
${#dates.setFormatISO(datesSet)}
 
/*
 * Format date with the specified pattern
 * Also works with arrays, lists or sets
 */
${#dates.format(date, 'dd/MMM/yyyy HH:mm')}
${#dates.arrayFormat(datesArray, 'dd/MMM/yyyy HH:mm')}
${#dates.listFormat(datesList, 'dd/MMM/yyyy HH:mm')}
${#dates.setFormat(datesSet, 'dd/MMM/yyyy HH:mm')}
 
/*
 * Obtain date properties
 * Also works with arrays, lists or sets
 */
${#dates.day(date)}                    // also arrayDay(...), listDay(...), etc.
${#dates.month(date)}                  // also arrayMonth(...), listMonth(...), etc.
${#dates.monthName(date)}              // also arrayMonthName(...), listMonthName(...), etc.
${#dates.monthNameShort(date)}         // also arrayMonthNameShort(...), listMonthNameShort(...), etc.
${#dates.year(date)}                   // also arrayYear(...), listYear(...), etc.
${#dates.dayOfWeek(date)}              // also arrayDayOfWeek(...), listDayOfWeek(...), etc.
${#dates.dayOfWeekName(date)}          // also arrayDayOfWeekName(...), listDayOfWeekName(...), etc.
${#dates.dayOfWeekNameShort(date)}     // also arrayDayOfWeekNameShort(...), listDayOfWeekNameShort(...), etc.
${#dates.hour(date)}                   // also arrayHour(...), listHour(...), etc.
${#dates.minute(date)}                 // also arrayMinute(...), listMinute(...), etc.
${#dates.second(date)}                 // also arraySecond(...), listSecond(...), etc.
${#dates.millisecond(date)}            // also arrayMillisecond(...), listMillisecond(...), etc.
 
/*
 * Create date (java.util.Date) objects from its components
 */
${#dates.create(year,month,day)}
${#dates.create(year,month,day,hour,minute)}
${#dates.create(year,month,day,hour,minute,second)}
${#dates.create(year,month,day,hour,minute,second,millisecond)}
 
/*
 * Create a date (java.util.Date) object for the current date and time
 */
${#dates.createNow()}
${#dates.createNowForTimeZone()}
 
/*
 * Create a date (java.util.Date) object for the current date (time set to 00:00)
 */
${#dates.createToday()}
${#dates.createTodayForTimeZone()}
```

