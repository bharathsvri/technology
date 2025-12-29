# Date and Time API

## Overview
Java 8 introduced a new Date and Time API (java.time package) to replace the old Date and Calendar classes, which had design issues.

## LocalDate
Represents a date without time.

```java
import java.time.LocalDate;

// Current date
LocalDate today = LocalDate.now();
System.out.println(today); // 2024-01-15

// Specific date
LocalDate date = LocalDate.of(2024, 1, 15);
LocalDate date2 = LocalDate.parse("2024-01-15");

// Get components
int year = date.getYear();
int month = date.getMonthValue();
int day = date.getDayOfMonth();
DayOfWeek dayOfWeek = date.getDayOfWeek();

// Manipulate dates
LocalDate tomorrow = today.plusDays(1);
LocalDate nextWeek = today.plusWeeks(1);
LocalDate nextMonth = today.plusMonths(1);
LocalDate nextYear = today.plusYears(1);

LocalDate yesterday = today.minusDays(1);
```

## LocalTime
Represents a time without date.

```java
import java.time.LocalTime;

// Current time
LocalTime now = LocalTime.now();
System.out.println(now); // 14:30:45.123

// Specific time
LocalTime time = LocalTime.of(14, 30, 45);
LocalTime time2 = LocalTime.parse("14:30:45");

// Get components
int hour = time.getHour();
int minute = time.getMinute();
int second = time.getSecond();

// Manipulate time
LocalTime later = time.plusHours(2);
LocalTime earlier = time.minusMinutes(30);
```

## LocalDateTime
Represents both date and time.

```java
import java.time.LocalDateTime;

// Current date and time
LocalDateTime now = LocalDateTime.now();

// Specific date and time
LocalDateTime dateTime = LocalDateTime.of(2024, 1, 15, 14, 30);
LocalDateTime dateTime2 = LocalDateTime.parse("2024-01-15T14:30:00");

// From LocalDate and LocalTime
LocalDate date = LocalDate.of(2024, 1, 15);
LocalTime time = LocalTime.of(14, 30);
LocalDateTime dateTime3 = LocalDateTime.of(date, time);

// Manipulate
LocalDateTime later = dateTime.plusDays(1).plusHours(2);
```

## ZonedDateTime
Date and time with timezone.

```java
import java.time.ZonedDateTime;
import java.time.ZoneId;

// Current date and time with timezone
ZonedDateTime zonedNow = ZonedDateTime.now();

// Specific timezone
ZonedDateTime zoned = ZonedDateTime.now(ZoneId.of("America/New_York"));

// From LocalDateTime
LocalDateTime localDateTime = LocalDateTime.now();
ZonedDateTime zoned2 = localDateTime.atZone(ZoneId.of("UTC"));

// Convert timezone
ZonedDateTime tokyo = zoned.withZoneSameInstant(ZoneId.of("Asia/Tokyo"));
```

## Instant
Represents a point in time on UTC timeline.

```java
import java.time.Instant;

// Current instant
Instant now = Instant.now();

// From epoch seconds
Instant instant = Instant.ofEpochSecond(1609459200);

// To epoch seconds
long epochSeconds = instant.getEpochSecond();

// Convert to LocalDateTime
LocalDateTime localDateTime = LocalDateTime.ofInstant(
    instant, ZoneId.systemDefault());
```

## Duration
Represents a time-based amount of time.

```java
import java.time.Duration;

LocalTime start = LocalTime.of(9, 0);
LocalTime end = LocalTime.of(17, 30);
Duration duration = Duration.between(start, end);

System.out.println(duration.toHours()); // 8
System.out.println(duration.toMinutes()); // 510

// Create duration
Duration duration2 = Duration.ofHours(2);
Duration duration3 = Duration.ofMinutes(30);
```

## Period
Represents a date-based amount of time.

```java
import java.time.Period;

LocalDate start = LocalDate.of(2024, 1, 1);
LocalDate end = LocalDate.of(2024, 12, 31);
Period period = Period.between(start, end);

System.out.println(period.getMonths()); // 11
System.out.println(period.getDays()); // 30

// Create period
Period period2 = Period.ofYears(1);
Period period3 = Period.ofMonths(6);
Period period4 = Period.ofDays(30);
```

## DateTimeFormatter
Format and parse dates.

```java
import java.time.format.DateTimeFormatter;

LocalDate date = LocalDate.of(2024, 1, 15);

// Predefined formatters
String formatted1 = date.format(DateTimeFormatter.ISO_DATE);
String formatted2 = date.format(DateTimeFormatter.ISO_LOCAL_DATE);

// Custom formatter
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("dd/MM/yyyy");
String formatted3 = date.format(formatter); // "15/01/2024"

// Parse
LocalDate parsed = LocalDate.parse("15/01/2024", formatter);
```

## Common Patterns
```java
// Pattern examples
DateTimeFormatter.ofPattern("dd-MM-yyyy");      // 15-01-2024
DateTimeFormatter.ofPattern("MMM dd, yyyy");    // Jan 15, 2024
DateTimeFormatter.ofPattern("EEEE, MMMM dd");   // Monday, January 15
DateTimeFormatter.ofPattern("HH:mm:ss");        // 14:30:45
DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm"); // 2024-01-15 14:30
```

## Comparing Dates
```java
LocalDate date1 = LocalDate.of(2024, 1, 15);
LocalDate date2 = LocalDate.of(2024, 2, 15);

boolean isBefore = date1.isBefore(date2); // true
boolean isAfter = date1.isAfter(date2);   // false
boolean isEqual = date1.isEqual(date2);   // false

int comparison = date1.compareTo(date2); // negative
```

## Complete Example
```java
import java.time.*;
import java.time.format.DateTimeFormatter;

public class DateTimeDemo {
    public static void main(String[] args) {
        // Current date
        LocalDate today = LocalDate.now();
        System.out.println("Today: " + today);
        
        // Specific date
        LocalDate birthday = LocalDate.of(1990, 5, 15);
        System.out.println("Birthday: " + birthday);
        
        // Calculate age
        Period age = Period.between(birthday, today);
        System.out.println("Age: " + age.getYears() + " years");
        
        // Current time
        LocalTime now = LocalTime.now();
        System.out.println("Current time: " + now);
        
        // Date and time
        LocalDateTime dateTime = LocalDateTime.now();
        System.out.println("Date and time: " + dateTime);
        
        // Formatting
        DateTimeFormatter formatter = 
            DateTimeFormatter.ofPattern("dd MMM yyyy HH:mm");
        String formatted = dateTime.format(formatter);
        System.out.println("Formatted: " + formatted);
        
        // Timezone
        ZonedDateTime zoned = ZonedDateTime.now(ZoneId.of("America/New_York"));
        System.out.println("New York time: " + zoned);
        
        // Duration
        LocalTime start = LocalTime.of(9, 0);
        LocalTime end = LocalTime.of(17, 30);
        Duration workDuration = Duration.between(start, end);
        System.out.println("Work hours: " + workDuration.toHours());
    }
}
```

## Converting Old API to New API
```java
import java.util.Date;
import java.util.Calendar;

// Old Date to LocalDateTime
Date oldDate = new Date();
LocalDateTime newDateTime = LocalDateTime.ofInstant(
    oldDate.toInstant(), ZoneId.systemDefault());

// Calendar to LocalDateTime
Calendar calendar = Calendar.getInstance();
LocalDateTime newDateTime2 = LocalDateTime.ofInstant(
    calendar.toInstant(), ZoneId.systemDefault());

// LocalDateTime to Date
LocalDateTime localDateTime = LocalDateTime.now();
Date date = Date.from(
    localDateTime.atZone(ZoneId.systemDefault()).toInstant());
```

## Best Practices
1. Use LocalDate for dates only
2. Use LocalTime for times only
3. Use LocalDateTime when you need both
4. Use ZonedDateTime for timezone-aware operations
5. Use Instant for timestamps
6. Use Duration for time-based amounts
7. Use Period for date-based amounts
8. Always specify timezone explicitly when needed

