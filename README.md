# S21–S22: Android Security, Debugging, Testing & Quality Assurance
### Android Development | Kotlin + XML Views

This combined session guide covers Android security and permissions, authentication and authorization, debugging tools, Logcat, breakpoints, stack traces, testing fundamentals, JUnit, Android testing, and code quality.

## Learning Outcomes

By the end of these sessions, students should be able to:

- Explain the purpose of Android security architecture and mobile app security.
- Distinguish normal and dangerous permissions and describe runtime permission handling.
- Differentiate authentication from authorization.
- Identify common Android security risks and apply secure coding practices.
- Explain common application errors and use Logcat, the debugger, breakpoints and stack traces.
- Understand unit testing, JUnit and the Android testing framework.
- Describe a basic testing lifecycle and apply debugging and quality practices.

## Session Map

| Session | Main focus |
|---|---|
| **S21 — Part A** | Android security architecture, permission model, normal/dangerous permissions, runtime permissions and security best practices |
| **S21 — Part B** | Authentication vs authorization, authentication techniques, security risks and secure coding |
| **S22 — Part A** | Debugging, error types, Logcat, Android Studio Debugger, breakpoints and stack traces |
| **S22 — Part B** | Software testing, unit testing, JUnit, Android testing framework, testing lifecycle, code quality and error prevention |

---

# Part 1 — Android Security & Permissions

## 1. Why Mobile Application Security Matters

Android apps may process personal information, user credentials, location, photos, messages or business data. Poor security can expose data, allow unauthorized actions, or damage user trust.

Security should be considered throughout development—not added only after the app is complete.

## 2. Android Security Architecture: Basic Ideas

Android uses multiple layers of protection. At a beginner level, remember these concepts:

- **Application sandboxing:** apps are isolated from one another by the platform.
- **App identity:** each app runs under an application identity.
- **Permission controls:** access to protected resources is governed by permissions and platform rules.
- **Secure communication:** sensitive network traffic should use secure channels.
- **App signing:** signing identifies the publisher/signing identity and supports app update trust.
- **User control:** some sensitive access requires the user to grant permission.

These mechanisms help, but developers must still protect data, validate inputs and configure access correctly.

## 3. Android Permission Model

Permissions control access to protected resources or operations. The app declares required permissions, and the Android platform applies the relevant permission rules.

### Normal vs Dangerous Permissions

| Type | General meaning | Example |
|---|---|---|
| **Normal** | Lower-risk access that the system generally grants when declared, subject to platform rules. | `INTERNET` is commonly declared for network access. |
| **Dangerous** | Access to sensitive data or actions that generally requires runtime approval on supported Android versions. | Camera or location access. |

The exact permission behaviour depends on Android version, target SDK, permission group and the specific permission. Always verify current platform requirements for a real app.

## 4. Declaring a Permission

For example, an app that needs network access may declare this in `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

This declaration does not mean every permission is automatically granted. Runtime-sensitive permissions follow additional rules.

## 5. Runtime Permissions

For a dangerous permission, a typical flow is:

1. Explain why the feature needs the permission.
2. Check whether permission is already granted.
3. If needed, request permission using the appropriate Android API.
4. Handle both approval and denial.
5. Continue to provide a reasonable experience if permission is denied.
6. Request access only when the feature needs it.

Do not repeatedly request a permission without explaining its purpose. Avoid treating permission denial as an app crash.

## 6. Security Best Practices

- Request only the permissions the app actually needs.
- Explain sensitive permission requests clearly.
- Store sensitive data carefully; do not hard-code secrets in source code.
- Use HTTPS for network communication.
- Validate and sanitize untrusted input.
- Avoid logging passwords, access tokens or personal data.
- Apply least privilege: grant only the access needed.
- Keep dependencies and target SDK configuration appropriately maintained.
- Handle permission denial and errors gracefully.
- Use secure authentication and authorization checks for protected operations.

---

# Part 2 — Authentication, Authorization & Secure Coding

## 7. Authentication vs Authorization

| Concept | Question answered | Example |
|---|---|---|
| **Authentication** | “Who are you?” | A user signs in with an approved credential. |
| **Authorization** | “What are you allowed to do?” | A signed-in student can view their own profile but cannot edit another student's record. |

Authentication does not automatically grant access to every resource. The app or backend must still check authorization.

## 8. Common Authentication Techniques

Common approaches include:

- **Password-based login:** user provides a username/email and password.
- **One-time code (OTP):** a temporary code verifies control of an eligible channel.
- **Multi-factor authentication (MFA):** combines different categories of verification.
- **Biometric authentication:** uses a supported device biometric flow to confirm the user locally; app and server security design still matters.
- **Federated sign-in:** uses an identity provider to authenticate the user.

Each approach has trade-offs. Choose based on risk, user experience, platform support and the application's requirements.

## 9. Common Security Risks

- Storing passwords or tokens in plain text.
- Exposing API keys or private credentials in source code.
- Requesting unnecessary permissions.
- Sending sensitive information over insecure connections.
- Trusting client-side checks as the only authorization control.
- Logging secrets or personal information.
- Failing to validate input.
- Leaving cloud database rules too permissive.
- Using outdated or vulnerable dependencies.
- Revealing sensitive data in error messages or debug builds.

## 10. Secure Coding Checklist

Before shipping an app, ask:

- [ ] Are only required permissions requested?
- [ ] Is sensitive data stored and transmitted safely?
- [ ] Are secrets absent from source code and logs?
- [ ] Is input validated?
- [ ] Are protected operations authorized?
- [ ] Are network calls using secure transport?
- [ ] Are failures handled without exposing private details?
- [ ] Are dependencies and platform settings reviewed?
- [ ] Are security rules tested for both allowed and denied access?

---

# Part 3 — Android Debugging

## 11. What Is Debugging?

**Debugging** is the process of finding, understanding and fixing defects that cause an application to behave incorrectly.

A useful approach is to reproduce the issue, inspect evidence, identify the cause, make a small fix and retest the affected behaviour.

## 12. Types of Errors

| Error type | Meaning | Example |
|---|---|---|
| **Syntax/compile-time error** | Code or resources fail to compile. | Missing bracket or invalid reference. |
| **Runtime error** | A failure occurs while the app is running. | A null-related crash. |
| **Logical error** | The app runs but produces the wrong result. | Incorrect calculation or condition. |
| **UI/resource error** | Layout, ID, drawable or resource configuration is incorrect. | A view ID is not found in the loaded layout. |
| **Configuration/build error** | Build settings or dependencies prevent a successful build. | Missing dependency or Gradle configuration issue. |

## 13. Logcat Overview

Logcat displays logs from Android apps and the system. It helps developers inspect messages, warnings, exceptions and stack traces.

Typical levels include:

- `Verbose`
- `Debug`
- `Info`
- `Warn`
- `Error`

Use meaningful messages that help explain what happened. Do not log passwords, tokens or sensitive user information.

Example:

```kotlin
Log.d("ProfileScreen", "Submit button clicked")
Log.e("ProfileScreen", "Unable to load profile", exception)
```

Add the appropriate `android.util.Log` import. In real applications, ensure log messages do not expose private data.

## 14. Android Studio Debugger & Breakpoints

The debugger lets you pause execution and inspect program state.

### Basic workflow

1. Run the app in debug mode.
2. Open the Kotlin file.
3. Click the gutter beside a line to add a breakpoint.
4. Reproduce the action that reaches that line.
5. Inspect variable values and the call stack.
6. Step over or into code to follow execution.
7. Resume execution after understanding the issue.

A **breakpoint** is a marker that pauses execution when the program reaches a selected line, subject to its conditions.

## 15. Stack Trace Analysis

A stack trace shows the sequence of calls associated with an exception or failure.

When reading a stack trace:

1. Identify the exception type and message.
2. Find the first relevant line from your app's package.
3. Inspect the code around that line.
4. Trace the call path to understand how execution reached it.
5. Reproduce the issue and verify the fix.

Do not assume the first line shown is always the root cause; inspect the exception and relevant app frames.

## 16. Common Debugging Strategies

- Reproduce the issue consistently.
- Read the full error message rather than guessing.
- Check the exact line and input values.
- Inspect XML IDs, resource names and the layout loaded by the Activity.
- Reduce the problem to a small example.
- Change one thing at a time.
- Retest the original scenario and nearby cases.
- Keep debug logs useful but free of secrets.

---

# Part 4 — Software Testing & Quality Assurance

## 17. Why Testing Matters

Testing checks whether the app behaves as expected and helps catch regressions before release. Debugging finds and fixes a problem; testing provides evidence about behaviour and helps prevent future problems.

## 18. Unit Testing Concepts

A **unit test** checks a small, isolated unit of code, such as a function that validates input or calculates a result.

A useful unit test should be:
- Focused on one behaviour.
- Repeatable.
- Clear about expected and actual results.
- Independent of unrelated external services where possible.

## 19. JUnit Introduction

JUnit is a testing framework commonly used to write and run JVM unit tests in Android projects.

Illustrative example:

```kotlin
import org.junit.Assert.assertEquals
import org.junit.Test

class GreetingTest {

    private fun greeting(name: String): String {
        return "Hello, $name!"
    }

    @Test
    fun greetingIncludesName() {
        assertEquals("Hello, Asha!", greeting("Asha"))
    }
}
```

Place JVM unit tests in the project's appropriate test source set, commonly `src/test`. Confirm the JUnit dependency and version used by the project.

## 20. Android Testing Framework Overview

Android projects commonly use different test types for different needs:

| Test type | What it checks | Common location |
|---|---|---|
| Local unit test | Logic that can run on the local JVM | `src/test` |
| Instrumented test | Code that needs Android framework/device or emulator execution | `src/androidTest` |
| UI test | User-interface interactions and visible behaviour; often instrumented | Commonly `src/androidTest` depending on setup |

The precise framework and dependencies depend on the project configuration. A test should be placed where its runtime requirements can be satisfied.

## 21. Testing Lifecycle

A simple testing lifecycle:

```text
Understand requirement
        |
        v
Write test case and expected result
        |
        v
Run the test
        |
        v
Compare actual vs expected
        |
        +---- Failure ---> Investigate / fix ---> Run again
        |
        v
Record result and regression-check
```

### Example test case

| Field | Example |
|---|---|
| Feature | Name validation |
| Input | Empty string |
| Expected result | Validation rejects the input |
| Actual result | Record what the app/test returns |
| Status | Pass or fail |

## 22. Testing Best Practices

- Test normal, boundary and invalid inputs.
- Give tests descriptive names.
- Keep unit tests focused and repeatable.
- Test permission denial and other failure paths.
- Test UI on different screen sizes when layout is involved.
- Re-run relevant tests after fixing a bug.
- Avoid relying only on manual testing.
- Keep test data safe and avoid real secrets.
- Test loading, empty, error and success states.

---

# Part 5 — Code Quality & Error Prevention

## 23. Code Quality Basics

Readable code is easier to review, debug and maintain.

- Use meaningful variable and function names.
- Keep functions focused.
- Avoid unnecessary duplication.
- Handle expected failures explicitly.
- Separate UI logic from business rules when appropriate.
- Remove unused code and debug statements before release.
- Use consistent formatting.
- Add comments to explain non-obvious decisions rather than restating every line.

## 24. Error Prevention Checklist

- [ ] Build the project regularly while developing.
- [ ] Validate input at appropriate boundaries.
- [ ] Handle null or missing data where possible.
- [ ] Use clear error and empty states.
- [ ] Test edge cases.
- [ ] Inspect Logcat after crashes.
- [ ] Add unit tests for important logic.
- [ ] Re-test after each meaningful fix.
- [ ] Review permissions, secrets, logs and dependencies before release.

---

# Part 6 — Practice Questions

## Short-answer questions

1. What is Android application security?
2. What is the difference between normal and dangerous permissions?
3. What is a runtime permission?
4. Differentiate authentication and authorization.
5. Name four common Android security risks.
6. What is Logcat?
7. What is a breakpoint?
8. What information does a stack trace provide?
9. What is a unit test?
10. What is JUnit?
11. Differentiate local unit tests and instrumented tests.
12. What is regression testing?

## Descriptive questions

1. Explain the Android permission model and runtime permission flow.
2. Discuss mobile application security best practices.
3. Explain authentication vs authorization with examples.
4. Describe how to investigate a crash using Logcat, the debugger and a stack trace.
5. Explain common types of errors in Android applications.
6. Describe unit testing, JUnit and the Android testing framework.
7. Explain a basic testing lifecycle with an example.
8. Discuss testing best practices and code quality measures.
9. Explain how secure coding and testing help prevent defects.

## Viva & Interview Questions

1. Why should an app request only the permissions it needs?
2. Does a user being authenticated mean they can access every resource?
3. Why should sensitive information not be written to Logcat?
4. When is a breakpoint useful?
5. How do you find the relevant line in a stack trace?
6. What is the difference between a runtime error and a logical error?
7. Why should tests include invalid input?
8. Why might a test need an emulator or device?
9. What should be done after fixing a bug?
10. How can code quality reduce future debugging effort?

---

# Final Revision Checklist

- [ ] I can explain Android security architecture at a high level.
- [ ] I understand normal, dangerous and runtime permissions.
- [ ] I can distinguish authentication from authorization.
- [ ] I can identify common security risks and secure coding practices.
- [ ] I can use Logcat, breakpoints and stack traces to investigate problems.
- [ ] I can distinguish syntax, runtime, logical and configuration errors.
- [ ] I understand unit tests, JUnit and Android instrumented tests.
- [ ] I can explain the testing lifecycle and regression testing.
- [ ] I can apply code quality and error prevention practices.

## Key Takeaway

**Secure and reliable Android development requires both prevention and verification:** request access responsibly, protect data, authenticate and authorize correctly, investigate errors with evidence, and test important behaviour before release.
