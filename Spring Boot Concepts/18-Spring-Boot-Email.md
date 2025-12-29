# Spring Boot Email

## Sending Emails with Spring Boot

Spring Boot provides easy email support through Spring's email abstraction. It supports both simple text emails and HTML emails with attachments.

## Setting Up Email

### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

### Configuration

```properties
# SMTP Configuration
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=your-email@gmail.com
spring.mail.password=your-password
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
spring.mail.properties.mail.smtp.starttls.required=true
```

## Sending Simple Email

### Using JavaMailSender

```java
@Service
public class EmailService {
    
    private final JavaMailSender mailSender;
    
    public EmailService(JavaMailSender mailSender) {
        this.mailSender = mailSender;
    }
    
    public void sendSimpleEmail(String to, String subject, String text) {
        SimpleMailMessage message = new SimpleMailMessage();
        message.setTo(to);
        message.setSubject(subject);
        message.setText(text);
        mailSender.send(message);
    }
}
```

## Sending HTML Email

### HTML Email with MimeMessageHelper

```java
@Service
public class EmailService {
    
    private final JavaMailSender mailSender;
    
    public void sendHtmlEmail(String to, String subject, String htmlContent) {
        MimeMessage message = mailSender.createMimeMessage();
        
        try {
            MimeMessageHelper helper = new MimeMessageHelper(message, true);
            helper.setTo(to);
            helper.setSubject(subject);
            helper.setText(htmlContent, true); // true = HTML
            
            mailSender.send(message);
        } catch (MessagingException e) {
            throw new EmailException("Failed to send email", e);
        }
    }
}
```

## Sending Email with Attachments

### Email with Attachment

```java
@Service
public class EmailService {
    
    private final JavaMailSender mailSender;
    
    public void sendEmailWithAttachment(String to, String subject, 
                                        String text, String attachmentPath) {
        MimeMessage message = mailSender.createMimeMessage();
        
        try {
            MimeMessageHelper helper = new MimeMessageHelper(message, true);
            helper.setTo(to);
            helper.setSubject(subject);
            helper.setText(text);
            
            FileSystemResource file = new FileSystemResource(new File(attachmentPath));
            helper.addAttachment(file.getFilename(), file);
            
            mailSender.send(message);
        } catch (MessagingException e) {
            throw new EmailException("Failed to send email", e);
        }
    }
}
```

## Sending Email to Multiple Recipients

### Multiple Recipients

```java
public void sendEmailToMultiple(String[] to, String subject, String text) {
    SimpleMailMessage message = new SimpleMailMessage();
    message.setTo(to);
    message.setSubject(subject);
    message.setText(text);
    mailSender.send(message);
}

public void sendEmailWithCcBcc(String to, String[] cc, String[] bcc, 
                               String subject, String text) {
    SimpleMailMessage message = new SimpleMailMessage();
    message.setTo(to);
    message.setCc(cc);
    message.setBcc(bcc);
    message.setSubject(subject);
    message.setText(text);
    mailSender.send(message);
}
```

## Email Templates

### Using Thymeleaf Templates

#### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

#### Template

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Welcome Email</title>
</head>
<body>
    <h1>Welcome, <span th:text="${name}">User</span>!</h1>
    <p>Thank you for registering with us.</p>
</body>
</html>
```

#### Service

```java
@Service
public class EmailService {
    
    private final JavaMailSender mailSender;
    private final TemplateEngine templateEngine;
    
    public void sendWelcomeEmail(String to, String name) {
        Context context = new Context();
        context.setVariable("name", name);
        
        String htmlContent = templateEngine.process("welcome-email", context);
        
        MimeMessage message = mailSender.createMimeMessage();
        try {
            MimeMessageHelper helper = new MimeMessageHelper(message, true);
            helper.setTo(to);
            helper.setSubject("Welcome!");
            helper.setText(htmlContent, true);
            
            mailSender.send(message);
        } catch (MessagingException e) {
            throw new EmailException("Failed to send email", e);
        }
    }
}
```

## Async Email Sending

### Using @Async

```java
@Configuration
@EnableAsync
public class AsyncConfig {
}

@Service
public class EmailService {
    
    @Async
    public void sendEmailAsync(String to, String subject, String text) {
        sendSimpleEmail(to, subject, text);
    }
}
```

## Email Configuration for Different Providers

### Gmail

```properties
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=your-email@gmail.com
spring.mail.password=your-app-password
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
```

### Outlook

```properties
spring.mail.host=smtp-mail.outlook.com
spring.mail.port=587
spring.mail.username=your-email@outlook.com
spring.mail.password=your-password
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
```

### Custom SMTP

```properties
spring.mail.host=smtp.example.com
spring.mail.port=465
spring.mail.username=your-email@example.com
spring.mail.password=your-password
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.ssl.enable=true
```

## Complete Email Service Example

```java
@Service
public class EmailService {
    
    private final JavaMailSender mailSender;
    private final TemplateEngine templateEngine;
    
    public EmailService(JavaMailSender mailSender, TemplateEngine templateEngine) {
        this.mailSender = mailSender;
        this.templateEngine = templateEngine;
    }
    
    @Async
    public void sendSimpleEmail(String to, String subject, String text) {
        SimpleMailMessage message = new SimpleMailMessage();
        message.setTo(to);
        message.setSubject(subject);
        message.setText(text);
        mailSender.send(message);
    }
    
    @Async
    public void sendHtmlEmail(String to, String subject, String htmlContent) {
        MimeMessage message = mailSender.createMimeMessage();
        try {
            MimeMessageHelper helper = new MimeMessageHelper(message, true, "UTF-8");
            helper.setTo(to);
            helper.setSubject(subject);
            helper.setText(htmlContent, true);
            mailSender.send(message);
        } catch (MessagingException e) {
            throw new EmailException("Failed to send email", e);
        }
    }
    
    @Async
    public void sendEmailWithAttachment(String to, String subject, String text, 
                                       String attachmentPath, String attachmentName) {
        MimeMessage message = mailSender.createMimeMessage();
        try {
            MimeMessageHelper helper = new MimeMessageHelper(message, true);
            helper.setTo(to);
            helper.setSubject(subject);
            helper.setText(text);
            
            FileSystemResource file = new FileSystemResource(new File(attachmentPath));
            helper.addAttachment(attachmentName, file);
            
            mailSender.send(message);
        } catch (MessagingException e) {
            throw new EmailException("Failed to send email", e);
        }
    }
    
    @Async
    public void sendWelcomeEmail(String to, String name) {
        Context context = new Context();
        context.setVariable("name", name);
        String htmlContent = templateEngine.process("welcome-email", context);
        sendHtmlEmail(to, "Welcome!", htmlContent);
    }
}
```

## Best Practices

1. **Use Async**: Send emails asynchronously
2. **Use Templates**: Use templates for consistent emails
3. **Handle Exceptions**: Properly handle email exceptions
4. **Validate Email Addresses**: Validate before sending
5. **Use App Passwords**: Use app-specific passwords for Gmail
6. **Queue Emails**: Use message queue for high volume
7. **Log Email Sending**: Log email sending for debugging
8. **Test Email Configuration**: Test email configuration

## Summary

Spring Boot Email:
- Easy to configure and use
- Supports text and HTML emails
- Supports attachments
- Works with email templates
- Can send emails asynchronously
- Essential for notification systems

