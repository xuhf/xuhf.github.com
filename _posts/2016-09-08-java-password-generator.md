---
layout : post
title : "Java生成随机密码，包含特殊符号"
category : "java"
tags : [java]
---

## 前言

项目中有需要需要随机生成密码，就自己百度了下，并自己修改了下，支持特殊符号。

## 代码

```java
import java.util.List;
import java.util.Random;

import com.google.common.collect.Lists;

public class PasswordGenerator {

    private static final int LENGTH = 12;

    private static final StringBuffer lowerCases = new StringBuffer(
            "a,b,c,d,e,f,g,h,i,g,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z");
    private static final StringBuffer upperCases = new StringBuffer(
            "A,B,C,D,E,F,G,H,I,G,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z");
    private static final StringBuffer symbols = new StringBuffer("~,@,#,$,%,^,&,*,(,),_,+,|,`,.");
    private static final StringBuffer numbers = new StringBuffer("1,2,3,4,5,6,7,8,9,0");

    public static void main(String[] args) {
        System.out.println(generate());
    }

    /**
     * 生成随机密码，包含大写，小写，数字，符号
     *
     * @return
     */
    public static String generate() {
        String[] lowerCaseArray = lowerCases.toString().split(",");
        String[] upperCaseArray = upperCases.toString().split(",");
        String[] symbolArray = symbols.toString().split(",");
        String[] numberArray = numbers.toString().split(",");
        List<String[]> codes = Lists.newArrayList();
        codes.add(lowerCaseArray);
        codes.add(upperCaseArray);
        codes.add(symbolArray);
        codes.add(numberArray);
        return generate(codes);
    }

    private static String generate(List<String[]> codes) {
        StringBuffer b = new StringBuffer();
        Random random;
        int k;
        // 先每样选一个
        for (String[] fragment : codes) {
            random = new Random();
            k = random.nextInt();
            b.append(String.valueOf(fragment[Math.abs(k % fragment.length)]));
        }
        // 在从每个中随机选择
        // 在从各个数组中选择数据
        for (int i = 0; i < LENGTH - codes.size(); i++) {
            random = new Random();
            k = random.nextInt();
            // 参考hashmap put方法
            int index = k & (codes.size() - 1);
            String[] fragment = codes.get(index);
            b.append(fragment[Math.abs(k % fragment.length)]);
        }
        return b.toString();
    }
}
```