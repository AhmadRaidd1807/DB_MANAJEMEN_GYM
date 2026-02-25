# 🏋️‍♂️ Database Management System - GYM

Proyek ini merupakan implementasi **Database GYM** menggunakan **MySQL (MariaDB)** yang dijalankan melalui **XAMPP for Windows**.  
Database ini digunakan untuk mengelola data member, paket gym, dan pendaftaran.

----------

## 🗂️ Struktur Database

### 📌 1. Tabel `member`

Menyimpan data anggota gym.

| Field     | Tipe Data    | Keterangan     |
| --------- | ------------ | -------------- |
| id_member | INT (PK, AI) | ID unik member |
| nama      | VARCHAR(50)  | Nama member    |
| umur      | INT          | Umur member    |



----------

### 📌 2. Tabel `paket`

Menyimpan data paket gym yang tersedia.

| Field      | Tipe Data     | Keterangan    |
| ---------- | ------------- | ------------- |
| id_paket   | INT (PK, AI)  | ID unik paket |
| nama_paket | VARCHAR(50)   | Nama paket    |
| harga      | DECIMAL(10,2) | Harga paket   |

----------

### 📌 3. Tabel `pendaftaran`

Menyimpan data pendaftaran member ke paket gym.

| Field         | Tipe Data    | Keterangan             |
| ------------- | ------------ | ---------------------- |
| id_daftar     | INT (PK, AI) | ID unik pendaftaran    |
| id_member     | INT (FK)     | Relasi ke tabel member |
| id_paket      | INT (FK)     | Relasi ke tabel paket  |
| tanggal_mulai | DATE         | Tanggal mulai paket    |





🔗 **Relasi:**

-   `id_member` → member(id_member)
    
-   `id_paket` → paket(id_paket)
    

----------

# ⚙️ Setup Environment (XAMPP - Windows)

### 1️⃣ Masuk ke MySQL melalui CMD

```bash
mysql -u root -p

```

----------

### 2️⃣ Membuat Database

```sql
CREATE DATABASE db_gym;
USE db_gym;

```

----------

### 3️⃣ Membuat Tabel

```sql
CREATE TABLE member(
    id_member INT PRIMARY KEY AUTO_INCREMENT,
    nama VARCHAR(50) NOT NULL,
    umur INT NOT NULL
);

CREATE TABLE paket(
    id_paket INT PRIMARY KEY AUTO_INCREMENT,
    nama_paket VARCHAR(50) NOT NULL,
    harga DECIMAL(10,2) NOT NULL
);

CREATE TABLE pendaftaran(
    id_daftar INT PRIMARY KEY AUTO_INCREMENT,
    id_member INT,
    id_paket INT,
    tanggal_mulai DATE,
    FOREIGN KEY(id_member) REFERENCES member(id_member),
    FOREIGN KEY(id_paket) REFERENCES paket(id_paket)
);

```

----------

# 🧾 Sample Data

### 👥 Data Member

-   Reva (17)

-   Andi (18)
    
-   Budi (20)
    
-   Sinta (19)
    
-   Rina (21)
    

### 💳 Data Paket

-   Paket Personal Trainer 3 Bulan – 1.000.000
    
-   Paket 3 Bulan – 400.000
    
-   Paket 6 Bulan – 750.000
    
-   Paket Pelajar – 100.000
    

----------

# 🔎 Query yang Digunakan

### 🔗 INNER JOIN (Menampilkan Nama Member + Paket)

```sql
SELECT member.nama, paket.nama_paket, paket.harga
FROM pendaftaran
INNER JOIN member ON pendaftaran.id_member = member.id_member
INNER JOIN paket ON pendaftaran.id_paket = paket.id_paket;

```

----------

### 📅 Menampilkan Tanggal Mulai

```sql
SELECT member.nama, pendaftaran.tanggal_mulai
FROM pendaftaran
INNER JOIN member ON pendaftaran.id_member = member.id_member;

```

----------

### 💰 Total Pendapatan

```sql
SELECT SUM(paket.harga) AS total_pendapatan
FROM pendaftaran
INNER JOIN paket ON pendaftaran.id_paket = paket.id_paket;

```

Hasil:

```
2.650.000

```
---
DML
----------

# ✏️ DML (Data Manipulation Language)

Bagian ini berisi proses manipulasi data menggunakan perintah **INSERT, SELECT, UPDATE, dan DELETE**.

---
## ✅  INSERT (Menginput Data)

```sql
INSERT INTO member (nama, umur) VALUES
('Reid', 17),
('Andi', 18),
('Budi', 20),
('Sinta', 19),
('Rina', 21);
```

```sql
INSERT INTO paket (nama_paket, harga) VALUES
('Paket Bulanan', 150000),
('Paket 3 Bulan', 400000),
('Paket 6 Bulan', 750000),
('Paket Personal Trainer', 500000),
('Paket Pelajar', 100000);
```

```sql
INSERT INTO pendaftaran (id_member, id_paket, tanggal_mulai) VALUES
(1, 1, '2026-02-01'),
(2, 2, '2026-02-03'),
(3, 3, '2026-02-05'),
(4, 5, '2026-02-07'),
(5, 4, '2026-02-10');
```

---



## 🔎 SELECT (Menampilkan Data)

```sql
SELECT * FROM member;
SELECT * FROM paket;
SELECT * FROM pendaftaran;

SELECT nama, umur FROM member;
SELECT nama_paket, harga FROM paket;
SELECT id_paket, tanggal_mulai FROM pendaftaran;
```

---

## ✏️ UPDATE (Mengubah Data)

```sql
UPDATE member
SET nama = 'Akilah'
WHERE id_member = 1;

UPDATE paket
SET nama_paket = 'Paket Personal Trainer 4 Bulan',
    harga = 1300000
WHERE id_paket = 1;

UPDATE pendaftaran
SET tanggal_mulai = '2026-02-10'
WHERE id_daftar = 5;
```

---

## ❌ DELETE (Menghapus Data)

```sql
DELETE FROM pendaftaran
WHERE id_member = 5;

DELETE FROM member
WHERE id_member = 5;

DELETE FROM paket
WHERE id_paket = 4;
```


### 📊 Aggregate Function yang Digunakan

| Fungsi  | Keterangan       |
| ------- | ---------------- |
| SUM()   | Total pendapatan |
| COUNT() | Jumlah data      |
| MIN()   | Harga terendah   |
| MAX()   | Harga tertinggi  |

---

## 📌 Operasi DDL pada Database db_gym

Dokumentasi ini berisi beberapa perintah DDL (Data Definition Language) yang digunakan untuk memodifikasi struktur tabel pada database GYM.


tambahan
1.rename table member jadi anggota
MariaDB [db_gym]> RENAME TABLE member TO anggota;
Query OK, 0 rows affected (0.021 sec)
MariaDB [db_gym]> SHOW TABLES;
+------------------+
| Tables_in_db_gym |
+------------------+
| anggota          |
| paket            |
| pendaftaran      |
+------------------+
3 rows in set (0.001 sec)

2.truncate table anggota

MariaDB [db_gym]> TRUNCATE TABLE anggota;
Query OK, 0 rows affected (0.033 sec)

3.drop table anggota

MariaDB [db_gym]> DROP TABLE anggota;
Query OK, 0 rows affected (0.020 sec)

4.mengubah tipe data

MariaDB [db_gym]> ALTER TABLE anggota
-> MODIFY umur SMALLINT NOT NULL;
Query OK, 0 rows affected (0.093 sec)
Records: 0  Duplicates: 0  Warnings: 0

DESCRIBE anggota;

+-------------+----------+------+-----+---------+----------------+
| Field       | Type     | Null | Key | Default | Extra          |
+-------------+----------+------+-----+---------+----------------+
| id_member   | int      | NO   | PRI | NULL    | auto_increment |
| nama        | varchar  | NO   |     | NULL    |                |
| umur        | smallint | NO   |     | NULL    |                |
+-------------+----------+------+-----+---------+----------------+

5.mengganti nama kolom

MariaDB [db_gym]> ALTER TABLE anggota
-> CHANGE nama nama_member VARCHAR(50) NOT NULL;
Query OK, 0 rows affected (0.014 sec)
Records: 0  Duplicates: 0  Warnings: 0

DESCRIBE anggota;

+-------------+--------------+------+-----+---------+----------------+
| Field       | Type         | Null | Key | Default | Extra          |
+-------------+--------------+------+-----+---------+----------------+
| id_member   | int          | NO   | PRI | NULL    | auto_increment |
| nama_member | varchar(50)  | NO   |     | NULL    |                |
| umur        | smallint     | NO   |     | NULL    |                |
+-------------+--------------+------+-----+---------+----------------+


# 📈 Hasil Analisis Data

-   👥 Total Member: 5
    
-   📝 Total Pendaftaran: 5
    
-   💰 Total Pendapatan: 2.650.000
    
-   📉 Harga Termurah: 100.000
    
-   📈 Harga Termahal: 1.000.000
    

----------


