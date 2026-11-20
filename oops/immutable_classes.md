# Core Java: Immutable Classes

An **immutable class** is a class whose object state cannot be changed after the object is created.

In simple words, once an object is created, its instance variables cannot be modified.

---

## 1. Example: Java `String`

`String` is an immutable class.

```java
String s = "Hello";

s.concat(" World");

System.out.println(s);
```

**Output:**

```text
Hello
```

The `concat()` method does not modify the existing `String` object. Instead, it creates a new object.

```java
s = s.concat(" World");

System.out.println(s);
```

**Output:**

```text
Hello World
```

Here, `s` now refers to a new `String` object.

---

## 2. Rules for Creating an Immutable Class

1. Declare the class as `final` so it cannot be extended.
2. Declare all instance variables as `private`.
3. Declare all instance variables as `final`.
4. Do not provide setter methods.
5. Perform defensive copying for mutable objects.

---

## 3. Immutable Class Example with a Mutable Object

```java
class Main {
    public static void main(String[] args) {
        Dept d = new Dept("CSE", 1998);
        Employee e = new Employee("Nitish", d);

        System.out.println(e.getDept().getEst());

        d.setEst(2000);
        System.out.println(e.getDept().getEst());

        Dept d1 = e.getDept();
        d1.setEst(2000);
        System.out.println(e.getDept().getEst());
    }
}

final class Employee {
    private final String name;
    private final Dept dept;

    Employee(String name, Dept dept) {
        this.name = name;

        // Copy the mutable object during construction.
        this.dept = new Dept(dept.getName(), dept.getEst());
    }

    public String getName() {
        return name;
    }

    public Dept getDept() {
        // Return a copy instead of the original mutable object.
        return new Dept(dept.getName(), dept.getEst());
    }
}

class Dept {
    private String name;
    private int est;

    Dept(String name, int est) {
        this.name = name;
        this.est = est;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setEst(int est) {
        this.est = est;
    }

    public int getEst() {
        return est;
    }
}
```

**Output:**

```text
1998
1998
1998
```

The output remains `1998` because `Employee` creates and returns copies of the mutable `Dept` object. External changes cannot modify the state stored inside `Employee`.

---

## 4. Defensive Copying

Defensive copying is required when an immutable class contains mutable objects.

### During Construction

```java
this.dept = new Dept(dept.getName(), dept.getEst());
```

This creates a separate `Dept` object, preventing changes to the original object from affecting the `Employee`.

### When Returning the Object

```java
public Dept getDept() {
    return new Dept(dept.getName(), dept.getEst());
}
```

This prevents the caller from modifying the internal `Dept` object stored in `Employee`.

---

## 5. Important Points

- `final` on a reference variable prevents reassignment, but it does not make the referenced object immutable.
- Immutable objects are safe to share between multiple threads.
- `String`, `Integer`, `LocalDate`, and `BigInteger` are examples of immutable classes.
- Mutable objects inside an immutable class must be copied both when received and when returned.
