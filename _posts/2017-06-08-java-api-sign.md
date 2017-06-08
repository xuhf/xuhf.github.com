---
layout : post
title : "接口开发中常用的签名生成算法"
category : "java"
tags : [java]
---

**本文章参考了支付宝签名生成的规则**

```java

import java.io.UnsupportedEncodingException;
import java.security.SignatureException;
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.apache.commons.codec.digest.DigestUtils;

/**
 * 接口参数传递过程中的签名生成算法
 *
 */
public class Sign {

    private static final String CHARSET = "UTF-8";

    public static String sign(String key, Map<String, String> parameters) {
        Map<String, String> filterParameters = filterParameters(parameters);
        String link = linkParameters(filterParameters);
        String mysign = MD5.sign(link, key, CHARSET);
        return mysign;
    }

    /**
     * sign不参与生成签名
     *
     * @param parameters
     * @return
     */
    private static Map<String, String> filterParameters(Map<String, String> parameters) {
        Map<String, String> result = new HashMap<String, String>();
        if (parameters == null || parameters.size() <= 0) {
            return result;
        }

        for (String key : parameters.keySet()) {
            String value = parameters.get(key);
            if (value == null || value.equals("") || key.equalsIgnoreCase("sign")) {
                continue;
            }
            result.put(key, value);
        }

        return result;
    }

    private static String linkParameters(Map<String, String> parameters) {
        List<String> keys = new ArrayList<String>(parameters.keySet());
        Collections.sort(keys);
        String prestr = "";
        for (int i = 0; i < keys.size(); i++) {
            String key = keys.get(i);
            String value = parameters.get(key);
            if (i == keys.size() - 1) {
                prestr = prestr + key + "=" + value;
            } else {
                prestr = prestr + key + "=" + value + "&";
            }
        }
        return prestr;
    }

    public static class MD5 {

        /**
         * 签名字符串
         *
         * @param text
         *            需要签名的字符串
         * @param key
         *            密钥
         * @param input_charset
         *            编码格式
         * @return 签名结果
         */
        public static String sign(String text, String key, String charset) {
            text = text + key;
            return DigestUtils.md5Hex(getContentBytes(text, charset));
        }

        /**
         * 签名字符串
         *
         * @param text
         *            需要签名的字符串
         * @param sign
         *            签名结果
         * @param key
         *            密钥
         * @param input_charset
         *            编码格式
         * @return 签名结果
         */
        public static boolean verify(String text, String sign, String key, String charset) {
            text = text + key;
            String mysign = DigestUtils.md5Hex(getContentBytes(text, charset));
            if (mysign.equals(sign)) {
                return true;
            } else {
                return false;
            }
        }

        /**
         * @param content
         * @param charset
         * @return
         * @throws SignatureException
         * @throws UnsupportedEncodingException
         */
        private static byte[] getContentBytes(String content, String charset) {
            if (charset == null || "".equals(charset)) {
                return content.getBytes();
            }
            try {
                return content.getBytes(charset);
            } catch (UnsupportedEncodingException e) {
                throw new RuntimeException("MD5签名过程中出现错误,指定的编码集不对,您目前指定的编码集是:" + charset);
            }
        }
    }
}
```