---
layout : post
title : "使用commons-email来发送邮件"
category : "java"
tags : [java]
---


## 前言

在很多时候我们都需要发送邮件

这里使用apache的commons-email的邮件工具类

```java
import java.io.File;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

import org.apache.commons.mail.EmailAttachment;
import org.apache.commons.mail.HtmlEmail;

/**
 * 使用Commons-email发送邮件
 *
 * @author xuhf
 *
 */
public final class MailSender {

    public static void main(String[] args) throws Exception {
        MailSender sender = new MailSender();
        MailConfig config = new MailConfig();
        config.setHost("your host");
        config.setPort(25);
        config.setUsername("your email username");
        config.setPassword("your email password");
        config.addTo("tos ");
        config.addCc("ccs ");
        config.addAttachment(new File("path"));
        config.setSubject("来自Hudor的邮件");
        config.setContent("lalalalala");
        config.setSendDate(new Date());
        sender.sendHtml(config);
    }

    public void sendHtml(MailConfig config) throws Exception {
        HtmlEmail email = new HtmlEmail();
        email.setCharset(config.getCharset());
        email.setHostName(config.getHost());
        email.setSmtpPort(config.getPort());
        email.setAuthentication(config.getUsername(), config.getPassword());
        // 设置收信人
        for (String to : config.getToList()) {
            email.addTo(to);
        }
        // 设置抄送
        for (String cc : config.getCcList()) {
            email.addCc(cc);
        }
        // 设置密送
        for (String bcc : config.getBccList()) {
            email.addBcc(bcc);
        }
        // 携带附件
        for (File f : config.getAttchmentList()) {
            EmailAttachment attachment = new EmailAttachment();
            attachment.setPath(f.getAbsolutePath());
            attachment.setName(f.getName());
            attachment.setDisposition(EmailAttachment.ATTACHMENT);
            email.attach(attachment);
        }
        email.setFrom(config.getUsername());
        email.setSubject(config.getSubject());
        email.setHtmlMsg(config.getContent());
        email.setSentDate(config.getSendDate());
        email.send();
    }

    public static class MailConfig {

        private String charset = "UTF-8";

        /** 收件人 **/
        private List<String> toList = new ArrayList<String>();

        /** 抄送 **/
        private List<String> ccList = new ArrayList<String>();

        /** 密送 **/
        private List<String> bccList = new ArrayList<String>();

        /** 附件 **/
        private List<File> attchmentList = new ArrayList<File>();

        /** 邮件服务器 **/
        private String host;

        /** 邮件端口号 **/
        private int port = 25;

        /** 邮件用户名 **/
        private String username;

        /** 邮件密码 **/
        private String password;

        /** 邮件From **/
        private String from;

        /** 邮件主题 **/
        private String subject;

        /** 邮件内容 **/
        private String content;

        /** 发送时间 **/
        private Date sendDate = new Date();

        public String getCharset() {
            return charset;
        }

        public void setCharset(String charset) {
            this.charset = charset;
        }

        public List<String> getToList() {
            return toList;
        }

        public void setToList(List<String> toList) {
            this.toList = toList;
        }

        public List<String> getCcList() {
            return ccList;
        }

        public void setCcList(List<String> ccList) {
            this.ccList = ccList;
        }

        public List<String> getBccList() {
            return bccList;
        }

        public void setBccList(List<String> bccList) {
            this.bccList = bccList;
        }

        public List<File> getAttchmentList() {
            return attchmentList;
        }

        public void setAttchmentList(List<File> attchmentList) {
            this.attchmentList = attchmentList;
        }

        public String getHost() {
            return host;
        }

        public void setHost(String host) {
            this.host = host;
        }

        public int getPort() {
            return port;
        }

        public void setPort(int port) {
            this.port = port;
        }

        public String getUsername() {
            return username;
        }

        public void setUsername(String username) {
            this.username = username;
        }

        public String getPassword() {
            return password;
        }

        public void setPassword(String password) {
            this.password = password;
        }

        public String getFrom() {
            return from;
        }

        public void setFrom(String from) {
            this.from = from;
        }

        public String getSubject() {
            return subject;
        }

        public void setSubject(String subject) {
            this.subject = subject;
        }

        public String getContent() {
            return content;
        }

        public void setContent(String content) {
            this.content = content;
        }

        public Date getSendDate() {
            return sendDate;
        }

        public void setSendDate(Date sendDate) {
            this.sendDate = sendDate;
        }

        public void addTo(String to) {
            toList.add(to);
        }

        public void addCc(String cc) {
            ccList.add(cc);
        }

        public void addBcc(String bcc) {
            bccList.add(bcc);
        }

        public void addAttachment(File f) {
            attchmentList.add(f);
        }
    }
}
```