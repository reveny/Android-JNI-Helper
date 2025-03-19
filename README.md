# Android-JNI-Helper
A header-only C++ library that makes working with the Java Native Interface (JNI) safer and more convenient.

## Overview

Working with JNI directly can be error-prone and verbose. This library wraps the raw JNI calls with modern C++ abstractions to provide:

- Exception safety with automatic error checking
- RAII-based resource management
- Simplified string conversion
- Cleaner syntax for common JNI operations

## Features

- **Exception Handling**: Automatic JNI exception checking and propagation to C++ exceptions
- **Safe Resource Management**: RAII wrapper for JNI references to prevent leaks (`ScopedLocalRef`)
- **Type-Safe Calls**: Template-based method calls with proper type handling
- **String Conversion**: Easy conversion between Java and C++ strings
- **Simplified API**: Readable wrapper functions for common JNI operations
- **Support for All Basic Types**: Complete implementations for all Java primitive types and object references

## Installation

This is a header-only library. Simply include the header in your Android NDK project:

```cpp
#include "JniHelper.hpp"
```
Make sure to place the header file in your project's include path.

## Usage Examples

### Basic Method Call
```cpp
// Call a Java method from C++
jstring result = jni::CallMethod<jstring>(env, javaObj, "getMessage", "()Ljava/lang/String;");
std::string cppString = jni::JStringToString(env, result);
```

### Static Method Call
```cpp
// Call a static Java method
jint value = jni::CallStaticMethod<jint>(env, "com/example/Utils", "calculateValue", "(I)I", 42);
```

### Creating Java Objects
```cpp
// Create a new Java object
jobject newObj = jni::NewObject(env, "java/lang/StringBuilder", "()V");
jni::CallMethod<void>(env, newObj, "append", "(Ljava/lang/String;)Ljava/lang/StringBuilder;", "Hello, JNI!");
jstring result = jni::CallMethod<jstring>(env, newObj, "toString", "()Ljava/lang/String;");
```

### Field Access
```cpp
// Get field values
jint count = jni::GetField<jint>(env, javaObj, "count");
jstring name = jni::GetField<jstring>(env, javaObj, "name", "Ljava/lang/String;");

// Get static field value
jlong timeout = jni::GetStaticField<jlong>(env, "com/example/Config", "TIMEOUT_MS");
```

### Safe Resource Management
```cpp
// Automatically manage JNI local references
{
    jclass cls = env->FindClass("java/lang/String");
    jni::ScopedLocalRef<jclass> safeClass(env, cls); // Will be released automatically
    
    // Use safeClass.get() to access the reference
    jmethodID mid = env->GetMethodID(safeClass.get(), "length", "()I");
}
// No need to call DeleteLocalRef, it's handled by ScopedLocalRef's destructor
```

### Exception Handling
```cpp
try {
    // This will automatically check for JNI exceptions
    jni::CallMethod<void>(env, javaObj, "methodThatMightThrow", "()V");
} catch (const jni::JNIException& e) {
    std::cerr << "Java exception occurred: " << e.what() << std::endl;
}
```

## API Reference

### Exception Handling

- `JNIException`: Custom exception class for JNI errors
- `JNI_CHECK_EXCEPTION`: Macro to check and throw JNI exceptions

### Resource Management

- `ScopedLocalRef<T>`: RAII wrapper for JNI local references

### String Operations

- `JStringToString(JNIEnv*, jstring)`: Convert a Java string to C++ string
- `StringToJString(JNIEnv*, const std::string&)`: Convert a C++ string to Java string

### Class and Method Operations

- `FindClass(JNIEnv*, const char*)`: Find a Java class with exception checking
- `GetMethodID(JNIEnv*, jclass, const char*, const char*)`: Get a method ID with exception checking
- `GetStaticMethodID(JNIEnv*, jclass, const char*, const char*)`: Get a static method ID with exception checking

### Field Operations

- `GetFieldID(JNIEnv*, jclass, const char*, const char*)`: Get a field ID with exception checking
- `GetStaticFieldID(JNIEnv*, jclass, const char*, const char*)`: Get a static field ID with exception checking
- `GetField<T>(JNIEnv*, jobject, const char*, const char*)`: Get field value with type safety
- `GetStaticField<T>(JNIEnv*, const char*, const char*, const char*)`: Get static field value with type safety

### Method Calling

- `CallMethod<ReturnType, Args...>(JNIEnv*, jobject, const char*, const char*, Args...)`: Call instance methods
- `CallStaticMethod<ReturnType, Args...>(JNIEnv*, const char*, const char*, const char*, Args...)`: Call static methods
- `NewObject<Args...>(JNIEnv*, const char*, const char*, Args...)`: Create new Java objects

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Contact
For any questions, collaboration requests, or updates, feel free to reach out via:

Telegram Channel: [Join Channel](https://t.me/reveny1) <br>
Telegram Contact: [Contact Me](https://t.me/revenyy) <br>
Website: [My Website](https://reveny.me) <br>
Email: [contact@reveny.me](mailto:contact@reveny.me) <br>

## Support Me
If you'd like to, you can support me on my [Website](https://reveny.me/donate.html)

## Version History

- **1.0.0** (2025-03-19): Initial release

## License

MIT License

---
