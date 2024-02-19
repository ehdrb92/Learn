Writing unit tests is an essential part of software development, especially in Java, where it ensures that your code behaves as expected. To demonstrate how to write a unit test, let's use a simple Java class example and test it with JUnit, a popular framework for testing in Java.

### Step 1: Write a Simple Java Class

First, we'll create a simple Java class that we want to test. Let's say we have a class `Calculator` with a method `add` that adds two numbers:

```java
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
}
```

### Step 2: Add JUnit to Your Project

To write and run the unit test, you need to add JUnit to your project. If you're using Maven, you can add the following dependency to your `pom.xml`:

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.13.2</version>
    <scope>test</scope>
</dependency>
```

If you're using a different build tool or IDE, the process will be similar but adjusted to your specific environment.

### Step 3: Write the Unit Test

Next, we write a unit test for the `add` method in the `Calculator` class. We create a new class for our test, typically in a separate source folder designated for tests (e.g., `src/test/java`).

```java
import static org.junit.Assert.assertEquals;
import org.junit.Test;

public class CalculatorTest {
    @Test
    public void testAdd() {
        Calculator calculator = new Calculator();
        int result = calculator.add(10, 5);
        assertEquals(15, result);
    }
}
```

In this test class, we:

- Import `assertEquals` from JUnit to check the expected result.
- Use the `@Test` annotation to indicate that the `testAdd` method is a test method.
- Instantiate the `Calculator` and call the `add` method with two numbers.
- Assert that the result is as expected using `assertEquals`.

### Step 4: Run the Unit Test

The way you run the unit test can depend on your IDE or build tool. Generally, you can run it directly from the IDE by right-clicking the test class or method and selecting the option to run the test. If you're using a command-line tool like Maven, you can run your tests with a command like `mvn test`.

This simple example demonstrates the basics of writing and running a unit test in Java with JUnit. As your applications grow in complexity, you'll write more tests for different scenarios, including negative tests, tests for exceptions, and more. Testing frameworks like JUnit also offer a rich set of features for grouping tests, setting up and tearing down test environments, and testing asynchronous code.
