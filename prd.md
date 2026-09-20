# Product Requirement Document (PRD)
## Proyek: Portal Berita Modern "Nexus News"

| Parameter | Detail Informasi |
| :--- | :--- |
| **Pengembang Utama** | Mochamad Dapa Adhari |
| **Nama Sistem** | Nexus News Portal |
| **Target Waktu** | 1 Minggu (Pengerjaan Efektif) |
| **Model Hosting** | Demo Statis (Vercel) → Production (Laravel + VPS/Shared Hosting) |

---

## 1. Tujuan Proyek & Lingkup Kerja

Membangun aplikasi portal berita berbasis web yang *custom*, aman, cepat, dan responsif. Sistem dirancang dengan tampilan **Modern Dark Mode** (mengacu pada karakter visual Narasi, Indozone, dan Kumparan) serta difokuskan pada kekuatan **SEO** dan **monetisasi iklan dinamis**.

### Scope In (Dikerjakan):
* Pengembangan rancangan UI/UX responsif (Desktop, Tablet, Mobile).
* Pembuatan skema basis data relasional untuk sistem berita & iklan.
* Pengembangan CMS Redaksi kustom berbasis Laravel (Multi-role).
* Modul Manajemen Iklan (*Ad Inventory System*).
* Integrasi SEO dasar (Meta Tag, OpenGraph, XML Sitemap).
* Setup infrastruktur (Domain .com / .id + Server/Hosting).

### Scope Out (Tidak Dikerjakan di Tahap Ini):
* Aplikasi mobile native (Android/iOS APK).
* Sistem pembayaran *paywall* / konten berlangganan otomatis.

---

## 2. Spesifikasi Teknis & Infrastruktur

### Tech Stack
* **Frontend:** HTML5, Tailwind CSS, Plus Jakarta Sans, CSS Glassmorphism.
* **Backend:** Laravel Framework (PHP 8.x).
* **Database:** MySQL / PostgreSQL.
* **Deployment & Demo:** Vercel (Demo Prototype) → Server/VPS Hosting (Production).

### Breakdown Anggaran (Tahun Pertama)
* **Domain (.com / .id):** ~Rp 200.000 / tahun
* **Server / Hosting (Laravel Ready):** ~Rp 600.000 / tahun (Estimasi operasional ~Rp 50.000/bulan)
* **Jasa Pembuatan Sistem:** Rp 700.000 (Sekali bayar - Harga Teman)
* **Total Biaya Paket Terima Beres:** **Rp 1.500.000**
* *Catatan Perpanjangan Tahun ke-2:* ~Rp 800.000 / tahun.

---

## 3. Peta Halaman (Sitemap) & Antarmuka UI

Sistem terdiri dari **7 halaman utama** berbasis folder statis untuk tahap awal demo:

| Nama File | Fungsi & Komponen Utama |
| :--- | :--- |
| **`index.html`** | **Halaman Utama (Beranda):** Top bar, navbar *glassmorphism*, *trending ticker*, *hero article split layout*, grid berita terbaru, *sidebar* terpopuler, dan slot iklan *leaderboard*. |
| **`detail.html`** | **Halaman Detail Artikel:** *Breadcrumbs*, judul utama, metadata penulis/editor, foto utama, *text body*, *blockquote*, *social share*, *tags*, dan artikel terkait. |
| **`kategori.html`** | **Halaman Indeks Kategori:** Header kategori, deskripsi topik, *grid card* artikel 3 kolom, dan *pagination*. |
| **`pasang-iklan.html`** | **Media Kit & Info Iklan:** Informasi posisi slot iklan, spesifikasi dimensi banner, dan formulir kontak pengiklan. |
| **`search.html`** | **Hasil Pencarian:** Bilah pencarian kata kunci, filter tanggal/kategori, dan daftar hasil pencarian. |
| **`redaksi.html`** | **Struktur Redaksi & Pedoman:** Susunan dewan redaksi, alamat kantor, dan teks Pedoman Media Siber Dewan Pers (Syarat Google News/AdSense). |
| **`404.html`** | **Custom Error Page:** Penanganan tautan rusak dengan navigasi kembali ke beranda & rekomendasi berita populer. |

---

## 4. Kebutuhan Fungsional (Functional Requirements)

### A. Sisi Pembaca (Frontend)
1. **Navigasi Responsif:** *Header sticky* dengan menu hamburger yang dapat dibuka di perangkat seluler.
2. **Indikator Trending Topic:** Bilah *ticker* di bawah navigasi utama untuk topik-topik hangat.
3. **Pencatatan Populer Otomatis:** Sistem mencatat *views* setiap artikel secara *real-time* untuk menyusun daftar berita "Terpopuler".
4. **Optimasi Berbagi (Social Share):** Tombol bagi cepat ke WhatsApp dan Twitter dengan pratinjau gambar (*OpenGraph*) yang rapi.

### B. Sisi Redaksi (Backend CMS)
1. **Role-Based Access Control (RBAC):**
   * **Super Admin:** Akses penuh ke seluruh sistem, manajemen user, dan modul iklan.
   * **Editor:** Meninjau, menyunting, dan menyetujui (*publish*) artikel dari jurnalis.
   * **Author/Jurnalis:** Menulis dan mengunggah draf artikel milik sendiri.
2. **WYSIWYG Rich-Text Editor:** Fasilitas format teks, penyisipan gambar dalam paragraf, dan kutipan (*blockquote*).
3. **Manajemen Kategori & Tag:** Pengelompokan isu secara fleksibel.

### C. Sistem Manajemen Iklan (Ad Inventory)
1. **Slot Iklan Terstruktur:**
   * **Leaderboard Banner:** Posisi atas (ukuran 728x90 px).
   * **Sidebar Banner:** Posisi samping (ukuran 300x250 px).
   * **In-Article Native Ad:** Posisi di tengah paragraf artikel.
2. **Aktivasi Dinamis:** Admin dapat menyalakan/mematikan iklan, mengatur batas tanggal penayangan, dan memasukkan *redirect URL* tujuan.

---

## 5. Skema Basis Data (Database Structure)

```text
+-------------------+       +-------------------+       +-------------------+
|       users       |       |     articles      |       |    categories     |
+-------------------+       +-------------------+       +-------------------+
| id (PK)           |1     *| id (PK)           |*     1| id (PK)           |
| name              |-------| author_id (FK)    |-------| name              |
| email             |       | category_id (FK)  |       | slug              |
| password          |       | title             |       +-------------------+
| role (enum)       |       | slug              |
+-------------------+       | content           |       +-------------------+
                            | thumbnail_path    |       |       tags        |
                            | views_count       |       +-------------------+
                            | status (enum)     |       | id (PK)           |
                            | published_at      |       | name              |
                            +-------------------+       | slug              |
                                      |                 +-------------------+
                                      |                           |
                                      +-------------+-------------+
                                                    |
                                          +-------------------+
                                          |    article_tag    |
                                          +-------------------+
                                          | article_id (FK)   |
                                          | tag_id (FK)       |
                                          +-------------------+

+-------------------+
|  advertisements   |
+-------------------+
| id (PK)           |
| title             |
| image_url         |
| redirect_url      |
| placement (enum)  |
| is_active (bool)  |
+-------------------+


6. Rencana Kerja & Jadwal Pelaksanaan (1 Minggu)
Hari 1: Finalisasi Wireframe & Penyusunan File Statis HTML/Tailwind (index, detail, kategori, dll).

Hari 2: Deployment Demo Statis ke Vercel untuk Peninjauan Klien (Bang Andi).

Hari 3: Inisialisasi Proyek Laravel, Setup Migrations, dan Skema Database.

Hari 4: Pembangunan Dashboard Redaksi (CMS) & Modul Manajemen Iklan.

Hari 5: Integrasi Frontend Blade Templating dengan API/Backend Laravel.

Hari 6: Pengujian Responsif, Optimasi SEO, dan Setup Domain/Hosting Production.

Hari 7: Go Live (Penyerahan Web Siap Pakai & Serah Terima Akses Admin).