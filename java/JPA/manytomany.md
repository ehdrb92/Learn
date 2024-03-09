Java 지속성 API(JPA)에서 엔티티 관계 매핑에는 어노테이션을 사용해 데이터베이스 내에서 엔티티가 서로 어떻게 관련되는지 정의하는 작업이 포함됩니다. 관계는 일반적으로 일대일(1:1), 일대다(1:N), 다대일(N:1) 및 다대다(N:N)입니다. JPA를 사용하여 이러한 관계를 가진 테이블을 만드는 방법은 다음과 같습니다:

### 1. One-to-One (1:1) Relationship

`@OneToOne`을 사용하여 두 엔티티 간의 일대일 관계를 정의할 수 있습니다. 이 관계의 각 엔티티는 다른 엔티티의 인스턴스를 최대 하나만 가질 수 있습니다.

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "profile_id", referencedColumnName = "id")
    private UserProfile profile;
}

@Entity
public class UserProfile {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    // If bidirectional
    @OneToOne(mappedBy = "profile")
    private User user;
}
```

### 2. One-to-Many (1:N) and Many-to-One (N:1) Relationship

일대다 및 다대일 관계는 일반적으로 함께 사용됩니다. 일측에는 `@OneToMany`를, 다측에는 `@ManyToOne`을 사용합니다.

```java
@Entity
public class Post {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @OneToMany(mappedBy = "post", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Comment> comments = new ArrayList<>();
}

@Entity
public class Comment {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne
    @JoinColumn(name = "post_id")
    private Post post;
}
```

### 3. Many-to-Many (N:N) Relationship

다대다 관계를 정의하려면 `@ManyToMany`를 사용합니다. 여기에는 일반적으로 조인 테이블이 포함됩니다.

```java
@Entity
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToMany(cascade = { CascadeType.ALL })
    @JoinTable(
        name = "Student_Course",
        joinColumns = { @JoinColumn(name = "student_id") },
        inverseJoinColumns = { @JoinColumn(name = "course_id") }
    )
    private Set<Course> courses = new HashSet<>();
}

@Entity
public class Course {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToMany(mappedBy = "courses")
    private Set<Student> students = new HashSet<>();
}
```

이러한 관계를 만들 때는 `CascadeType.ALL`과 같은 계단식 연산을 관리하는 방법과 관계가 양방향인지 단방향인지 여부를 고려하는 것이 중요합니다. 양방향 관계의 경우, 한 쪽은 소유자(`mappedBy` 속성이 사용되지 않는 경우)이고 다른 쪽은 역방향(`mappedBy`가 역방향 엔티티를 참조하는 소유자의 필드 이름을 지정하는 경우)입니다.

최대 절전 모드와 같은 JPA 구현은 `ddl-auto` 속성이 적절하게 구성되면(예: `update`, `create`, `create-drop`) 이러한 매핑을 기반으로 테이블의 생성 및 업데이트를 자동으로 관리하지만, 운영 환경에서는 마이그레이션을 통해 스키마 업데이트를 관리하는 것이 권장되는 경우가 많다는 점을 기억하세요.
