## TKTracker Integration Code
```java
  TKTracker tkTracker = new TKTracker(this);
  tkTracker.setID("a4a3cd56-a584-4fa4-8973-1120d14b18b7")
          .setTitle("[超瞎] 超人大戰蝙蝠俠，鹿死誰手？")
          .send();
```
## Custom Optional
```java
tkTracker.setID("a4a3cd56-a584-4fa4-8973-1120d14b18b7")
        .setTitle("[超瞎] 超人大戰蝙蝠俠，鹿死誰手？")
        .setURL("http://www.google.com")
        .setImg("http://lorempixel.com/82/82/sports/")
        .setReference("aotter")
        .setCategories(new String[]{"News", "News_domestic"}) //[String[] Type]
        .setPublishedDate(1438090882490L) //[Long Type] unix time 
        .setTimespan(65) //[Integer Type] seconds 
        .send();
```