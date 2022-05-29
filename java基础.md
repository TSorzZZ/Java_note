## 字符串处理

## 1、IDEA 

1、快速生成get/set方法(alt + Insert)

## 2、关于String

**2.1、 获得子串： substring(a,b)   左闭右开 不符合<font color="red">小驼峰命名法，是小写</font>**

```java
String str = str.substring(a,b);
```

各种类型转String用xxx.toString()

xxx = Character||Integer||.....等包装类



**2.2、 字符串分割：.split(String str,int limit)**

注： .  $  |  *等的转义字符要加\\,

注：其中 limit为非正整数表示 模式被应用尽可能多的次数 比如-1（**会保留结尾的空字符串**）

limit =0 表示模式应用尽可能多的次数，数组可以是任意长度，并且**结尾空字符串将被丢弃。**

limit>0时 那么模式将会应用**limit-1**次 数组长度不会超过limit

```java
String[] strarr = str.split("",-1);
```

**2.3、获得指定位置字符：**

```java
char ch = str.charAt(int pos)
```

**2.4、获得某个字符的下标：**

```java
int pos = str.indexOf('s');
```

**2.5、保留小数**

```java
String a = "123.4567"
String b = String.format("%.2f",a);	// b = 123.45
```



## 3、StringBuffer

**3.1、构造方法**

```java
StringBuffer sb = new StringBuffer();
```

**3.2、和String的相互转换**

```java
StringBuffer sb = new StringBuffer(str);
String str = sb.toString();
```

**3.3、末尾拼接**

```java
sb.append();
```

**3.4、反转**

```java
sb.reverse();
```

## 4、BigDecimal

import java.math.*;

作用： 

+ 大数计算
+ 保留小数

```java
BigDecimal a = new BigDecimal();
BigDecimal b = new BigDecimal();
a = a.add(b);
a = a.substract(b);
a = a.multiply(b);
a = a.divide(b,2,BigDecimal.ROUND_HALF_UP);	//四舍五入 保留2位

a = a.setScale(3,BigDecimal.ROUND_HALF_UP); //四舍五入 保留3位
```

