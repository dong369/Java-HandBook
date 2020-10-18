

```java
@PersistenceContext
private EntityManager entityManager;

public Session getSession() {
    return this.entityManager.unwrap(Session.class);
}

public Session getSessionDelegate() {
    return (Session) entityManager.getDelegate();
}
```





