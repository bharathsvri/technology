# JPA and Hibernate

## What is JPA?
JPA (Java Persistence API) is a specification for managing relational data in Java applications.

## Entity Class
```java
import javax.persistence.*;

@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(name = "username", nullable = false, unique = true)
    private String username;
    
    @Column(name = "email")
    private String email;
    
    @Column(name = "age")
    private Integer age;
    
    // Constructors, getters, setters
    public User() {}
    
    public User(String username, String email) {
        this.username = username;
        this.email = email;
    }
    
    // Getters and setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    public String getUsername() { return username; }
    public void setUsername(String username) { this.username = username; }
    // ... other getters/setters
}
```

## Entity Relationships

### One-to-Many
```java
@Entity
public class Department {
    @Id
    @GeneratedValue
    private Long id;
    
    @OneToMany(mappedBy = "department", cascade = CascadeType.ALL)
    private List<Employee> employees = new ArrayList<>();
}

@Entity
public class Employee {
    @Id
    @GeneratedValue
    private Long id;
    
    @ManyToOne
    @JoinColumn(name = "department_id")
    private Department department;
}
```

### Many-to-Many
```java
@Entity
public class Student {
    @Id
    @GeneratedValue
    private Long id;
    
    @ManyToMany
    @JoinTable(
        name = "student_course",
        joinColumns = @JoinColumn(name = "student_id"),
        inverseJoinColumns = @JoinColumn(name = "course_id")
    )
    private List<Course> courses = new ArrayList<>();
}

@Entity
public class Course {
    @Id
    @GeneratedValue
    private Long id;
    
    @ManyToMany(mappedBy = "courses")
    private List<Student> students = new ArrayList<>();
}
```

## EntityManager
```java
import javax.persistence.*;

public class UserDAO {
    private EntityManagerFactory emf = 
        Persistence.createEntityManagerFactory("my-persistence-unit");
    
    public void save(User user) {
        EntityManager em = emf.createEntityManager();
        EntityTransaction transaction = em.getTransaction();
        try {
            transaction.begin();
            em.persist(user);
            transaction.commit();
        } catch (Exception e) {
            transaction.rollback();
            throw e;
        } finally {
            em.close();
        }
    }
    
    public User findById(Long id) {
        EntityManager em = emf.createEntityManager();
        try {
            return em.find(User.class, id);
        } finally {
            em.close();
        }
    }
    
    public void update(User user) {
        EntityManager em = emf.createEntityManager();
        EntityTransaction transaction = em.getTransaction();
        try {
            transaction.begin();
            em.merge(user);
            transaction.commit();
        } catch (Exception e) {
            transaction.rollback();
            throw e;
        } finally {
            em.close();
        }
    }
    
    public void delete(Long id) {
        EntityManager em = emf.createEntityManager();
        EntityTransaction transaction = em.getTransaction();
        try {
            transaction.begin();
            User user = em.find(User.class, id);
            if (user != null) {
                em.remove(user);
            }
            transaction.commit();
        } catch (Exception e) {
            transaction.rollback();
            throw e;
        } finally {
            em.close();
        }
    }
}
```

## JPQL (Java Persistence Query Language)
```java
public List<User> findByName(String name) {
    EntityManager em = emf.createEntityManager();
    try {
        TypedQuery<User> query = em.createQuery(
            "SELECT u FROM User u WHERE u.username = :name", User.class);
        query.setParameter("name", name);
        return query.getResultList();
    } finally {
        em.close();
    }
}

public List<User> findUsersOlderThan(int age) {
    EntityManager em = emf.createEntityManager();
    try {
        return em.createQuery(
            "SELECT u FROM User u WHERE u.age > :age", User.class)
            .setParameter("age", age)
            .getResultList();
    } finally {
        em.close();
    }
}
```

## Criteria API
```java
import javax.persistence.criteria.*;

public List<User> findUsers(String name, Integer minAge) {
    EntityManager em = emf.createEntityManager();
    CriteriaBuilder cb = em.getCriteriaBuilder();
    CriteriaQuery<User> query = cb.createQuery(User.class);
    Root<User> user = query.from(User.class);
    
    List<Predicate> predicates = new ArrayList<>();
    if (name != null) {
        predicates.add(cb.equal(user.get("username"), name));
    }
    if (minAge != null) {
        predicates.add(cb.greaterThan(user.get("age"), minAge));
    }
    query.where(predicates.toArray(new Predicate[0]));
    
    return em.createQuery(query).getResultList();
}
```

## Best Practices
1. Use @Entity annotation correctly
2. Define proper relationships
3. Use lazy loading appropriately
4. Handle transactions properly
5. Use JPQL for complex queries
6. Avoid N+1 query problem
7. Use connection pooling
8. Cache entities when appropriate
9. Use optimistic locking
10. Handle exceptions properly

