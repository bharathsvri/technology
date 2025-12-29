# Java Mail API

## What is JavaMail?
JavaMail API provides a platform-independent framework for sending and receiving email.

## Setup
```xml
<dependency>
    <groupId>com.sun.mail</groupId>
    <artifactId>javax.mail</artifactId>
    <version>1.6.2</version>
</dependency>
```

## Sending Email

### Basic Email
```java
import javax.mail.*;
import javax.mail.internet.*;
import java.util.Properties;

public class EmailSender {
    public static void sendEmail(String to, String subject, String body) {
        Properties props = new Properties();
        props.put("mail.smtp.host", "smtp.gmail.com");
        props.put("mail.smtp.port", "587");
        props.put("mail.smtp.auth", "true");
        props.put("mail.smtp.starttls.enable", "true");
        
        Session session = Session.getInstance(props, new Authenticator() {
            @Override
            protected PasswordAuthentication getPasswordAuthentication() {
                return new PasswordAuthentication("your-email@gmail.com", "your-password");
            }
        });
        
        try {
            Message message = new MimeMessage(session);
            message.setFrom(new InternetAddress("your-email@gmail.com"));
            message.setRecipients(Message.RecipientType.TO, 
                InternetAddress.parse(to));
            message.setSubject(subject);
            message.setText(body);
            
            Transport.send(message);
            System.out.println("Email sent successfully");
        } catch (MessagingException e) {
            e.printStackTrace();
        }
    }
}
```

### HTML Email
```java
public static void sendHtmlEmail(String to, String subject, String htmlBody) {
    // ... session setup ...
    
    try {
        Message message = new MimeMessage(session);
        message.setFrom(new InternetAddress("your-email@gmail.com"));
        message.setRecipients(Message.RecipientType.TO, 
            InternetAddress.parse(to));
        message.setSubject(subject);
        message.setContent(htmlBody, "text/html");
        
        Transport.send(message);
    } catch (MessagingException e) {
        e.printStackTrace();
    }
}
```

### Email with Attachments
```java
public static void sendEmailWithAttachment(String to, String subject, 
        String body, String attachmentPath) {
    // ... session setup ...
    
    try {
        Message message = new MimeMessage(session);
        message.setFrom(new InternetAddress("your-email@gmail.com"));
        message.setRecipients(Message.RecipientType.TO, 
            InternetAddress.parse(to));
        message.setSubject(subject);
        
        MimeBodyPart messageBodyPart = new MimeBodyPart();
        messageBodyPart.setText(body);
        
        Multipart multipart = new MimeMultipart();
        multipart.addBodyPart(messageBodyPart);
        
        MimeBodyPart attachmentPart = new MimeBodyPart();
        attachmentPart.attachFile(attachmentPath);
        multipart.addBodyPart(attachmentPart);
        
        message.setContent(multipart);
        Transport.send(message);
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

## Reading Email
```java
public static void readEmails() {
    Properties props = new Properties();
    props.put("mail.store.protocol", "imaps");
    props.put("mail.imaps.host", "imap.gmail.com");
    props.put("mail.imaps.port", "993");
    
    Session session = Session.getDefaultInstance(props);
    
    try {
        Store store = session.getStore("imaps");
        store.connect("your-email@gmail.com", "your-password");
        
        Folder inbox = store.getFolder("INBOX");
        inbox.open(Folder.READ_ONLY);
        
        Message[] messages = inbox.getMessages();
        for (Message message : messages) {
            System.out.println("Subject: " + message.getSubject());
            System.out.println("From: " + message.getFrom()[0]);
            System.out.println("Text: " + message.getContent().toString());
        }
        
        inbox.close(false);
        store.close();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

## Best Practices
1. Use environment variables for credentials
2. Handle exceptions properly
3. Use connection pooling for multiple emails
4. Validate email addresses
5. Use HTML templates for complex emails
6. Implement retry logic
7. Log email operations
8. Use async sending for better performance
9. Secure credentials
10. Test with different email providers

