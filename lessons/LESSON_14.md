# 🧠 Практика по Java: паттерны проектирования (GoF)

---

## 🧩 Задание 1. Порождающий паттерн — Singleton (Одиночка)

**Описание:**  
Паттерн Singleton гарантирует, что у класса будет **только один экземпляр**, и предоставляет глобальную точку доступа к нему.

**Что нужно сделать:**  
1. Реализуй класс Singleton:
```java
class Database {
    private static Database instance;

    private Database() {
        System.out.println("Подключение к базе данных...");
    }

    public static Database getInstance() {
        if (instance == null) {
            instance = new Database();
        }
        return instance;
    }
}
```
2. Проверь, что создаётся только один объект:
```java
public class Main {
    public static void main(String[] args) {
        Database db1 = Database.getInstance();
        Database db2 = Database.getInstance();

        System.out.println(db1 == db2); // true
    }
}
```

---

## 🧩 Задание 2. Улучшенный Singleton (ленивая и потокобезопасная инициализация)

**Описание:**  
Улучшенная версия Одиночки использует `synchronized` или внутренний статический класс для потокобезопасности.

**Что нужно сделать:**  
1. Реализуй потокобезопасный вариант:
```java
class SafeDatabase {
    private SafeDatabase() {}

    private static class Holder {
        private static final SafeDatabase INSTANCE = new SafeDatabase();
    }

    public static SafeDatabase getInstance() {
        return Holder.INSTANCE;
    }
}
```
2. Проверь, что при многопоточности создаётся один объект:
```java
Runnable task = () -> System.out.println(SafeDatabase.getInstance());
new Thread(task).start();
new Thread(task).start();
```

---

## 🧩 Задание 3. Структурный паттерн — Adapter (Адаптер)

**Описание:**  
Адаптер позволяет использовать несовместимые интерфейсы, «оборачивая» один в другой.

**Что нужно сделать:**  
1. Допустим, у нас есть старый принтер:
```java
class OldPrinter {
    void printOld(String text) {
        System.out.println("Старый принтер: " + text);
    }
}
```
2. Создай адаптер для нового интерфейса:
```java
interface Printer {
    void print(String text);
}

class PrinterAdapter implements Printer {
    private final OldPrinter oldPrinter = new OldPrinter();

    @Override
    public void print(String text) {
        oldPrinter.printOld(text);
    }
}
```
3. Используй адаптер:
```java
public class Main {
    public static void main(String[] args) {
        Printer printer = new PrinterAdapter();
        printer.print("Hello, Adapter!");
    }
}
```

---

## 🧩 Задание 4. Структурный паттерн — Decorator (Декоратор)

**Описание:**  
Декоратор добавляет функциональность объекту **без изменения его кода**.

**Что нужно сделать:**  
1. Создай базовый интерфейс и реализацию:
```java
interface Coffee {
    String getDescription();
    double getCost();
}

class SimpleCoffee implements Coffee {
    public String getDescription() { return "Обычный кофе"; }
    public double getCost() { return 5.0; }
}
```
2. Добавь декоратор (например, молоко):
```java
class MilkDecorator implements Coffee {
    private final Coffee coffee;

    MilkDecorator(Coffee coffee) {
        this.coffee = coffee;
    }

    public String getDescription() {
        return coffee.getDescription() + " с молоком";
    }

    public double getCost() {
        return coffee.getCost() + 2.0;
    }
}
```
3. Проверь:
```java
public class Main {
    public static void main(String[] args) {
        Coffee coffee = new MilkDecorator(new SimpleCoffee());
        System.out.println(coffee.getDescription() + " — " + coffee.getCost() + "₽");
    }
}
```

---

## 🧩 Задание 5. Поведенческий паттерн — Strategy (Стратегия)

**Описание:**  
Позволяет менять поведение объекта **во время выполнения программы**.

**Что нужно сделать:**  
1. Создай стратегию:
```java
interface PaymentStrategy {
    void pay(int amount);
}

class CardPayment implements PaymentStrategy {
    public void pay(int amount) {
        System.out.println("Оплата картой: " + amount + "₽");
    }
}

class CashPayment implements PaymentStrategy {
    public void pay(int amount) {
        System.out.println("Оплата наличными: " + amount + "₽");
    }
}
```
2. Класс контекста:
```java
class Checkout {
    private PaymentStrategy strategy;

    public void setStrategy(PaymentStrategy strategy) {
        this.strategy = strategy;
    }

    public void pay(int amount) {
        strategy.pay(amount);
    }
}
```
3. Используй разные стратегии:
```java
public class Main {
    public static void main(String[] args) {
        Checkout checkout = new Checkout();
        checkout.setStrategy(new CardPayment());
        checkout.pay(100);

        checkout.setStrategy(new CashPayment());
        checkout.pay(50);
    }
}
```

---

## 🧩 Задание 6. Поведенческий паттерн — Observer (Наблюдатель)

**Описание:**  
Наблюдатель позволяет объектам автоматически уведомляться об изменениях состояния другого объекта.

**Что нужно сделать:**  
1. Создай интерфейсы:
```java
interface Observer {
    void update(String message);
}

interface Subject {
    void addObserver(Observer o);
    void removeObserver(Observer o);
    void notifyObservers(String message);
}
```
2. Реализуй класс рассылки:
```java
import java.util.*;

class NewsPublisher implements Subject {
    private final List<Observer> observers = new ArrayList<>();

    public void addObserver(Observer o) { observers.add(o); }
    public void removeObserver(Observer o) { observers.remove(o); }

    public void notifyObservers(String message) {
        for (Observer o : observers) o.update(message);
    }
}
```
3. И класс подписчика:
```java
class Subscriber implements Observer {
    private final String name;
    public Subscriber(String name) { this.name = name; }

    public void update(String message) {
        System.out.println(name + " получил уведомление: " + message);
    }
}
```
4. Проверь:
```java
public class Main {
    public static void main(String[] args) {
        NewsPublisher publisher = new NewsPublisher();
        Subscriber alice = new Subscriber("Алиса");
        Subscriber bob = new Subscriber("Боб");

        publisher.addObserver(alice);
        publisher.addObserver(bob);
        publisher.notifyObservers("Новая статья о паттернах!");
    }
}
```

---

## ✅ Цель

После выполнения этих заданий ты:
- поймёшь три ключевые группы паттернов GoF: **порождающие**, **структурные**, **поведенческие**;
- научишься реализовывать и комбинировать их на практике;
- закрепишь базовые концепции: **инкапсуляция поведения**, **обёртывание объектов** и **гибкое создание экземпляров**.
