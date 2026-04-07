Berikut adalah **Product Requirements Document (PRD)** untuk pengembangan website **Momis Bakery** menggunakan tech stack Laravel 8.1 dan MySQL, sesuai dengan aset kode yang telah kamu sediakan.

---

# Product Requirements Document (PRD) - Momis Bakery Website

## 1. Ringkasan Proyek

Membangun platform _fullstack_ untuk Momis Bakery yang berfungsi sebagai profil perusahaan, katalog produk interaktif, dan sistem pemesanan yang terintegrasi dengan WhatsApp. Sistem ini juga mencakup Dashboard Admin untuk pengelolaan konten dan pemantauan data penjualan.

## 2. Tech Stack

- **Backend:** Laravel (PHP 8.1)
- **Database:** MySQL di localhost user : root dan password : suparman gak usah cek install atau enggak karena mysql gw pake docker container jd langsung akses aja
- **Frontend:** Ubah ke bootrsap latest
- **Fitur Utama:** Admin Panel (Filament atau Custom), Integrasi API Instagram, WhatsApp Link Generator.

## 3. Fitur & Spesifikasi Halaman (Frontend)

### A. Halaman Home

- **Tone Warna:** Mempertahankan warna sesuai file `index.html`.
- **Floating Action:** Menghilangkan ringkasan pemesanan, diganti dengan **Floating Icon WhatsApp** yang melayang (sticky).
- **Section Menu:** Menghilangkan icon keranjang pada _product card_. Tombol "Lihat Semua Menu" akan mengarah langsung ke halaman **Pesanan/Menu**.
- **Section Ulasan:** Mengubah tampilan _review card_ menjadi **Carousel** (menampilkan 3 kartu sekaligus). Kartu di sebelahnya tetap ada dan mengarah ke Instagram Momis Bakery.
- **Section Instagram Feed:** Menampilkan 6 postingan terbaru. Implementasi menggunakan API Instagram Basic Display atau _library_ feed.
- **Footer:** Informasi kontak, link navigasi, sosial media, dan hak cipta.

### B. Halaman Lokasi / Cabang `outlet.html`

- **Floating Action:** Menghilangkan ringkasan pemesanan, diganti dengan **Floating Icon WhatsApp**.
- **Data Cabang:** Menghapus cabang Cimahi dari daftar.
- **Footer:** Konsisten dengan halaman Home.

### C. Halaman Pesanan / Menu `menu.html`

- **Interaksi:** User dapat memilih menu. Setiap menu yang dipilih akan masuk ke dalam **Floating Pesanan** (Ringkasan Belanja) di bagian bawah/samping.
- **Checkout:** Tombol "Checkout WA" akan mengarahkan user ke halaman **Review Pesanan**.

### D. Halaman Review Pesanan `review_pesanan.html`

- **Navbar:** Disamakan dengan navbar utama di halaman Home.
- **Fungsi:** Menampilkan detail pesanan. Saat user menekan tombol "Kirim", data pesanan disimpan ke database dengan status **"Draft"**, lalu secara otomatis membuka link WhatsApp dengan format pesan teks pesanan yang rapi.

---

## 4. Fitur Admin Panel (Backoffice)

Akses login hanya untuk Admin.

- **Manajemen Hero:** Mengubah teks, gambar, dan tombol pada section Hero di Home.
- **Manajemen Produk:** CRUD (Create, Read, Update, Delete) kategori dan daftar menu, termasuk update harga.
- **Manajemen Ulasan:** Mengelola daftar testimoni yang muncul di carousel.
- **Manajemen Promo:** Mengelola pop-up sales atau banner promo yang aktif.
- **Manajemen Cabang:** Mengelola daftar lokasi outlet yang aktif.
- **Report Penjualan:** Melihat history pesanan masuk (status draft).

---

## 5. Struktur Database (ERD Detail) - MySQL

Berdasarkan kebutuhan di atas, berikut adalah rancangan tabel database-nya:

### Tabel 1: `users` (Admin)

- `id` (PK)
- `name`, `email`, `password`, `remember_token`

### Tabel 2: `products`

- `id` (PK)
- `category_id` (FK)
- `name`, `description`, `price` (decimal), `image_path`, `is_available` (boolean)

### Tabel 3: `categories`

- `id` (PK)
- `category_name`

### Tabel 4: `branches`

- `id` (PK)
- `branch_name`, `address`, `maps_url`, `whatsapp_number`, `is_active`

### Tabel 5: `reviews`

- `id` (PK)
- `customer_name`, `rating` (int), `comment`, `photo_path`, `is_featured`

### Tabel 6: `orders` (Status Draft)

- `id` (PK)
- `order_code` (e.g., MOMIS-20240403-001)
- `customer_name`, `customer_phone`
- `total_price`, `status` (default: 'draft')
- `created_at`, `updated_at`

### Tabel 7: `order_items`

- `id` (PK)
- `order_id` (FK), `product_id` (FK)
- `quantity`, `subtotal_price`

### Tabel 8: `settings` (General Content)

- `id` (PK)
- `key` (e.g., 'hero_title', 'hero_image', 'promo_popup_active')
- `value` (text/blob)

---

## 6. Rancangan Dashboard Admin

Dashboard akan menampilkan metrik utama penjualan makanan:

1.  **Sales Summary:** Total nilai pesanan (rupiah) dan jumlah transaksi harian/bulanan.
2.  **Top Selling Products:** Chart/List produk yang paling banyak dipesan via WhatsApp.
3.  **Branch Performance:** Statistik pesanan berdasarkan preferensi cabang (jika ada pilihan cabang saat order).
4.  **Recent Orders Table:** Tabel 10 pesanan terakhir dengan status draft untuk _follow-up_.
5.  **Quick Action:** Tombol cepat untuk mengaktifkan/menonaktifkan Pop-up Promo jika stok habis atau ada _flash sale_.

---

## 7. Alur Kerja (Workflow)

1.  **User Journey:** Home -> Menu -> Pilih Produk -> Review Pesanan -> Klik Kirim -> Data tersimpan di DB Laravel -> Redirect ke WhatsApp.
2.  **Admin Journey:** Login -> Kelola Produk/Harga/Promo -> Cek Report Penjualan di Dashboard.
