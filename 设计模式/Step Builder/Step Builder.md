# Step Builder

## 目录

*   [代码分析](#代码分析)

*   [普通的构建器](#普通的构建器)

*   [Step builder](#step-builder-1)

## 代码分析

需要创建的对象：

```java
package com.stepbuilder.bar;
import java.util.List;
public class Panino {

 private final String name;
 private String breadType;
 private String fish;
 private String cheese;
 private String meat;
 private List vegetables;

 // 省略set get方法

 @Override
 public String toString() {
  return "Panino [name=" + name + ", breadType=" + breadType + ", fish="
    + fish + ", cheese=" + cheese + ", meat=" + meat
    + ", vegetables=" + vegetables + "]";
 }
 
}
```

Builder模式的扩展，完全指导用户创建对象，不会造成混淆。
用户体验将得到更大的改善，因为他只会看到可用的下一步方法，直到正确的时间来构建对象时才会看到构建方法。

编写内部类或接口的创建步骤，其中每个方法都知道接下来可以显示什么

在一个内部静态类中实现所有的step接口

最后一步是BuildStep，负责创建需要构建的对象

当创建复杂对象的算法应独立于组成对象的零件以及它们的组装方式时，请使用Step Builder模式。在构建顺序的过程中，构建过程必须允许对构建的对象使用不同的表示。

## 普通的构建器

```java
public class PaninoBuilder {
 
 private String name;
 private String breadType;
 private String fish;
 private String cheese;
 private String meat;
 private List vegetables = new ArrayList();

   public PaninoBuilder paninoCalled(String name){
    this.name = name;
    return this;
   }
   
   public PaninoBuilder breadType(String breadType){
    this.breadType = breadType;
    return this;
   }
   
   public PaninoBuilder withFish(String fish){
    this.fish = fish;
    return this;
   }
   
   public PaninoBuilder withCheese(String cheese){
    this.cheese = cheese;
    return this;
   }
   
   public PaninoBuilder withMeat(String meat){
    this.meat = meat;
    return this;
   }
   
   public PaninoBuilder withVegetable(String vegetable){  
    vegetables.add(vegetable);
    return this;
   }
 
   public Panino build(){  
    Panino panino = new Panino(name);
    panino.setBreadType(breadType);
    panino.setCheese(cheese);
    panino.setFish(fish);
    panino.setMeat(meat);
    panino.setVegetables(vegetables);
    return panino;
   }
 }
```

使用build

```java
package com.stepbuilder.bar.client;
import com.stepbuilder.bar.Panino;
import com.stepbuilder.bar.PaninoBuilder;
public class Bar {

 public static void main(String[] args) {
  Panino marcoPanino = new PaninoBuilder().paninoCalled("marcoPanino")
    .breadType("baguette").withCheese("gorgonzola").withMeat("ham")
    .withVegetable("tomatos").build();

  System.out.println(marcoPanino);
 }
}
```

So far so good. &#x20;

But what I don't like of the traditional Builder pattern is the following: &#x20;

*   It does not really guide the user through the creation.

*   A user can always call the build method in any moment, even without the needed information. 

*   There is no way to guide the user from a creation path instead of another based on conditions. 

*   There is always the risk to leave your object in an inconsistent state.

*   All methods are always available, leaving the responsibility of which to use and when to use to the user who did not write the api.

For instance, in the Panino example, a user could write something like this:

```java
package com.stepbuilder.bar.client;
import com.stepbuilder.bar.Panino;
import com.stepbuilder.bar.PaninoBuilder;
public class Bar {

 public static void main(String[] args) {
  Panino marcoPanino = new PaninoBuilder().paninoCalled("marcoPanino")
    .withCheese("gorgonzola").build();

  // or
  marcoPanino = new PaninoBuilder().withCheese("gorgonzola").build();
  // or
  marcoPanino = new PaninoBuilder().withMeat("ham").build();
  // or
  marcoPanino = new PaninoBuilder().build();
 }
}
```

以上所有的panino实例都是错误的，用户直到运行时才会知道他什么时候会使用panino对象。

当然，您可以在构建方法中添加验证，但用户仍然无法从糟糕的构建器使用中恢复过来。

您也可以在所有必需的属性周围设置默认值，但这样代码的可读性就会丢失(new PaninoBuilder().build();您在这里构建的是什么?)，而且通常需要用户的一些输入(例如，用户和连接的密码)。

因此，这是我的解决方案，称为Step builder，它是builder模式的扩展，完全指导用户创建对象，不会有任何混淆的机会

## Step builder

StepBuilder，是builder模式的扩展，它完全引导用户创建对象，而不会产生混淆。

```java
package com.stepbuilder.bar;
import java.util.ArrayList;
import java.util.List;
public class PaninoStepBuilder {

        public static FirstNameStep newBuilder() {
                return new Steps();
        }

        private PaninoStepBuilder() {
        }

        /**
         * First Builder Step in charge of the Panino name. 
         * Next Step available : BreadTypeStep
         */
        public static interface FirstNameStep {
                BreadTypeStep paninoCalled(String name);
        }

        /**
         * This step is in charge of the BreadType. 
         * Next Step available : MainFillingStep
         */
        public static interface BreadTypeStep {
                MainFillingStep breadType(String breadType);
        }

        /**
         * This step is in charge of setting the main filling (meat or fish). 
         * Meat choice : Next Step available : CheeseStep 
         * Fish choice : Next Step available : VegetableStep
         */
        public static interface MainFillingStep {
                CheeseStep meat(String meat);

                VegetableStep fish(String fish);
        }

        /**
         * This step is in charge of the cheese. 
         * Next Step available : VegetableStep
         */
        public static interface CheeseStep {
                VegetableStep noCheesePlease();

                VegetableStep withCheese(String cheese);
        }

        /**
         * This step is in charge of vegetables. 
         * Next Step available : BuildStep
         */
        public static interface VegetableStep {
                BuildStep noMoreVegetablesPlease();

                BuildStep noVegetablesPlease();

                VegetableStep addVegetable(String vegetable);
        }

        /**
         * This is the final step in charge of building the Panino Object.
         * Validation should be here.
         */
        public static interface BuildStep {
                Panino build();
        }

        private static class Steps implements FirstNameStep, BreadTypeStep, MainFillingStep, CheeseStep, VegetableStep, BuildStep {

                private String name;
                private String breadType;
                private String meat;
                private String fish;
                private String cheese;
                private final List<String> vegetables = new ArrayList<String>();

                public BreadTypeStep paninoCalled(String name) {
                        this.name = name;
                        return this;
                }

                public MainFillingStep breadType(String breadType) {
                        this.breadType = breadType;
                        return this;
                }

                public CheeseStep meat(String meat) {
                        this.meat = meat;
                        return this;
                }

                public VegetableStep fish(String fish) {
                        this.fish = fish;
                        return this;
                }

                public BuildStep noMoreVegetablesPlease() {
                        return this;
                }

                public BuildStep noVegetablesPlease() {
                        return this;
                }

                public VegetableStep addVegetable(String vegetable) {
                        this.vegetables.add(vegetable);
                        return this;
                }

                public VegetableStep noCheesePlease() {
                        return this;
                }

                public VegetableStep withCheese(String cheese) {
                        this.cheese = cheese;
                        return this;
                }

                public Panino build() {
                        Panino panino = new Panino(name);
                        panino.setBreadType(breadType);
                        if (fish != null) {
                                panino.setFish(fish);
                        } else {
                                panino.setMeat(meat);
                        }
                        if (cheese != null) {
                                panino.setCheese(cheese);
                        }
                        if (!vegetables.isEmpty()) {
                                panino.setVegetables(vegetables);
                        }
                        return panino;
                }

        }
}
```

概念很简单:

编写内部类或接口的创建步骤，每个方法都知道接下来要显示什么。

在内部静态类中实现所有步骤接口。

最后一步是BuildStep，负责创建需要构建的对象。

用户体验将得到更大的改善，因为他只会看到可用的下一步方法，直到正确的时间来构建对象时才会看到构建方法。

```java
package com.stepbuilder.bar.client;
import com.stepbuilder.bar.Panino;
import com.stepbuilder.bar.PaninoBuilder;
import com.stepbuilder.bar.PaninoStepBuilder;
public class Bar {

 public static void main(String[] args) {
  Panino solePanino = PaninoStepBuilder.newBuilder()
        .paninoCalled("sole panino")
        .breadType("baguette")
        .fish("sole")
        .addVegetable("tomato")
        .addVegetable("lettece")
        .noMoreVegetablesPlease()
        .build();

  
 }
}
```

The user will not be able to call the method breadType(String breadType) before calling the paninoCalled(String name) method, and so on. &#x20;

Plus, using this pattern, if the user choose a fish panino, I will not give him the possibility to add cheese (i'm the chef, I know how to prepare a panino). &#x20;

In the end, I will have  a consistent Object and user will find extremely easy to use the API because he will have very few and selected choices per time. &#x20;

We could have different panino traditional builders, FishPaninoBuilder, MeatPaninoBuilder, etc, but still we would face the inconsistent problems and the
user will still need to understand exactly what was your idea behind the builder. &#x20;

With the Step Builder the user will know without any doubt what input is required, making his and your life easier.
