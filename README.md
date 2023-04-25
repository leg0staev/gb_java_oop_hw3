# Geek Brains. Некоторые стандартные интерфейсы Java и примеры их использования

## Домашнее задание 3.

ЗАДАНИЕ

> Доработать приложение, которое мы разрабатывали на уроке. Мы доллжны поработать с сортировкой объектов, освоить работу с интерфейсами Comparator, Comparable:
> 1. Доработать класс Freelancer, при желании можно разработать и свой собтственный тип сотрудника.
> 2. Переработать метод generateEmployee, метод должен создавать случайного сотрудника (Worker, Freelancer или любого другого). Метод должен быть один!
> 3. Придумать свой собственный компаратор (Возможно отсортировать сотрудников по возрасту? Тогда добавьте соответствующее состояние на уровне ваших классов).
Продемонстрировать сортировку объектов различного типа с использованием собственного компаратора.

## Решение

1. 
```java
class Freelancer extends Employee {

    protected int orders;

    public Freelancer(String name, String surName, int age, double salary, int orders) {
        super(name, surName, age, salary);
        this.orders = orders;

    }

    @Override
    public double calculateSalary() {
        return orders * salary;
    }

    @Override
    public String toString() {
        return String.format(
                "%s %s Возраст: %d; Фрилансер; З.п. (кол-во зак-ов в мес. (%d шт.) * ставку за зак.): %.2f (руб.)",
                name, surName, age, orders, salary * orders);
    }
}
```
2.
```java
static Employee generateEmployee() {
    String[] names = new String[] { "Анатолий", "Глеб", "Клим", "Мартин", "Лазарь", "Владлен", "Клим", "Панкратий",
                "Рубен", "Герман" };
    String[] surnames = new String[] { "Григорьев", "Фокин", "Шестаков", "Хохлов", "Шубин", "Бирюков", "Копылов",
                "Горбунов", "Лыткин", "Соколов" };

    Random random = new Random();
    int index = random.nextInt(1, 3);
    int age = random.nextInt(21, 45);
    int workerSalary = random.nextInt(50, 100) * 1000;
    int freelancerSalary = random.nextInt(1, 5) * 1000;
    int freelancerOrders = random.nextInt(4, 9);

    if (index % 2 == 0) {
        return new Freelancer(names[random.nextInt(names.length)], surnames[random.nextInt(surnames.length)], age,
                    freelancerSalary, freelancerOrders);
        }
    return new Worker(names[random.nextInt(names.length)], surnames[random.nextInt(surnames.length)], age,
                workerSalary);
}
```

3.
```java
class AgeComporator implements Comparator<Employee>{

    @Override
    public int compare(Employee o1, Employee o2){
        return Integer.compare(o1.age, o2.age);
    }
}
```
рузцльтат сортировки с моим компоратором:
```
*** Отсортированный по возрасту массив сотрудников ***

Рубен Григорьев; Возраст: 24; Рабочий; Ср. з/п (фикс): 64000,00 (руб.)
Клим Шубин Возраст: 24; Фрилансер; З.п. (кол-во зак-ов в мес. (4 шт.) * ставку за зак.): 12000,00 (руб.)
Герман Шестаков Возраст: 27; Фрилансер; З.п. (кол-во зак-ов в мес. (7 шт.) * ставку за зак.): 14000,00 (руб.)
Рубен Шестаков Возраст: 33; Фрилансер; З.п. (кол-во зак-ов в мес. (5 шт.) * ставку за зак.): 5000,00 (руб.)
Мартин Копылов; Возраст: 36; Рабочий; Ср. з/п (фикс): 66000,00 (руб.)
Клим Бирюков Возраст: 38; Фрилансер; З.п. (кол-во зак-ов в мес. (5 шт.) * ставку за зак.): 15000,00 (руб.)
Клим Бирюков; Возраст: 42; Рабочий; Ср. з/п (фикс): 74000,00 (руб.)
Глеб Копылов Возраст: 42; Фрилансер; З.п. (кол-во зак-ов в мес. (4 шт.) * ставку за зак.): 8000,00 (руб.)
Панкратий Хохлов; Возраст: 43; Рабочий; Ср. з/п (фикс): 93000,00 (руб.)
Рубен Соколов Возраст: 44; Фрилансер; З.п. (кол-во зак-ов в мес. (7 шт.) * ставку за зак.): 7000,00 (руб.)
```