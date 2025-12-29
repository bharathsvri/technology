# Apache Commons

## What is Apache Commons?
Apache Commons provides reusable Java components and utilities.

## Commons Lang
```java
import org.apache.commons.lang3.*;

// String utilities
StringUtils.isEmpty(str);
StringUtils.isBlank(str);
StringUtils.defaultIfEmpty(str, "default");
StringUtils.abbreviate(str, 10);
StringUtils.reverse(str);

// Array utilities
ArrayUtils.contains(array, value);
ArrayUtils.reverse(array);
ArrayUtils.add(array, element);

// Object utilities
ObjectUtils.defaultIfNull(obj, defaultValue);
ObjectUtils.equals(obj1, obj2);
```

## Commons Collections
```java
import org.apache.commons.collections4.*;

// Collection utilities
CollectionUtils.isEmpty(collection);
CollectionUtils.isNotEmpty(collection);
CollectionUtils.filter(collection, predicate);
CollectionUtils.transform(collection, transformer);

// Map utilities
MapUtils.isEmpty(map);
MapUtils.getObject(map, key, defaultValue);
```

## Commons IO
```java
import org.apache.commons.io.*;

// File utilities
FileUtils.readFileToString(file, StandardCharsets.UTF_8);
FileUtils.writeStringToFile(file, content, StandardCharsets.UTF_8);
FileUtils.copyFile(source, target);
FileUtils.deleteQuietly(file);

// IOUtils
IOUtils.copy(inputStream, outputStream);
IOUtils.toString(inputStream, StandardCharsets.UTF_8);
IOUtils.toByteArray(inputStream);
```

## Commons Codec
```java
import org.apache.commons.codec.digest.*;
import org.apache.commons.codec.binary.*;

// Hashing
String md5 = DigestUtils.md5Hex("text");
String sha256 = DigestUtils.sha256Hex("text");

// Base64
String encoded = Base64.encodeBase64String("text".getBytes());
byte[] decoded = Base64.decodeBase64(encoded);
```

## Best Practices
1. Use appropriate Commons library
2. Check for null safety
3. Use latest versions
4. Understand performance implications
5. Use for common operations
6. Don't reinvent the wheel
7. Read documentation
8. Test thoroughly

