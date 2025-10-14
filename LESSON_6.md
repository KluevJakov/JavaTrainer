# 🧠 Практика по Java: наследование, полиморфизм, static и final

---

## 🧩 Задание 1. Наследование

**Описание:**  
Познакомься с механизмом наследования в Java.

**Что нужно сделать:**  
1. Создай базовый класс `Animal` с полем:
```java
String name;
```
2. Добавь в `Animal` метод:
```java
void makeSound() {
    System.out.println("Животное издает звук");
}
```
3. Создай подклассы `Dog` и `Cat`, которые **наследуют** `Animal`.
4. В каждом подклассе **переопредели** метод `makeSound()`:
```java
class Dog extends Animal {
    void makeSound() {
        System.out.println(name + " лает");
    }
}

class Cat extends Animal {
    void makeSound() {
        System.out.println(name + " мяукает");
    }
}
```
5. В `main` создай объекты и вызови их методы:
```java
Dog dog = new Dog();
dog.name = "Бим";
dog.makeSound();

Cat cat = new Cat();
cat.name = "Мурка";
cat.makeSound();
```

---

## 🧩 Задание 2. Динамический полиморфизм (переопределение)

**Описание:**  
Пойми, как работает полиморфизм при использовании ссылок базового типа.

**Что нужно сделать:**  
1. Создай массив типа `Animal[]`, который содержит разных животных:
```java
Animal[] animals = { new Dog(), new Cat(), new Dog() };
```
2. Задай имена и вызови `makeSound()` в цикле:
```java
animals[0].name = "Шарик";
animals[1].name = "Снежок";
animals[2].name = "Тузик";

for (Animal a : animals) {
    a.makeSound(); // Вызовется метод из конкретного подкласса!
}
```
3. Убедись, что работает **динамическое связывание** (переопределенные методы вызываются корректно).

---

## 🧩 Задание 3. Статический полиморфизм (перегрузка методов)

**Описание:**  
Освой перегрузку методов — статический (на этапе компиляции) полиморфизм.

**Что нужно сделать:**  
1. В классе `Calculator` создай несколько методов `sum`:
```java
class Calculator {
    int sum(int a, int b) {
        return a + b;
    }

    double sum(double a, double b) {
        return a + b;
    }

    int sum(int a, int b, int c) {
        return a + b + c;
    }
}
```
2. В `main` протестируй их:
```java
Calculator calc = new Calculator();
System.out.println(calc.sum(2, 3));
System.out.println(calc.sum(2.5, 4.3));
System.out.println(calc.sum(1, 2, 3));
```

---

## 🧩 Задание 4. static поля и методы

**Описание:**  
Разбери, как использовать `static` для общих данных и методов класса.

**Что нужно сделать:**  
1. Создай класс `Counter`:
```java
class Counter {
    static int count = 0;

    Counter() {
        count++;
    }

    static void showCount() {
        System.out.println("Создано объектов: " + count);
    }
}
```
2. В `main` создай несколько объектов:
```java
new Counter();
new Counter();
Counter.showCount(); // должно вывести: "Создано объектов: 2"
```
3. Обрати внимание: `count` и `showCount()` принадлежат **всему классу**, а не отдельным объектам.

---

## 🧩 Задание 5. final класс, метод и переменная

**Описание:**  
Пойми, как работает модификатор `final`.

**Что нужно сделать:**  
1. Создай `final` класс:
```java
final class Constants {
    static final double PI = 3.14159;
}
```
2. Попробуй создать подкласс от `Constants` — убедись, что компилятор не разрешает.
3. Добавь класс `Shape` с `final` методом:
```java
class Shape {
    final void draw() {
        System.out.println("Рисуется фигура");
    }
}
```
4. Создай класс `Circle`, который **наследует** `Shape`, и попробуй переопределить `draw()` — компилятор выдаст ошибку.
5. Используй `Constants.PI` в коде:
```java
System.out.println("Площадь круга радиуса 3: " + Constants.PI * 3 * 3);
```

---

## ✅ Цель

После выполнения этих заданий ты научишься:
- использовать **наследование** и **переопределение**;
- различать **статический** и **динамический полиморфизм**;
- понимать, зачем нужны `static` и `final`;
- писать более гибкий и безопасный код в стиле ООП.
