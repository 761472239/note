# jackson学习之三：常用API操作

## 目录

*   [单个对象序列化](#单个对象序列化)

*   [单个对象反序列化](#单个对象反序列化)

*   [集合序列化](#集合序列化)

*   [JsonNode](#jsonnode)

*   [时间字段格式化](#时间字段格式化)

*   [JSON数组的反序列化](#json数组的反序列化)

## 单个对象序列化

1）对象转字符串

```java
String jsonStr = mapper.writeValueAsString(twitterEntry);

```

2）对象转文件

```java
mapper.writeValue(new File("twitter.json"), twitterEntry);

```

3）对象转byte数组

```java
byte[] array = mapper.writeValueAsBytes(twitterEntry);

```

## 单个对象反序列化

1）字符串转对象

```java
TwitterEntry tFromStr = mapper.readValue(objectJsonStr, TwitterEntry.class);

```

2）文件转对象

```java
TwitterEntry tFromFile = mapper.readValue(new File("twitter.json"), TwitterEntry.class);

```

3）byte数组转对象

```java
TwitterEntry tFromBytes = mapper.readValue(array, TwitterEntry.class);

```

4）字符串网络地址转对象

```java
String testJsonDataUrl = "https://.../twitteer_message.json";

TwitterEntry tFromUrl = mapper.readValue(new URL(testJsonDataUrl), TwitterEntry.class);

```

## 集合序列化

序列化

```java
String mapJsonStr = mapper.writeValueAsString(map);

```

反序列化

```java
Map<String, Object> mapFromStr = mapper.readValue(mapJsonStr, new TypeReference<Map<String, Object>>() {});

```

## JsonNode

1）如果您不想使用XXX.class来做反序列化，也能使用JsonNode来操作：

```java
JsonNode jsonNode = mapper.readTree(mapJsonStr);
String name = jsonNode.get("name").asText();
int age = jsonNode.get("age").asInt();
String city = jsonNode.get("addr").get("city").asText();
String street = jsonNode.get("addr").get("street").asText();
```

## 时间字段格式化

1）对于Date字段，默认的反序列化是时间戳，可以修改配置：

```java
ObjectMapper mapper = new ObjectMapper();
System.out.println(mapper.writeValueAsString(new Date())); // 1633327143569
```

修改配置

```java
mapper.setDateFormat(new SimpleDateFormat("yyyy-MM-dd hh:mm:ss"));
dateMapStr = mapper.writeValueAsString(dateMap);

```

## JSON数组的反序列化

假设jsonArrayStr是个json数组格式的字符串：

1）JSON数组转对象数组：

```java
TwitterEntry[] twitterEntryArray = mapper.readValue(jsonArrayStr, TwitterEntry[].class);

```

2）JSON数组转对象集合(ArrayList)：

```java
List<TwitterEntry> twitterEntryList = mapper.readValue(jsonArrayStr, new TypeReference<List<TwitterEntry>>() {});

```
