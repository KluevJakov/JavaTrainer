# 🧠 Практика по Java: коллекции (без Stream API)

---

## 🧩 Задание 1. Списки (`List`)

**Описание:**  
`List` — это упорядоченная коллекция, где элементы имеют индексы.  
Наиболее часто используются реализации — `ArrayList` и `LinkedList`.

**Что нужно сделать:**  
1. Создай список и добавь несколько элементов:
```java
import java.util.*;

public class ListExample {
    public static void main(String[] args) {
        List<String> names = new ArrayList<>();
        names.add("Анна");
        names.add("Борис");
        names.add("Сергей");

        System.out.println("Список имён: " + names);
    }
}
```

2. Удали элемент и вставь новый:
```java
names.remove("Борис");
names.add(1, "Мария");
System.out.println("После изменений: " + names);
```

3. Обойди список с помощью `for`:
```java
for (String name : names) {
    System.out.println("Имя: " + name);
}
```

---

## 🧩 Задание 2. Множества (`Set`)

**Описание:**  
`Set` — это коллекция, которая **не допускает дубликатов**.  
Порядок элементов не гарантируется (в `HashSet`).

**Что нужно сделать:**
```java
import java.util.*;

public class SetExample {
    public static void main(String[] args) {
        Set<Integer> numbers = new HashSet<>();
        numbers.add(5);
        numbers.add(3);
        numbers.add(5); // дубликат — не добавится

        System.out.println("Множество: " + numbers);
        System.out.println("Содержит 3? " + numbers.contains(3));
    }
}
```

3. Попробуй заменить `HashSet` на `TreeSet`:
```java
Set<Integer> sorted = new TreeSet<>(numbers);
System.out.println("Отсортированное множество: " + sorted);
```

---

## 🧩 Задание 3. Отображения (`Map`)

**Описание:**  
`Map` хранит пары **ключ–значение**.  
Популярные реализации — `HashMap`, `LinkedHashMap`, `TreeMap`.

**Что нужно сделать:**
```java
import java.util.*;

public class MapExample {
    public static void main(String[] args) {
        Map<String, Integer> ages = new HashMap<>();
        ages.put("Анна", 25);
        ages.put("Борис", 30);
        ages.put("Сергей", 28);

        System.out.println("Возраст Анны: " + ages.get("Анна"));

        for (String key : ages.keySet()) {
            System.out.println(key + " — " + ages.get(key));
        }
    }
}
```

---

## 🧩 Задание 4. Очередь (`Queue`)

**Описание:**  
`Queue` — это структура данных **FIFO (First In – First Out)**.  
Элементы извлекаются в том порядке, в котором были добавлены.

**Что нужно сделать:**
```java
import java.util.*;

public class QueueExample {
    public static void main(String[] args) {
        Queue<String> tasks = new LinkedList<>();

        tasks.add("Проснуться");
        tasks.add("Позавтракать");
        tasks.add("Учить Java");

        System.out.println("Очередь задач: " + tasks);

        System.out.println("Выполняется: " + tasks.poll()); // извлекает первый элемент
        System.out.println("Следующая задача: " + tasks.peek()); // не удаляет
    }
}
```

---

## 🧩 Задание 5. Реализация собственного стека 🧱

**Описание:**  
`Stack` — это структура данных **LIFO (Last In – First Out)**.  
Создай свой собственный стек на основе `ArrayList`.

**Что нужно сделать:**
```java
import java.util.*;

class MyStack<T> {
    private List<T> elements = new ArrayList<>();

    public void push(T item) {
        elements.add(item);
    }

    public T pop() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        return elements.remove(elements.size() - 1);
    }

    public T peek() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        return elements.get(elements.size() - 1);
    }

    public boolean isEmpty() {
        return elements.isEmpty();
    }

    public int size() {
        return elements.size();
    }

    @Override
    public String toString() {
        return elements.toString();
    }
}

public class StackExample {
    public static void main(String[] args) {
        MyStack<Integer> stack = new MyStack<>();
        stack.push(10);
        stack.push(20);
        stack.push(30);

        System.out.println("Стек: " + stack);
        System.out.println("Верхний элемент: " + stack.peek());

        System.out.println("Извлечено: " + stack.pop());
        System.out.println("После pop: " + stack);
    }
}
```

**Пояснение:**
- `push()` — добавляет элемент в стек.  
- `pop()` — извлекает последний добавленный элемент.  
- `peek()` — смотрит верхний элемент, не удаляя его.  
- `isEmpty()` и `size()` — служебные методы.

---

## 🧩 Задание 6. Итераторы

**Описание:**  
`Iterator` позволяет проходить коллекцию вручную.

**Что нужно сделать:**
```java
import java.util.*;

public class IteratorExample {
    public static void main(String[] args) {
        List<String> fruits = Arrays.asList("Яблоко", "Банан", "Апельсин");

        Iterator<String> it = fruits.iterator();
        while (it.hasNext()) {
            String fruit = it.next();
            System.out.println("Фрукт: " + fruit);
        }
    }
}
```

---

## ✅ Цель

После выполнения этих заданий ты научишься:
- работать с основными коллекциями: `List`, `Set`, `Map`, `Queue`;
- использовать итераторы;
- понимать принципы **FIFO** и **LIFO**;
- **самостоятельно реализовывать стек**;
- разбираться в различиях между коллекциями.
