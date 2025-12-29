# Serialization

## What is Serialization?
Serialization is the process of converting an object into a byte stream for storage or transmission. Deserialization is the reverse process.

## Serializable Interface
```java
import java.io.*;

class Person implements Serializable {
    private static final long serialVersionUID = 1L;
    private String name;
    private int age;
    private transient String password; // Not serialized
    
    public Person(String name, int age, String password) {
        this.name = name;
        this.age = age;
        this.password = password;
    }
    
    // Getters and setters
    public String getName() { return name; }
    public int getAge() { return age; }
    public String getPassword() { return password; }
}
```

## Serializing Objects
```java
import java.io.*;

Person person = new Person("John", 25, "secret123");

// Serialize
try (ObjectOutputStream oos = new ObjectOutputStream(
        new FileOutputStream("person.ser"))) {
    oos.writeObject(person);
    System.out.println("Object serialized");
} catch (IOException e) {
    e.printStackTrace();
}
```

## Deserializing Objects
```java
// Deserialize
try (ObjectInputStream ois = new ObjectInputStream(
        new FileInputStream("person.ser"))) {
    Person person = (Person) ois.readObject();
    System.out.println("Name: " + person.getName());
    System.out.println("Age: " + person.getAge());
    System.out.println("Password: " + person.getPassword()); // null (transient)
} catch (IOException | ClassNotFoundException e) {
    e.printStackTrace();
}
```

## Custom Serialization

### writeObject and readObject
```java
class CustomPerson implements Serializable {
    private String name;
    private int age;
    
    private void writeObject(ObjectOutputStream oos) throws IOException {
        oos.defaultWriteObject(); // Default serialization
        oos.writeObject(name.toUpperCase()); // Custom logic
    }
    
    private void readObject(ObjectInputStream ois) 
            throws IOException, ClassNotFoundException {
        ois.defaultReadObject(); // Default deserialization
        name = ((String) ois.readObject()).toLowerCase(); // Custom logic
    }
}
```

### Externalizable Interface
```java
class ExternalPerson implements Externalizable {
    private String name;
    private int age;
    
    public ExternalPerson() {
        // Required no-arg constructor
    }
    
    public ExternalPerson(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    @Override
    public void writeExternal(ObjectOutput out) throws IOException {
        out.writeUTF(name);
        out.writeInt(age);
    }
    
    @Override
    public void readExternal(ObjectInput in) 
            throws IOException, ClassNotFoundException {
        name = in.readUTF();
        age = in.readInt();
    }
}
```

## Serialization with Inheritance
```java
class Parent implements Serializable {
    private String parentField;
    
    public Parent(String parentField) {
        this.parentField = parentField;
    }
}

class Child extends Parent {
    private String childField;
    
    public Child(String parentField, String childField) {
        super(parentField);
        this.childField = childField;
    }
}
```

## Serial Version UID
```java
class Person implements Serializable {
    // Explicit serialVersionUID prevents InvalidClassException
    private static final long serialVersionUID = 1L;
    
    private String name;
    // Adding new fields won't break deserialization if UID matches
    private String email; // New field
}
```

## Transient Keyword
Fields marked as transient are not serialized.

```java
class User implements Serializable {
    private String username;
    private transient String password; // Not serialized
    private transient Date lastLogin; // Not serialized
    
    // Sensitive data should be transient
}
```

## JSON Serialization (Jackson)
```java
import com.fasterxml.jackson.databind.ObjectMapper;

class Person {
    private String name;
    private int age;
    
    // Getters and setters required
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
}

// Serialize to JSON
ObjectMapper mapper = new ObjectMapper();
Person person = new Person();
person.setName("John");
person.setAge(25);

String json = mapper.writeValueAsString(person);
// {"name":"John","age":25}

// Deserialize from JSON
Person deserialized = mapper.readValue(json, Person.class);
```

## XML Serialization (JAXB)
```java
import javax.xml.bind.annotation.*;

@XmlRootElement
@XmlAccessorType(XmlAccessType.FIELD)
class Person {
    @XmlElement
    private String name;
    
    @XmlElement
    private int age;
    
    // Getters and setters
}

// Serialize to XML
JAXBContext context = JAXBContext.newInstance(Person.class);
Marshaller marshaller = context.createMarshaller();
marshaller.marshal(person, new File("person.xml"));

// Deserialize from XML
Unmarshaller unmarshaller = context.createUnmarshaller();
Person person = (Person) unmarshaller.unmarshal(new File("person.xml"));
```

## Best Practices
1. Always declare serialVersionUID
2. Mark sensitive fields as transient
3. Implement custom serialization for complex objects
4. Handle version compatibility
5. Consider security implications
6. Use JSON/XML for cross-platform compatibility
7. Validate deserialized data
8. Be careful with serialization of large objects

