# Sistem Manajemen Penjualan dan Order Produk

**Mutmainnah Talib**  
**D0223008**  

_Mata Kuliah Framework Web Based_  
_Tahun 2025_

---

## Deskripsi

**Sistem Manajemen Penjualan dan Order Produk** adalah aplikasi berbasis web yang dirancang untuk membantu pengelolaan penjualan produk dan pemrosesan order secara efisien.  
Sistem ini mendukung tiga jenis role pengguna:

1. **Admin**  
   Pengelola utama aplikasi yang memiliki akses penuh untuk mengatur pengguna, produk, pesanan, kategori, serta melihat laporan transaksi.

2. **Penjual**  
   Pengguna yang dapat menambahkan produk, mengelola stok, melihat pesanan masuk, mengatur status pesanan, serta memantau ulasan dari pembeli.

3. **Pembeli**  
   Pengguna yang dapat melihat produk, melakukan pemesanan, memberikan ulasan, serta melacak status pesanan mereka.

---

## Fitur Utama

1. **Autentikasi Pengguna**  
   Pendaftaran, login, pengelolaan profil (Admin, Penjual, atau Pembeli).

2. **Pengelolaan Produk**  
   Tambah, edit, hapus produk, kelola stok dan harga.

3. **Pemesanan dan Pembelian Produk**  
   Keranjang, checkout, pemantauan status pesanan.

4. **Pengelolaan Order**  
   Admin & Penjual dapat memproses dan update status pengiriman.

5. **Manajemen Role Akses**  
   Hak akses dan tampilan berdasarkan peran.

6. **Pencarian Produk**  
   Cari produk berdasarkan nama/kategori.

7. **Laporan Penjualan**  
   Riwayat transaksi, pendapatan, status pesanan.

---

## Tabel Database

### A. Tabel User (Pengguna)

| Field              | Tipe Data          | Keterangan                             |
|--------------------|--------------------|----------------------------------------|
| id                 | BIGINT (PK)        | Primary key (auto increment)           |
| name               | VARCHAR            | Nama lengkap pengguna                  |
| email              | VARCHAR            | Email unik pengguna (digunakan untuk login) |
| password           | VARCHAR            | Password terenkripsi (bcrypt)          |
| role               | ENUM               | 'admin', 'penjual', 'pembeli'          |
| phone_number       | VARCHAR            | Nomor HP pengguna                      |
| address            | TEXT               | Alamat pengguna                        |
| email_verified_at  | TIMESTAMP          | Tanggal verifikasi email (nullable)    |
| remember_token     | VARCHAR            | Token untuk “remember me”              |
| created_at         | TIMESTAMP          | Otomatis diisi Laravel                 |
| updated_at         | TIMESTAMP          | Otomatis diisi Laravel                 |

---

### B. Tabel Products (Produk)

| Field         | Tipe Data          | Deskripsi                              |
|---------------|--------------------|----------------------------------------|
| id            | INT AUTO_INCREMENT | ID unik produk                         |
| name          | VARCHAR(255)       | Nama produk                            |
| description   | TEXT               | Deskripsi produk                       |
| price         | DECIMAL(10,2)      | Harga produk                           |
| stock         | INT                | Jumlah stok produk                     |
| category_id   | INT                | FK ke categories.id                    |
| image         | VARCHAR(255)       | Path gambar produk                     |
| created_at    | TIMESTAMP          | Waktu produk ditambahkan               |
| updated_at    | TIMESTAMP          | Waktu data diperbarui                  |

---

### C. Tabel Orders (Pesanan)

| Field             | Tipe Data          | Deskripsi                              |
|-------------------|--------------------|----------------------------------------|
| id                | INT AUTO_INCREMENT | ID unik pesanan                        |
| customer_id       | INT (FK ke users.id) | ID pembeli                             |
| order_date        | TIMESTAMP          | Tanggal dan waktu pemesanan            |
| status            | ENUM('pending','processed','shipped','completed','cancelled') | Status pemesanan |
| total_price       | DECIMAL(10,2)      | Total harga seluruh item               |
| shipping_address  | TEXT               | Alamat pengiriman                      |
| created_at        | TIMESTAMP          | Waktu data dibuat                      |
| updated_at        | TIMESTAMP          | Waktu data diperbarui                  |

---

### D. Tabel Order_Items (Detail Item Dalam Pesanan)

| Field         | Tipe Data          | Deskripsi                              |
|---------------|--------------------|----------------------------------------|
| id            | INT AUTO_INCREMENT | ID unik item pesanan                   |
| order_id      | INT (FK ke orders.id) | ID pesanan terkait                    |
| product_id    | INT (FK ke products.id) | ID produk yang dipesan                |
| quantity      | INT                | Jumlah item dipesan                    |
| unit_price    | DECIMAL(10,2)      | Harga satuan saat pemesanan            |
| subtotal      | DECIMAL(10,2)      | Total = quantity × unit_price          |

---

## Relasi Antar Tabel

1. **users → orders**  
   _One-to-Many._  
   Seorang pembeli (role 'pembeli') dapat membuat banyak order.

2. **orders → order_items**  
   _One-to-Many._  
   Setiap order memiliki banyak order_items.

3. **products → order_items**  
   _One-to-Many._  
   Setiap produk bisa muncul di banyak order_items.

4. **products → categories**  
   _Many-to-One._  
   Setiap produk memiliki satu kategori.

5. **users → logs**  
   _One-to-Many._  
   Setiap user (admin, penjual, pembeli) dapat memiliki banyak log aktivitas.

6. **orders → payments**  
   _One-to-One._  
   Setiap order memiliki satu pembayaran.

---

