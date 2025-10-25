# 🧠 Практика по Java: Generics и Stream API

---

## 🧩 Задание 1. Введение в Generics

**Описание:**  
Generics (обобщения) позволяют писать **универсальные классы и методы**,  
которые работают с любыми типами данных, сохраняя **типовую безопасность**.

**Что нужно сделать:**
```java
public class Box<T> {
    private T value;

    public void set(T value) {
        this.value = value;
    }

    public T get() {
        return value;
    }

    public static void main(String[] args) {
        Box<String> stringBox = new Box<>();
        stringBox.set("Привет, Java!");
        System.out.println(stringBox.get());

        Box<Integer> intBox = new Box<>();
        intBox.set(42);
        System.out.println(intBox.get());
    }
}
```

---

## 🧩 Задание 2. Generics в методах

```java
public class GenericMethodExample {
    public static <T> void printTwice(T value) {
        System.out.println(value);
        System.out.println(value);
    }

    public static void main(String[] args) {
        printTwice("Привет");
        printTwice(123);
        printTwice(3.14);
    }
}
```

---

## 🧩 Задание 3. Ограниченные типы (bounded types)

```java
public class NumberBox<T extends Number> {
    private T number;

    public NumberBox(T number) {
        this.number = number;
    }

    public double doubleValue() {
        return number.doubleValue() * 2;
    }

    public static void main(String[] args) {
        NumberBox<Integer> intBox = new NumberBox<>(10);
        NumberBox<Double> doubleBox = new NumberBox<>(3.14);

        System.out.println("Удвоенный int: " + intBox.doubleValue());
        System.out.println("Удвоенный double: " + doubleBox.doubleValue());
    }
}
```

---

## 🧩 Задание 4. Wildcards (`?`)

```java
import java.util.*;

public class WildcardExample {
    public static void printList(List<?> list) {
        for (Object item : list) {
            System.out.println(item);
        }
    }

    public static void main(String[] args) {
        List<String> names = Arrays.asList("Анна", "Борис", "Сергей");
        List<Integer> numbers = Arrays.asList(1, 2, 3);

        printList(names);
        printList(numbers);
    }
}
```

---

## 🧩 Задание 5. Введение в Stream API

```java
import java.util.*;
import java.util.stream.*;

public class StreamExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Анна", "Борис", "Сергей", "Алина");

        List<String> result = names.stream()
                .filter(name -> name.startsWith("А"))
                .map(String::toUpperCase)
                .sorted()
                .collect(Collectors.toList());

        System.out.println("Результат: " + result);
    }
}
```

---

## 🧩 Задание 6. Потоки чисел

```java
import java.util.stream.*;

public class IntStreamExample {
    public static void main(String[] args) {
        int sum = IntStream.range(1, 6) // 1, 2, 3, 4, 5
                .map(x -> x * x)
                .sum();

        System.out.println("Сумма квадратов 1..5 = " + sum);
    }
}
```

---

## 🧩 Задание 7. Работа с объектами и Stream API

```java
import java.util.*;
import java.util.stream.*;

class Person {
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

public class PersonStreamExample {
    public static void main(String[] args) {
        List<Person> people = List.of(
                new Person("Анна", 25),
                new Person("Борис", 30),
                new Person("Сергей", 19)
        );

        List<String> adults = people.stream()
                .filter(p -> p.age >= 21)
                .map(p -> p.name)
                .collect(Collectors.toList());

        System.out.println("Совершеннолетние: " + adults);
    }
}
```

---

## ✅ Цель

После выполнения этих заданий ты научишься:
- создавать **обобщённые классы и методы**;
- использовать **ограниченные типы** и **wildcards**;
- понимать разницу между `T` и `?`;
- применять **Stream API** для обработки коллекций;
- использовать методы `.filter()`, `.map()`, `.collect()` и др.
