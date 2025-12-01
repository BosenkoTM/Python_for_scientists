# ДОКУМЕНТАЦИЯ ПО БАЗАМ ДАННЫХ
---

## 📚 Общая информация

Созданы **5 баз данных SQLite** с тестовыми данными для экзаменационных вариантов.

**Характеристики:**
- Формат: SQLite 3
- Кодировка: UTF-8
- Период данных: 2023-2024 гг.
- Количество записей: 40-50 в основных таблицах

---

## 1️⃣ ELECTRONICS_SHOP.DB (Вариант 1)

### Описание
База данных интернет-магазина электроники с информацией о товарах, клиентах и заказах.

### Структура таблиц

#### Таблица: `products` (15 записей)
```sql
CREATE TABLE products (
    ProductID INTEGER PRIMARY KEY,
    ProductName TEXT NOT NULL,
    Category TEXT NOT NULL,
    Price REAL NOT NULL
)
```

**Категории товаров:**
- Смартфоны (5 моделей)
- Ноутбуки (3 модели)
- Планшеты (2 модели)
- Наушники (2 модели)
- Часы (2 модели)
- Телевизоры (1 модель)

**Диапазон цен:** 19,990 - 189,990 руб.

#### Таблица: `customers` (30 записей)
```sql
CREATE TABLE customers (
    CustomerID INTEGER PRIMARY KEY,
    CustomerName TEXT NOT NULL,
    City TEXT NOT NULL,
    Country TEXT NOT NULL,
    RegistrationDate TEXT NOT NULL
)
```

**Города:** Москва, Санкт-Петербург, Новосибирск, Екатеринбург, Казань, и др. (10 городов)  
**Период регистрации:** 2023 год

#### Таблица: `orders` (50 записей)
```sql
CREATE TABLE orders (
    OrderID INTEGER PRIMARY KEY,
    CustomerID INTEGER,
    OrderDate TEXT NOT NULL,
    ProductID INTEGER,
    Quantity INTEGER NOT NULL,
    TotalAmount REAL NOT NULL,
    FOREIGN KEY (CustomerID) REFERENCES customers(CustomerID),
    FOREIGN KEY (ProductID) REFERENCES products(ProductID)
)
```

**Период заказов:** январь - ноябрь 2024  
**Количество товаров в заказе:** 1-3 шт.

### Примеры запросов

```sql
-- Топ-5 товаров по выручке
SELECT p.ProductName, p.Category, SUM(o.TotalAmount) as Revenue
FROM orders o
JOIN products p ON o.ProductID = p.ProductID
GROUP BY o.ProductID
ORDER BY Revenue DESC
LIMIT 5;

-- Заказы смартфонов с данными клиентов
SELECT o.OrderDate, p.ProductName, o.Quantity, o.TotalAmount, c.City
FROM orders o
JOIN products p ON o.ProductID = p.ProductID
JOIN customers c ON o.CustomerID = c.CustomerID
WHERE p.Category = 'Смартфоны';
```

### Особенности данных
- Есть популярные товары (iPhone, Samsung) с большим количеством заказов
- Распределение заказов неравномерное по городам
- Временной ряд продаж имеет сезонность

---

## 2️⃣ BANK_LOANS.DB (Вариант 2)

### Описание
База данных банка с информацией о клиентах, кредитах и платежах.

### Структура таблиц

#### Таблица: `customers` (30 записей)
```sql
CREATE TABLE customers (
    CustomerID INTEGER PRIMARY KEY,
    Age INTEGER NOT NULL,
    Income REAL NOT NULL,
    CreditScore INTEGER NOT NULL,
    EmploymentType TEXT NOT NULL,
    City TEXT NOT NULL
)
```

**Характеристики:**
- Возраст: 25-65 лет
- Доход: 30,000 - 200,000 руб/мес
- Кредитный рейтинг: 300-850
- Типы занятости: Полная, Частичная, Самозанятый, Госслужащий

#### Таблица: `loans` (50 записей)
```sql
CREATE TABLE loans (
    LoanID INTEGER PRIMARY KEY,
    CustomerID INTEGER,
    LoanAmount REAL NOT NULL,
    InterestRate REAL NOT NULL,
    Term INTEGER NOT NULL,
    IssueDate TEXT NOT NULL,
    Status TEXT NOT NULL,
    FOREIGN KEY (CustomerID) REFERENCES customers(CustomerID)
)
```

**Характеристики:**
- Сумма кредита: 50,000 - 1,500,000 руб
- Процентная ставка: 5-25% (зависит от CreditScore)
- Срок: 12, 24, 36, 48, 60 месяцев
- Статусы: Active, Paid, Default

**Корреляция:** Низкий CreditScore → высокая InterestRate

#### Таблица: `payments` (72 записи)
```sql
CREATE TABLE payments (
    PaymentID INTEGER PRIMARY KEY,
    LoanID INTEGER,
    PaymentDate TEXT NOT NULL,
    Amount REAL NOT NULL,
    Status TEXT NOT NULL,
    FOREIGN KEY (LoanID) REFERENCES loans(LoanID)
)
```

**Статусы платежей:** Paid, Late

### Примеры запросов

```sql
-- Кредиты со статусом Default с данными клиентов
SELECT l.LoanID, l.LoanAmount, l.InterestRate, c.Age, c.CreditScore
FROM loans l
JOIN customers c ON l.CustomerID = c.CustomerID
WHERE l.Status = 'Default';

-- Средний кредитный рейтинг по типам занятости
SELECT EmploymentType, AVG(CreditScore) as AvgScore
FROM customers
GROUP BY EmploymentType
ORDER BY AvgScore DESC;
```

### Особенности данных
- Реалистичная корреляция между CreditScore и InterestRate
- Около 15-20% кредитов имеют статус Default
- Разнообразие типов занятости и доходов

---

## 3️⃣ RESTAURANT_DELIVERY.DB (Вариант 3)

### Описание
База данных сети ресторанов с системой доставки.

### Структура таблиц

#### Таблица: `restaurants` (10 записей)
```sql
CREATE TABLE restaurants (
    RestaurantID INTEGER PRIMARY KEY,
    RestaurantName TEXT NOT NULL,
    City TEXT NOT NULL,
    CuisineType TEXT NOT NULL
)
```

**Типы кухни:** Японская, Итальянская, Американская, Русская, Узбекская  
**Города:** Москва, Санкт-Петербург, Казань

#### Таблица: `customers` (25 записей)
```sql
CREATE TABLE customers (
    CustomerID INTEGER PRIMARY KEY,
    CustomerName TEXT NOT NULL,
    RegistrationDate TEXT NOT NULL,
    TotalOrders INTEGER NOT NULL
)
```

**Период регистрации:** 2023 год  
**Количество заказов:** 1-20

#### Таблица: `orders` (50 записей)
```sql
CREATE TABLE orders (
    OrderID INTEGER PRIMARY KEY,
    RestaurantID INTEGER,
    CustomerID INTEGER,
    OrderDate TEXT NOT NULL,
    DeliveryTime INTEGER NOT NULL,
    OrderAmount REAL NOT NULL,
    Rating REAL NOT NULL,
    FOREIGN KEY (RestaurantID) REFERENCES restaurants(RestaurantID),
    FOREIGN KEY (CustomerID) REFERENCES customers(CustomerID)
)
```

**Характеристики:**
- Время доставки: 15-90 минут
- Сумма заказа: 500-3,000 руб
- Рейтинг: 3.0-5.0

**Корреляция:** Меньше время доставки → выше рейтинг

### Примеры запросов

```sql
-- Топ-3 ресторанов по средней сумме заказа
SELECT r.RestaurantName, r.City, r.CuisineType, AVG(o.OrderAmount) as AvgAmount
FROM orders o
JOIN restaurants r ON o.RestaurantID = r.RestaurantID
GROUP BY r.RestaurantID
ORDER BY AvgAmount DESC
LIMIT 3;

-- Заказы с рейтингом > 4.0
SELECT o.OrderDate, r.RestaurantName, r.CuisineType, 
       o.OrderAmount, o.DeliveryTime, o.Rating
FROM orders o
JOIN restaurants r ON o.RestaurantID = r.RestaurantID
WHERE o.Rating > 4.0;
```

### Особенности данных
- Явная корреляция между временем доставки и рейтингом
- Разные типы кухонь с разным средним временем доставки
- Сезонность в заказах (выходные vs будни)

---

## 4️⃣ HR_ANALYTICS.DB (Вариант 4)

### Описание
База данных HR-аналитики IT-компании.

### Структура таблиц

#### Таблица: `departments` (5 записей)
```sql
CREATE TABLE departments (
    DepartmentID INTEGER PRIMARY KEY,
    DepartmentName TEXT NOT NULL,
    City TEXT NOT NULL,
    Budget REAL NOT NULL
)
```

**Отделы:** IT, Маркетинг, Продажи, HR, Финансы

#### Таблица: `employees` (40 записей)
```sql
CREATE TABLE employees (
    EmployeeID INTEGER PRIMARY KEY,
    Name TEXT NOT NULL,
    Department TEXT NOT NULL,
    Position TEXT NOT NULL,
    HireDate TEXT NOT NULL,
    Salary REAL NOT NULL,
    PerformanceScore INTEGER NOT NULL,
    HasLeft INTEGER NOT NULL
)
```

**Характеристики:**
- Зарплата: 50,000 - 250,000 руб/мес
- Показатель эффективности: 1-5
- Дата найма: 2020-2024
- HasLeft: 0 (работает) или 1 (уволился)

**Корреляция:** Низкая эффективность → выше вероятность ухода

#### Таблица: `attendance` (400 записей)
```sql
CREATE TABLE attendance (
    AttendanceID INTEGER PRIMARY KEY,
    EmployeeID INTEGER,
    Date TEXT NOT NULL,
    HoursWorked REAL NOT NULL,
    OvertimeHours REAL NOT NULL,
    FOREIGN KEY (EmployeeID) REFERENCES employees(EmployeeID)
)
```

**Период:** январь 2024  
**Часы работы:** 7-9 часов в день  
**Переработки:** 0-3 часа (у ~30% сотрудников)

### Примеры запросов

```sql
-- Уволившиеся сотрудники
SELECT e.Name, e.Department, e.Position, e.Salary, e.PerformanceScore
FROM employees e
WHERE e.HasLeft = 1;

-- Процент оттока по отделам
SELECT Department, 
       COUNT(*) as Total,
       SUM(HasLeft) as Left,
       ROUND(SUM(HasLeft) * 100.0 / COUNT(*), 2) as TurnoverRate
FROM employees
GROUP BY Department
ORDER BY TurnoverRate DESC;
```

### Особенности данных
- Около 20-25% сотрудников уволились (HasLeft=1)
- Корреляция между PerformanceScore и HasLeft
- Разный уровень оттока по отделам

---

## 5️⃣ MARKETPLACE.DB (Вариант 5)

### Описание
База данных онлайн-маркетплейса.

### Структура таблиц

#### Таблица: `categories` (6 записей)
```sql
CREATE TABLE categories (
    CategoryID INTEGER PRIMARY KEY,
    CategoryName TEXT NOT NULL,
    Commission REAL NOT NULL
)
```

**Категории:** Электроника, Одежда, Книги, Спорт, Дом и сад, Игрушки  
**Комиссия:** 3-12%

#### Таблица: `products` (24 записи)
```sql
CREATE TABLE products (
    ProductID INTEGER PRIMARY KEY,
    ProductName TEXT NOT NULL,
    CategoryID INTEGER,
    Price REAL NOT NULL,
    Stock INTEGER NOT NULL,
    SupplierID INTEGER NOT NULL,
    FOREIGN KEY (CategoryID) REFERENCES categories(CategoryID)
)
```

**Характеристики:**
- Цена: 500 - 50,000 руб
- Остаток на складе: 0-100 шт
- 4 товара в каждой категории

#### Таблица: `sales` (50 записей)
```sql
CREATE TABLE sales (
    SaleID INTEGER PRIMARY KEY,
    ProductID INTEGER,
    SaleDate TEXT NOT NULL,
    Quantity INTEGER NOT NULL,
    Discount REAL NOT NULL,
    TotalPrice REAL NOT NULL,
    FOREIGN KEY (ProductID) REFERENCES products(ProductID)
)
```

**Характеристики:**
- Количество: 1-5 шт
- Скидка: 0-50%
  - Без скидки (0%): 30%
  - Малая (5-10%): 30%
  - Средняя (11-30%): 25%
  - Высокая (31-50%): 15%

### Примеры запросов

```sql
-- Топ-5 категорий по выручке
SELECT c.CategoryName, SUM(s.TotalPrice) as Revenue
FROM sales s
JOIN products p ON s.ProductID = p.ProductID
JOIN categories c ON p.CategoryID = c.CategoryID
GROUP BY c.CategoryID
ORDER BY Revenue DESC
LIMIT 5;

-- Продажи с данными о категориях
SELECT s.SaleDate, p.ProductName, c.CategoryName, 
       s.Quantity, s.Discount, s.TotalPrice
FROM sales s
JOIN products p ON s.ProductID = p.ProductID
JOIN categories c ON p.CategoryID = c.CategoryID;
```

### Особенности данных
- Разнообразие уровней скидок
- Корреляция между скидкой и количеством проданных товаров
- Временная динамика продаж

---

## 📊 Общая статистика по БД

| База данных | Таблиц | Основных записей | Размер | Период данных |
|-------------|--------|------------------|--------|---------------|
| electronics_shop.db | 3 | 50 заказов | 16 KB | 2024 |
| bank_loans.db | 3 | 50 кредитов | 16 KB | 2023-2024 |
| restaurant_delivery.db | 3 | 50 заказов | 16 KB | 2024 |
| hr_analytics.db | 3 | 40 сотрудников | 32 KB | 2020-2024 |
| marketplace.db | 3 | 50 продаж | 16 KB | 2024 |

---

## 🔧 Инструкция по использованию

### Для студентов

1. **Скачивание БД:**
   ```bash
   # Скачайте соответствующую БД для вашего варианта
   wget https://github.com/.../electronics_shop.db
   ```

2. **Подключение в Python:**
   ```python
   import sqlite3
   import pandas as pd
   
   # Подключение
   conn = sqlite3.connect('electronics_shop.db')
   
   # Выполнение запроса
   query = "SELECT * FROM products"
   df = pd.read_sql_query(query, conn)
   
   # Закрытие соединения
   conn.close()
   ```

3. **Проверка структуры:**
   ```python
   # Список таблиц
   tables = pd.read_sql_query(
       "SELECT name FROM sqlite_master WHERE type='table'", 
       conn
   )
   print(tables)
   
   # Структура таблицы
   info = pd.read_sql_query("PRAGMA table_info(products)", conn)
   print(info)
   ```

### Для преподавателей

**Расположение файлов:**
- Все БД находятся в `/mnt/user-data/outputs/`
- Размер каждой БД: 16-32 KB
- Формат: SQLite 3

**Верификация данных:**
```python
import sqlite3

def verify_database(db_path):
    conn = sqlite3.connect(db_path)
    cursor = conn.cursor()
    
    # Проверка целостности
    cursor.execute("PRAGMA integrity_check")
    print(cursor.fetchone())
    
    # Статистика
    cursor.execute("SELECT name FROM sqlite_master WHERE type='table'")
    tables = cursor.fetchall()
    
    for table in tables:
        cursor.execute(f"SELECT COUNT(*) FROM {table[0]}")
        print(f"{table[0]}: {cursor.fetchone()[0]} записей")
    
    conn.close()
```

---

## 💡 Особенности реализации

### Реалистичные корреляции

1. **bank_loans.db:** CreditScore ↔ InterestRate
2. **restaurant_delivery.db:** DeliveryTime ↔ Rating
3. **hr_analytics.db:** PerformanceScore ↔ HasLeft
4. **marketplace.db:** Discount ↔ Quantity

### Временные данные

Все даты представлены в формате `YYYY-MM-DD` (ISO 8601) для корректной работы с pandas datetime.

### Внешние ключи

Все таблицы имеют корректные FOREIGN KEY связи для проверки целостности данных.
