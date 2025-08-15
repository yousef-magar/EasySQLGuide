
# 🔥 SQL for Legends – الدليل الكامل للمبتدئين بالعربي والإنجليزي

## 🧱 1. إنشاء قاعدة بيانات (Create Database)

```sql
CREATE DATABASE MyDatabase; -- إنشاء قاعدة بيانات
USE MyDatabase;             -- استخدام القاعدة
```

💡 **TIP**: قاعدة البيانات هي المكان اللي بتخزن فيه كل جداولك.

---

## 🗑️ 2. حذف قاعدة البيانات (Drop Database)

```sql
DROP DATABASE MyDatabase;
```

⚠️ بيحذف القاعدة بالكامل. مفيش تراجع.

---

## 📊 3. إنشاء جدول (Create Table)

```sql
CREATE TABLE Students (
    first_name VARCHAR(30),
    last_name VARCHAR(30),
    email VARCHAR(50),
    age INT
);
```

👨‍🎓 جدول `Students` فيه 4 أعمدة:

* `first_name`, `last_name`, `email`: نصوص
* `age`: عدد صحيح

---

## 🛠️ 4. تعديل الجدول (Alter Table)

➕ **إضافة عمود**:

```sql
ALTER TABLE Students ADD phone VARCHAR(15);
```

🗑️ **حذف عمود**:

```sql
ALTER TABLE Students DROP COLUMN phone;
```

✏️ **إعادة تسمية عمود**:

```sql
ALTER TABLE Students RENAME COLUMN old_name TO new_name;
```

(✅ SQL Server 2016+ / PostgreSQL / بعض الأنظمة)

---

## 📥 5. إدخال وعرض البيانات (Insert & Select)

```sql
INSERT INTO Students (first_name, last_name, email, age)
VALUES ('You', 'Amin', 'usf@domain.com', 20);
```

🔍 عرض كل البيانات:

```sql
SELECT * FROM Students;
```

---

## ❌ 6. حذف البيانات (Delete Data)

🧹 **حذف كل الصفوف** (سريع):

```sql
TRUNCATE TABLE Students;
```

🗑️ **حذف كل الصفوف** (يمكن التراجع عنه أحيانًا):

```sql
DELETE FROM Students;
```

---

## 💾 7. نسخ احتياطي (Backup)

📤 نسخة كاملة:

```sql
BACKUP DATABASE MyDatabase TO DISK = 'C:\Backup\mydb_full.bak';
```

♻️ نسخة تفاضلية:

```sql
BACKUP DATABASE MyDatabase TO DISK = 'C:\Backup\mydb_diff.bak' WITH DIFFERENTIAL;
```

---

## 💡 8. الفكرة الأساسية

```
Data + Data = Information
Information + Information = Knowledge
```

---

## 💬 9. تعليقات SQL

```sql
-- This is a comment
```

---

## 🔐 SQL Constraints – القيود المهمة

| القيد         | المعنى               | المثال                               |
| ------------- | -------------------- | ------------------------------------ |
| `NOT NULL`    | لازم قيمة            | `age INT NOT NULL`                   |
| `UNIQUE`      | لازم تكون قيمة مميزة | `email VARCHAR(50) UNIQUE`           |
| `PRIMARY KEY` | معرف أساسي           | `employee_id INT PRIMARY KEY`        |
| `FOREIGN KEY` | علاقة بين جدولين     | انظر الأمثلة أدناه                   |
| `CHECK`       | شرط للتأكد           | `CHECK (salary >= 1000)`             |
| `DEFAULT`     | قيمة تلقائية         | `city VARCHAR(30) DEFAULT 'Unknown'` |

---

## 📈 الفهارس (Index)

```sql
CREATE INDEX idx_email ON staff(email);
```

💨 تستخدم للبحث السريع داخل الجدول.

---

## 🔗 مثال متكامل – قاعدة بيانات BookstoreDB

### 🏗️ إنشاء الجداول:

```sql
CREATE DATABASE BookstoreDB;
USE BookstoreDB;

CREATE TABLE Authors (
	AuthorID INT PRIMARY KEY,
	AuthorName NVARCHAR(30) NOT NULL,
	Countary NVARCHAR(50)
);

CREATE TABLE Publishers (
	PublisherID INT PRIMARY KEY,
	PublisherName NVARCHAR(50) NOT NULL,
	Location NVARCHAR(100)
);

CREATE TABLE Categories (
	CategoryID INT PRIMARY KEY,
	CategoryName NVARCHAR(50) UNIQUE NOT NULL
);

CREATE TABLE Books (
	BookID INT PRIMARY KEY,
	title NVARCHAR(150) NOT NULL,
	AuthorID INT,
	PublisherID INT,
	CategoryID INT,
	Price DECIMAL(8,2) CHECK(Price > 0),
	FOREIGN KEY (AuthorID) REFERENCES Authors(AuthorID),
	FOREIGN KEY (PublisherID) REFERENCES Publishers(PublisherID),
	FOREIGN KEY (CategoryID) REFERENCES Categories(CategoryID)
);

CREATE TABLE Inventory (
	InventoryID INT PRIMARY KEY,
	BookID INT UNIQUE,
	Quantity INT DEFAULT 0 CHECK (Quantity >= 0),
	FOREIGN KEY (BookID) REFERENCES Books(BookID)
);
```

---

## 🧾 إدخال بيانات (Insert)

```sql
-- Authors
INSERT INTO Authors VALUES
	(1, 'Fares Khalil', 'Jordan'),
	(2, 'Nour Ahmed', 'Egypt'),
	(3, 'Lina Saeed', 'UAE'),
	(4, 'Yousef Omar', 'Saudi Arabia'),
	(5, 'Mona Samir', 'Lebanon');

-- Publishers
INSERT INTO Publishers VALUES 
	(1, 'AlManhal Publishing', 'Amman'),
	(2, 'Horizon Books', 'Cairo'),
	(3, 'Falak Publications', 'Dubai'),
	(4, 'Nawa Publishing', 'Riyadh'),
	(5, 'Cedars House', 'Beirut');

-- Categories
INSERT INTO Categories VALUES 
	(1, 'History'),
	(2, 'Technology'),
	(3, 'Art'),
	(4, 'Science'),
	(5, 'Philosophy');

-- Books
INSERT INTO Books VALUES 
	(1, 'Journey Through Time', 1, 1, 1, 25.50),
	(2, 'Tech World', 2, 2, 2, 40.75),
	(3, 'Colors of Life', 3, 3, 3, 35.00),
	(4, 'Space Mysteries', 4, 4, 4, 50.25),
	(5, 'Mind Matters', 5, 5, 5, 45.60);

-- Inventory
INSERT INTO Inventory VALUES 
	(1, 1, 15),
	(2, 2, 20),
	(3, 3, 12),
	(4, 4, 18),
	(5, 5, 10);
```

---

## 🛠️ أوامر التعديل (UPDATE + DELETE)

```sql
-- تعديل السعر
UPDATE Books SET Price = 27.00 WHERE BookID = 1;

-- تعديل الكمية
UPDATE Inventory SET Quantity = 25 WHERE InventoryID = 2;

-- حذف عنصر
DELETE FROM Inventory WHERE InventoryID = 5;
```

---

## 🔍 استعلامات (Queries)

📘 كتب سعرها أكبر من 30:

```sql
SELECT * FROM Books WHERE Price > 30;
```

📚 عناوين وأسعار مرتبة تنازليًا:

```sql
SELECT Title, Price FROM Books ORDER BY Price DESC;
```

📊 عدد الكتب حسب الفئة:

```sql
SELECT CategoryId, COUNT(BookID) AS NumberOfBooks
FROM Books
GROUP BY CategoryID;
```

---

## 🧠 Joins – الربط بين الجداول

👥 **INNER JOIN** (كتب + مؤلفين):

```sql
SELECT Books.Title, Authors.AuthorName
FROM Books
INNER JOIN Authors ON Books.AuthorID = Authors.AuthorID;
```

🏢 **LEFT JOIN** (كتب + ناشرين):

```sql
SELECT Books.Title, Publishers.PublisherName
FROM Books
LEFT JOIN Publishers ON Books.PublisherID = Publishers.PublisherID;
```

🎨 **RIGHT JOIN** (كتب + فئات):

```sql
SELECT Books.Title, Categories.CategoryName
FROM Books
RIGHT JOIN Categories ON Books.CategoryID = Categories.CategoryID;
```

---

## 🎯 بيانات أعلى من المتوسط:

```sql
SELECT Title
FROM Books
WHERE Price > (SELECT AVG(Price) FROM Books);
```

---

## 🔢 IDENTITY – الترقيم التلقائي (SQL Server Only)

```sql
CREATE TABLE customers (
    customerId INT IDENTITY(1,1) PRIMARY KEY,
    customerName VARCHAR(50),
    address VARCHAR(100)
);
```

📝 لا تكتب الـ `customerId` وقت الإدخال.

```sql
INSERT INTO customers (customerName, address)
VALUES ('Ali', 'Cairo'),
       ('Sara', NULL);
```

---

## ✅ أخطاء شائعة

❌ `updata` ❌
✅ `UPDATE`

❌ `= "Yousef"`
✅ `= 'Yousef'`

---
