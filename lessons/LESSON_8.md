# 🧠 Практика по Java: чтение и запись файлов

---

## 🧩 Задание 1. Запись текста в файл

**Описание:**  
Научись создавать и записывать текст в файл с помощью класса `FileWriter`.

**Что нужно сделать:**  
1. Создай новый файл и запиши в него текст:
```java
import java.io.FileWriter;
import java.io.IOException;

public class WriteToFileExample {
    public static void main(String[] args) {
        try (FileWriter writer = new FileWriter("notes.txt")) {
            writer.write("Привет, Java!\nЭто запись в файл.");
            System.out.println("Данные успешно записаны в файл.");
        } catch (IOException e) {
            System.out.println("Ошибка при записи: " + e.getMessage());
        }
    }
}
```

2. Проверь, что файл `notes.txt` появился в папке проекта и содержит твой текст.

---

## 🧩 Задание 2. Чтение из файла построчно

**Описание:**  
Познакомься с классом `BufferedReader`, который позволяет читать файл **построчно**.

**Что нужно сделать:**  
1. Прочитай созданный файл и выведи его содержимое:
```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class ReadFromFileExample {
    public static void main(String[] args) {
        try (BufferedReader reader = new BufferedReader(new FileReader("notes.txt"))) {
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            System.out.println("Ошибка при чтении файла: " + e.getMessage());
        }
    }
}
```

---

## 🧩 Задание 3. Запись с добавлением в конец файла

**Описание:**  
Иногда нужно не перезаписывать файл, а **дописывать** данные в конец. Для этого есть флаг `append = true`.

**Что нужно сделать:**  
1. Измени запись, чтобы она не перезаписывала файл:
```java
try (FileWriter writer = new FileWriter("notes.txt", true)) {
    writer.write("\nДополнительная строка!");
    System.out.println("Строка добавлена в конец файла.");
} catch (IOException e) {
    System.out.println("Ошибка при записи: " + e.getMessage());
}
```

---

## 🧩 Задание 4. Копирование содержимого из одного файла в другой

**Описание:**  
Научись копировать данные, читая и записывая их по строкам.

**Что нужно сделать:**  
1. Создай файл `copy.txt` и скопируй в него содержимое `notes.txt`:
```java
import java.io.*;

public class CopyFileExample {
    public static void main(String[] args) {
        try (BufferedReader reader = new BufferedReader(new FileReader("notes.txt"));
             BufferedWriter writer = new BufferedWriter(new FileWriter("copy.txt"))) {

            String line;
            while ((line = reader.readLine()) != null) {
                writer.write(line);
                writer.newLine();
            }
            System.out.println("Файл успешно скопирован!");
        } catch (IOException e) {
            System.out.println("Ошибка при копировании: " + e.getMessage());
        }
    }
}
```

---

## 🧩 Задание 5. Работа с бинарными файлами (байтовый поток)

**Описание:**  
Познакомься с классами `FileInputStream` и `FileOutputStream` для работы с **не текстовыми** файлами.

**Что нужно сделать:**  
1. Скопируй изображение или другой бинарный файл:
```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class CopyBinaryFile {
    public static void main(String[] args) {
        try (FileInputStream in = new FileInputStream("image.jpg");
             FileOutputStream out = new FileOutputStream("image_copy.jpg")) {

            int byteData;
            while ((byteData = in.read()) != -1) {
                out.write(byteData);
            }
            System.out.println("Бинарный файл успешно скопирован!");
        } catch (IOException e) {
            System.out.println("Ошибка при копировании: " + e.getMessage());
        }
    }
}
```

---

## 🧩 Задание 6. Использование `Files` из `java.nio`

**Описание:**  
Современный способ работы с файлами — через утилитный класс `Files`.

**Что нужно сделать:**  
1. Прочитай весь файл одной строкой:
```java
import java.nio.file.Files;
import java.nio.file.Path;
import java.io.IOException;

public class FilesExample {
    public static void main(String[] args) throws IOException {
        String content = Files.readString(Path.of("notes.txt"));
        System.out.println(content);
    }
}
```

2. Или запиши в файл сразу целую строку:
```java
Files.writeString(Path.of("notes2.txt"), "Содержимое через NIO API!");
```

---

## ✅ Цель

После выполнения этих заданий ты научишься:
- создавать и записывать текст в файлы (`FileWriter`, `BufferedWriter`);
- читать файлы построчно (`BufferedReader`);
- дописывать данные в конец файла;
- копировать текстовые и бинарные файлы;
- использовать современный API `java.nio.file.Files`.
