## A Database menggunakan PostgreSQL

1. Carilah 7 transaksi yang memiliki nilai transaksi lebih dari Rp 500.000 dari
pembeli yang memiliki id 1, diurutkan berdasarkan nilai transaksi dari yang
tertinggi.
```
SELECT * FROM Transactions
WHERE 
id = 1 AND amount >= 500000 AND buyer_id NOT NULL
order by amount desc limit 7
```

penjelasan : untuk mencari transaksi diatas 500 ribu dan id = 1 maka harus di filter sesuai dengan WHERE yang ada diatas. lalu untuk mengurutkan berdasarkan tertinggi ke rendah maka diperlukan descending dengan code order by seperti yang diatas.

2. Carilah semua transaksi yang memiliki nilai transaksi antara Rp 1.000.000 dan Rp
10.000.000. Selain informasi transaksi, cantumkan juga nama pembelinya dan
urutkan berdasarkan nama pembeli sesuai abjad.

```
SELECT  a.id, b.name, a.seller_id, a.buyer_id, a.amount
FROM Transactions a
JOIN Users b 
ON a.id = b.id
WHERE a.amount >= 1000000 AND a.amount <= 10000000
GROUP BY a.id, b.name, a.seller_id, a.buyer_id
order by a.name asc
```
penjelasan : dibutuhkan nama pembeli dan nilai transaksi tetapi berasal dari 2 tabel yang berbeda maka dibutuhkan join table. join disini menggunakan join (sama dengan inner join) lalu diberi variabel a dan b untuk memudahkan dan mempersingkat penulisan code. lalu difilter (menggunakan where) berdasarkan amount yaitu antara 1 sampai 10 juta. untuk pengurutan sesuai abjad yaitu a ke z maka menggunakan ascending.

3. Carilah semua id pembeli unik yang juga pernah melakukan transaksi sebagai
penjual dan urutkan berdasarkan id pembeli yang tertinggi ke terendah.
```
SELECT  id, buyer_id, seller_id
FROM Transactions 
WHERE buyer_id NOT NULL AND seller_id NOT NULL
GROUP BY id, buyer_id, seller_id
order by id desc
```
penjelasan: untuk mencari id yang pernah menjual dan membeli maka dibutuhkan kolom seller_id dan buyer_id yang tidak kosong maka difilter dengan NOT NULL dan descending dari kolom id. mencari id unik harus dengan groupby agar tidak muncul yang berbeda.

4. Carilah semua nama penjual yang rata-rata nilai transaksinya lebih dari Rp
1.000.000, lalu urutkan berdasarkan rata-rata nilai transaksinya dari tertinggi ke terendah.
```
SELECT  b.name, a.seller_id, avg(a.amount) AS rata_rata
FROM Transactions a
JOIN Users b 
ON a.id = b.id
WHERE a.rata_rata >= 1000000 AND a.seller_id NOT NULL
group by b.name, a.seller_id
order by a.rata_rata desc
```
penjelasan: untuk mencari penjual maka dibutuhkan filter bagian seller_id yang tidak kosong (dengan NOT NULL) dan rata-rata lebih dari 1 juta. mencari rata-rata menggunakan aggregate yaitu AVERAGE (avg)


5. Carilah semua nama pembeli beserta nomor teleponnya yang memiliki setiap nilai
transaksinya selalu lebih dari Rp 2.000.000, lalu urutkan berdasarkan nama pembeli
sesuai abjad.
```
SELECT  b.name, a.seller_id, b.phone, a.amount
FROM Transactions a
JOIN Users b 
ON a.id = b.id
WHERE a.amount >= 2000000 AND a.seller_id NOT NULL
group by b.name, a.seller_id, b.phone
order by b.name asc
```
penjelasan: untuk mencari transaksi pembeli harus difilter seller_id yang tidak kosong dan amount lebih dari 2 juta dan mengurutkan nama pembeli dengan ascending.

6. Carilah semua id unik dan nama user yang menggunakan email dengan domain
'@gmail.com', yang tidak pernah bertransaksi sebagai pembeli.
```
SELECT  a.id, b.name, b.email, a.seller_id
FROM Transactions a
JOIN Users b 
ON a.id = b.id
WHERE a.buyer_id IS NULL AND  b.email LIKE '%@gmail.com%'
group by a.id, b.name, b.email, a.seller_id
```
penjelasan: untuk mencari user dengan domain @gmail.com maka menggunakan filter LIKE dan untuk mencari yang tidak pernah bertransaksi sebagai pembeli dengan menambahkan filter pada buyer_id (IS NULL) yang yang kosong.

7. Carilah semua id unik dan nama user yang menggunakan nomor handphone Simpati
(0811xxx, 0812xxx, 0813xxx), yang tidak pernah bertransaksi baik sebagai pembeli
ataupun penjual.
```
SELECT  a.id, b.name, b.phone
FROM Transactions a
JOIN Users b 
ON a.id = b.id
WHERE a.buyer_id IS NULL AND a.seller_id IS NULL AND b.phone LIKE '0811%' AND b.phone LIKE '0812%' AND b.phone LIKE '0813%'
group by a.id, b.name, b.phone
```
penjelasan: untuk mencari user yang tidak pernah menjadi penjual ataupun pembeli sama degan nomor 6 diatas dengan filter buyer dan seller is null. lalu untuk mencari dengan nomor telepon dengan angka depan 0811 diperlukan LIKE dan & untuk mencari angka yang sama pada depannya.


8. Carilah semua id pembeli dan jumlah nilai transaksi yang dibuat oleh pembeli
yang membuat akun di tahun 2014 dan berbelanja di tahun 2015.
```
SELECT  a.id, a.buyer_id, b.created_at, a.created_at AS tahun_transaksi, sum(a.amount) AS total
FROM Transactions a
JOIN Users b 
ON a.id = b.id
WHERE year(b.created_at) = '2014' AND year(a.tahun_transaksi) = '2015' AND a.buyer_id NOT NULL
group by a.id, a.buyer_id, b.created_at
```
penjelasan: untuk mencari pembeli yang membuat akun pada tahun 2014 maka difilter pada bagian created_at pada tabel Users dan untuk transaksi 2015 maka pada tabel Transactions. untuk mencari jumlah maka diperlukan aggregate sum pada amount.

9. Carilah semua transaksi yang dibuat dari tanggal 15 Januari 2015 s/d 21 Juni
2015, hanya untuk nilai transaksi lebih dari Rp 5.000.000, lalu cantumkan juga
nama dan email pembeli juga penjualnya.
```
SELECT  a.id, b.name, b.email, a.buyer_id, a.seller_id, a.created_at, a.amount
FROM Transactions a
JOIN Users b 
ON a.id = b.id
WHERE date(a.created_at) >= '2015-01-15' AND date(a.created_at) <= '2015-06-21' AND a.amount >= 5000000
group by a.id, b.name, b.email, a.buyer_id, a.seller_id, a.created_at
```
penjelasan: untuk mencari transaksi pada tanggal tersebut diperlukan filter date seperti code diatas, dan untuk transaksi juga diperlukan filter pada amount yang lebih dari 5 juta

10. Carilah jumlah nilai transaksi, jumlah banyaknya transaksi, dan rata¬rata nilai
transaksi per kuartal dari tahun 2014 sampai 2015.
```
SELECT  extract(quarter from created_at) as quarterly, sum(amount),count(created_at), avg(amount)
FROM Transactions
WHERE
year(created_at) = '2014' AND year(created_at) = '2015'
group by extract(quarter from created_at)
```
penjelasan: untuk mencari transaksi per kuartal maka dibutuhkan extract per kuartal. setelah itu difilter berdasarkan transaksi yang terjadi pada tahun 2014 dan 2015. setelah itu, diperlukan beberapa aggregate untuk mencari jumlah, rata-rata, dan berapa banyak transaksi(dengan count pada created at. tetapi untuk mencari transaksi unik hanya perlu menambah COUNT DISTINCT)

### B. ETL Script (Python) untuk membuat tabel-tabel (datamart) yang berisi segmentation atau klasifikasi per department (Marketing, Sales, IT).

Langkah-langkah yang dilakukan dalam membuat datamart segmented berdasarkan department, yaitu :
1. membuat data dummy terlebih dahulu yang langsung dimasukkan ke dalam database. (dalam hal ini saya menggunakan dBeaver)
2. setelah itu masuk ke dalam jupyter untuk mengubah datanya menjadi tersegmentasi. pertama-tama dengan mengimport dataset agar dapat membaca data yang berada di dalam database.
```
import dataset
```
3. setelah itu agar dapat membaca database, diperlukan connection langsung ke dalam database.
```
db = dataset.connect('(location)')
```
4. lalu untuk menarik data dan mengetest connection pada tabel yang ada di database sudah benar, maka diperlukan print untuk melihat.
```
query_get_karyawan = """
select * from karyawan
"""
result_karyawan = db.query(query_get_karyawan)
for data in result_karyawan:
    print(data)
```
5. setuelah itu untuk mensegmentasikan data berdasarkan department, maka dibutuhkan query memisahkan berdasarkan angka department.
```
query_dept_marketing = """
CREATE TABLE mysql.marketing AS
SELECT * 
FROM mysql.karyawan
WHERE dept = 1
"""

query_dept_sales = """
CREATE TABLE mysql.sales AS
SELECT * 
FROM mysql.karyawan
WHERE dept = 2
"""

query_dept_it = """
CREATE TABLE mysql.it AS
SELECT * 
FROM mysql.karyawan
WHERE dept = 3
"""
```
6. setelah itu kita push kembali ke dalam database query yang sudah kita segmentasikan menjadi 3 tabel tersebut.
```
db.query(query_dept_marketing)
db.query(query_dept_sales)
db.query(query_dept_it)
```
7. setelah itu database sudah tersegmentasi dan terbagi menjadi 3 tabel sesuai department. untuk melihat contohnya dapat menulis code seperti di bawah ini (untuk marketing dengan kode debt=1)
```
query_get_marketing = """
select * from marketing
"""
result_karyawan = db.query(query_get_marketing)
for data in result_karyawan:
    print(data)
```
