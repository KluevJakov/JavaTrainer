# 🧠 Практика по Java: анонимные классы, функциональные интерфейсы и лямбда-выражения

---

## 🧩 Задание 1. Анонимные классы

**Описание:**  
Анонимный класс — это **класс без имени**, который создаётся "на лету" для реализации интерфейса или наследования.

**Что нужно сделать:**  
1. Создай интерфейс с одним методом:
```java
interface Greeter {
    void sayHello();
}
```

2. Реализуй интерфейс через **анонимный класс**:
```java
public class AnonymousExample {
    public static void main(String[] args) {
        Greeter greeter = new Greeter() {
            @Override
            public void sayHello() {
                System.out.println("Привет из анонимного класса!");
            }
        };
        greeter.sayHello();
    }
}
```

3. Объяснение:  
   - Класс `Greeter` реализуется прямо внутри `main`.  
   - Это удобно, когда реализация нужна **один раз**.

---

## 🧩 Задание 2. Функциональные интерфейсы

**Описание:**  
Функциональный интерфейс — это интерфейс с **одним абстрактным методом**.  
Он используется как основа для лямбда-выражений.

**Что нужно сделать:**  
1. Объяви функциональный интерфейс:
```java
@FunctionalInterface
interface Printer {
    void print(String message);
}
```

2. Реализуй его обычным способом:
```java
class ConsolePrinter implements Printer {
    public void print(String message) {
        System.out.println("Сообщение: " + message);
    }
}

public class PrinterExample {
    public static void main(String[] args) {
        Printer printer = new ConsolePrinter();
        printer.print("Привет, мир!");
    }
}
```

3. Теперь реализуй через **анонимный класс**:
```java
Printer anonPrinter = new Printer() {
    @Override
    public void print(String message) {
        System.out.println("Анонимно: " + message);
    }
};
anonPrinter.print("Привет ещё раз!");
```

---

## 🧩 Задание 3. Лямбда-выражения

**Описание:**  
Лямбда — это короткий способ написать анонимный класс для функционального интерфейса.

**Что нужно сделать:**  
1. Реализуй `Printer` через лямбду:
```java
Printer lambdaPrinter = (message) -> {
    System.out.println("Лямбда: " + message);
};
lambdaPrinter.print("Это сообщение из лямбды!");
```

2. Если тело метода короткое, можно писать короче:
```java
Printer shortPrinter = message -> System.out.println("Короткая лямбда: " + message);
shortPrinter.print("Компактный вариант!");
```

---

## 🧩 Задание 4. Лямбды с параметрами и возвратом значения

**Описание:**  
Лямбда может принимать аргументы и возвращать результат, как обычный метод.

**Что нужно сделать:**  
1. Создай интерфейс:
```java
@FunctionalInterface
interface MathOperation {
    int operate(int a, int b);
}
```

2. Реализуй несколько лямбд:
```java
public class LambdaMath {
    public static void main(String[] args) {
        MathOperation sum = (a, b) -> a + b;
        MathOperation multiply = (a, b) -> a * b;
        MathOperation subtract = (a, b) -> a - b;

        System.out.println("Сумма: " + sum.operate(3, 5));
        System.out.println("Произведение: " + multiply.operate(4, 6));
        System.out.println("Разность: " + subtract.operate(10, 3));
    }
}
```

3. Обрати внимание:  
   - Не нужно писать `return`, если выражение простое.  
   - Типы `a` и `b` можно не указывать — Java сама их выводит.

---

## 🧩 Задание 5. Использование встроенных функциональных интерфейсов

**Описание:**  
Java уже содержит много функциональных интерфейсов в пакете `java.util.function`.  
Познакомься с самыми простыми.

**Что нужно сделать:**  
1. Пример с `Runnable` (тоже функциональный интерфейс):
```java
public class RunnableExample {
    public static void main(String[] args) {
        Runnable task = () -> System.out.println("Выполняется задача в отдельном потоке!");
        new Thread(task).start();
    }
}
```

2. Пример с `Comparator`:
```java
import java.util.*;

public class ComparatorExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Сергей", "Анна", "Борис");

        // Сортировка по длине имени
        names.sort((a, b) -> a.length() - b.length());

        System.out.println(names);
    }
}
```

---

## ✅ Цель

После выполнения этих заданий ты научишься:
- создавать **анонимные классы**;
- использовать **функциональные интерфейсы** с аннотацией `@FunctionalInterface`;
- писать **лямбда-выражения** разной сложности;
- использовать стандартные функциональные интерфейсы (`Runnable`, `Comparator` и др.);
- понимать, как лямбды делают код компактнее и удобнее.
