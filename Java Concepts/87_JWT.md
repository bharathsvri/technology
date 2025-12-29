# JWT (JSON Web Tokens)

## What is JWT?
JWT is a compact, URL-safe token format for securely transmitting information.

## JWT Structure
```
Header.Payload.Signature
```

## Creating JWT
```java
import io.jsonwebtoken.*;
import io.jsonwebtoken.security.Keys;
import java.security.Key;
import java.util.Date;

public class JWTUtil {
    private static final Key key = Keys.secretKeyFor(SignatureAlgorithm.HS256);
    private static final long EXPIRATION_TIME = 86400000; // 24 hours
    
    public static String generateToken(String username, String role) {
        return Jwts.builder()
            .setSubject(username)
            .claim("role", role)
            .setIssuedAt(new Date())
            .setExpiration(new Date(System.currentTimeMillis() + EXPIRATION_TIME))
            .signWith(key)
            .compact();
    }
    
    public static Claims parseToken(String token) {
        return Jwts.parserBuilder()
            .setSigningKey(key)
            .build()
            .parseClaimsJws(token)
            .getBody();
    }
    
    public static boolean validateToken(String token) {
        try {
            Jwts.parserBuilder()
                .setSigningKey(key)
                .build()
                .parseClaimsJws(token);
            return true;
        } catch (JwtException e) {
            return false;
        }
    }
}
```

## Spring Security Integration
```java
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.authority.SimpleGrantedAuthority;

@Component
public class JWTTokenProvider {
    
    public Authentication getAuthentication(String token) {
        Claims claims = JWTUtil.parseToken(token);
        String username = claims.getSubject();
        String role = claims.get("role", String.class);
        
        List<SimpleGrantedAuthority> authorities = 
            Collections.singletonList(new SimpleGrantedAuthority("ROLE_" + role));
        
        return new UsernamePasswordAuthenticationToken(username, null, authorities);
    }
}
```

## REST Controller
```java
@RestController
@RequestMapping("/api/auth")
public class AuthController {
    
    @PostMapping("/login")
    public ResponseEntity<?> login(@RequestBody LoginRequest request) {
        // Authenticate user
        if (authenticate(request.getUsername(), request.getPassword())) {
            String token = JWTUtil.generateToken(request.getUsername(), "USER");
            return ResponseEntity.ok(new AuthResponse(token));
        }
        return ResponseEntity.status(HttpStatus.UNAUTHORIZED).build();
    }
    
    @GetMapping("/validate")
    public ResponseEntity<?> validate(@RequestHeader("Authorization") String token) {
        if (JWTUtil.validateToken(token.replace("Bearer ", ""))) {
            return ResponseEntity.ok().build();
        }
        return ResponseEntity.status(HttpStatus.UNAUTHORIZED).build();
    }
}
```

## Best Practices
1. Store secret key securely
2. Set appropriate expiration
3. Use HTTPS
4. Include user roles/permissions
5. Validate token on each request
6. Handle token refresh
7. Use strong signing algorithm
8. Don't store sensitive data in payload

