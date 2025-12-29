# JNI (Java Native Interface)

## What is JNI?
JNI allows Java code to call native code (C/C++) and vice versa.

## Creating Native Method
```java
public class NativeExample {
    // Declare native method
    public native void printMessage();
    public native int add(int a, int b);
    public native String getString();
    
    // Load native library
    static {
        System.loadLibrary("NativeExample");
    }
    
    public static void main(String[] args) {
        NativeExample example = new NativeExample();
        example.printMessage();
        int sum = example.add(5, 3);
        System.out.println("Sum: " + sum);
    }
}
```

## Generating Header File
```bash
javac NativeExample.java
javah -jni NativeExample
# Creates NativeExample.h
```

## Native Implementation (C)
```c
#include <jni.h>
#include "NativeExample.h"
#include <stdio.h>

JNIEXPORT void JNICALL
Java_NativeExample_printMessage(JNIEnv *env, jobject obj) {
    printf("Hello from native code!\n");
}

JNIEXPORT jint JNICALL
Java_NativeExample_add(JNIEnv *env, jobject obj, jint a, jint b) {
    return a + b;
}

JNIEXPORT jstring JNICALL
Java_NativeExample_getString(JNIEnv *env, jobject obj) {
    return (*env)->NewStringUTF(env, "Hello from C");
}
```

## Compiling Native Library
```bash
# Linux
gcc -shared -fpic -o libNativeExample.so NativeExample.c -I$JAVA_HOME/include -I$JAVA_HOME/include/linux

# Windows
cl /LD NativeExample.c /I"%JAVA_HOME%\include" /I"%JAVA_HOME%\include\win32"

# macOS
gcc -shared -fpic -o libNativeExample.dylib NativeExample.c -I$JAVA_HOME/include -I$JAVA_HOME/include/darwin
```

## Best Practices
1. Use JNI only when necessary
2. Minimize JNI calls (overhead)
3. Handle exceptions properly
4. Release native resources
5. Be careful with memory management
6. Test thoroughly
7. Document native methods
8. Consider alternatives (JNA, GraalVM)

