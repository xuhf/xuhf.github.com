---
layout : post
title : "Java关于数值、货币值、百分数的格式化"
category : "Java"
tags : [java,formatter]
---

## 关于格式化

在日常的软件需求中，很多时候需要将数值格式化成其他格式，例如：百分比形式，货币形式。

example：

我们需要将`123456.789`格式化成`123,456.789`的形式，如何做的，往下看。

## 格式化成百分比形式

```java
double d = 0.3333;
NumberFormat percentFormatter = NumberFormat.getPercentInstance();
percentFormatter.setMinimumFractionDigits(2);
System.out.println(percentFormatter.format(d));
```

输出
```
33.33%
```

## 格式化成货币形式

```java
double money = 123456.891;
NumberFormat currencyFormatter = NumberFormat.getCurrencyInstance();
System.out.println(currencyFormatter.format(money));
```

输出
```
￥123,456.89
```

## 格式化成123,456.78形式

```java
double money = 123456.891;
DecimalFormat decimalFormat = new DecimalFormat("###,###.00");
System.out.println(decimalFormat.format(money));
```

输出
```
123,456.89
```

## 消息格式化

```java
String pattern = "今天是：{0,date,yyyy-MM-dd},您是第{1}位等待者，您的名字是:{2},您的账户余额是:{3,number,currency}";
MessageFormat messageFormat = new MessageFormat(pattern);
Object[] data = { new Date(), 3, "vvkee", 12345678.90 };
String message = messageFormat.format(data);
System.out.println(message);
```

输出

```
今天是：2015-10-25,您是第3位等待者，您的名字是:vvkee,您的账户余额是:￥12,345,678.90
```
