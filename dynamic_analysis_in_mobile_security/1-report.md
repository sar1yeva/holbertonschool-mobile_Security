# Task 1 — Android Native Function Hooking

## 1. Assessment Objective

The purpose of this exercise was to perform dynamic analysis against an Android application that uses native code through the Java Native Interface (JNI).

The application contains a native function named `getSecretMessage`. The function processes an obfuscated value inside native code and returns the resulting string to the Java layer. However, the resulting secret is not directly exposed through the application's user interface.

The objective of the analysis was therefore to:

* identify the native library used by the application;
* locate the JNI implementation of `getSecretMessage`;
* determine the corresponding native symbol;
* instrument the function at runtime with Frida;
* capture its return value;
* recover the hidden flag;
* validate the result using an additional dynamic-analysis approach.

---

## 2. Tools and Environment

The following tools were used during the analysis:

| Tool                      | Purpose                                             |
| ------------------------- | --------------------------------------------------- |
| ADB                       | Application installation and device interaction     |
| Frida                     | Runtime instrumentation and native function hooking |
| Objection                 | Additional runtime inspection and hook validation   |
| Android environment       | Execution of the target application                 |
| Native ELF analysis tools | Inspection of exported symbols and native libraries |

Target application:

```text
APK: task1_d.apk
Native library: libnative-lib.so
Architecture: Android
```

---

## 3. Application Reconnaissance

The APK was first installed on the Android environment using ADB:

```bash
adb install task1_d.apk
```

After installation, the application was launched and its interface was examined.

The application provides only a minimal interface and does not display the secret value directly. This indicated that the required information was likely being generated or processed internally rather than stored as a visible UI element.

The application was therefore investigated from the native-code perspective.

---

## 4. Identifying the Native Library

The APK was inspected for native shared libraries. The application contains:

```text
libnative-lib.so
```

The library was available for multiple Android architectures, including:

```text
arm64-v8a
armeabi-v7a
x86
x86_64
```

The Java-side application code loads the library through the standard JNI mechanism:

```java
System.loadLibrary("native-lib");
```

This confirmed that functionality implemented in `libnative-lib.so` is exposed to the Java application through JNI.

---

## 5. Locating the JNI Function

The next step was to determine which native symbol corresponds to the Java method `getSecretMessage`.

The native library was inspected for exported JNI symbols. The relevant function was identified as the JNI implementation of:

```text
getSecretMessage
```

The corresponding JNI naming convention follows the pattern:

```text
Java_<package>_<class>_<method>
```

The exported symbol associated with the target method was identified as:

```text
Java_com_holberton_task2_1d_MainActivity_getSecretMessage
```

The library also contained another exported native function:

```text
lit
```

The presence of the JNI export provided a direct entry point for runtime instrumentation.

---

## 6. Dynamic Instrumentation with Frida

Instead of attempting to recover the secret solely through static analysis, the native function was instrumented while the application was executing.

A Frida hook was created for the exported JNI function. The main purpose of the hook was to inspect the value returned by `getSecretMessage`.

The important part of the instrumentation was the use of:

```javascript
Interceptor.attach()
```

A simplified version of the hook logic is:

```javascript
const address = Module.findExportByName(
    "libnative-lib.so",
    "Java_com_holberton_task2_1d_MainActivity_getSecretMessage"
);

Interceptor.attach(address, {
    onEnter(args) {
        console.log("[+] getSecretMessage() called");
    },

    onLeave(retval) {
        console.log("[+] Return value:", retval);
    }
});
```

Because the JNI function returns a Java `String` object rather than a plain native C string, the return value was further processed through the JNI environment.

The returned `jstring` was converted into a UTF-8 string so that the actual plaintext value could be observed.

---

## 7. Executing the Hook

The application was started under Frida with:

```bash
frida -U -f com.holberton.task2_d -l hook_getSecretMessage.js --no-pause
```

The relevant options are:

* `-U` — connect to the USB/Android device;
* `-f` — spawn the target application;
* `-l` — load the custom JavaScript instrumentation script;
* `--no-pause` — allow the application to continue execution after spawning.

When the application invoked `getSecretMessage`, the Frida interceptor was triggered.

This provided visibility into a value that was otherwise unavailable through the normal application interface.

---

## 8. Recovering the Native Return Value

The return value from the JNI function was handled as a `jstring`.

The JNI environment was used to obtain the underlying UTF-8 representation of the returned string. Conceptually, the extraction process was:

```text
getSecretMessage()
        |
        v
JNI function
        |
        v
jstring return value
        |
        v
JNI string conversion
        |
        v
Plain-text secret
```

The important observation was that the decrypted value existed in memory at runtime even though it was not rendered by the application.

This is one of the main advantages of dynamic instrumentation: rather than reconstructing every transformation manually, the analyst can observe the value at the point where it becomes available to the application.

---

## 9. Static Analysis Correlation

The runtime result was also correlated with the native implementation.

Inspection of the native code showed that the secret was not stored directly as a readable plaintext flag. Instead, an obfuscated byte sequence was processed before being returned.

The native implementation used the `lit` function during the transformation.

The relevant key sequence was derived from a Fibonacci-based calculation:

```text
0, 1, 1, 2, 3, 5, 8, 13, 21, 34
```

The native routine used these values to transform the encoded data.

This explained why searching the binary for the final flag string did not directly reveal the plaintext value.

The dynamic hook therefore provided a more reliable extraction point: the analysis intercepted the function after the native processing had produced the final string.

---

## 10. Runtime Validation with Objection

As an additional validation step, Objection was used to inspect the application's runtime behavior.

The application was opened through:

```bash
objection -g com.holberton.task2_d explore
```

The Java method was then monitored with:

```text
android hooking watch class_method com.holberton.task2_d.MainActivity.getSecretMessage --dump-return
```

This provided an additional observation point at the Java/JNI boundary and confirmed that the `getSecretMessage` method returned the secret value during execution.

Using both Frida and Objection helped confirm that the recovered value was not simply an artifact of static analysis.

---

## 11. Extracted Flag

The final plaintext returned by the native function was:

```text
Holberton{native_hooking_is_no_different_at_all}
```

The value was recovered at runtime from the native function rather than from the application's visible interface.

---

## 12. Technical Findings

The analysis demonstrated the following:

1. The application relies on a native shared library for part of its functionality.
2. The native library exposes a JNI implementation of `getSecretMessage`.
3. The secret is processed inside native code before being returned.
4. The plaintext value can be observed at runtime using Frida.
5. JNI return values can be inspected by accessing the application's JNI environment.
6. Objection can be used as a complementary technique for observing Java-level method calls and return values.
7. Native code does not inherently prevent extraction of sensitive runtime data when an analyst has control over the application's execution environment.

---

## 13. Security Relevance

This exercise demonstrates an important limitation of client-side secret protection.

Obfuscating or transforming a secret inside native code can make static extraction more difficult, but it does not guarantee confidentiality. If the application must eventually decrypt or reconstruct the value locally, the plaintext will exist in memory during execution.

An attacker with sufficient control over the device can potentially:

* instrument native functions;
* intercept function arguments;
* inspect return values;
* trace JNI calls;
* dump relevant memory;
* modify application behavior.

For genuinely sensitive secrets, security-sensitive values should therefore not rely solely on client-side native-code obfuscation.

---

## 14. Conclusion

The target application's native functionality was successfully identified and instrumented.

The analysis began with application reconnaissance and identification of `libnative-lib.so`, followed by inspection of its JNI exports. The `getSecretMessage` native entry point was then selected as the most effective interception point.

Frida's `Interceptor.attach()` was used to monitor the native function and inspect its return value. The returned JNI string was converted into readable text, revealing the decrypted flag. Static analysis of the native implementation and an additional Objection hook were used to validate the runtime findings.

The final recovered flag was:

```text
Holberton{native_hooking_is_no_different_at_all}
```

This exercise demonstrates how dynamic instrumentation can bypass the limitations of static inspection and expose data at the exact point where an Android application processes it in memory.
