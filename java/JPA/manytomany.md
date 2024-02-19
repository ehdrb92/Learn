In Java Persistence API (JPA), mapping entity relationships involves using annotations to define how entities relate to each other within the database. The relationships are typically one-to-one (1:1), one-to-many (1:N), many-to-one (N:1), and many-to-many (N:N). Here's how you can create tables with these relationships using JPA:

### 1. One-to-One (1:1) Relationship

Use `@OneToOne` to define a one-to-one relationship between two entities. Each entity in this relationship can have at most one instance of the other entity.

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

One-to-Many and Many-to-One relationships are usually together. Use `@OneToMany` for the one side and `@ManyToOne` for the many side.

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

Use `@ManyToMany` to define a many-to-many relationship. This typically involves a join table.

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

When creating these relationships, it's important to consider how you manage cascading operations (like `CascadeType.ALL`), and whether the relationships are bidirectional or unidirectional. For bidirectional relationships, one side is the owner (where the `mappedBy` attribute is not used), and the other side is the inverse (where `mappedBy` specifies the owner's field name that references the inverse entity).

Remember, JPA implementations like Hibernate automatically manage the creation and update of tables based on these mappings when the `ddl-auto` property is appropriately configured (e.g., `update`, `create`, `create-drop`), but for production environments, it's often recommended to manage schema updates through migrations.
