# Jackson自定义序列化和反序列化

## 目录

*   [自定义序列化](#自定义序列化)

*   [自定义反序列化](#自定义反序列化)

## 自定义序列化

```java
package com.example.jacksondemo;

import java.io.IOException;
import java.util.List;
import java.util.Map;

import com.example.jacksondemo.bean.Home;
import com.example.jacksondemo.bean.Item;
import com.example.jacksondemo.bean.Person;
import com.fasterxml.jackson.core.JsonGenerator;
import com.fasterxml.jackson.databind.JsonSerializer;
import com.fasterxml.jackson.databind.SerializerProvider;

/**
 * @author rensiyu
 **/
public class MyDataSerialize extends JsonSerializer<Home> {
  static final String S = "自定义序列化";
  
  @Override
  public void serialize(Home value, JsonGenerator gen, SerializerProvider serializers) throws IOException {
    gen.writeStartObject();
    gen.writeStringField("home_name", value.getHomeName() + S);
    gen.writeFieldName("person_list");
    gen.writeStartArray();
    {
      List<Person> personList = value.getPersonList();
      for (int i = 0; i < personList.size(); i++) {
        gen.writeStartObject();
        gen.writeStringField("person_name", personList.get(i).getName() + S);
        gen.writeStringField("person_age", personList.get(i).getName() + S);
        {
          gen.writeFieldName("item_map");
          gen.writeStartObject();
          Map<String, Item> itemMap = personList.get(i).getItemMap();
          itemMap.forEach((key, item) -> {
            // "111"
            try{
              gen.writeFieldName(key);
              {
                gen.writeStartObject();
                {
                  gen.writeStringField("id", item.getId());
                  gen.writeStringField("name", item.getName());
                }
                gen.writeEndObject();
              }
            }catch (IOException e){
              e.printStackTrace();
            }

          });
          gen.writeEndObject();
        }
        gen.writeStringField("person_item_map", personList.get(i).getName() + S);
        gen.writeEndObject();
      }
    }
    gen.writeEndArray();
    gen.writeEndObject();
  }
}

```

```json
{
  "home_name": "我的家自定义序列化",
  "person_list": [
    {
      "person_name": "张三自定义序列化",
      "person_age": "张三自定义序列化",
      "item_map": {
        "111": {
          "id": "111",
          "name": "鼠标"
        },
        "112": {
          "id": "112",
          "name": "键盘"
        }
      },
      "person_item_map": "张三自定义序列化"
    },
    {
      "person_name": "李四自定义序列化",
      "person_age": "李四自定义序列化",
      "item_map": {
        "113": {
          "id": "113",
          "name": "money"
        }
      },
      "person_item_map": "李四自定义序列化"
    }
  ]
}
```

## 自定义反序列化

太麻烦了，不想搞了，但是不难理解

```java
    @Override
    public Home deserialize(JsonParser parser, DeserializationContext ctxt) throws IOException, JacksonException {
        Home home = new Home();
        while (!parser.isClosed()) {
            JsonToken jsonToken = parser.nextToken();
            if (jsonToken == JsonToken.FIELD_NAME) {
                String key = parser.getCurrentName();
                if (key.equalsIgnoreCase("home_name")) {
                    home.setHomeName(parser.nextTextValue());
                } else if (key.equalsIgnoreCase("person_list")) {
                    List<Person> list = new ArrayList<>();
                    while (!parser.isClosed()) {
                        jsonToken = parser.nextToken();
                        if (jsonToken == JsonToken.START_OBJECT) {
                            Person person = new Person();
                            while (!parser.isClosed()) {
                                jsonToken = parser.nextToken();
                                if (jsonToken == JsonToken.FIELD_NAME) {
                                    key = parser.getCurrentName();
                                    if (key.equalsIgnoreCase("person_name")) {
                                        person.setName(parser.nextTextValue());
                                    } else if (key.equalsIgnoreCase("person_age")) {
                                        person.setAge(parser.nextIntValue(0));
                                    } else { // item_map
                                        Map<String, Item> itemMap = new HashMap<>();
                                        while (!parser.isClosed()) {
                                            if (jsonToken == JsonToken.FIELD_NAME) {
                                                key = parser.getCurrentName();
                                                while (!parser.isClosed()) {
                                                    JsonToken jsonToken1 = parser.nextToken();
                                                    Item item = new Item();
                                                    if (jsonToken1.equals("id")) {
                                                        item.setId(parser.nextTextValue());
                                                    } else { // name
                                                        item.setName(parser.nextTextValue());
                                                    }
                                                    itemMap.put(key, item);
                                                }
                                            }
                                        }
                                        person.setItemMap(itemMap);
                                    }
                                    list.add(person);
                                }

                            }
                        }

                    }
                    home.setPersonList(list);
                }
            }
        }
        System.out.println(home);
        return home;
    }
```

```json
{
    "homeName": "我的家自定义序列化",
    "personList": [
        {
            "name": "张三自定义序列化",
            "age": 0,
            "itemMap": {
                "item_map": {
                    "id": null,
                    "name": null
                }
            },
            "school": null
        },
        {
            "name": "张三自定义序列化",
            "age": 0,
            "itemMap": {
                "item_map": {
                    "id": null,
                    "name": null
                }
            },
            "school": null
        },
        {
            "name": "张三自定义序列化",
            "age": 0,
            "itemMap": {
                "item_map": {
                    "id": null,
                    "name": null
                }
            },
            "school": null
        }
    ]
}
```
