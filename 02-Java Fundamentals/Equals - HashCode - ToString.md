# The Java Object Contracts: equals(), hashCode(), and toString()

In Java, every class implicitly extends `java.lang.Object`. This means every object inherits three foundational methods: `toString()`, `equals()`, and `hashCode()`.

Relying on the default implementations is rarely a good idea in enterprise applications. Overriding them correctly requires adhering to strict rules known as "Contracts."

## 1. The `toString()` Contract

**Purpose:** Returns a string representation of the object. It should be concise, informative, and easy for a person to read.

### The Default Behavior

If you do not override it, `Object.toString()` returns the class name followed by the `@` symbol and the unsigned hexadecimal representation of the hash code (e.g., `Student@7a81197d`). This is usually unhelpful for debugging.

### The Contract & Best Practices

While there is no strict mathematical contract for `toString()`, the official Java documentation recommends that **all subclasses should override this method**.

- **Best Practice:** Include the class name and the values of the most important fields.
    
- **Modern Java Tip:** You can use string formatting or libraries like Apache Commons `ToStringBuilder`.
    

### Code Example

```
@Override
public String toString() {
    return "Student{" +
           "id=" + id +
           ", name='" + name + '\'' +
           '}';
}
```

## 2. The `equals()` Contract

**Purpose:** Defines **logical equality**. By default, the `==` operator only checks for _reference equality_ (do these two variables point to the exact same object in memory?). `equals()` should check if two distinct objects hold the same logical data.

### The Contract (The 5 Rules)

If you override `equals()`, your implementation MUST adhere to these five principles:

1. **Reflexive:** For any non-null reference value `x`, `x.equals(x)` must return `true`.
    
2. **Symmetric:** For any non-null reference values `x` and `y`, `x.equals(y)` must return `true` if and only if `y.equals(x)` returns `true`.
    
3. **Transitive:** For any non-null reference values `x`, `y`, and `z`, if `x.equals(y)` returns `true` and `y.equals(z)` returns `true`, then `x.equals(z)` must return `true`.
    
4. **Consistent:** Multiple invocations of `x.equals(y)` consistently return `true` or consistently return `false`, provided no information used in `equals` comparisons is modified.
    
5. **Non-Nullity:** For any non-null reference value `x`, `x.equals(null)` must return `false`.
    

### Code Example (Standard Template)

```
@Override
public boolean equals(Object obj) {
    // 1. Reflexive check (performance optimization)
    if (this == obj) return true;
    
    // 2. Null check and Type check
    if (obj == null || this.getClass() != obj.getClass()) return false;
    
    // 3. Cast to the specific type
    Student student = (Student) obj;
    
    // 4. Compare the actual fields (using Objects.equals for null-safe object comparison)
    return this.id == student.id && 
           Objects.equals(this.name, student.name);
}
```

## 3. The `hashCode()` Contract

**Purpose:** Returns an integer hash code value for the object. This is used exclusively for the benefit of hash-based data structures like `HashMap`, `HashSet`, and `Hashtable`.

### The Contract (The 3 Rules)

1. **Internal Consistency:** Whenever it is invoked on the same object more than once during an execution of a Java application, `hashCode()` must consistently return the same integer, provided no data used in `equals` comparisons is modified.
    
2. **The Golden Rule:** If two objects are equal according to the `equals(Object)` method, then calling the `hashCode()` method on each of the two objects **MUST** produce the same integer result.
    
3. **Collisions are allowed:** It is _not_ required that if two objects are unequal, their hash codes must be distinct. However, producing distinct integer results for unequal objects improves the performance of hash tables.
    

### 🚨 The Fatal Mistake

**If you override `equals()`, you MUST override `hashCode()`.** If you fail to do this, two logically equal objects will have different hash codes. If you use them as keys in a `HashMap`, the map will look in the wrong "bucket" and fail to find your data.

### Code Example

```
@Override
public int hashCode() {
    // Use java.util.Objects utility method to hash the exact same fields used in equals()
    return Objects.hash(id, name);
}
```

## Pro-Tips for Modern Java Developers

1. **Don't write them by hand!** Every modern IDE (IntelliJ IDEA, Eclipse, VS Code) has shortcuts to auto-generate `equals()`, `hashCode()`, and `toString()`.
    
2. **Project Lombok:** If you use the Lombok library, you can simply add `@Data` or `@EqualsAndHashCode` and `@ToString` annotations to the top of your class, and the compiler writes the code for you.
    
3. **Java Records (Java 14+):** If your class is just meant to carry data (a DTO), use a `record`. Records automatically generate perfect `equals()`, `hashCode()`, and `toString()` methods behind the scenes!