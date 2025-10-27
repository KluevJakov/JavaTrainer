# 🧠 Практика по Java: многопоточность (основы, `synchronized`, `volatile`, `Atomic`)

---

## 🧩 Задание 1. Создание и запуск потоков

**Описание:**  
В Java можно запускать потоки двумя способами — через наследование `Thread` или реализацию `Runnable`.

**Что нужно сделать:**  
1. Через наследование `Thread`:
```java
public class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Поток " + getName() + " запущен!");
    }

    public static void main(String[] args) {
        MyThread thread = new MyThread();
        thread.start(); // Запуск нового потока
    }
}
```

2. Через интерфейс `Runnable`:
```java
public class RunnableExample {
    public static void main(String[] args) {
        Runnable task = () -> System.out.println("Поток через Runnable работает!");
        Thread t = new Thread(task);
        t.start();
    }
}
```

3. Объяснение:  
   - `run()` — это тело потока.  
   - `start()` запускает **новый поток**, а не просто вызывает `run()`.

---

## 🧩 Задание 2. Работа нескольких потоков

**Описание:**  
Запустим несколько потоков, которые выполняются параллельно.

**Что нужно сделать:**  
```java
public class MultiThreadExample {
    public static void main(String[] args) {
        for (int i = 1; i <= 5; i++) {
            int threadNum = i;
            new Thread(() -> {
                System.out.println("Поток " + threadNum + " начал работу");
                try {
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("Поток " + threadNum + " завершился");
            }).start();
        }
    }
}
```

**Попробуй поэкспериментировать:**  
- Измени время `sleep`  
- Добавь `Thread.currentThread().getName()` для лучшего логирования  

---

## 🧩 Задание 3. Проблема гонки (race condition)

**Описание:**  
Когда несколько потоков одновременно изменяют общие данные — могут возникнуть ошибки.

**Что нужно сделать:**  
```java
public class RaceConditionExample {
    static int counter = 0;

    public static void main(String[] args) throws InterruptedException {
        Runnable increment = () -> {
            for (int i = 0; i < 1000; i++) {
                counter++; // Несинхронизированный доступ
            }
        };

        Thread t1 = new Thread(increment);
        Thread t2 = new Thread(increment);
        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println("Ожидалось 2000, а получили: " + counter);
    }
}
```

💡 **Вывод:** результат может быть **меньше 2000**, потому что `counter++` не является атомарной операцией.

---

## 🧩 Задание 4. Решение через `synchronized`

**Описание:**  
Ключевое слово `synchronized` обеспечивает **взаимное исключение** — только один поток может выполнять блок кода одновременно.

**Что нужно сделать:**  
```java
public class SyncExample {
    static int counter = 0;

    public static synchronized void increment() {
        counter++;
    }

    public static void main(String[] args) throws InterruptedException {
        Runnable task = () -> {
            for (int i = 0; i < 1000; i++) increment();
        };

        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);
        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println("Счётчик = " + counter);
    }
}
```

**Попробуй:**  
- убрать `synchronized` и посмотреть, что изменится;  
- сделать `synchronized` не метод, а блок кода:  
  ```java
  synchronized (SyncExample.class) { counter++; }
  ```

---

## 🧩 Задание 5. Ключевое слово `volatile`

**Описание:**  
`volatile` гарантирует, что переменная читается **напрямую из памяти**, а не из кэша потока.  
Это полезно, когда переменную читают/пишут **несколько потоков**, но без сложных операций.

**Что нужно сделать:**  
```java
public class VolatileExample {
    private static volatile boolean running = true;

    public static void main(String[] args) {
        Thread worker = new Thread(() -> {
            while (running) {
                // имитация работы
            }
            System.out.println("Поток остановлен!");
        });

        worker.start();

        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {}

        running = false; // сигнал потоку завершиться
    }
}
```

💡 Без `volatile` поток может не заметить изменение `running` и зациклиться навсегда.

---

## 🧩 Задание 6. `Atomic` классы

**Описание:**  
Пакет `java.util.concurrent.atomic` содержит классы для **атомарных операций** без `synchronized`.

**Что нужно сделать:**  
```java
import java.util.concurrent.atomic.AtomicInteger;

public class AtomicExample {
    static AtomicInteger counter = new AtomicInteger(0);

    public static void main(String[] args) throws InterruptedException {
        Runnable task = () -> {
            for (int i = 0; i < 1000; i++) {
                counter.incrementAndGet();
            }
        };

        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);
        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println("Результат: " + counter.get());
    }
}
```

💡 `incrementAndGet()` — атомарная операция, потокобезопасная без синхронизации.

---

## 🧩 Задание 7. Управление потоком с `join()` и `sleep()`

**Описание:**  
`join()` заставляет один поток дождаться другого, а `sleep()` — приостанавливает поток на заданное время.

**Что нужно сделать:**  
```java
public class JoinExample {
    public static void main(String[] args) throws InterruptedException {
        Thread worker = new Thread(() -> {
            try {
                System.out.println("Рабочий поток выполняется...");
                Thread.sleep(2000);
                System.out.println("Рабочий поток завершён.");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });

        worker.start();
        worker.join(); // Ждём завершения worker

        System.out.println("Главный поток продолжает работу после join()");
    }
}
```

---

## ✅ Цель

После выполнения этих заданий ты:
- научишься **создавать и запускать потоки**;
- поймёшь, что такое **race condition** и как её избежать;
- освоишь ключевые механизмы синхронизации: `synchronized`, `volatile`, `Atomic*`;
- узнаешь, как работает `join()` и `sleep()`.
