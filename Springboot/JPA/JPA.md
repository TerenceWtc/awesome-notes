## QA

### Two-way One-To-Many

Situation: One `Group` can have multiple `User`, and one `User` can have one `Group`

Problems:
- Hibernate Lazy Initialization
```
org.hibernate.LazyInitializationException: failed to lazily initialize a collection of role: org.terence.backend.dao.entity.admin.Group.user, could not initialize proxy - no Session
```
Once I do some Junit test, the error occcured.  
The param `fetch = FetchType.EAGER` in @OneToMany annotation is required.
- Stack Overflow
```
java.lang.StackOverflowError
	at java.util.Date.getYear(Date.java:651)
	at java.sql.Timestamp.toString(Timestamp.java:279)
	at java.lang.String.valueOf(String.java:2994)
	at java.lang.StringBuilder.append(StringBuilder.java:131)
	at org.terence.backend.dao.entity.admin.User.toString(User.java:13)
	at java.lang.String.valueOf(String.java:2994)
	at java.lang.StringBuilder.append(StringBuilder.java:131)
	at java.util.AbstractCollection.toString(AbstractCollection.java:462)
	at org.hibernate.collection.internal.PersistentBag.toString(PersistentBag.java:538)
	at java.lang.String.valueOf(String.java:2994)
	at java.lang.StringBuilder.append(StringBuilder.java:131)
	at org.terence.backend.dao.entity.admin.Group.toString(Group.java:14)
	at java.lang.String.valueOf(String.java:2994)
	at java.lang.StringBuilder.append(StringBuilder.java:131)
	at org.terence.backend.dao.entity.admin.User.toString(User.java:13)
	at java.lang.String.valueOf(String.java:2994)
	at java.lang.StringBuilder.append(StringBuilder.java:131)...
```
Once I call the method toString, the error occcured.  
`Group` has a reference in `User`, and `User` has a reference in `Group`.  
The `@ToString(exclude = "group")` annotation is required in two classed to exclude the reference fields.

Class User:
```
@Data
@ToString(exclude = "group")
@Entity(name = "base_user")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;

    // omitting some other fields

    @ManyToOne
    @JoinColumn(name = "group_id")
    private Group group;
}
```

Class Group:
```
@Data
@ToString(exclude = "user")
@Entity(name = "base_group")
public class Group {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;

    // omitting some other fields

    // fetch = FetchType.EAGER is required (May be only in Junit Test)
    @OneToMany(mappedBy = "group", fetch = FetchType.EAGER)
    private List<User> user;
}
```

Unit test:
```
@Test
public void test() {
    long userId = 5L;
    List<Group> group = groupRepository.findAll((Root<Group> root, CriteriaQuery<?> query, CriteriaBuilder builder) -> {
        List<Predicate> predicateList = new ArrayList<>();
        Join<Group, User> join = root.join("user", JoinType.LEFT);
        predicateList.add(builder.le(root.get("id"), userId));
        Predicate[] predicates = new Predicate[predicateList.size()];
        query.where(predicateList.toArray(predicates));
        return builder.and(predicateList.toArray(predicates));
    });

    // if there's no @ToString(exclude = "group") annotation
    // ok
    group.forEach(item -> System.out.println(item.getId()));
    // throw stackoverflow exception
    group.forEach(System.out::println);
}
```
