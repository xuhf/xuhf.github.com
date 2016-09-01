---
layout : post
title : "Java执行命令并获取返回结果工具类"
category : "java"
tags : [java]
---

## 前言

本文借鉴了sshxcute-1.0的部分代码

在参考sshxcute的基础上写了以下的代码，目前不支持执行脚本。

## 需求

需要一个可以执行系统命令，并获取返回结果的工具类

## 代码实现

1，先来看命令的抽象类

```java
public abstract class AbstractCommand {

    protected static String DELIMETER = ";";

    protected String[] errorSysoutKeywords = { "Usage", "usage", "not found", "fail", "Fail", "error", "Error",
            "exception", "Exception", "not a valid" };

    public Boolean isSuccess(String stdout, int exitCode) {
        if (checkStdOut(stdout) && checkExitCode(exitCode))
            return true;
        else
            return false;
    }

    protected abstract Boolean checkStdOut(String stdout);

    protected abstract Boolean checkExitCode(int exitCode);

    public abstract String getCommand();

    public abstract String getInfo();

    protected String cat(String... args) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < args.length; i++) {
            sb.append(args[i]);
            sb.append(" ");
        }
        return sb.toString();
    }

}
```

2，具体的命令类

```java
public class ExecCommand extends AbstractCommand {

    private String command;

    public ExecCommand(String... args) {
        List<String> commands = Lists.newArrayList();
        for (int i = 0; i < args.length; i++) {
            commands.add(args[i]);
        }
        command = Joiner.on(DELIMETER).join(commands);
    }

    @Override
    protected Boolean checkStdOut(String stdout) {
        if (StringUtils.isBlank(stdout)) {
            return true;
        }
        for (String errorKeyword : errorSysoutKeywords) {
            if (stdout.contains(errorKeyword)) {
                return false;
            }
        }
        return true;
    }

    @Override
    protected Boolean checkExitCode(int exitCode) {
        return exitCode == 0;
    }

    @Override
    public String getCommand() {
        return command;
    }

    @Override
    public String getInfo() {
        return "Exec Command : " + command;
    }

}
```

3，命令的返回结果

```java
public class ExecResult {

    private int rc = -1;

    private String sysout;

    private String errorMessage;

    private boolean isSuccess = false;

    public int getRc() {
        return rc;
    }

    public void setRc(int rc) {
        this.rc = rc;
    }

    public String getSysout() {
        return sysout;
    }

    public void setSysout(String sysout) {
        this.sysout = sysout;
    }

    public String getErrorMessage() {
        return errorMessage;
    }

    public void setErrorMessage(String errorMessage) {
        this.errorMessage = errorMessage;
    }

    public boolean isSuccess() {
        return isSuccess;
    }

    public void setSuccess(boolean isSuccess) {
        this.isSuccess = isSuccess;
    }

    @Override
    public String toString() {
        return "ExecResult [rc=" + rc + ", sysout=" + sysout + ", errorMessage=" + errorMessage + ", isSuccess="
                + isSuccess + "]";
    }
}
```

4，命令的执行者

```java
public class CommandExecutor {

    public ExecResult exec(AbstractCommand task) {
        ExecResult r = new ExecResult();
        try {
            String command = task.getCommand();
            System.out.println("Command is : " + command);
            Process p = Runtime.getRuntime().exec(command);
            final InputStream in = p.getErrorStream();
            final StringBuffer info = new StringBuffer();
            final String systemLineSeparator = System.getProperty("line.separator", "\n");
            Thread errorThread = new Thread() {
                public void run() {
                    try {
                        BufferedReader reader = new BufferedReader(new InputStreamReader(in, "GB18030"));
                        String line = null;
                        while ((line = reader.readLine()) != null) {
                            info.append(line);
                            info.append(systemLineSeparator);
                        }
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            };
            errorThread.start();
            p.waitFor();
            int exitCode = p.exitValue();
            r.setRc(exitCode);
            if (task.isSuccess(info.toString(), exitCode)) {
                r.setSuccess(true);
                r.setErrorMessage("");
                r.setSysout(info.toString());
            } else {
                r.setSuccess(false);
                r.setErrorMessage(info.toString());
                r.setSysout("");
            }
            return r;
        } catch (Exception e) {
            r.setSuccess(false);
            r.setErrorMessage(e.getMessage());
        }
        return r;
    }
}
```

5，测试

```java
public class CommandTest {

    public static void main(String[] args) {
        AbstractCommand command = new ExecCommand("java -version");
        CommandExecutor executor = new CommandExecutor();
        ExecResult r = executor.exec(command);
        System.out.println(r);
    }
}
```

## 参考链接

<http://www.ibm.com/developerworks/cn/opensource/os-sshxcute/>