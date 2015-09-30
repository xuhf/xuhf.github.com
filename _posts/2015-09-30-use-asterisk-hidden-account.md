---
layout : post
title : "javascript中使用星号(*)隐藏帐号"
category : "javascript"
tags : [javascript]
---

## 写在前面

在我们日常开发中，经常会涉及到用户的银行招行，支付宝帐号等信息。

为了安全，不至于用户此类信息的泄漏，在前端显示时，需要对此类帐号隐藏其中一段。

实现的方式就是用`*`隐藏其中部分。

## 代码

```js
function accountEncrypt(account) {
    var accountLength,atIndex,prefix,endfix;
    accountLength = account.length;
    if (accountLength <= 8) {
        return account;
    }
    atIndex = account.indexOf("@");
    if (atIndex >= 0 && atIndex < 2) {
        return account;
    }
    if (atIndex > 2) {
        prefix = account.substring(0,2);
        endfix = account.substring(atIndex);
        return prefix + asterisk(atIndex - 2) + endfix;
    }
    if (/^\d{11}$/g.test(account)) {
        prefix = account.substring(0,3);
        endfix = account.substring(accountLength - 4);
        return prefix + asterisk(4) + endfix;
    }
    prefix = account.substring(0,4);
    endfix = account.substring(accountLength - 4);
    return prefix + asterisk(accountLength - 8) + endfix;
}

function asterisk(length) {
    var str = "";
    for (var i=0;i<length;i++) {
        str += "*";
    }
    return str;
}
```
