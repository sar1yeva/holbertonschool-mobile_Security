# Android Security Challenge: Cryptographic Data Interception

## 1. Assessment Objective

The objective of this challenge was to analyze an Android application that uses cryptographic mechanisms to protect sensitive data and determine whether the protected information could be recovered through static and dynamic analysis.

The assessment focused on identifying how cryptographic material was handled, tracing the application's cryptographic workflow, and using runtime analysis to recover the protected secret without modifying the application's source code.

The final objective was to retrieve the hidden flag from the application.

---

## 2. Target and Analysis Environment

### Target

* **Application:** `Apk_task2`
* **Platform:** Android
* **Analysis type:** Static and Dynamic Analysis

### Tools Used

* JADX
* APKTool
* Frida
* Objection
* ADB
* Python
* Android Emulator

These tools were used to inspect the application structure, identify cryptographic operations, monitor runtime behavior, and reproduce the relevant cryptographic process.

---

## 3. Initial Application Analysis

The first step was to decompile the APK and inspect its application structure.

JADX was used to recover the Java/Kotlin source representation:

```bash
jadx -d decompiled Apk_task2.apk
```

The decompiled project was then reviewed to identify:

* Activities and application entry points
* Cryptographic APIs
* Key management operations
* Hardcoded strings
* Encryption and decryption routines
* Android Keystore references
* Potential locations where the flag was processed

The analysis showed that the application relied on Android's cryptographic functionality and included logic associated with the Android Keystore.

---

## 4. Identifying Android Keystore Usage

The most relevant discovery during static analysis was the application's use of the Android Keystore mechanism.

The Android Keystore is designed to provide a protected environment for cryptographic keys. However, using the Keystore API does not automatically make an application secure.

The security of the implementation also depends on:

* How keys are generated
* Which aliases are used
* How encrypted data is handled
* Where plaintext values are exposed
* Whether sensitive material can be observed during runtime
* Whether cryptographic operations can be instrumented

Therefore, the presence of the Keystore was treated as an area for further investigation rather than as evidence that the secret was inaccessible.

---

## 5. Tracing the Cryptographic Workflow

After identifying the cryptographic functionality, the next step was to understand how the application processed the protected data.

The analysis followed the application's execution flow from the point where the protected value was loaded through the cryptographic operation until the resulting plaintext became available.

Particular attention was given to:

1. Key retrieval or generation
2. Data retrieval
3. Cipher initialization
4. Encryption/decryption operations
5. Conversion of decrypted data into a usable string
6. Any subsequent use or display of the plaintext value

This approach allowed the cryptographic boundary to be identified.

Instead of attempting to break the underlying cryptographic primitive, the analysis focused on the application's implementation and the point where protected information became available to the application itself.

---

## 6. Dynamic Analysis

Static analysis provided the structure of the cryptographic workflow, but runtime instrumentation was required to observe the application's behavior.

Frida was selected for dynamic instrumentation because it allows Java methods and Android cryptographic APIs to be monitored while the application is running.

The application was launched on the Android emulator and attached to using Frida.

The runtime analysis focused on identifying calls related to:

* Keystore access
* Key retrieval
* Cipher initialization
* Encryption/decryption
* Plaintext generation

The purpose was not to bypass Android security mechanisms directly, but to observe the application's own legitimate execution of the cryptographic process.

---

## 7. Recovering the Protected Data

The key observation was that Android Keystore protection does not prevent an application from using the key.

Once the application performs a decryption operation, the resulting plaintext must become available to the application in memory.

Therefore, monitoring the cryptographic workflow at runtime made it possible to observe the data after the protection mechanism had been applied by the application.

This demonstrated an important distinction:

> Protecting a cryptographic key is not equivalent to protecting every value produced by a cryptographic operation.

The application could securely store or manage a key while still exposing sensitive plaintext during execution.

---

## 8. Analysis of the Security Weakness

The challenge demonstrates a weakness in relying solely on Android Keystore as a security boundary.

Android Keystore provides important protections for cryptographic keys, but it does not prevent:

* Instrumentation of the application's runtime
* Observation of plaintext after decryption
* Hooking application-level cryptographic operations
* Reverse engineering of application logic
* Extraction of sensitive values that are embedded or deterministically produced by the application

If an attacker controls or instruments the runtime environment, application-level secrets can potentially be recovered at the point where the application itself accesses them.

This is especially relevant for secrets that are expected to remain completely inaccessible to the user.

---

## 9. Key Findings

### Finding 1 — Sensitive plaintext exposed during runtime

**Description:**
The application performs cryptographic operations that eventually expose the protected information in plaintext during normal execution.

**Impact:**
An attacker capable of instrumenting the application can potentially recover sensitive information without compromising the underlying cryptographic algorithm.

**Risk:**
The confidentiality of application-embedded secrets cannot rely solely on Keystore-based key protection.

---

### Finding 2 — Client-side secret protection is not an absolute security boundary

The application contains sufficient information and functionality to process the protected value locally.

Consequently, an attacker who can reverse engineer and instrument the application can analyze the cryptographic workflow and observe sensitive values during execution.

This illustrates a general mobile application security principle:

**If a secret must never be accessible to the client, it should not ultimately be delivered to the client in recoverable plaintext.**

---

## 10. Challenges Encountered

Several aspects of the application made the analysis less straightforward than simply searching for the flag.

The most significant challenge was distinguishing between:

* Cryptographic key storage
* The encrypted representation of the secret
* The actual decryption operation
* The resulting plaintext

The Android Keystore initially appeared to provide a strong barrier. However, tracing the complete application workflow showed that the more useful analysis point was the runtime cryptographic operation rather than the stored key itself.

This shifted the investigation from attempting to recover the Keystore key to observing how the application used the protected key.

---

## 11. Extracted Flag

The protected value was successfully recovered through analysis of the application's cryptographic workflow.

```text
Holberton{keystore_is_not_as_safe_as_u_think!}
```

---

## 12. Security Recommendations

For production applications, sensitive secrets should not be embedded in a client application when the client is not supposed to know them.

Recommended measures include:

* Keep high-value secrets on a trusted backend.
* Use Android Keystore for key protection where appropriate.
* Use hardware-backed keys when supported by the target device.
* Avoid embedding long-term application secrets in APK resources or code.
* Minimize the lifetime of sensitive plaintext values in memory.
* Apply appropriate authentication and authorization controls.
* Treat a compromised or instrumented client as an untrusted environment.
* Use server-side validation for sensitive operations.
* Avoid relying on obfuscation as a replacement for cryptographic security.

Keystore should therefore be considered one component of a broader security architecture rather than a guarantee that application data can never be recovered.

---

## 13. Conclusion

The assessment demonstrated that Android Keystore can provide strong protection for cryptographic keys while still leaving sensitive plaintext observable during application execution.

Through static analysis, the application's cryptographic workflow was identified. Dynamic analysis then allowed the execution of the relevant cryptographic operations to be observed and the protected value to be recovered.

The challenge highlights an important mobile security principle: **the security of a secret depends not only on how its key is stored, but also on where and how the secret is ultimately used.**

The final flag was successfully extracted:

```text
Holberton{keystore_is_not_as_safe_as_u_think!}
```
