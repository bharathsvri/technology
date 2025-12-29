# JAXB (Java Architecture for XML Binding)

## What is JAXB?
JAXB provides mapping between Java objects and XML documents.

## Annotating Classes
```java
import javax.xml.bind.annotation.*;

@XmlRootElement(name = "person")
@XmlAccessorType(XmlAccessType.FIELD)
public class Person {
    @XmlElement(name = "name")
    private String name;
    
    @XmlElement(name = "age")
    private int age;
    
    @XmlAttribute(name = "id")
    private Long id;
    
    // Constructors, getters, setters
    public Person() {}
    
    public Person(String name, int age, Long id) {
        this.name = name;
        this.age = age;
        this.id = id;
    }
    
    // Getters and setters
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
}
```

## Marshalling (Java to XML)
```java
import javax.xml.bind.*;

public class JAXBExample {
    public static void main(String[] args) {
        try {
            Person person = new Person("John", 25, 1L);
            
            JAXBContext context = JAXBContext.newInstance(Person.class);
            Marshaller marshaller = context.createMarshaller();
            marshaller.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            
            // Marshal to file
            marshaller.marshal(person, new File("person.xml"));
            
            // Marshal to string
            StringWriter writer = new StringWriter();
            marshaller.marshal(person, writer);
            String xml = writer.toString();
            System.out.println(xml);
        } catch (JAXBException e) {
            e.printStackTrace();
        }
    }
}
```

## Unmarshalling (XML to Java)
```java
import javax.xml.bind.*;

public class JAXBUnmarshal {
    public static void main(String[] args) {
        try {
            JAXBContext context = JAXBContext.newInstance(Person.class);
            Unmarshaller unmarshaller = context.createUnmarshaller();
            
            // Unmarshal from file
            Person person = (Person) unmarshaller.unmarshal(
                new File("person.xml"));
            
            System.out.println("Name: " + person.getName());
            System.out.println("Age: " + person.getAge());
        } catch (JAXBException e) {
            e.printStackTrace();
        }
    }
}
```

## Collections
```java
@XmlRootElement
@XmlAccessorType(XmlAccessType.FIELD)
public class People {
    @XmlElement(name = "person")
    private List<Person> people = new ArrayList<>();
    
    public List<Person> getPeople() {
        return people;
    }
    
    public void setPeople(List<Person> people) {
        this.people = people;
    }
}
```

## Best Practices
1. Use appropriate annotations
2. Handle namespaces properly
3. Validate XML when unmarshalling
4. Use adapters for complex types
5. Handle exceptions properly
6. Consider using JAXB with JAX-RS
7. Use schemas for validation
8. Document XML structure

