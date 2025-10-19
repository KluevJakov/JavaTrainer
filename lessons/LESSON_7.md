# 🧠 Практика по Java: обработка исключений и собственные исключения

---

## 🧩 Задание 1. Базовая обработка исключений (try-catch)

**Описание:**  
Познакомься с конструкцией `try-catch`, которая позволяет перехватывать ошибки во время выполнения программы.

**Что нужно сделать:**  
1. Напиши код, который делит одно число на другое:
```java
int a = 10;
int b = 0;
System.out.println(a / b);
```
2. Оберни этот код в блок `try-catch`, чтобы программа не завершалась с ошибкой:
```java
try {
    System.out.println(a / b);
} catch (ArithmeticException e) {
    System.out.println("Ошибка: деление на ноль!");
}
```
3. Добавь `finally`, чтобы увидеть, что он выполняется всегда:
```java
finally {
    System.out.println("Операция завершена.");
}
```

---

## 🧩 Задание 2. Множественные catch-блоки

**Описание:**  
Научись обрабатывать разные типы исключений по-разному.

**Что нужно сделать:**  
1. Напиши метод, который может вызвать несколько исключений:
```java
void riskyMethod(String text) {
    System.out.println(text.length()); // может вызвать NullPointerException
    int num = Integer.parseInt(text);  // может вызвать NumberFormatException
    System.out.println("Число: " + num);
}
```
2. В `main` вызови этот метод в блоке `try-catch` с несколькими `catch`:
```java
try {
    riskyMethod(null);
} catch (NullPointerException e) {
    System.out.println("Ошибка: передана пустая ссылка!");
} catch (NumberFormatException e) {
    System.out.println("Ошибка: невозможно преобразовать в число!");
}
```

---

## 🧩 Задание 3. Проброс исключений (throws)

**Описание:**  
Пойми, как метод может передавать ответственность за обработку исключений дальше.

**Что нужно сделать:**  
1. Напиши метод, который **пробрасывает** исключение:
```java
void readFile(String filename) throws IOException {
    FileReader reader = new FileReader(filename);
    reader.read();
    reader.close();
}
```
2. В `main` вызови его с обработкой:
```java
try {
    readFile("data.txt");
} catch (IOException e) {
    System.out.println("Ошибка при чтении файла: " + e.getMessage());
}
```

---

## 🧩 Задание 4. Создание собственного исключения

**Описание:**  
Научись создавать собственные типы исключений для бизнес-логики.

**Что нужно сделать:**  
1. Создай класс собственного исключения:
```java
class NegativeBalanceException extends Exception {
    public NegativeBalanceException(String message) {
        super(message);
    }
}
```
2. Напиши класс `BankAccount` с методом снятия средств:
```java
class BankAccount {
    private int balance = 100;

    void withdraw(int amount) throws NegativeBalanceException {
        if (amount > balance) {
            throw new NegativeBalanceException("Недостаточно средств на счете!");
        }
        balance -= amount;
        System.out.println("Снято: " + amount + ", остаток: " + balance);
    }
}
```
3. В `main` протестируй работу исключения:
```java
try {
    BankAccount account = new BankAccount();
    account.withdraw(200);
} catch (NegativeBalanceException e) {
    System.out.println("Ошибка: " + e.getMessage());
}
```

---

## 🧩 Задание 5. Объединение: try-with-resources

**Описание:**  
Познакомься с автоматическим закрытием ресурсов в Java.

**Что нужно сделать:**  
1. Используй конструкцию `try-with-resources`:
```java
try (FileWriter writer = new FileWriter("output.txt")) {
    writer.write("Hello, world!");
    System.out.println("Файл успешно записан.");
} catch (IOException e) {
    System.out.println("Ошибка при работе с файлом: " + e.getMessage());
}
```
2. Проверь, что ресурс закрывается автоматически даже при исключении.

---

## ✅ Цель

После выполнения этих заданий ты научишься:
- обрабатывать исключения с помощью `try-catch-finally`;
- различать типы исключений;
- пробрасывать исключения с помощью `throws`;
- создавать **собственные исключения**;
- использовать **try-with-resources** для безопасной работы с файлами.
