To set the HTTP status code for a JSON response in a Spring Boot application using `@RestController`, you have several options depending on your specific needs. Here are the most common approaches:

### 1. Use `ResponseEntity`

`ResponseEntity` allows you to fully control the HTTP response, including status code, headers, and body. It's a generic type, allowing for any type of body.

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

You can annotate your controller method with `@ResponseStatus` to indicate the status code to be returned. This approach is straightforward but less flexible since it does not allow setting the status conditionally based on the method's execution.

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

For more control or to set the status code dynamically, you can use the `HttpServletResponse` directly. This approach gives you fine-grained control over the response but requires more manual coding.

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

- **Use `ResponseEntity`** when you need full control over the response, such as setting custom headers or different status codes based on method execution.
- **Use `@ResponseStatus`** for simple cases where the status code does not depend on the method execution.
- **Use `HttpServletResponse`** when you need to manipulate the response directly in ways not covered by the other approaches, though this is less common in typical Spring applications.

Each of these approaches can be used to effectively set the status code of your JSON responses in a Spring Boot application.
