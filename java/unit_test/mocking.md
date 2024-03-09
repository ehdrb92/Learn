가짜 데이터를 사용하여 데이터베이스와 통신하는 메서드를 테스트하려면 모킹을 사용할 수 있습니다. 모킹을 사용하면 실제 구현에 의존하지 않고 복잡한 컴포넌트(예: 데이터베이스 상호 작용)의 동작을 시뮬레이션할 수 있습니다. 이는 테스트 중인 컴포넌트를 격리하려는 단위 테스트에서 특히 유용합니다. 널리 사용되는 Java용 모의 테스트 프레임워크인 Mockito를 사용한 간단한 예제를 살펴보겠습니다.

### Step 1: Add Mockito to Your Project

먼저 프로젝트에 Mockito를 추가합니다. Maven을 사용하는 경우 `pom.xml`에 다음 종속성을 추가합니다:

```xml
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-core</artifactId>
    <version>3.3.3</version>
    <scope>test</scope>
</dependency>
```

### Step 2: Create a Java Class to Test

데이터베이스 상호 작용을 위해 `UserRepository`에 의존하는 `UserService` 클래스가 있다고 가정해 봅시다:

```java
public class UserService {
    private UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public User findUserByEmail(String email) {
        return userRepository.findByEmail(email);
    }
}
```

The `UserRepository` might look like this:

```java
public interface UserRepository {
    User findByEmail(String email);
}
```

### Step 3: Write the Test with Mock Data

이제 Mockito를 사용하여 `UserRepository`를 모의하기 위해 `UserService.findUserByEmail`에 대한 단위 테스트를 작성해 보겠습니다:

```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

class UserServiceTest {

    private UserRepository mockUserRepository;
    private UserService userService;

    @BeforeEach
    void setUp() {
        // Create a mock UserRepository
        mockUserRepository = mock(UserRepository.class);
        // Initialize UserService with the mocked UserRepository
        userService = new UserService(mockUserRepository);
    }

    @Test
    void testFindUserByEmail() {
        String email = "user@example.com";
        User mockUser = new User(email);

        // Configure the mock to return the mockUser when findByEmail is called
        when(mockUserRepository.findByEmail(email)).thenReturn(mockUser);

        // Call the method under test
        User result = userService.findUserByEmail(email);

        // Verify the result
        assertNotNull(result);
        assertEquals(email, result.getEmail());

        // Verify interaction with the mock
        verify(mockUserRepository).findByEmail(email);
    }
}
```

이 테스트에서는

- `mock(UserRepository.class)`를 사용하여 `UserRepository`를 모의합니다.
- 특정 이메일로 `findByEmail`이 호출될 때 미리 정의된 `User` 객체를 반환하도록 모의 객체를 구성합니다.
- UserService.findUserByEmail` 메서드가 올바른 사용자를 반환하는지 확인합니다.
- `UserRepository` mock에서 `findByEmail`이 호출되었는지 확인합니다.

Mockito를 사용하면 실제 데이터베이스에서 `UserService`를 분리하여 `UserService` 내의 로직에 테스트를 집중하고 데이터베이스 상호 작용에 가짜 데이터를 사용할 수 있습니다.

`@Mock` 어노테이션을 사용하여 단위 테스트 예제를 단순화하기 위해 Mockito의 어노테이션 중심 접근 방식을 활용할 수 있습니다. 이 접근 방식은 모의 객체를 자동으로 생성하여 상용구 코드를 줄입니다. 이전 `UserServiceTest` 클래스를 리팩터링하여 `@Mock`을 사용하여 모의 `UserRepository`를 만들고 `@InjectMocks`를 사용하여 이 모형을 `UserService`에 자동으로 주입하도록 하겠습니다.

### Step 1: Add Mockito Annotations Dependency

프로젝트에 Mockito 어노테이션 종속성이 있는지 확인합니다. Maven을 사용하는 경우 `mockito-core`에 포함되어 있습니다.

### Step 2: Annotate Mocks and Setup

Here's how you can refactor the `UserServiceTest` to use `@Mock` and `@InjectMocks`:

```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

class UserServiceTest {

    @Mock
    private UserRepository mockUserRepository;

    @InjectMocks
    private UserService userService;

    @BeforeEach
    void setUp() {
        // Initialize mocks and inject them
        MockitoAnnotations.openMocks(this);
    }

    @Test
    void testFindUserByEmail() {
        String email = "user@example.com";
        User mockUser = new User(email);

        // Configure the mock to return the mockUser when findByEmail is called
        when(mockUserRepository.findByEmail(email)).thenReturn(mockUser);

        // Call the method under test
        User result = userService.findUserByEmail(email);

        // Verify the result
        assertNotNull(result);
        assertEquals(email, result.getEmail());

        // Verify interaction with the mock
        verify(mockUserRepository).findByEmail(email);
    }
}
```

### Key Changes and Annotations

- `@Mock`: 이 주석은 `UserRepository`의 모의 인스턴스를 만드는 데 사용됩니다. Mockito는 `@InjectMocks` 주석이 달린 필드에 이 모의 객체를 자동으로 주입합니다.
- `@InjectMocks`: 이 주석은 `UserService` 필드에 사용됩니다. Mockito는 `UserService`를 인스턴스화하고 여기에 `UserRepository` 모의를 삽입합니다.
- `MockitoAnnotations.openMocks(this)`: 이 메서드는 Mockito 주석이 달린 필드를 초기화합니다. JUnit 5에서는 테스트 클래스에 `@ExtendWith(MockitoExtension.class)` 주석을 추가하여 Mockito 확장을 사용하여 이 초기화를 자동화함으로써 `setUp` 메서드가 필요하지 않게 할 수 있습니다.

이러한 주석을 사용하면 모의 객체 설정보다는 테스트 중인 동작에 초점을 맞춰 테스트 클래스가 더 깔끔하고 간결해집니다. 이 접근 방식은 종속성 수가 증가할 때 특히 유용하므로 테스트를 더 쉽게 관리하고 읽을 수 있습니다.
