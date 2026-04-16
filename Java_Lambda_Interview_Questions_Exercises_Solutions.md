# Java Lambda Expressions -- Interview Questions, Exercises, and Solutions

## 📌 Introduction

This document provides a comprehensive collection of **interview
questions**, **hands-on exercises**, and their **detailed solutions**
related to **Java Lambda Expressions** and **JVM internals**. It is
ideal for interview preparation, teaching, and self-study.

------------------------------------------------------------------------
• Java 8 Let you Write code in Declarative way(**What to Do**) rather imperative way(** How to Do**).
<img width="653" height="25" alt="image" src="https://github.com/user-attachments/assets/49c38ef5-2adb-461d-afb9-58ffa501e0d0" />
<img width="1284" height="209" alt="image" src="https://github.com/user-attachments/assets/b39ebe6c-fec4-4017-b6bb-5ac786f3c77f" />

## 🟢 1. Beginner-Level Interview Questions

1.  What is a lambda expression in Java?
2.  In which Java version were lambda expressions introduced?
3.  What is a functional interface?
4.  Why is the `@FunctionalInterface` annotation used?
5.  What is the basic syntax of a lambda expression?
6.  Can a lambda expression exist without a functional interface?
7.  What is the difference between a lambda expression and an anonymous
    inner class?
8.  Name some commonly used functional interfaces in
    `java.util.function`.
9.  What is a method reference?
10. When should you prefer a method reference over a lambda expression?

1) What is Lambda Expression? 
Ans: Lambda Expressions are the shorthand way for writing small pieces of behavior . 
        • No need to write an entire class. 
------------------------------------------------------------------------

## 🟡 2. Intermediate-Level Interview Questions

1.  Explain the concept of "effectively final" variables.
2.  How does the `this` keyword behave in lambdas compared to anonymous
    classes?
3.  Can lambda expressions throw checked exceptions?
4.  What are the different types of method references?
5.  What is variable capture in lambda expressions?
6.  Are lambda expressions serializable?
7.  How are lambda expressions used in the Streams API?
8.  What are default and static methods in functional interfaces?
9.  Can a functional interface extend another interface?
10. How do lambda expressions improve readability and maintainability?

------------------------------------------------------------------------

## 🔴 3. Advanced-Level Interview Questions (JVM Internals)

1.  How are lambda expressions implemented at the bytecode level?
2.  What is the role of the `invokedynamic` instruction?
3.  What is `java.lang.invoke.LambdaMetafactory`?
4.  Do lambda expressions generate separate `.class` files?
5.  What are synthetic methods like `lambda$main$0`?
6.  What is the difference between stateless and stateful lambdas?
7.  How does the JVM optimize lambda expressions?
8.  What are hidden classes introduced in Java 15?
9.  Explain the bootstrap method used with `invokedynamic`.
10. Compare the memory footprint of lambdas and anonymous classes.

------------------------------------------------------------------------

## 🧪 4. Hands-On Exercises with Solutions

### ✅ Exercise 1: Basic Arithmetic Operations

**Problem:** Implement arithmetic operations using a functional
interface.

``` java
@FunctionalInterface
interface Calculator {
    int operate(int a, int b);
}

public class CalculatorDemo {
    public static void main(String[] args) {
        Calculator add = (a, b) -> a + b;
        Calculator subtract = (a, b) -> a - b;
        Calculator multiply = (a, b) -> a * b;
        Calculator divide = (a, b) -> {
            if (b == 0) throw new ArithmeticException("Cannot divide by zero");
            return a / b;
        };

        System.out.println(add.operate(10, 5));
        System.out.println(subtract.operate(10, 5));
        System.out.println(multiply.operate(10, 5));
        System.out.println(divide.operate(10, 5));
    }
}
```

------------------------------------------------------------------------

### ✅ Exercise 2: Employee Filtering and Grouping

``` java
import java.util.*;
import java.util.stream.Collectors;

class Employee {
    private String name;
    private double salary;
    private String department;

    public Employee(String name, double salary, String department) {
        this.name = name;
        this.salary = salary;
        this.department = department;
    }

    public String getName() { return name; }
    public double getSalary() { return salary; }
    public String getDepartment() { return department; }
}

public class EmployeeDemo {
    public static void main(String[] args) {
        List<Employee> employees = Arrays.asList(
            new Employee("Amit", 60000, "IT"),
            new Employee("Raj", 45000, "HR"),
            new Employee("Neha", 75000, "IT"),
            new Employee("Priya", 50000, "Finance"),
            new Employee("Vikas", 82000, "Finance")
        );

        List<Employee> highEarners = employees.stream()
                .filter(e -> e.getSalary() > 50000)
                .collect(Collectors.toList());

        List<Employee> sortedByName = employees.stream()
                .sorted(Comparator.comparing(Employee::getName))
                .collect(Collectors.toList());

        Map<String, List<Employee>> groupedByDept =
                employees.stream()
                .collect(Collectors.groupingBy(Employee::getDepartment));

        System.out.println(highEarners);
        System.out.println(sortedByName);
        System.out.println(groupedByDept);
    }
}
```

------------------------------------------------------------------------

### ✅ Exercise 3: Predicate Chaining

``` java
import java.util.function.Predicate;
import java.util.*;

public class PredicateDemo {
    public static void main(String[] args) {
        Predicate<Integer> greaterThan10 = x -> x > 10;
        Predicate<Integer> isEven = x -> x % 2 == 0;

        Predicate<Integer> combined = greaterThan10.and(isEven);

        List<Integer> numbers = Arrays.asList(5, 12, 18, 7, 20, 3, 14);
        numbers.stream()
               .filter(combined)
               .forEach(System.out::println);
    }
}
```

------------------------------------------------------------------------

### ✅ Exercise 4: Constructor Reference

``` java
import java.util.function.Function;

class Person {
    private String name;

    public Person(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Person{name='" + name + "'}";
    }
}

public class ConstructorReferenceDemo {
    public static void main(String[] args) {
        Function<String, Person> personCreator = Person::new;
        Person p1 = personCreator.apply("Amit");
        Person p2 = personCreator.apply("Neha");
        System.out.println(p1);
        System.out.println(p2);
    }
}
```

------------------------------------------------------------------------

### ✅ Exercise 5: Stream Processing Pipeline

``` java
import java.util.*;
import java.util.stream.Collectors;

public class StreamPipelineDemo {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Amit", "Raj", "Anita", "John", "Aman");

        List<String> result = names.stream()
                .filter(name -> name.startsWith("A"))
                .map(String::toUpperCase)
                .sorted()
                .collect(Collectors.toList());

        System.out.println(result);
    }
}
```

------------------------------------------------------------------------

### ✅ Exercise 6: Handling Checked Exceptions

``` java
import java.util.function.Consumer;
import java.util.*;

@FunctionalInterface
interface ThrowingConsumer<T> {
    void accept(T t) throws Exception;
}

public class ExceptionHandlingDemo {
    public static <T> Consumer<T> wrap(ThrowingConsumer<T> throwingConsumer) {
        return i -> {
            try {
                throwingConsumer.accept(i);
            } catch (Exception e) {
                throw new RuntimeException(e);
            }
        };
    }

    public static void main(String[] args) {
        List<String> files = Arrays.asList("file1.txt", "file2.txt");
        files.forEach(wrap(file -> {
            if (file.contains("2")) {
                throw new Exception("Error processing " + file);
            }
            System.out.println("Processing " + file);
        }));
    }
}
```

------------------------------------------------------------------------

### ✅ Mini Project: Transaction Analyzer

``` java
import java.util.*;
import java.util.stream.Collectors;

class Transaction {
    private String type;
    private double amount;
    private String accountId;

    public Transaction(String type, double amount, String accountId) {
        this.type = type;
        this.amount = amount;
        this.accountId = accountId;
    }

    public String getType() { return type; }
    public double getAmount() { return amount; }
    public String getAccountId() { return accountId; }
}

public class TransactionAnalyzer {
    public static void main(String[] args) {
        List<Transaction> transactions = Arrays.asList(
            new Transaction("DEPOSIT", 1000, "A1"),
            new Transaction("WITHDRAWAL", 500, "A1"),
            new Transaction("DEPOSIT", 2000, "A2"),
            new Transaction("WITHDRAWAL", 700, "A2"),
            new Transaction("DEPOSIT", 1500, "A1")
        );

        double totalDeposits = transactions.stream()
                .filter(t -> t.getType().equals("DEPOSIT"))
                .mapToDouble(Transaction::getAmount)
                .sum();

        Optional<Transaction> largestTransaction = transactions.stream()
                .max(Comparator.comparing(Transaction::getAmount));

        Map<String, List<Transaction>> groupedByAccount =
                transactions.stream()
                .collect(Collectors.groupingBy(Transaction::getAccountId));

        long withdrawalCount = transactions.stream()
                .filter(t -> t.getType().equals("WITHDRAWAL"))
                .count();

        List<Transaction> sortedDescending = transactions.stream()
                .sorted(Comparator.comparing(Transaction::getAmount).reversed())
                .collect(Collectors.toList());

        System.out.println("Total Deposits: " + totalDeposits);
        System.out.println("Largest Transaction: " + largestTransaction.orElse(null));
        System.out.println("Grouped by Account: " + groupedByAccount);
        System.out.println("Withdrawal Count: " + withdrawalCount);
        System.out.println("Sorted Transactions: " + sortedDescending);
    }
}
```

------------------------------------------------------------------------

## 📊 Summary

  Section                  Description
  ------------------------ --------------------------
  Beginner Questions       Fundamental concepts
  Intermediate Questions   Deeper understanding
  Advanced Questions       JVM internals
  Exercises                Hands-on coding
  Solutions                Detailed implementations
  Mini Project             Real-world application

------------------------------------------------------------------------

## 🎯 Usage

This Markdown file can be used for: - 📚 Interview preparation - 🎓
Teaching and workshops - 🧑‍💻 Self-study - 📁 Documentation (GitHub,
internal knowledge bases)

------------------------------------------------------------------------

## 📌 Conclusion

Java Lambda Expressions provide a powerful way to write concise and
expressive code. Understanding their usage along with JVM internals such
as `invokedynamic` and `LambdaMetafactory` is essential for modern Java
development.
