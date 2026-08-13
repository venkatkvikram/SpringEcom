# SpringEcom

## Why `ProductRepo` is an interface

`ProductRepo` is an interface because Spring Data JPA creates the concrete repository implementation automatically when the application starts. You define what entity the repository manages and the type of its primary key:

```java
public interface ProductRepo extends JpaRepository<Product, Integer> {
}
```

Here, `Product` is the entity and `Integer` is the type of its `id` field. Spring creates an implementation at runtime and makes it available for dependency injection, so there is no need to write a repository class yourself for standard database operations.

Extending `JpaRepository` already provides common CRUD methods, including:

- `save(product)`
- `findById(id)`
- `findAll()`
- `deleteById(id)`

Therefore, do **not** redeclare `findAll()` with a method body in `ProductRepo`. This is invalid in a normal interface, and it duplicates the method Spring Data JPA already provides.

```java
// Incorrect
public List<Product> findAll() {
}
```

For custom queries, declare only the method signature. Spring Data JPA reads the method name and generates the query implementation.

```java
public interface ProductRepo extends JpaRepository<Product, Integer> {
    List<Product> findByBrand(String brand);
    List<Product> findByProductAvailableTrue();
}
```

If a custom query is too complex to express with a method name, use `@Query` on an interface method instead.
