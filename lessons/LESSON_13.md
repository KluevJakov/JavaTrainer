# 🧠 Практика по Java: JDBC с PostgreSQL (через Docker)

---

## 🧩 Задание 1. Подготовка окружения (Docker + PostgreSQL)

**Описание:**  
Создадим контейнер с PostgreSQL и подготовим таблицу для практики.

**Что нужно сделать:**
```bash
# 1. Подними PostgreSQL в Docker
docker run --name pg-practice -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres -e POSTGRES_DB=testdb -p 5432:5432 -d postgres:16

# 2. Подключись к базе
docker exec -it pg-practice psql -U postgres -d testdb

# 3. Создай таблицу
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    department VARCHAR(50),
    salary DECIMAL(10,2),
    hire_date DATE DEFAULT CURRENT_DATE
);
```

💡 Теперь БД готова для подключения из Java по адресу:
```
jdbc:postgresql://localhost:5432/testdb
```

---

## 🧩 Задание 2. Подключение к базе данных

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class JdbcConnectExample {
    public static void main(String[] args) {
        String url = "jdbc:postgresql://localhost:5432/testdb";
        String user = "postgres";
        String password = "postgres";

        try (Connection conn = DriverManager.getConnection(url, user, password)) {
            System.out.println("✅ Подключение к PostgreSQL успешно!");
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

💡 Если подключение не удалось — проверь, что контейнер работает:  
```bash
docker ps
```

---

## 🧩 Задание 3. Добавление данных (`INSERT`)

```java
import java.math.BigDecimal;
import java.sql.*;

public class InsertExample {
    public static void main(String[] args) throws SQLException {
        String sql = "INSERT INTO employees (name, department, salary) VALUES (?, ?, ?)";

        try (Connection conn = DriverManager.getConnection(
                "jdbc:postgresql://localhost:5432/testdb", "postgres", "postgres");
             PreparedStatement ps = conn.prepareStatement(sql)) {

            ps.setString(1, "Иван Иванов");
            ps.setString(2, "IT");
            ps.setBigDecimal(3, new BigDecimal("120000"));
            ps.executeUpdate();

            System.out.println("✅ Сотрудник добавлен!");
        }
    }
}
```

---

## 🧩 Задание 4. Чтение данных (`SELECT`)

```java
import java.sql.*;

public class SelectExample {
    public static void main(String[] args) throws SQLException {
        String sql = "SELECT id, name, department, salary FROM employees";

        try (Connection conn = DriverManager.getConnection(
                "jdbc:postgresql://localhost:5432/testdb", "postgres", "postgres");
             Statement stmt = conn.createStatement();
             ResultSet rs = stmt.executeQuery(sql)) {

            while (rs.next()) {
                System.out.printf("%d | %s | %s | %.2f%n",
                        rs.getInt("id"),
                        rs.getString("name"),
                        rs.getString("department"),
                        rs.getDouble("salary"));
            }
        }
    }
}
```

---

## 🧩 Задание 5. Обновление и удаление (`UPDATE` / `DELETE`)

```java
import java.sql.*;

public class UpdateDeleteExample {
    public static void main(String[] args) throws SQLException {
        try (Connection conn = DriverManager.getConnection(
                "jdbc:postgresql://localhost:5432/testdb", "postgres", "postgres")) {

            // Обновление зарплаты
            PreparedStatement update = conn.prepareStatement(
                    "UPDATE employees SET salary = ? WHERE name = ?");
            update.setBigDecimal(1, new java.math.BigDecimal("135000"));
            update.setString(2, "Иван Иванов");
            update.executeUpdate();

            // Удаление
            PreparedStatement delete = conn.prepareStatement(
                    "DELETE FROM employees WHERE name = ?");
            delete.setString(1, "Иван Иванов");
            delete.executeUpdate();

            System.out.println("✅ Изменения применены");
        }
    }
}
```

---

## 🧩 Задание 6. Транзакции

```java
import java.sql.*;

public class TransactionExample {
    public static void main(String[] args) throws SQLException {
        try (Connection conn = DriverManager.getConnection(
                "jdbc:postgresql://localhost:5432/testdb", "postgres", "postgres")) {

            conn.setAutoCommit(false);

            try (PreparedStatement ps1 = conn.prepareStatement(
                    "UPDATE employees SET salary = salary - 5000 WHERE id = 1");
                 PreparedStatement ps2 = conn.prepareStatement(
                    "UPDATE employees SET salary = salary + 5000 WHERE id = 2")) {

                ps1.executeUpdate();
                ps2.executeUpdate();

                conn.commit();
                System.out.println("✅ Транзакция успешно выполнена");
            } catch (SQLException e) {
                conn.rollback();
                System.err.println("❌ Ошибка! Изменения отменены");
            }
        }
    }
}
```

---

## 🧩 Задание 7. DAO — чтение в Java-объекты

```java
import java.math.BigDecimal;
import java.sql.*;
import java.util.ArrayList;
import java.util.List;

public class Employee {
    private int id;
    private String name;
    private String department;
    private BigDecimal salary;

    // геттеры и сеттеры ...
}

public class EmployeeDao {
    private static final String URL = "jdbc:postgresql://localhost:5432/testdb";
    private static final String USER = "postgres";
    private static final String PASSWORD = "postgres";

    public List<Employee> getAll() throws SQLException {
        List<Employee> list = new ArrayList<>();
        try (Connection conn = DriverManager.getConnection(URL, USER, PASSWORD);
             Statement stmt = conn.createStatement();
             ResultSet rs = stmt.executeQuery("SELECT * FROM employees")) {

            while (rs.next()) {
                Employee e = new Employee();
                e.setId(rs.getInt("id"));
                e.setName(rs.getString("name"));
                e.setDepartment(rs.getString("department"));
                e.setSalary(rs.getBigDecimal("salary"));
                list.add(e);
            }
        }
        return list;
    }
}
```

---

## ✅ Цель

После выполнения этой практики ты:
- научишься **подключаться к PostgreSQL** из Java через JDBC;  
- освоишь **CRUD-операции** (`INSERT`, `SELECT`, `UPDATE`, `DELETE`);  
- научишься работать с **транзакциями**;  
- реализуешь **DAO-паттерн** для получения объектов из БД.  
