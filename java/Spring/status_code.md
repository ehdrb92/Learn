Spring Boot 애플리케이션에서 `@RestController`를 사용하여 JSON 응답에 대한 HTTP 상태 코드를 설정하려면 특정 요구 사항에 따라 몇 가지 옵션이 있습니다. 다음은 가장 일반적인 접근 방식입니다:

### 1. Use `ResponseEntity`

`ResponseEntity` 를 사용하면 상태 코드, 헤더, 본문 등 HTTP 응답을 완전히 제어할 수 있습니다. 일반 유형이므로 모든 유형의 본문을 사용할 수 있습니다.

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyController {

    @GetMapping("/data")
    public ResponseEntity<ResponseData> getData() {
        ResponseData responseData = new ResponseData();
        responseData.setStatus("Success");
        responseData.setData(new DataObject()); // Assuming DataObject is a class you've defined
        return new ResponseEntity<>(responseData, HttpStatus.OK);
    }
}
```

### 2. Use `@ResponseStatus` Annotation

컨트롤러 메소드에 `@ResponseStatus` 주석을 달아 반환할 상태 코드를 표시할 수 있습니다. 이 방법은 간단하지만 메서드의 실행에 따라 조건부로 상태를 설정할 수 없으므로 유연성이 떨어집니다.

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseStatus;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.http.HttpStatus;

@RestController
public class MyController {

    @ResponseStatus(HttpStatus.OK) // Sets the status code to 200 OK
    @GetMapping("/data")
    public ResponseData getData() {
        ResponseData responseData = new ResponseData();
        responseData.setStatus("Success");
        responseData.setData(new DataObject());
        return responseData;
    }
}
```

### 3. Use `HttpServletResponse`

더 많은 제어를 원하거나 상태 코드를 동적으로 설정하려면 `HttpServletResponse`를 직접 사용할 수 있습니다. 이 방법을 사용하면 응답을 세밀하게 제어할 수 있지만 수동 코딩이 더 많이 필요합니다.

```java
import javax.servlet.http.HttpServletResponse;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyController {

    @GetMapping("/data")
    public ResponseData getData(HttpServletResponse response) {
        ResponseData responseData = new ResponseData();
        responseData.setStatus("Success");
        responseData.setData(new DataObject());
        response.setStatus(HttpServletResponse.SC_OK); // Set status code to 200
        return responseData;
    }
}
```

### Choosing the Right Approach

- 사용자 정의 헤더를 설정하거나 메서드 실행에 따라 다른 상태 코드를 설정하는 등 응답을 완전히 제어해야 하는 경우 **`ResponseEntity`**를 사용하세요.
- 상태 코드가 메서드 실행에 의존하지 않는 간단한 경우에는 **`@ResponseStatus`**를 사용합니다.
- 다른 접근 방식에서 다루지 않는 방식으로 응답을 직접 조작해야 하는 경우 **`HttpServletResponse`**를 사용하지만, 일반적인 Spring 애플리케이션에서는 덜 일반적입니다.

이러한 각 접근 방식은 Spring Boot 애플리케이션에서 JSON 응답의 상태 코드를 효과적으로 설정하는 데 사용할 수 있습니다.
