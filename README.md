> This readme file is a [Spring Academy](https://spring.academy/) LEARNING NOTE.

> Learning & memoing starts on 2024-02-07, by Thisoe.

# 1. Spring Initializr
https://start.spring.io/

# 2. Set the Goal
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
-  Test Driven Development (TDD)
- The Red, Green, Refactor Loop
  1. Red: Write a failing test for the desired functionality.
  2. Green: Implement the simplest thing that can work to make the test pass.
  3. Refactor: Look for opportunities to simplify, reduce duplication, or otherwise improve the code without changing any behavior—to refactor.
  4. Repeat!

## Lab
1. [Write a Failing Test](https://spring.academy/courses/building-a-rest-api-with-spring-boot/lessons/test-first-lab/lab)