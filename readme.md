
### **Dremio: Menyatukan Lautan Data yang Terpisah**


### **1. Terjebak dalam Silo Data**

Bayangkan Anda seorang analis data yang harus membuat laporan penjualan. Data Anda tidak berada di satu tempat yang nyaman. Data transaksi penjualan ada di **MySQL**, sementara data detail pelanggan disimpan di **PostgreSQL**. Untuk mendapatkan gambaran lengkap tentang "siapa yang membeli apa," Anda harus menjalankan kueri yang rumit, bolak-balik antara dua sistem yang berbeda.

Kemudian sedang data log activiy sales saat menggunakana aplikasi di database mongodb, membuat menjadikan nya semakin rumit untuk mengintegrasikan nya 

Ini adalah masalah klasik di dunia data: **silo data**. Data tersebar, terisolasi, dan tidak saling terhubung. Jalan keluar tradisionalnya adalah **ETL (Extract, Transform, Load)**—sebuah proses yang memakan waktu, dan membuat data Anda tidak *real-time*. Tim Anda harus menunggu untuk mendapatkan data yang sudah usang.

ditemukan  **Dremio**.

### **2. Dremio, Jembatan Ajaib di Antara Data**

Dremio bukanlah *data warehouse* atau database. Dremio adalah sebuah **mesin virtualisasi data** yang berfungsi sebagai jembatan cerdas di antara semua sumber data Anda. Tanpa perlu menyalin atau memindahkan data, Dremio menciptakan sebuah lapisan virtual yang menyatukan semuanya. Dremio memungkinkan untuk melakukan query data dari mana pun asalnya, seolah-olah semuanya sudah berada di satu tempat.

Eksperimen ini dimulai dengan satu tujuan sederhana: **menggabungkan data pelanggan dari PostgreSQL dengan data transaksi dari MySQL secara *real-time***.

### **3. Menghubungkan Dua Dunia**

Langkah pertama adalah memperkenalkan Dremio kepada kedua sumber data. membuka interface Dremio dan memilih konektor untuk **MySQL** dan **PostgreSQL**.

memasukkan detail koneksi, dan dalam hitungan detik, Dremio berhasil terhubung. Di panel kiri, kami melihatnya: `MySQL - B2C Database` ,  `Postgres - Source Data - CRM`,  dan `MongoDB- Data Log`. Tiga silo yang terpisah kini berdampingan, seolah-olah mereka sudah berteman lama.


### **4. Menyatukan Cerita dengan SQL**

Sekarang, tiba saatnya untuk sihir yang sesungguhnya. Di dalam editor SQL Dremio,  menulis sebuah kueri sederhana. 

**Data Sales Berdasarkan Customer**

Kueri ini akan menggabungkan data transaksi (`sales_order` dari MySQL) dengan data pelanggan (`pelanggan` dari PostgreSQL) berdasarkan `id` pelanggan.

```sql
SELECT p.nama_depan, so.amount_sale, so.date_order from "MySQL - B2C Database".sbiz_opensource.sales_order as so 
JOIN "Postgres - Source Data - CRM".public.pelanggan as p
on so.client_id = p.pelanggan_id
```

Dengan satu klik `Run`, Dremio melakukan keajaiban. Ia mengirimkan perintah tersebut ke kedua database, mengambil potongan data yang dibutuhkan, dan menggabungkannya di lapisan virtual Dremio.


**4.1. Data Sales Berdasarkan Customer**

Kueri ini akan menggabungkan data transaksi (`sales_order` dari MySQL) dengan data pelanggan (`pelanggan` dari PostgreSQL) berdasarkan `id` pelanggan.

```sql
SELECT p.nama_depan, so.amount_sale, so.date_order from "MySQL - B2C Database".sbiz_opensource.sales_order as so 
JOIN "Postgres - Source Data - CRM".public.pelanggan as p
on so.client_id = p.pelanggan_id
```

Dengan satu klik `Run`, Dremio melakukan keajaiban. Ia mengirimkan perintah tersebut ke kedua database, mengambil potongan data yang dibutuhkan, dan menggabungkannya di lapisan virtual Dremio.



**4.2. Data Log Activity Sales**

Kueri ini akan menggabungkan data transaksi sales (`memmber` dari MySQL) dengan data activiy user (`log_activity` dari Mongodb) berdasarkan `username`

```sql
SELECT l.*, u.username
FROM "MySQL - B2C Database".sbiz_opensource."user" as u
INNER JOIN "MongoDB- Data Log"."microservice-log-activity-user".log_activity_user_test as l
ON l.username = u.username
```

Dengan satu klik `Run`, Dremio melakukan keajaiban. Ia mengirimkan perintah tersebut ke kedua database, mengambil potongan data yang dibutuhkan, dan menggabungkannya di lapisan virtual Dremio.



### **Kesimpulan: Masa Depan Analisis Data**

Eksperimen ini membuktikan bahwa **Dremio** bukan hanya alat, tetapi sebuah revolusi dalam cara kita bekerja dengan data. Kami berhasil memecahkan masalah silo data tanpa proses ETL yang membosankan. Dremio mengubah data yang terpisah menjadi satu sumber kebenaran yang konsisten, memberdayakan siapa pun untuk mengakses, menggabungkan, dan menganalisis data secara *real-time*.

Masa depan analisis data adalah tentang fleksibilitas dan kecepatan, dan Dremio adalah kuncinya.