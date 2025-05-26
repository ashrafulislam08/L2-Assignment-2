## 1. What is PostgreSQL?

PostgreSQL হচ্ছে পাওয়ারফুল,ওপেন সোর্স, অব্জেক্ট রিলেশনাল ডাটাবেজ। এইটা ইন্টারপ্রাইজ ওয়েব এপ্লিক্যাশনের জন্য ব্যবহার করে।

PostgreSQL সিকুয়েল (SQL - Structured Query Language) ডাটাবেজ ব্যবহার করে ডাটা ব্যবস্থাপনা করে, এইটা ডাটাবেজ এর জগতে ব্যাপকভাবে ব্যবহার করে।

PostgreSQL এর অনেক এডভান্সড ফিচার আছে যেগুলা অন্যান্য database-class এর ডাটাবেস ম্যানেজমেন্ট সিস্টেমগুলি অফার করে।

PostgreSQL এর অনেক ল্যাঙ্গুয়েজ সাপোর্ট আছে, যেমনঃ

- Python
- Java
- C#
- C/C++
- Ruby
- JavaScript (Node.js)
- Perl
- Go
- Tcl

## 2. Explain the **Primary Key** and **Foreign Key** concepts in PostgreSQL

Primary key কখনো null হতে পারবে না, Primary key অবশ্যই unique হতে হবে। যে কলাম বা এট্রিবিউট উপরে primary key ডিক্লার করে দিবেন, সেই ভেল্যু গুলো কখনো Null বা Duplicate ভেল্যু হতে পারবে না।

                                       **Student Table**

| id  | name | age |
| --- | ---- | --- |
|     |      |     |
|     |      |     |

এই student table এর Id যদি আমরা Primary key হিসেবে বলে দেই, তাহলে id এর এখানে কখনো Null হতে পারবে না এবং ভেল্যু গুলো Duplicate হতে পারবে না।

কিভাবে আমরা এই টেবিল টা তৈরি করতে পারি? দেখা যাক

```sql
CREATE TABLE student (
	id INT PRIMARY KEY,
	name VARCHAR(20),
	age INT
)
```

আমরা বলে দিছি যে, id এর ডাটা টাইপ টা integer হবে এবং primary key হবে। তার মানে আমার id টা Not Null হবে এবং Unique হতে হবে।

এইটা আরেকটু ভালো ভাবে বোঝার জন্য আমরা কিছু ডাটা ইন্সারট করে দেখতে পারি

```sql
INSERT INTO student(id, name, age) VALUES (1, 'Ashraful', 10);
INSERT INTO student(id, name, age) VALUES (1, 'Sam', 18); -- It will throw an error
INSERT INTO student(id, name, age) VALUES ('Sam', 15); -- Also, it will throw an error because we don't tell the id
INSERT INTO student(id, name, age) VALUES (2, 'Siam', 20);
```

আমরা এখানে বলছি student table ভেল্যু গুলা insert করার জন্য

এখানে বলে দিছি যে student table এ id, name and age এর ভেল্যু দিবা

যেখানে আমরা প্রথমে বলে দিছি যে id হবে 1, name হবে Ashraful এবং age হবে 10 ।

দ্বিতীয়বারে আমরা বলে দিছি যে id হবে 1, name হবে Sam এবং age হবে 15 ।

এখানেই আমাদের এরর থ্রো করবে, কারন আমরা জানি যে আমরা যদি কোনো column/attribute কে primary key হিসেবে ডিক্লার করি তখন সেটা null কিংবা duplicate হতে পারবে না।

পরের বার আমরা আইডি না বলেই নাম এবং age দিয়েছি যার কারণে এখানে এরর থ্রো করেছে।

তারপরে যে ডাটা আমরা insert করেছি সেটা ঠিক ভাবে insert হয়েছে।

**Foreign Key**

কোনো একটা টেবিলের primary key যদি অন্য কোনো টেবিলে reference key হিসেবে ব্যবহার করা হয় তাহলে সে key কে বলা হয় Foreign key।

**Example**

```sql
CREATE TABLE sections (
	section_id int primary key,
	section_name varchar(20)
);

CREATE TABLE student (
		id int primary key,
		name varchar(20),
		section_id int REFERENCES sections(section_id);
)
```

         **Sections Table**

| section_id | section_name |
| ---------- | ------------ |
| 1          | A            |
| 2          | B            |

                                        **Student Table**

| id  | name     | age | section_id |
| --- | -------- | --- | ---------- |
| 1   | Ashraful | 20  | 1          |
| 2   | Siam     | 19  | 2          |
| 3   | Karim    | 42  | 1          |
| 4   | Jarim    | 31  | 2          |

তার মানে আমরা যে section_id নিলাম সেটার সাথে Sections table একটা references আছে এই রেফারেন্স কে বলা হয় Foreign Key।

## 3. How can you modify data using `UPDATE` statements?

আমরা update statement এর মাধ্যমে কন্ডিশনালি ডাটা আপডেট করতে পারি।

মনে করেন আমাদের একটা student table আছে, এবং কিছু স্টুডেন্ট এর তথ্য ভুল দিয়ে ফেলসি এখন এগুলা উপডেট করার জন্য আমরা এই DELETE statement টা ব্যবহার করবো।

ধরুন আমাদের টেবিল টা দেখতে এই রকমঃ

                                        **Student Table**

| id  | name     | age | section_id |
| --- | -------- | --- | ---------- |
| 1   | Ashraful | 20  | 1          |
| 2   | Siam     | 19  | 2          |
| 3   | Karim    | 42  | 1          |
| 4   | Jarim    | 31  | 2          |

```sql
UPDATE student SET name = 'Ashraful Islam' WHERE id = 1;
```

আপডেট করার শুরুতে আমরা Update keyword টা ব্যবহার করবো এবং update keyword এর পরে বলে দিবো যে কোন টেবিল এর ডাটা আমরা আপডেট করতে চাচ্ছি । তারপর আমরা কোন প্রপার্টি টা উপডেট করবো সেটা SET keyword এর পরে property name এবং ইকুয়াল দিয়ে updated মান টা দিয়ে বলে দিবো

পরবর্তীতে আমরা WHERE দিয়ে কন্ডিশন লিখবো যে আমরা আসলে কোন row এর প্রোপার্টির মান পরিবর্তন করতে চাই।

আমরা চাইলে এক সাথে মাল্টিপল প্রোপার্টির মান/ভেল্যু চেঞ্জ করতে পারি।

```sql
UPDATE students SET name = 'Siam Hawlader', age = 21 WHERE id = 2;
```

আমরা এইভাবে চাইলে কমা দিয়ে একাদিক প্রোপার্টির মান/ভেল্যু পরিবর্তন করতে পারি।

## 4. How can you calculate aggregate functions like `COUNT()`, `SUM()`, and `AVG()` in PostgreSQL?

### COUNT() FUNCTION

যখন আমাদের কোনো table এর টোটাল কলামের কতগুলো ভেল্যু আছে, সেটা জানার জন্য আমরা COUNT() ফাংশনটি ব্যবহার করবো;

```sql
SELECT COUNT(*) FROM student;
```

এইটা আমাদের student table এর ভিতরের সবগুলো ভেল্যুর COUNT টা আমাদের কে দিবে।

COUNT ফাংশনের ভিতর (\*) দেয়ার মানে হলো আমাদের যে student table আছে এইটার টোটাল কত গুলো ভেল্যু আছে সেটা জানতে পারবো।

আমরা চাইলে স্পেসিফিক কলামের count ও জানতে পারি।

```sql
SELECT COUNT(name) FROM students;
```

এইভাবে আমরা চাইলে স্পেসিফিক টেবিলের কলামের নাম দিয়ে টোটাল কয়টা ভেল্যু আছে সেটা জানতে পারি।

### SUM() FUNCTION

মনে করেন আমাদের student table এর যত জন student আছে তাদের সবার age এর যোগ করবো আর এই যোগ করার জন্য আমরা SUM() ফাংশনটি ব্যবহার করবো;

অবশ্যই আমাদের কলামের ভেল্যু গুলা নামেরিক - Numeric হতে হবে।

**For Example:**

```sql
SELECT SUM(age) FROM students;

```

এই ফাংশন SUM() এর প্যারামিটার হিসেবে আমরা বলে দিছি যে আমরা কোন কলামের ভেল্যু গুলো যোগ বা সাম করতে চাই।

### AVG() FUNCTION

আমাদের কলামের টোটাল ভেল্যুর এভারেজ বের করার জন্য এই AVG() ফুংশনটি ব্যবহার করা হয়।

আমাদের কলামের ভেল্যু গুলা অবশ্যই Numeric ভেল্যু হতে হবে।

মনে করে আমরা আমাদের স্টুডেন্ট দের বয়সের একটা এভারেজ বের করতে চাই,

```sql
SELECT AVG(age) FROM students;
```

তাহলে, আমরা এইভাবে এভারেজ বের করতে পারি।
