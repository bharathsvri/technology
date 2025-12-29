# JNDI (Java Naming and Directory Interface)

## What is JNDI?
JNDI provides a unified interface for accessing naming and directory services.

## Basic Lookup
```java
import javax.naming.*;

public class JNDIExample {
    public static void main(String[] args) {
        try {
            Context context = new InitialContext();
            
            // Lookup DataSource
            DataSource dataSource = (DataSource) context.lookup("java:comp/env/jdbc/MyDB");
            
            // Lookup EJB
            UserService userService = (UserService) context.lookup("java:comp/env/ejb/UserService");
            
            // Lookup JMS ConnectionFactory
            ConnectionFactory connectionFactory = 
                (ConnectionFactory) context.lookup("java:comp/env/jms/ConnectionFactory");
            
        } catch (NamingException e) {
            e.printStackTrace();
        }
    }
}
```

## Binding Objects
```java
import javax.naming.*;

public class JNDIBinding {
    public static void main(String[] args) {
        try {
            Context context = new InitialContext();
            
            // Bind object
            context.bind("java:comp/env/myObject", myObject);
            
            // Rebind (replace if exists)
            context.rebind("java:comp/env/myObject", newObject);
            
            // Unbind
            context.unbind("java:comp/env/myObject");
            
        } catch (NamingException e) {
            e.printStackTrace();
        }
    }
}
```

## Environment Properties
```java
import javax.naming.*;

public class JNDIEnvironment {
    public static void main(String[] args) {
        try {
            Context context = new InitialContext();
            
            // Get environment property
            String dbUrl = (String) context.lookup("java:comp/env/database/url");
            
            // List all bindings
            NamingEnumeration<NameClassPair> list = context.list("");
            while (list.hasMore()) {
                NameClassPair pair = list.next();
                System.out.println(pair.getName() + ": " + pair.getClassName());
            }
            
        } catch (NamingException e) {
            e.printStackTrace();
        }
    }
}
```

## LDAP Example
```java
import javax.naming.*;
import javax.naming.directory.*;

public class LDAPExample {
    public static void main(String[] args) {
        Hashtable<String, String> env = new Hashtable<>();
        env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
        env.put(Context.PROVIDER_URL, "ldap://localhost:389");
        env.put(Context.SECURITY_AUTHENTICATION, "simple");
        env.put(Context.SECURITY_PRINCIPAL, "cn=admin,dc=example,dc=com");
        env.put(Context.SECURITY_CREDENTIALS, "password");
        
        try {
            DirContext context = new InitialDirContext(env);
            
            // Search
            String filter = "(cn=John Doe)";
            SearchControls controls = new SearchControls();
            controls.setSearchScope(SearchControls.SUBTREE_SCOPE);
            
            NamingEnumeration<SearchResult> results = 
                context.search("dc=example,dc=com", filter, controls);
            
            while (results.hasMore()) {
                SearchResult result = results.next();
                Attributes attrs = result.getAttributes();
                // Process attributes
            }
            
        } catch (NamingException e) {
            e.printStackTrace();
        }
    }
}
```

## Best Practices
1. Use appropriate context factory
2. Handle NamingException properly
3. Close contexts when done
4. Use connection pooling
5. Cache lookups when possible
6. Use environment variables for configuration
7. Secure credentials
8. Test with different providers

