# User Setting

```java
//Register user info anywhere
AotterTrek.setPhone("0912456789") //使用者的手機號碼
    .setEmail("XXXXX@gmail.con") //使用者的Email信箱
    .setBirthday("1999/01/01") //使用者的生日，僅接受yyyy/MM/dd格式
    .setFbId("123456789") //使用者的facebook id
    .setGender("FEMALE"); // 傳送任一值 M,MALE,BOY,MEN,MAN代表男性， F,FEMALE,GIRL,WOMEN,WOMAN代表女性。
    .setUserMeta(JSONObject meta); //自定義Meta物件

AotterTrek.clear(); // can clear user info
```
