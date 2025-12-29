# Internationalization (i18n)

## What is Internationalization?
Internationalization (i18n) is the process of designing applications to support multiple languages and regions.

## Locale
Represents a specific geographical, political, or cultural region.

```java
import java.util.Locale;

// Create locales
Locale us = new Locale("en", "US");
Locale fr = new Locale("fr", "FR");
Locale jp = new Locale("ja", "JP");

// Using constants
Locale english = Locale.ENGLISH;
Locale french = Locale.FRENCH;
Locale japanese = Locale.JAPANESE;

// Get default locale
Locale defaultLocale = Locale.getDefault();

// Set default locale
Locale.setDefault(new Locale("fr", "FR"));
```

## ResourceBundle
Stores locale-specific data.

### Properties Files
```properties
# messages.properties (default)
greeting=Hello
farewell=Goodbye
welcome=Welcome to our application

# messages_fr.properties (French)
greeting=Bonjour
farewell=Au revoir
welcome=Bienvenue dans notre application

# messages_es.properties (Spanish)
greeting=Hola
farewell=Adiós
welcome=Bienvenido a nuestra aplicación
```

### Using ResourceBundle
```java
import java.util.ResourceBundle;
import java.util.Locale;

// Load bundle
ResourceBundle bundle = ResourceBundle.getBundle("messages", Locale.FRENCH);

// Get values
String greeting = bundle.getString("greeting");
String farewell = bundle.getString("farewell");
String welcome = bundle.getString("welcome");

System.out.println(greeting); // "Bonjour"
```

### ResourceBundle with Locale
```java
Locale locale = new Locale("fr", "FR");
ResourceBundle bundle = ResourceBundle.getBundle("messages", locale);

// Fallback to default if French not found
String value = bundle.getString("key");
```

## Number Formatting
```java
import java.text.NumberFormat;
import java.util.Locale;

double number = 1234567.89;

// US format
NumberFormat usFormat = NumberFormat.getNumberInstance(Locale.US);
System.out.println(usFormat.format(number)); // "1,234,567.89"

// French format
NumberFormat frFormat = NumberFormat.getNumberInstance(Locale.FRENCH);
System.out.println(frFormat.format(number)); // "1 234 567,89"

// Currency format
NumberFormat currency = NumberFormat.getCurrencyInstance(Locale.US);
System.out.println(currency.format(number)); // "$1,234,567.89"
```

## Date Formatting
```java
import java.text.DateFormat;
import java.util.Date;
import java.util.Locale;

Date date = new Date();

// US format
DateFormat usFormat = DateFormat.getDateInstance(DateFormat.FULL, Locale.US);
System.out.println(usFormat.format(date));

// French format
DateFormat frFormat = DateFormat.getDateInstance(DateFormat.FULL, Locale.FRENCH);
System.out.println(frFormat.format(date));

// DateTime format
DateFormat dateTime = DateFormat.getDateTimeInstance(
    DateFormat.FULL, DateFormat.FULL, Locale.US);
System.out.println(dateTime.format(date));
```

## MessageFormat
Format messages with parameters.

```java
import java.text.MessageFormat;
import java.util.Locale;
import java.util.ResourceBundle;

ResourceBundle bundle = ResourceBundle.getBundle("messages", Locale.US);
String pattern = bundle.getString("welcome.message");
// Pattern: "Welcome {0}, you have {1} messages"

String message = MessageFormat.format(pattern, "John", 5);
System.out.println(message); // "Welcome John, you have 5 messages"
```

## Complete Example
```java
import java.util.*;
import java.text.*;

public class I18nDemo {
    public static void main(String[] args) {
        // Set locale
        Locale locale = new Locale("fr", "FR");
        Locale.setDefault(locale);
        
        // Load resource bundle
        ResourceBundle bundle = ResourceBundle.getBundle("messages", locale);
        
        // Get messages
        String greeting = bundle.getString("greeting");
        System.out.println(greeting);
        
        // Format numbers
        NumberFormat numberFormat = NumberFormat.getNumberInstance(locale);
        System.out.println(numberFormat.format(1234567.89));
        
        // Format currency
        NumberFormat currencyFormat = NumberFormat.getCurrencyInstance(locale);
        System.out.println(currencyFormat.format(1234.56));
        
        // Format dates
        DateFormat dateFormat = DateFormat.getDateInstance(DateFormat.FULL, locale);
        System.out.println(dateFormat.format(new Date()));
        
        // Message formatting
        String pattern = bundle.getString("welcome");
        String message = MessageFormat.format(pattern, "John");
        System.out.println(message);
    }
}
```

## Best Practices
1. Externalize all user-visible strings
2. Use ResourceBundle for translations
3. Test with different locales
4. Handle missing translations gracefully
5. Consider text direction (RTL languages)
6. Format numbers and dates appropriately
7. Use MessageFormat for parameterized messages
8. Store locale preferences per user

