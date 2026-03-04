# join_PemesananHotel

## 1. Deskripsi Materi
### 1.1 INNER JOIN
INNER JOIN di SQL adalah jenis join yang digunakan untuk mengambil data dari dua atau lebih tabel, hanya pada baris-baris yang memiliki nilai yang cocok (matching) di kolom yang dijadikan kunci penghubung (join condition).
### 1.2 LEFT JOIN
LEFT JOIN di SQL adalah jenis join yang digunakan untuk menggabungkan dua tabel, di mana semua baris dari tabel kiri (left table) akan ditampilkan, dan baris dari tabel kanan (right table) hanya akan ditampilkan jika ada kecocokan berdasarkan kondisi join.
### 1.3 RIGHT JOIN
RIGHT JOIN di SQL adalah jenis join yang mengembalikan semua baris dari tabel kanan (right table) dan hanya baris yang cocok dari tabel kiri (left table).
Jika tidak ada kecocokan di tabel kiri, kolom dari tabel kiri akan berisi NULL.

## 2. Implementasi
### 2.1 Membuat Database
```
MariaDB [(none)]> CREATE DATABASE pemesanan_hotel;
Query OK, 1 row affected (0.002 sec)
```
### 2.2 Menggunakan Database yang Telah Dibuat 
```
MariaDB [(none)]> USE pemesanan_hotel;
Database changed
```
### 2.3 Membuat Tabel Pengunjung
```
MariaDB [pemesanan_hotel]> CREATE TABLE pengunjung(
    -> id_pengunjung INT AUTO_INCREMENT PRIMARY KEY,
    -> nama_pengunjung VARCHAR(100) NOT NULL,
    -> no_hp VARCHAR(15) NOT NULL,
    -> alamat VARCHAR(100) NOT NULL);
Query OK, 0 rows affected (0.022 sec)
```
### 2.4 Menginput Data Pada Tabel Pengunjung
```
MariaDB [pemesanan_hotel]> INSERT INTO pengunjung(nama_pengunjung, no_hp, alamat) VALUES
    -> ('Andi', '089535641023', 'Bogor'),
    -> ('Aulia', '0894325641023', 'Jakarta'),
    -> ('Bayu', '089432564234', 'Depok'),
    -> ('Ayu', '089437864234', 'Banjarmasin'),
    -> ('Andika', '089431234234', 'Medan');
Query OK, 5 rows affected (0.003 sec)
Records: 5  Duplicates: 0  Warnings: 0

MariaDB [pemesanan_hotel]> SELECT * FROM pengunjung;
+---------------+-----------------+---------------+-------------+
| id_pengunjung | nama_pengunjung | no_hp         | alamat      |
+---------------+-----------------+---------------+-------------+
|             1 | Andi            | 089535641023  | Bogor       |
|             2 | Aulia           | 0894325641023 | Jakarta     |
|             3 | Bayu            | 089432564234  | Depok       |
|             4 | Ayu             | 089437864234  | Banjarmasin |
|             5 | Andika          | 089431234234  | Medan       |
+---------------+-----------------+---------------+-------------+
5 rows in set (0.001 sec)
```
### 2.5 Membuat Tabel Kamar Hotel
```
MariaDB [pemesanan_hotel]> CREATE TABLE kamar(
    -> id_kamar INT AUTO_INCREMENT PRIMARY KEY,
    -> tipe_kamar VARCHAR(20) NOT NULL,
    -> harga DECIMAL(10,2) NOT NULL);
Query OK, 0 rows affected (0.015 sec)
```
### 2.6 Menginput Data Pada Tabel Kamar
```
MariaDB [pemesanan_hotel]> INSERT INTO kamar(tipe_kamar, harga) VALUES
    -> ('Standar', 200000),
    -> ('VIP', 300000),
    -> ('VVIP', 300000);
Query OK, 3 rows affected (0.013 sec)
Records: 3  Duplicates: 0  Warnings: 0

MariaDB [pemesanan_hotel]> SELECT * FROM kamar;
+----------+------------+-----------+
| id_kamar | tipe_kamar | harga     |
+----------+------------+-----------+
|        1 | Standar    | 200000.00 |
|        2 | VIP        | 300000.00 |
|        3 | VVIP       | 300000.00 |
+----------+------------+-----------+
3 rows in set (0.001 sec)
```
### 2.7 Membuat Tabel Pesanan
```
MariaDB [pemesanan_hotel]> CREATE TABLE pesanan(
    -> id_pesanan INT AUTO_INCREMENT PRIMARY KEY,
    -> id_pengunjung INT,
    -> id_kamar INT,
    -> tanggal_pesanan DATE,
    -> FOREIGN KEY (id_pengunjung) REFERENCES pengunjung(id_pengunjung),
    -> FOREIGN KEY (id_kamar) REFERENCES kamar(id_kamar));
Query OK, 0 rows affected (0.086 sec)
```
### 2.8 Menginput Data Pada Tabel Pesanan
```
MariaDB [pemesanan_hotel]> INSERT INTO pesanan(id_pengunjung, id_kamar, tanggal_pesanan) VALUES
    -> (1, 1, '2026-03-01'),
    -> (1, 2, '2025-03-01'),
    -> (1, 3, '2026-01-01'),
    -> (2, 3, '2026-01-01'),
    -> (2, 2, '2026-02-01'),
    -> (2, 1, '2026-02-11'),
    -> (3, 1, '2026-01-11'),
    -> (4, 1, '2025-01-21'),
    -> (4, 2, '2024-01-21');
Query OK, 9 rows affected (0.018 sec)
Records: 9  Duplicates: 0  Warnings: 0

MariaDB [pemesanan_hotel]> SELECT * FROM pesanan;
+------------+---------------+----------+-----------------+
| id_pesanan | id_pengunjung | id_kamar | tanggal_pesanan |
+------------+---------------+----------+-----------------+
|          1 |             1 |        1 | 2026-03-01      |
|          2 |             1 |        2 | 2025-03-01      |
|          3 |             1 |        3 | 2026-01-01      |
|          4 |             2 |        3 | 2026-01-01      |
|          5 |             2 |        2 | 2026-02-01      |
|          6 |             2 |        1 | 2026-02-11      |
|          7 |             3 |        1 | 2026-01-11      |
|          8 |             4 |        1 | 2025-01-21      |
|          9 |             4 |        2 | 2024-01-21      |
+------------+---------------+----------+-----------------+
9 rows in set (0.001 sec)
```
### 2.9 Menampilkan Data Menggunakan Inner Join
Query INNER JOIN tersebut bertujuan untuk mengintegrasikan data dari tabel pesanan, pengunjung, dan kamar berdasarkan relasi foreign key yang telah dibuat.
```
MariaDB [pemesanan_hotel]> SELECT pesanan.id_pesanan, pesanan.tanggal_pesanan, pengunjung.nama_pengunjung, kamar.tipe_kamar, kamar.harga
    -> FROM pesanan
    -> INNER JOIN pengunjung
    -> ON pesanan.id_pengunjung=pengunjung.id_pengunjung
    -> INNER JOIN kamar
    -> ON pesanan.id_kamar=kamar.id_kamar;
+------------+-----------------+-----------------+------------+-----------+
| id_pesanan | tanggal_pesanan | nama_pengunjung | tipe_kamar | harga     |
+------------+-----------------+-----------------+------------+-----------+
|          1 | 2026-03-01      | Andi            | Standar    | 200000.00 |
|          2 | 2025-03-01      | Andi            | VIP        | 300000.00 |
|          3 | 2026-01-01      | Andi            | VVIP       | 300000.00 |
|          4 | 2026-01-01      | Aulia           | VVIP       | 300000.00 |
|          5 | 2026-02-01      | Aulia           | VIP        | 300000.00 |
|          6 | 2026-02-11      | Aulia           | Standar    | 200000.00 |
|          7 | 2026-01-11      | Bayu            | Standar    | 200000.00 |
|          8 | 2025-01-21      | Ayu             | Standar    | 200000.00 |
|          9 | 2024-01-21      | Ayu             | VIP        | 300000.00 |
+------------+-----------------+-----------------+------------+-----------+
9 rows in set (0.001 sec)
```
### 2.10 Menampilkan Data Menggunakan Left Join
Query LEFT JOIN bertujuan untuk menggabungkan tabel pengunjung dengan tabel pesanan, dengan prioritas pada tabel pengunjung sebagai tabel kiri.
```
MariaDB [pemesanan_hotel]> SELECT *
    -> FROM pengunjung
    -> LEFT JOIN pesanan
    -> ON pengunjung.id_pengunjung=pesanan.id_pengunjung;
+---------------+-----------------+---------------+-------------+------------+---------------+----------+-----------------+
| id_pengunjung | nama_pengunjung | no_hp         | alamat      | id_pesanan | id_pengunjung | id_kamar | tanggal_pesanan |
+---------------+-----------------+---------------+-------------+------------+---------------+----------+-----------------+
|             1 | Andi            | 089535641023  | Bogor       |          1 |             1 |        1 | 2026-03-01      |
|             1 | Andi            | 089535641023  | Bogor       |          2 |             1 |        2 | 2025-03-01      |
|             1 | Andi            | 089535641023  | Bogor       |          3 |             1 |        3 | 2026-01-01      |
|             2 | Aulia           | 0894325641023 | Jakarta     |          4 |             2 |        3 | 2026-01-01      |
|             2 | Aulia           | 0894325641023 | Jakarta     |          5 |             2 |        2 | 2026-02-01      |
|             2 | Aulia           | 0894325641023 | Jakarta     |          6 |             2 |        1 | 2026-02-11      |
|             3 | Bayu            | 089432564234  | Depok       |          7 |             3 |        1 | 2026-01-11      |
|             4 | Ayu             | 089437864234  | Banjarmasin |          8 |             4 |        1 | 2025-01-21      |
|             4 | Ayu             | 089437864234  | Banjarmasin |          9 |             4 |        2 | 2024-01-21      |
|             5 | Andika          | 089431234234  | Medan       |       NULL |          NULL |     NULL | NULL            |
+---------------+-----------------+---------------+-------------+------------+---------------+----------+-----------------+
10 rows in set (0.001 sec)
```
### 2.11 Menampilkan Data Menggunakan Right Join
Query RIGHT JOIN menggabungkan tabel pengunjung dan pesanan dengan prioritas pada tabel pesanan sebagai tabel kanan.
```
MariaDB [pemesanan_hotel]> SELECT *
    -> FROM pengunjung
    -> RIGHT JOIN pesanan
    -> ON pengunjung.id_pengunjung=pesanan.id_pengunjung;
+---------------+-----------------+---------------+-------------+------------+---------------+----------+-----------------+
| id_pengunjung | nama_pengunjung | no_hp         | alamat      | id_pesanan | id_pengunjung | id_kamar | tanggal_pesanan |
+---------------+-----------------+---------------+-------------+------------+---------------+----------+-----------------+
|             1 | Andi            | 089535641023  | Bogor       |          1 |             1 |        1 | 2026-03-01      |
|             1 | Andi            | 089535641023  | Bogor       |          2 |             1 |        2 | 2025-03-01      |
|             1 | Andi            | 089535641023  | Bogor       |          3 |             1 |        3 | 2026-01-01      |
|             2 | Aulia           | 0894325641023 | Jakarta     |          4 |             2 |        3 | 2026-01-01      |
|             2 | Aulia           | 0894325641023 | Jakarta     |          5 |             2 |        2 | 2026-02-01      |
|             2 | Aulia           | 0894325641023 | Jakarta     |          6 |             2 |        1 | 2026-02-11      |
|             3 | Bayu            | 089432564234  | Depok       |          7 |             3 |        1 | 2026-01-11      |
|             4 | Ayu             | 089437864234  | Banjarmasin |          8 |             4 |        1 | 2025-01-21      |
|             4 | Ayu             | 089437864234  | Banjarmasin |          9 |             4 |        2 | 2024-01-21      |
+---------------+-----------------+---------------+-------------+------------+---------------+----------+-----------------+
9 rows in set (0.001 sec)
```
## 3. Kesimpulan
Setelah melakukan percobaan yang telah dilakukan pada database pemesanan_hotel, dapat disimpulkan bahwa:
```
INNER JOIN digunakan untuk menampilkan data yang memiliki kecocokan di kedua tabel yang digabungkan. Jika tidak terdapat relasi, maka data tersebut tidak akan ditampilkan.
```
```
LEFT JOIN menampilkan seluruh data dari tabel sebelah kiri, meskipun tidak memiliki pasangan di tabel sebelah kanan. Data yang tidak memiliki relasi akan ditampilkan dengan nilai NULL pada kolom tabel kanan.
```
```
RIGHT JOIN menampilkan seluruh data dari tabel sebelah kanan, meskipun tidak memiliki pasangan di tabel sebelah kiri. Jika tidak ada relasi, maka kolom dari tabel kiri akan bernilai NULL.
```
