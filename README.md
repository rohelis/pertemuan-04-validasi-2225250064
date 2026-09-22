`# Pertemuan 04 Seleksi Multi-Kondisi dan Validasi Input

**Nama:** Rohelis
**NIM:** 2225250064
**Kelas:** 3B

## Tujuan
Membangun program validasi dan klasifikasi dengan rantai `if-elif-else` untuk mengevaluasi input berdasar banyak kondisi (tipe data, rentang nilai, dan bobot penilaian) secara terstruktur tanpa adanya tumpang tindih logika.

## Cara Menjalankan
Jalankan program melalui terminal atau *command prompt* dengan mengeksekusi file Python berikut:

## Tabel Keputusan

| Kategori | Syarat (Kondisi) | Contoh Masukan (Ujian, Tugas, Hadir) |
| :--- | :--- | :--- |
| **Bukan Angka** | Gagal konversi ke `float` (`ValueError`) | `"A"`, `80`, `90` |
| **Rentang Tidak Valid** | Nilai `< 0` ATAU `> 100` | `110`, `80`, `90` |
| **Gagal Kehadiran** | `hadir < 80` (Otomatis Predikat E) | `90`, `90`, `75` |
| **Predikat A** | `nilai_akhir >= 85` dan `hadir >= 80` | `90`, `85`, `100` |
| **Predikat B** | `70 <= nilai_akhir < 85` dan `hadir >= 80` | `75`, `70`, `90` |
| **Predikat C** | `60 <= nilai_akhir < 70` dan `hadir >= 80` | `65`, `60`, `85` |
| **Predikat D** | `50 <= nilai_akhir < 60` dan `hadir >= 80` | `55`, `50`, `90` |
| **Predikat E** | `nilai_akhir < 50` dan `hadir >= 80` | `40`, `45`, `80` |

*Catatan: Nilai Akhir dihitung dari `(0.6 * ujian) + (0.4 * tugas)`.*


## Hasil Pengujian

| Masukan (U, T, H) | Keluaran Diharapkan | Keluaran Aktual | Status |
| :--- | :--- | :--- | :--- |
| `85`, `90`, `kosong` | Masukan ditolak: seluruh data harus berupa angka. | Masukan ditolak: seluruh data harus berupa angka. | ✅ Pass |
| `-10`, `80`, `90` | Masukan ditolak: nilai ujian di luar rentang 0 sampai 100. | Masukan ditolak: nilai ujian di luar rentang 0 sampai 100. | ✅ Pass |
| `100`, `100`, `70` | Nilai akhir=100.00, Predikat=E, Status=Belum lulus | Nilai akhir=100.00, Predikat=E, Status=Belum lulus | ✅ Pass |
| `90`, `80`, `100` | Nilai akhir=86.00, Predikat=A, Status=Lulus | Nilai akhir=86.00, Predikat=A, Status=Lulus | ✅ Pass |
| `65`, `65`, `80` | Nilai akhir=65.00, Predikat=C, Status=Lulus | Nilai akhir=65.00, Predikat=C, Status=Lulus | ✅ Pass |
| `45`, `50`, `90` | Nilai akhir=47.00, Predikat=E, Status=Belum lulus | Nilai akhir=47.00, Predikat=E, Status=Belum lulus | ✅ Pass |

## Refleksi

Dalam pengembangan program ini, satu skenario masukan tidak valid yang pada awalnya terlewat oleh saya adalah **kemungkinan pengguna memasukkan tipe data yang tidak sesuai**, seperti huruf alfabet (contoh: `"A"` atau `"Lulus"`), membiarkan input kosong (hanya menekan *Enter*), atau tidak sengaja menyisipkan spasi berlebih (contoh: `" 85  "`).

**Analisis Masalah (Penyebab Crash):**
Pada rancangan awal, fokus logika program saya hanya terpusat pada validasi rentang angka (misalnya `if nilai < 0 or nilai > 100:`). Saya membangun asumsi bahwa pengguna akan selalu memasukkan format angka dengan benar. Akibatnya, ketika pengguna memasukkan karakter non-angka, fungsi `float()` pada Python gagal melakukan konversi (*type casting*) dari tipe data *string* menjadi *float*. 

Kegagalan ini memicu *runtime error* berupa `ValueError`, yang menyebabkan program hancur (*crash*) seketika dan berhenti bekerja. Dari segi pengalaman pengguna (*user experience*), hal ini sangat fatal karena sistem berhenti secara paksa tanpa memberikan petunjuk yang ramah kepada pengguna tentang apa kesalahan mereka.

**Cara Menanganinya (Pendekatan Pemrograman Defensif):**
Untuk mengatasi celah tersebut dan membuat program menjadi lebih tangguh (*robust*), saya mengimplementasikan dua lapis keamanan:

1. **Pembersihan Data (*Data Sanitization*) dengan `.strip()`:** 
   Saya menambahkan *method* `.strip()` di setiap pemanggilan fungsi `input()`. Langkah ini berfungsi untuk memotong atau membuang spasi kosong di awal maupun di akhir teks yang mungkin tidak sengaja terketik. Dengan ini, input seperti `" 90  "` akan dibersihkan menjadi `"90"` sebelum diproses ke tahap selanjutnya.

2. **Penanganan Pengecualian (*Exception Handling*) dengan `try-except`:** 
   Untuk mencegah program berhenti paksa akibat huruf/teks kosong, saya membungkus proses konversi nilai (dari teks ke `float`) ke dalam blok pelindung `try`. Jika konversi gagal dan memicu `ValueError`, sistem tidak akan *crash*. Alih-alih hancur, aliran program akan ditangkap oleh blok `except ValueError` dan secara elegan menampilkan pesan *error* yang telah saya sesuaikan: *"Masukan ditolak: seluruh data harus berupa angka."*

**Kesimpulan Pembelajaran:**
Melalui studi kasus ini, saya menyadari bahwa dalam membangun sebuah algoritma, kita harus mempraktikkan *Defensive Programming*. Kita tidak boleh sepenuhnya mempercayai input dari pengguna. Validasi tidak hanya sebatas memeriksa batasan logika bisnis (seperti nilai 0-100), tetapi juga wajib memeriksa integritas tipe data sejak data tersebut pertama kali masuk ke dalam sistem.


```bash
python3 praktik/validasi_klasifikasi_nilai.py