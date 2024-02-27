Jakarta 지속성 API(이전의 Java 지속성 API 또는 JPA)에는 엔티티 생성 및 수정에 대한 타임스탬프를 자동으로 처리하는 데 사용할 수 있는 어노테이션이 있습니다. 이러한 어노테이션은 다음과 같습니다:

- `@PrePersist`: 이 어노테이션은 엔티티가 데이터베이스에 삽입(지속)되기 전에 실행되어야 하는 콜백 메서드를 지정하는 데 사용됩니다. 생성 타임스탬프를 설정하는 데 사용할 수 있습니다.
- `@PreUpdate`: 이 어노테이션은 데이터베이스에서 엔티티가 업데이트되기 전에 실행되어야 하는 콜백 메서드를 지정하는 데 사용됩니다. 수정 타임스탬프를 업데이트하는 데 사용할 수 있습니다.

그러나 콜백 메서드를 수동으로 구현할 필요 없이 생성 및 수정 타임스탬프를 직접 처리하려면 엔티티 클래스의 필드와 결합된 `@Temporal` 어노테이션을 고려할 수 있습니다. 그러나 이러한 목적으로 일반적으로 사용되는 보다 구체적인 어노테이션은 Jakarta 지속성 API 자체의 일부가 아니라 (JPA 사양의 인기 있는 구현인) Hibernate 프레임워크에서 찾거나 애플리케이션 수준 코드를 통해 관리할 수 있습니다:

- `@CreationTimestamp`: 이 최대 절전 모드 전용 어노테이션은 엔티티가 처음으로 유지될 때 타임스탬프를 자동으로 설정합니다.
- `@UpdateTimestamp`: 마찬가지로 이 하이버네이트 전용 어노테이션은 엔티티가 업데이트될 때마다 타임스탬프를 자동으로 업데이트합니다.

순수 JPA의 경우, 생성 및 업데이트 타임스탬프를 자동으로 처리하려면 엔티티 리스너를 사용하거나 엔티티의 `@PrePersist` 및 `@PreUpdate` 어노테이션 메서드에서 이러한 타임스탬프를 수동으로 설정해야 하는 경우가 종종 있습니다. 다음은 이를 수행하는 방법을 보여주는 예제입니다:

```java
import javax.persistence.*;
import java.util.Date;

@Entity
public class MyEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Temporal(TemporalType.TIMESTAMP)
    private Date createdAt;

    @Temporal(TemporalType.TIMESTAMP)
    private Date updatedAt;

    @PrePersist
    protected void onCreate() {
        createdAt = new Date();
    }

    @PreUpdate
    protected void onUpdate() {
        updatedAt = new Date();
    }

     // Getters and setters
}
```

이 코드 스니펫은 JPA의 콜백 어노테이션을 사용하여 `createdAt` 및 `updatedAt` 필드를 수동으로 설정합니다. 임시`는 날짜/시간 값의 정밀도를 지정하는 데 사용됩니다. 최신 버전의 JPA(2.2 이상)에서는 `@Temporal`어노테이션 없이도 Java 8 날짜 및 시간 API의`LocalDateTime`을 사용할 수 있습니다.
