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

### D. Keamanan Sistem (Security Requirements)

**Prioritas Tinggi (wajib ada sejak hari pertama development backend):**
1. **Sanitasi Konten WYSIWYG:** Semua output dari rich-text editor (kolom `content`) wajib disaring lewat library sanitasi HTML (mis. HTMLPurifier) sebelum disimpan ke database, untuk mencegah *stored XSS* dari akun jurnalis/editor yang disusupi.
2. **Validasi File Upload:** Upload gambar (`thumbnail_path` & gambar inline artikel) wajib divalidasi berdasarkan MIME type asli file (bukan hanya ekstensi), di-*re-encode* ulang sebagai gambar, disimpan dengan nama file acak (random hash) di luar direktori yang bisa dieksekusi PHP.
3. **Rate Limiting Login:** Middleware `throttle` Laravel wajib diaktifkan pada endpoint login CMS untuk mencegah *brute force* ke akun redaksi.
4. **Validasi `redirect_url` Iklan:** URL tujuan pada modul iklan wajib divalidasi format dan scheme-nya (hanya `http`/`https`), untuk mencegah penyalahgunaan sebagai *open redirect* ke situs phishing/malware.
5. **HTTPS/SSL Wajib:** Seluruh environment (demo & production) wajib berjalan di atas HTTPS (Let's Encrypt gratis untuk shared hosting/VPS).

**Prioritas Menengah:**
6. **Two-Factor Authentication (2FA):** Wajib untuk role Super Admin, disarankan juga untuk Editor, mengingat akses penuh ke sistem dan modul iklan.
7. **Anti-Spam Views Counter:** Pencatatan *views* otomatis wajib dilakukan deduplikasi per sesi/IP per artikel (mis. maksimal 1 view/artikel/24 jam per pengunjung) untuk mencegah manipulasi daftar "Terpopuler".
8. **Content Security Policy (CSP):** Header CSP wajib disetel di level aplikasi, khususnya karena sistem me-render `image_url` iklan dari sumber eksternal — mencegah satu iklan yang disusupi menjadi vektor XSS ke seluruh halaman.
9. **Audit Log Editorial:** Sistem wajib mencatat siapa membuat/menyunting/menghapus/mem-publish artikel apa dan kapan, untuk akuntabilitas editorial dan forensik insiden.

**Prioritas Rendah (dicatat, dieksekusi menyesuaikan waktu):**
10. **Kepatuhan UU PDP:** Karena sistem menyimpan data pribadi (email user, data pengiklan dari formulir kontak), perlu privacy policy minimal dan kebijakan retensi data.

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
| impressions_count |
| clicks_count      |
+-------------------+

+-------------------+
|    audit_logs     |
+-------------------+
| id (PK)           |
| user_id (FK)      |
| action (enum)     |
| subject_type      |
| subject_id        |
| created_at        |
+-------------------+

+-------------------+
|       media        |
+-------------------+
| id (PK)           |
| article_id (FK)   |
| file_path         |
| mime_type         |
| created_at        |
+-------------------+
```

### Penambahan Skema (Security & Operasional)

* **`articles.deleted_at`** — Kolom *soft delete* agar artikel yang tidak sengaja terhapus (sudah punya views/share) bisa dipulihkan, bukan hilang permanen.
* **Tabel `audit_logs`** — mencatat setiap aksi create/update/delete/publish per user, untuk akuntabilitas editorial (lihat requirement D.9 di atas).
* **`advertisements.impressions_count` & `clicks_count`** — dibutuhkan untuk pelaporan/billing ke klien pengiklan; tanpa ini tidak ada data untuk menjustifikasi tagihan iklan.
* **Tabel `media`** — media library terpisah agar file gambar yang di-upload tapi artikelnya terhapus (*orphan file*) tetap bisa dilacak dan dibersihkan.
* **Indexing wajib:** `slug` di tabel `articles`, `categories`, dan `tags` harus `unique` + `indexed`; `author_id` dan `category_id` di `articles` sebagai foreign key juga perlu index untuk performa query listing/kategori pada skala data besar.
* **`views_count` pada skala tinggi:** untuk artikel viral, pertimbangkan memindahkan counter ke cache (Redis) dengan sinkronisasi berkala ke DB, agar tidak terjadi *row lock contention* saat traffic tinggi.

---

## 6. Kebutuhan Non-Fungsional & Operasional Tambahan

1. **Mekanisme Search:** `search.html` memerlukan kejelasan implementasi backend. Query `LIKE '%keyword%'` di MySQL akan melambat seiring jumlah artikel bertambah — minimal gunakan *full-text index* MySQL, atau pertimbangkan Meilisearch/Typesense jika ingin performa pencarian yang serius.
2. **Sitemap XML Dinamis:** Sitemap wajib digenerate otomatis setiap ada artikel baru/terbit (bukan file statis), agar Google dapat mengindeks artikel baru — sitemap statis membuat investasi SEO tidak efektif.
3. **Backup Strategy:** Perlu backup database otomatis terjadwal (cron job), karena shared hosting kelas ~Rp50.000/bulan umumnya tidak menyediakan backup otomatis secara default. Konten redaksi adalah aset bisnis utama sistem ini.

---

## 7. Rencana Kerja & Jadwal Pelaksanaan (1 Minggu)

> **Catatan Kelayakan:** Menambahkan seluruh item keamanan (sanitasi upload, rate limiting, 2FA, audit log) ke dalam timeline 7 hari solo development sangat agresif. Disarankan memisahkan menjadi **MVP** (sanitasi WYSIWYG, validasi upload, rate limiting login, HTTPS, validasi `redirect_url` — wajib ada dari hari pertama) vs **Fase 2** (2FA, audit log, CSP, anti-spam views counter, kepatuhan UU PDP — bisa menyusul setelah go-live awal).

Hari 1: Finalisasi Wireframe & Penyusunan File Statis HTML/Tailwind (index, detail, kategori, dll).

Hari 2: Deployment Demo Statis ke Vercel untuk Peninjauan Klien (Bang Andi).

Hari 3: Inisialisasi Proyek Laravel, Setup Migrations, dan Skema Database (termasuk kolom/tabel security: `deleted_at`, `audit_logs`, tracking iklan).

Hari 4: Pembangunan Dashboard Redaksi (CMS) & Modul Manajemen Iklan — termasuk sanitasi WYSIWYG (HTMLPurifier) dan validasi file upload sejak awal, bukan ditambahkan belakangan.

Hari 5: Integrasi Frontend Blade Templating dengan API/Backend Laravel, plus rate limiting login dan validasi `redirect_url`.

Hari 6: Pengujian Responsif, Optimasi SEO (termasuk sitemap dinamis), Setup HTTPS/SSL, dan Setup Domain/Hosting Production.

Hari 7: Go Live (Penyerahan Web Siap Pakai & Serah Terima Akses Admin) — sertakan setup backup database otomatis sebelum serah terima.