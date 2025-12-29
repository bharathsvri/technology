# JMX (Java Management Extensions)

## What is JMX?
JMX provides tools for managing and monitoring applications, system objects, and devices.

## MBean Interface
```java
public interface CalculatorMBean {
    int getResult();
    void add(int value);
    void subtract(int value);
    void reset();
}
```

## Standard MBean
```java
import javax.management.*;

public class Calculator implements CalculatorMBean {
    private int result = 0;
    
    @Override
    public int getResult() {
        return result;
    }
    
    @Override
    public void add(int value) {
        result += value;
    }
    
    @Override
    public void subtract(int value) {
        result -= value;
    }
    
    @Override
    public void reset() {
        result = 0;
    }
}
```

## Registering MBean
```java
import javax.management.*;

public class JMXServer {
    public static void main(String[] args) throws Exception {
        MBeanServer mbs = ManagementFactory.getPlatformMBeanServer();
        ObjectName name = new ObjectName("com.example:type=Calculator");
        Calculator mbean = new Calculator();
        mbs.registerMBean(mbean, name);
        
        System.out.println("MBean registered. Waiting...");
        Thread.sleep(Long.MAX_VALUE);
    }
}
```

## Dynamic MBean
```java
import javax.management.*;

public class DynamicCalculator implements DynamicMBean {
    private int result = 0;
    
    @Override
    public Object getAttribute(String attribute) throws Exception {
        if ("Result".equals(attribute)) {
            return result;
        }
        throw new AttributeNotFoundException();
    }
    
    @Override
    public void setAttribute(Attribute attribute) throws Exception {
        throw new AttributeNotFoundException();
    }
    
    @Override
    public AttributeList getAttributes(String[] attributes) {
        AttributeList list = new AttributeList();
        for (String attr : attributes) {
            try {
                list.add(new Attribute(attr, getAttribute(attr)));
            } catch (Exception e) {
                // Handle
            }
        }
        return list;
    }
    
    @Override
    public AttributeList setAttributes(AttributeList attributes) {
        return new AttributeList();
    }
    
    @Override
    public Object invoke(String actionName, Object[] params, String[] signature) 
            throws Exception {
        if ("add".equals(actionName)) {
            result += (Integer) params[0];
            return null;
        }
        throw new ReflectionException(new NoSuchMethodException());
    }
    
    @Override
    public MBeanInfo getMBeanInfo() {
        MBeanAttributeInfo[] attributes = new MBeanAttributeInfo[]{
            new MBeanAttributeInfo("Result", "int", "Current result", 
                true, false, false)
        };
        
        MBeanOperationInfo[] operations = new MBeanOperationInfo[]{
            new MBeanOperationInfo("add", "Add value", 
                new MBeanParameterInfo[]{new MBeanParameterInfo("value", "int", "Value to add")},
                "void", MBeanOperationInfo.ACTION)
        };
        
        return new MBeanInfo(this.getClass().getName(), "Calculator MBean",
            attributes, null, operations, null);
    }
}
```

## Monitoring with JConsole
```bash
# Start application with JMX
java -Dcom.sun.management.jmxremote \
     -Dcom.sun.management.jmxremote.port=9999 \
     -Dcom.sun.management.jmxremote.authenticate=false \
     -Dcom.sun.management.jmxremote.ssl=false \
     MyApp

# Connect with JConsole
jconsole localhost:9999
```

## Best Practices
1. Use MBeans for monitoring
2. Expose only necessary information
3. Secure JMX connections in production
4. Use appropriate MBean types
5. Document MBean operations
6. Handle exceptions properly
7. Monitor MBean performance
8. Use notifications for events

