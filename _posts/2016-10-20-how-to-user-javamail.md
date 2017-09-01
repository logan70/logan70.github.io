---
layout: post
title:  "如何通过JAVA发送邮件"
categories: JAVA
tags:  mail 
---

* content
{:toc}



> 一般用户注册、找回密码，都是通过 **手机** 和 **邮箱** 找回密码的！
> 
>`我这通过示范126邮箱发送编写发送邮件服务!`

<!--more-->

导入JAR
==

 编写邮件服务需要导入对应的jar包：[http://mvnrepository.com/artifact/javax.mail/mail/1.4](http://mvnrepository.com/artifact/javax.mail/mail/1.4)


开启邮箱`STMP`服务
==
我们是通过SMTP服务发送邮件、所以要先登录邮箱网站开启对应的服务才行：[http://www.126.com/](http://www.126.com/)

激活SMTP服务
--
点击之后会弹框设置  **授权码**  按照对应的提示设置即可！
`授权码一定要记住、代码里面需要用到`




下面还有一个提示栏目、里面有服务器地址，我们是SMTP服务器所以选择`SMTP服务器: smtp.126.com`、代码需要使用到
![SMTP服务开启](/images/2016-10-20/01.jpg)


邮件服务代码
==
```java
//邮件服务类
public class SendEmail {
	//服务器地址
	public static final String HOST = "smtp.126.com";
	//协议
    public static final String PROTOCOL = "smtp";   
    //端口
    public static final int PORT = 25;
    //邮箱帐号
    public static final String FROM = "mrjanda@126.com";
    //填写自己的授权码
    public static final String PWD = "****";
    //发送人昵称,这个自定义
    public static final String nick="xxx"; 

    /**
     * 获取Session邮件
     * @return session
     */
    private static Session getSession() {
        Properties props = new Properties();
        props.put("mail.smtp.host", HOST);//设置服务器地址
        props.put("mail.store.protocol" , PROTOCOL);//设置协议
        props.put("mail.smtp.port", PORT);//设置端口
        props.put("mail.smtp.auth" , "true");

        Authenticator authenticator = new Authenticator() {
            @Override
            protected PasswordAuthentication getPasswordAuthentication() {
                return new PasswordAuthentication(FROM, PWD);
            }

        };
        Session session = Session.getDefaultInstance(props , authenticator);
        return session;
    }

    /**
     * 发送信息至邮箱
     * @param title			邮箱标题
     * @param toEmail		接收邮箱
     * @param content		邮件内容
     * @throws AddressException
     * @throws MessagingException
     */
    public static void send(String title,String toEmail , String content) throws AddressException, MessagingException {
        Session session = getSession();
        Message msg = new MimeMessage(session);
        //设置发送内容
        msg.setFrom(new InternetAddress(nick +"<"+FROM+">"));
        InternetAddress[] address = {new InternetAddress(toEmail)};
        msg.setRecipients(Message.RecipientType.TO, address);
        msg.setSubject(title);
        msg.setSentDate(new Date());
        msg.setContent(content , "text/html;charset=utf-8");
        //发送信息
        Transport.send(msg);
    }

    public static void main(String[] args) {
    	try {
			send("欢迎注册", "158464459@qq.com", "这是测试发送的方法！");
			System.out.println("发送成功！");
		} catch (AddressException e) {
			e.printStackTrace();
		} catch (MessagingException e) {
			e.printStackTrace();
		}
	}
}
```

邮件发送成功
![发送成功](/images/2016-10-20/02.jpg)

----

其它邮箱服务可以使用这段代码、只要修改对应的`服务器地址`,`协议`,`端口`,`帐号`,`授权码`就可以使用

> 服务器地址：每个邮箱提供商地址都是不一样、进入邮箱设置可以查看。<br/>
> 协议：调用自己对应使用的即可。<br/>
> 端口：协议不同端口也不相同。<br/>
> 帐号：发送邮件的帐号。<br/>
> 授权码：有的邮箱是称之为 **密码**。

