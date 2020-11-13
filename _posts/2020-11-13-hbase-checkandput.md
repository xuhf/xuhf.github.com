---
layout : post
title : "HBase中使用checkandput"
category : "大数据"
tags : [HBase]
---

## 背景

在HBase中Insert和Update操作没有了明确的区分，如果插入时不小心把以前的数据覆盖了怎么办？虽然可以通过timestamp将原先的数据找回，但事后弥补还是很麻烦。

如果我们想要经过验证再插入库怎么办呢。 HBase中有个CAS(compare-and-set)操作用来解决这个问题(数据一致性)，简单的说CAS操作可以让你在put数据之前先经过某些条件的验证，只有满足条件的put才会入库。

## HBase的API

HBase我们提供了相关API，如下：

```java
boolean checkAndPut(byte[] row, byte[] family, byte[] qualifier,
    byte[] value, Put put) throws IOException;
```

方法的最后一个参数是需要录入的数据的put对象，而前面的参数是与服务端check的预期值，只有服务器端对应rowkey的数据与预期的值相同时，put操作才能被提交的服务端。


## 案例代码

目前在hbase里已经存在表`order`,数据如下：

```
hbase(main):007:0> scan 'order'
ROW                            COLUMN+CELL                                                                           
 S1001                         column=m:age, timestamp=1605232470039, value=32                       
 S1001                         column=m:name, timestamp=1605232470039, value=xuhf                                    
 S1002                         column=m:mobile, timestamp=1605231017547, value=13584458545                           
 S1002                         column=m:name, timestamp=1605231017547, value=huanghai                                
 S1003                         column=m:name, timestamp=1605231017547, value=lili                                    
 S1003                         column=m:sex, timestamp=1605231017547, value=male 
```

HBaseUtil代码如下:

```java
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.hbase.HBaseConfiguration;
import org.apache.hadoop.hbase.TableName;
import org.apache.hadoop.hbase.client.Admin;
import org.apache.hadoop.hbase.client.Connection;
import org.apache.hadoop.hbase.client.ConnectionFactory;
import org.apache.hadoop.hbase.client.Table;

import java.io.IOException;

public class HBaseUtil {

    private final static String ZK_KEY = "hbase.zookeeper.quorum";

    private final static String ZK_VALUE = "hadoop007";

    private static Connection connection;

    static {
        try {
            Configuration conf = HBaseConfiguration.create();
            conf.set(ZK_KEY, ZK_VALUE);
            connection = ConnectionFactory.createConnection(conf);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static Admin getAdmin() throws IOException {
        Admin admin = connection.getAdmin();
        return admin;
    }

    public static void close(Admin admin) {
        if (admin != null) {
            try {
                admin.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    public static Table getTable(String tableName) throws IOException {
        Table table = connection.getTable(TableName.valueOf(tableName));
        return table;
    }

    public static void close(Table table) {
        if (table != null) {
            try {
                table.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

}
```

测试代码如下：

```java
// 在S1001的m:mobile不存在值的情况下去插入
@Test
public void test03() throws IOException {
    Table table = HBaseUtil.getTable("order");
    Put put = new Put(Bytes.toBytes("S1001"));
    put.addColumn(Bytes.toBytes("m"), Bytes.toBytes("mobile"), Bytes.toBytes("13584698547"));
    boolean flag = table.checkAndPut(Bytes.toBytes("S1001"), Bytes.toBytes("m"), Bytes.toBytes("mobile"), null, put);
    System.out.println(flag); // 此时返回true
    HBaseUtil.close(table);
}

// 在S1001的m:mobile已经存在值的情况下去插入
@Test
public void test04() throws IOException {
    Table table = HBaseUtil.getTable("order");
    Put put = new Put(Bytes.toBytes("S1001"));
    put.addColumn(Bytes.toBytes("m"), Bytes.toBytes("mobile"), Bytes.toBytes("18654785214"));
    boolean flag = table.checkAndPut(Bytes.toBytes("S1001"), Bytes.toBytes("m"), Bytes.toBytes("mobile"), null, put);
    System.out.println(flag); // 此时返回false
    HBaseUtil.close(table);
}

// 在S1001的m:name存在的值与我们期望的一致时插入
@Test
public void test05() throws IOException {
    Table table = HBaseUtil.getTable("order");
    Put put = new Put(Bytes.toBytes("S1001"));
    put.addColumn(Bytes.toBytes("m"), Bytes.toBytes("name"), Bytes.toBytes("lili"));
    boolean flag = table.checkAndPut(Bytes.toBytes("S1001"), Bytes.toBytes("m"), Bytes.toBytes("name"), Bytes.toBytes("xuhf"), put);
    System.out.println(flag); // 此时返回true
    HBaseUtil.close(table);
}

// 在S1001的m:name存在的值与我们期望的不一致时插入
@Test
public void test06() throws IOException {
    Table table = HBaseUtil.getTable("order");
    Put put = new Put(Bytes.toBytes("S1001"));
    put.addColumn(Bytes.toBytes("m"), Bytes.toBytes("name"), Bytes.toBytes("kevin"));
    boolean flag = table.checkAndPut(Bytes.toBytes("S1001"), Bytes.toBytes("m"), Bytes.toBytes("name"), Bytes.toBytes("xuhf"), put);
    System.out.println(flag); // 此时返回false
    HBaseUtil.close(table);
}
```


## 参考资料

<http://blog.sina.com.cn/s/blog_95532bcb0102vrqx.html>