> This readme file is a [Spring Academy](https://spring.academy/) learning note. <br/> The coding following Spring Academy was done in VS Code instead of Spring Academy Lab.

> Learning & memoing starts on 2024-02-07, by Thisoe.

> ### <span style="color:yellow">This repo is abandoned</span>
> This repo is currently stopped. I found Spring Academy a bit not beginner friendly-ish, and at the same time, a few steps are not working on my Windows machine. <br/> But this repo will be reserving archived since new SpringBoot learning repos [like this](https://github.com/ThisoeCode/springboot-tut_following-devtiro) is going to have tag-links to this file.

# 1. Spring Initializr
https://start.spring.io/

# 2. [Set the Goal](https://spring.academy/courses/building-a-rest-api-with-spring-boot/lessons/data-contracts)
Goal API:
```yaml
Request:
  URI: /cashcards/{id}
  HTTP Verb: GET
  Body: None

Response:
  HTTP Status:
    200 OK if the user is authorized and the Cash Card was successfully retrieved
    401 UNAUTHORIZED if the user is unauthenticated or unauthorized
    404 NOT FOUND if the user is authenticated and authorized but the Cash Card cannot be found
  Response Body Type: JSON
  Example Response Body:
    {
      "id": 99,
      "amount": 123.45
    }
```

# 3. [Testing First](https://spring.academy/courses/building-a-rest-api-with-spring-boot/lessons/test-first)
- Test Driven Development (TDD)
- The Red, Green, Refactor Loop
  1. Red: Write a failing test for the desired functionality.
  2. Green: Implement the simplest thing that can work to make the test pass.
  3. Refactor: Look for opportunities to simplify, reduce duplication, or otherwise improve the code without changing any behavior—to refactor.
  4. Repeat!


## [Lab](https://spring.academy/courses/building-a-rest-api-with-spring-boot/lessons/test-first-lab/lab)

1. Write a Failing Test
```java
package example.cashcard;

import org.junit.jupiter.api.Test;
import static org.assertj.core.api.Assertions.assertThat;

class CashCardJsonTest {
   @Test
   void myFirstTest() {
      assertThat(1).isEqualTo(42);
   }
}
```

[Install Gradle](https://gradle.org/install/) to run the test.
```bash
# Test after installation & setting env var. 
gradle -v
# Gradle 8.6

gradle init
# Selected
  # Generate a Gradle build from Maven build? y
  # Build script DSL: Groovy

gradle test
# ...
# BUILD SUCCESSFUL in 13s
# 4 actionable tasks: 4 executed
```
`4 executed` means that this is a <span style='color:darkred'>**failed**</span> test.
To "fix" it, change the `1` in `assertThat()` to `42`.
```java
class CashCardJsonTest {
   @Test
   void myFirstTest() {
      assertThat(42).isEqualTo(42);
   }
}
```
Now, it passes.
```bash
gradle test
# ...
# BUILD SUCCESSFUL in 1s
# 4 actionable tasks: 2 executed, 2 up-to-date
```

This whole process above, is called a "test-first development".


2. Testing the Data Contract

```java
package example.cashcard;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.json.JsonTest;
import org.springframework.boot.test.json.JacksonTester;
import java.io.IOException;
import static org.assertj.core.api.Assertions.assertThat;

@JsonTest // marks the `CashCardJsonTest` as a test class
class CashCardJsonTest {

  @Autowired // directs Spring to create an object of the requested type
  private JacksonTester<CashCard> json;
  // `JacksonTester` handles serialization and deserialization of JSON objects

  @Test
  void cashCardSerializationTest() throws IOException {
    CashCard cashCard = new CashCard(99L, 123.45);
    assertThat(json.write(cashCard)).isStrictlyEqualToJson("expected.json");
    assertThat(json.write(cashCard)).hasJsonPathNumberValue("@.id");
    assertThat(json.write(cashCard)).extractingJsonPathNumberValue("@.id")
      .isEqualTo(99);
    assertThat(json.write(cashCard)).hasJsonPathNumberValue("@.amount");
    assertThat(json.write(cashCard)).extractingJsonPathNumberValue("@.amount")
      .isEqualTo(123.45);
  }
}
```

> Terminal run test and get failure message.

We yet neither have created `CashCard` class, nor the `expected.json` file. Let's create it **in the `main/` directory**.

  1. Create file `src/main/java/example/cashcard/CashCard.java` and add content:
```java
package example.cashcard;

record CashCard(Long id, Double amount) {
}
```
  2. Create file `src/test/resources/example/cashcard/expected.json` (**right click `test` directory, select `New File`, type in `resources/example/cashcard/expected.json`**) and add content:
```json
{}
```

> Terminal run test, and...

```bash
./gradlew test
# ...
# Task :compileTestJava FAILED
#   error: records are not supported in -source 8
#   (use -source 16 or higher to enable records)
# ...
```
This error is unexpected — the tutorial did not mention this.

After Googling and GPTing, I found the solution:
  1. Check Java version in terminal:
```bash
java -version
# java version "17.0.10" 2024-01-16 LTS
```
  2. Go to `build.gradle`, find:
```js
java.sourceCompatibility = JavaVersion.VERSION_1_8
```
  3. Change `VERSION_1_8` to `VERSION_17`.

After these, run the test again.
```bash
./gradlew test --warning-mode all
# BUILD SUCCESSFUL in 931ms
```

Different from tutorial again: this expects a comparison failure, but got a success.

<br/><br/>

___
> Repo has stopped updating.