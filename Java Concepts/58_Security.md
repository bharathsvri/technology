# Java Security

## Security Manager
Controls what operations code can perform.

```java
SecurityManager manager = System.getSecurityManager();
if (manager != null) {
    manager.checkPermission(new FilePermission("/path/to/file", "read"));
}
```

## Permissions

### File Permissions
```java
import java.io.FilePermission;

FilePermission permission = new FilePermission("/path/to/file", "read,write");
```

### Socket Permissions
```java
import java.net.SocketPermission;

SocketPermission permission = 
    new SocketPermission("www.example.com:80", "connect,resolve");
```

### Property Permissions
```java
import java.util.PropertyPermission;

PropertyPermission permission = 
    new PropertyPermission("java.home", "read");
```

## Cryptography

### Message Digest
```java
import java.security.MessageDigest;

MessageDigest md = MessageDigest.getInstance("SHA-256");
byte[] data = "Hello World".getBytes();
byte[] hash = md.digest(data);

// Convert to hex
StringBuilder hexString = new StringBuilder();
for (byte b : hash) {
    String hex = Integer.toHexString(0xff & b);
    if (hex.length() == 1) {
        hexString.append('0');
    }
    hexString.append(hex);
}
System.out.println(hexString.toString());
```

### Encryption/Decryption
```java
import javax.crypto.Cipher;
import javax.crypto.KeyGenerator;
import javax.crypto.SecretKey;
import java.security.SecureRandom;

// Generate key
KeyGenerator keyGenerator = KeyGenerator.getInstance("AES");
keyGenerator.init(256);
SecretKey key = keyGenerator.generateKey();

// Encrypt
Cipher cipher = Cipher.getInstance("AES");
cipher.init(Cipher.ENCRYPT_MODE, key);
byte[] encrypted = cipher.doFinal("Secret message".getBytes());

// Decrypt
cipher.init(Cipher.DECRYPT_MODE, key);
byte[] decrypted = cipher.doFinal(encrypted);
String message = new String(decrypted);
```

## Secure Random
```java
import java.security.SecureRandom;

SecureRandom random = new SecureRandom();
byte[] bytes = new byte[16];
random.nextBytes(bytes);

// Generate random int
int randomInt = random.nextInt();
```

## Password Hashing
```java
import java.security.MessageDigest;
import java.security.SecureRandom;
import java.util.Base64;

class PasswordHasher {
    public static String hashPassword(String password, byte[] salt) 
            throws Exception {
        MessageDigest md = MessageDigest.getInstance("SHA-256");
        md.update(salt);
        byte[] hash = md.digest(password.getBytes());
        return Base64.getEncoder().encodeToString(hash);
    }
    
    public static byte[] generateSalt() {
        SecureRandom random = new SecureRandom();
        byte[] salt = new byte[16];
        random.nextBytes(salt);
        return salt;
    }
}
```

## Best Practices
1. Never store passwords in plain text
2. Use strong encryption algorithms
3. Use SecureRandom for random numbers
4. Validate and sanitize input
5. Use HTTPS for network communication
6. Keep dependencies updated
7. Follow principle of least privilege
8. Audit security regularly
9. Use security manager when appropriate
10. Encrypt sensitive data

## Input Validation
```java
public void processInput(String input) {
    // Validate input
    if (input == null || input.trim().isEmpty()) {
        throw new IllegalArgumentException("Input cannot be empty");
    }
    
    // Sanitize input
    String sanitized = input.replaceAll("[^a-zA-Z0-9]", "");
    
    // Check length
    if (sanitized.length() > 100) {
        throw new IllegalArgumentException("Input too long");
    }
}
```

## Complete Example
```java
import java.security.*;
import javax.crypto.*;
import java.util.Base64;

public class SecurityDemo {
    public static void main(String[] args) {
        try {
            // Generate key
            KeyGenerator keyGen = KeyGenerator.getInstance("AES");
            keyGen.init(256);
            SecretKey key = keyGen.generateKey();
            
            // Encrypt
            Cipher cipher = Cipher.getInstance("AES");
            cipher.init(Cipher.ENCRYPT_MODE, key);
            String plaintext = "Secret message";
            byte[] encrypted = cipher.doFinal(plaintext.getBytes());
            String encryptedBase64 = Base64.getEncoder().encodeToString(encrypted);
            System.out.println("Encrypted: " + encryptedBase64);
            
            // Decrypt
            cipher.init(Cipher.DECRYPT_MODE, key);
            byte[] decrypted = cipher.doFinal(Base64.getDecoder().decode(encryptedBase64));
            System.out.println("Decrypted: " + new String(decrypted));
            
            // Hash
            MessageDigest md = MessageDigest.getInstance("SHA-256");
            byte[] hash = md.digest(plaintext.getBytes());
            String hashBase64 = Base64.getEncoder().encodeToString(hash);
            System.out.println("Hash: " + hashBase64);
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

