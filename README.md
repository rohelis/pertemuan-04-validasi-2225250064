# Pertemuan 04 Seleksi Multi-Kondisi dan Validasi Input

Nama: Rohelis
NIM: 2225250064
Kelas: 3B

## Tujuan
Membangun program validasi dan klasifikasi dengan rantai `if-elif-else` serta penanganan penolakan input (*error handling*) yang memadai.

## Cara Menjalankan
```bash
python3 praktik/validasi_klasifikasi_nilai.py

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

Masukan tidak valid yang semula terlewat adalah saat pengguna memasukkan angka desimal menggunakan tanda koma (misalnya `"49,9"` alih-alih `"49.9"`), yang menyebabkan kegagalan konversi tipe data (`ValueError`) pada fungsi `float()`.

Untuk menanganinya:
1. Menggunakan pemanggilan metode `.strip()` untuk membersihkan spasi yang tidak sengaja terketik di awal atau akhir input.
2. Menggunakan struktur penanganan pengecualian `try-except ValueError` agar setiap input yang bukan berupa angka valid dapat ditangkap dengan baik dan menampilkan pesan peringatan tanpa menyebabkan program terhenti secara mendadak (*crash*).