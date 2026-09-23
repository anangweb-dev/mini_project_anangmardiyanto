Proyek ini merupakan sistem pengumpul dan pembersih data berita berbasis Python yang menggunakan **NewsAPI**. Sistem ini memanfaatkan pendekatan pemrograman berorientasi objek (*Object-Oriented Programming* / OOP) dengan kelas `KlienBerita` untuk melakukan ekstraksi data, transformasi, dan pembersihan dataset secara otomatis hingga siap dianalisis.

---

# Fitur Utama
- **Integrasi REST API**: Pengambilan data berita langsung dari endpoint NewsAPI.
- **Mekanisme Proteksi Timeout & Retry**: Mencegah kegagalan *request* saat jaringan tidak stabil.
- **Filter Berita**: Mendukung pencarian berbasis kata kunci (*query*), filter bahasa (Bahasa Indonesia), serta batas jumlah artikel.
- **Pembersihan Data Otomatis**: Handling *missing values*, penghapusan data duplikat, serta konversi zona waktu ke format `datetime` standar.

---

# Arsitektur Kode & Class

# **Class `KlienBerita`**
Berperan sebagai *wrapper* utama untuk menangani interaksi dengan NewsAPI.

# **Atribut:**
- `self.api_key`: Menyimpan API Key autentikasi.
- `self.alamat_api`: Endpoint dasar NewsAPI (`https://newsapi.org/v2/everything`).

# **Method Utama:**
- `__init__(self, api_key)`: Inisialisasi kredensial dan URL endpoint.
- `ambil_berita(self, kata_kunci, jumlah)`: 
  - Membentuk parameter *request* API.
  - Melakukan *fetching* data dengan proteksi `try-except` (timeout 20 detik, *retry delay* 3 detik).
  - Mengunduh dan mengekstrak respons JSON menjadi Pandas DataFrame.

---

# Kebijakan Pembersihan Data (*Data Cleaning*)

Proses pembersihan data dilakukan untuk memastikan kualitas dataset tetap tinggi sebelum tahap analisis:

| No | Tindakan Pembersihan | Keputusan | Alasan |
|:--:|:--- |:--- |:--- |
| 1 | **Handling Missing Deskripsi** | Mengisi nilai `NaN` dengan teks `"Tidak ada deskripsi"`. | Mengingat deskripsi bersifat kualitatif, pengisian *placeholder* mempertahankan baris berita tanpa merusak struktur data. |
| 2 | **Penghapusan Baris Tanpa Judul** | Melakukan `dropna(subset=["Judul"])`. | Judul adalah entitas utama berita. Baris tanpa judul tidak relevan untuk dianalisis. |
| 3 | **Deduplikasi (Penghapusan Duplikat)** | Melakukan `drop_duplicates(subset=["URL"])`. | Pengambilan berbasis banyak kata kunci berpotensi mengambil artikel sama. Terjadi pengurangan dari **194 baris** menjadi **155 baris** (39 duplikat terhapus). |
| 4 | **Standardisasi Tipe Data Tanggal** | Mengubah tipe data string ke `datetime64[us, UTC]`. | Memudahkan operasi analisis berbasis *time-series*, kronologi, dan pemfilteran tanggal. |

---

# Pratinjau Dataset Hasil Akhir

Berikut adalah sampel 5 baris pertama (`df_cek.head()`) dari total **155 baris data bersih** yang berhasil dikumpulkan:

| Judul | Deskripsi | Sumber | Tanggal | URL |
| :--- | :--- | :--- | :--- | :--- |
| Pendekatan Berpikir Maju untuk casino premium | Integrasi teknologi canggih ke dalam pengalama... | Heathereatsalmondbutter.com | 2026-09-09 03:00:00+00:00 | `https://www.heathereatsalmondbutter.com/pendek...` |
| Perbedaan Slot Online dan Mesin Slot Konvensional | Slot online dan mesin slot konvensional memili... | Heathereatsalmondbutter.com | 2026-09-13 06:41:51+00:00 | `https://www.heathereatsalmondbutter.com/perbed...` |
| Bagaimana Umpan Balik Meningkatkan cashback ca... | Penekanan pada keamanan dan kepercayaan telah ... | Heathereatsalmondbutter.com | 2026-09-03 18:07:49+00:00 | `https://www.heathereatsalmondbutter.com/bagaim...` |
| Analisis Mendalam Tentang Tren togel Sydney Te... | Perkembangan teknologi telah membawa transform... | Heathereatsalmondbutter.com | 2026-08-27 03:00:00+00:00 | `https://www.heathereatsalmondbutter.com/analis...` |
| Strategi Analisis bandar togel Berdasarkan Dat... | Analisis berbasis data telah mengubah cara pem... | Heathereatsalmondbutter.com | 2026-09-06 03:00:00+00:00 | `https://www.heathereatsalmondbutter.com/strate...` |

---

# Berkas & Presentasi
- 📑 **Slide Presentasi**: Berkas presentasi PowerPoint dapat diakses melalui link berikut: [`presentasi.pptx`](./presentasi.pptx) *(sesuaikan nama file PPT kamu)*.
- 🐍 **Source Code**: Kode sumber Python tersedia di file script `.py` / `.ipynb` di repository ini.

---

