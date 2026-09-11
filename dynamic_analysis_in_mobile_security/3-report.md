# Task 3 — Android Security Challenge: Revealing Hidden Functions

## 1. Assessment Objective

The objective of this challenge was to identify functionality that was intentionally excluded from the application's normal execution flow and use reverse engineering and dynamic-analysis techniques to recover the hidden secret.

The application contains a function responsible for transforming an encoded value into the final flag. However, this function is not invoked during normal application execution.

The assessment focused on:

* analyzing the APK structure;
* identifying application classes and methods;
* locating unused or hidden functionality;
* understanding the transformation performed by the hidden function;
* determining how the encoded data is processed;
* reproducing the transformation independently;
* recovering and validating the hidden flag.

---

## 2. Target and Analysis Environment

### Target Application

```text
APK: task3_d.apk
Package: com.holberton.task4_d
```

### Tools Used

| Tool      | Purpose                                         |
| --------- | ----------------------------------------------- |
| JADX      | APK decompilation and source-code inspection    |
| Frida     | Runtime instrumentation and function invocation |
| Objection | Runtime method inspection and hooking           |
| APKTool   | APK structure and resource analysis             |
| Python 3  | Reimplementation of the hidden transformation   |

The primary investigation was performed through static analysis, followed by dynamic-analysis considerations and independent reproduction of the discovered algorithm.

---

## 3. APK Decompilation

The application was first decompiled with JADX:

```bash
jadx -d decompiled task3_d.apk
```

The package structure revealed the application's primary source files under:

```text
sources/com/holberton/task4_d/
```

The relevant classes included:

```text
MainActivity.java
MainActivity$retrieveEncryptedData$1.java
MainActivityKt.java
ui/theme/*
```

`MainActivity.java` was examined first because it contained the application's visible UI logic and state management.

---

## 4. Reviewing the Normal Application Flow

The application maintains a `decodedFlag` state value which is initially empty.

Although the application contains functionality related to retrieving encrypted data, the normal execution path does not populate the flag.

One of the methods identified during the initial review was:

```text
retrieveEncryptedData()
```

However, this method does not perform the actual decoding operation required to obtain the flag. It invokes a callback without containing the relevant transformation logic.

Similarly, the `setDecodedFlag` functionality was not reached through the normal `onCreate()` execution flow.

This indicated that the visible application logic was not the correct location for extracting the secret.

At this point, the analysis was expanded beyond the activity lifecycle and all application methods were reviewed systematically.

---

## 5. Locating the Unreferenced Function

Further inspection of `MainActivityKt.java` revealed a private top-level function with an unusual identifier:

```text
aBcDeFgHiJkLmNoPqRsTuVwXyZ123456
```

The method signature was:

```kotlin
private static final void aBcDeFgHiJkLmNoPqRsTuVwXyZ123456(
    Function1<? super String, Unit> function1
)
```

The method name does not describe its functionality and visually resembles an encoded string or Base64-related identifier.

A search through the decompiled source confirmed that the function was not referenced elsewhere in the application.

This was significant because the challenge specifically stated that the function responsible for retrieving the secret was not executed during normal application usage.

The investigation therefore identified the following execution model:

```text
Normal application flow
        |
        v
MainActivity
        |
        v
UI displayed
        |
        X
Hidden function never reached
```

The hidden function instead represented an isolated execution path containing the actual flag-processing logic.

---

## 6. Dynamic Analysis Approach

Once the hidden method had been identified, Frida could be used to instrument or invoke it directly at runtime.

A Frida-based approach would locate the generated Kotlin class:

```javascript
Java.perform(function () {
    var MainActivityKt =
        Java.use("com.holberton.task4_d.MainActivityKt");

    console.log("[+] MainActivityKt loaded");
});
```

The target method can then be monitored or invoked through the Java runtime.

Conceptually, the interception point is:

```text
MainActivityKt
      |
      +--> aBcDeFgHiJkLmNoPqRsTuVwXyZ123456()
                    |
                    v
             Hidden transformation
                    |
                    v
                Callback
                    |
                    v
                  Flag
```

Objection provides another possible runtime workflow for identifying and interacting with hidden Java methods.

The important finding was that dynamic execution was not necessary to reconstruct the final value once the complete deterministic transformation had been identified in the decompiled code. However, the hidden method itself provided the correct runtime execution point for validating the analysis.

---

## 7. Analysis of the Hidden Transformation

The hidden function contains a hardcoded Base64 value:

```text
8CP4zSyn62t78lwwc383rxcgtv/UiMv3Pw+Mfw12LzXvorIpBypNK/oB7XvWNV0oWfoX
```

The first operation is Base64 decoding:

```kotlin
byte[] decodedBytes = Base64.decode(
    "8CP4zSyn62t78lwwc383rxcgtv/UiMv3Pw+Mfw12LzXvorIpBypNK/oB7XvWNV0oWfoX",
    0
)
```

The resulting bytes then pass through several transformations.

For every byte at position `index`, the application performs the following operations.

### Step 1 — Unsigned byte conversion

The byte is converted into an unsigned integer:

```text
value = byte & 0xFF
```

This ensures that the value is represented within:

```text
0–255
```

### Step 2 — XOR operation

The value is XORed with the constant:

```text
19
```

Result:

```text
temp = value XOR 19
```

### Step 3 — Bit rotation

The transformed value is then rotated using:

```text
((temp >> 2) | (temp << 6)) & 0xFF
```

This effectively performs an 8-bit rotation by two bits to the right.

### Step 4 — Position-dependent subtraction

The rotated value is modified according to its position:

```text
shifted - index * 3
```

The result is normalized to the byte range using modulo 256.

### Step 5 — Modular multiplication

The resulting value is multiplied by:

```text
183
```

and reduced modulo 256:

```text
charCode = (temp2 * 183) % 256
```

### Step 6 — Character construction

Finally, the resulting integer is converted to a character and appended to the output string.

The complete transformation can therefore be represented as:

```text
Base64 ciphertext
       |
       v
Base64 decode
       |
       v
Unsigned byte
       |
       v
XOR 19
       |
       v
8-bit bit rotation
       |
       v
Subtract index × 3
       |
       v
Modulo 256
       |
       v
Multiply by 183
       |
       v
Character conversion
       |
       v
Plaintext flag
```

---

## 8. Reproducing the Algorithm

To independently validate the result, the transformation was reproduced in Python.

```python
import base64

encoded = (
    "8CP4zSyn62t78lwwc383rxcgtv/UiMv3Pw+Mfw12LzXvorIpBypNK/"
    "oB7XvWNV0oWfoX"
)

decoded_bytes = base64.b64decode(encoded)

flag_chars = []

for index, byte_value in enumerate(decoded_bytes):
    value = byte_value & 0xFF

    # XOR transformation
    temp = value ^ 19

    # 8-bit rotation
    shifted = ((temp >> 2) | (temp << 6)) & 0xFF

    # Position-dependent transformation
    temp2 = (shifted - index * 3) % 256

    # Final modular transformation
    char_code = (temp2 * 183) % 256

    flag_chars.append(chr(char_code))

flag = "".join(flag_chars)

print(flag)
```

The resulting plaintext was:

```text
Holberton{calling_uncalled_functions_is_now_known!}
```

The result matched the expected output of the hidden transformation.

---

## 9. Why the Hidden Function Was Effective

The function was intentionally difficult to identify through casual code inspection.

Several characteristics contributed to this:

* the method name did not describe its purpose;
* the function was private;
* it was not part of the normal application execution path;
* the visible activity contained decoy functionality;
* the encoded flag was embedded directly in the hidden method;
* the transformation consisted of several custom byte-level operations.

A simple search for common terms such as `flag`, `secret`, or `decrypt` would therefore not necessarily identify the correct method.

The more effective approach was to inspect application-defined methods and determine which functions were unreferenced but contained suspicious constants or data-processing logic.

---

## 10. Security Observations

The challenge demonstrates that hiding a sensitive function does not provide meaningful protection against reverse engineering.

The function was not exposed through the application's normal UI, but its implementation remained inside the APK. An analyst with access to the application package could therefore inspect the bytecode and recover:

* the hidden function;
* the encoded data;
* the transformation constants;
* the complete decoding algorithm.

The custom transformation also does not provide cryptographic security. It is an obfuscation mechanism rather than a standard cryptographic construction.

The use of constants such as:

```text
19
3
183
```

does not provide secrecy because all of them can be recovered from the application binary.

---

## 11. Challenges Encountered

### Unused application logic

The first methods encountered appeared to relate to retrieving and displaying the flag, but they were not connected to the application's normal execution flow.

This required tracing method references rather than assuming that every apparently relevant method was actually responsible for the secret.

### Obfuscated method name

The actual function used a deliberately unusual name:

```text
aBcDeFgHiJkLmNoPqRsTuVwXyZ123456
```

Its appearance provided little indication of its purpose.

### Custom byte transformation

The hidden function did not implement a recognizable standard cipher. The algorithm had to be reconstructed operation by operation from the decompiled code.

Particular attention was required for:

* unsigned byte handling;
* the bit rotation;
* the position-dependent subtraction;
* modulo 256 arithmetic;
* conversion of the final values into characters.

---

## 12. Result

The hidden function was successfully identified and its transformation logic was reconstructed.

The encoded payload was decoded and processed using the same sequence of operations implemented by the application.

The recovered flag was:

```text
Holberton{calling_uncalled_functions_is_now_known!}
```

---

## 13. Conclusion

The challenge demonstrated a practical reverse-engineering workflow for locating functionality that is intentionally excluded from an application's normal execution path.

The investigation began with APK decompilation and examination of the application's activity and UI logic. The visible methods did not provide the expected flag, so the analysis was extended to unreferenced functions within the generated Kotlin classes.

This led to the discovery of the hidden function `aBcDeFgHiJkLmNoPqRsTuVwXyZ123456`, which contained both the encoded payload and the complete byte-level transformation required to recover the secret.

The transformation was independently reimplemented in Python, producing the same plaintext as the application's hidden logic.

The final recovered flag is:

```text
Holberton{calling_uncalled_functions_is_now_known!}
```

This exercise reinforces an important Android security principle: functionality cannot be considered protected simply because it is hidden from the normal application flow. If the code and required data are distributed inside the APK, reverse engineering and runtime instrumentation can expose them.
