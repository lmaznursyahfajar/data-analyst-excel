# Dashboard Ekspor Indonesia 2025

Analisis data ekspor nasional per kode HS (Januari–November 2025) dan direktori eksportir terdaftar, disajikan sebagai satu workbook Excel dengan dashboard interaktif. Seluruh angka dihitung dengan **rumus Excel** dari sheet data, sehingga otomatis menghitung ulang bila data atau parameter diubah.

**File utama:** `Dashboard_Ekspor_Indonesia_2025.xlsx`

---

## Sumber data

| File | Isi | Ukuran |
|---|---|---|
| `HS_Code.csv` | Ekspor nasional per kode HS 8 digit: berat bersih (kg), nilai FOB (USD), harga USD/kg. Periode Jan–Nov 2025 | 8.216 kode HS |
| `IndonesiaExportir.csv` | Direktori eksportir: kode HS, produk, minimum order, kapasitas bulanan, harga FOB, estimasi volume dan pendapatan bulanan | 3.400 listing, 773 perusahaan |

---

## Struktur workbook

| Sheet | Fungsi |
|---|---|
| **Dashboard** | Tampilan utama: 6 KPI, 3 kartu temuan, 8 grafik, tabel 10 HS prioritas |
| **Ringkasan** | 8 temuan dan 6 rekomendasi (kalimat dihitung otomatis), plus tabel metrik kunci yang menjadi sumber seluruh angka dashboard |
| **Bab_HS** | Analisis per bab HS (2 digit): nilai, volume, harga, pangsa, indeks keterwakilan eksportir, nilai yang belum terwakili |
| **Top_HS** | 50 kode HS terbesar, konsentrasi ekspor (Top 10/50/100/500), dan segmen harga (nilai vs volume) |
| **Eksportir** | Sebaran per provinsi, jenis usaha, incoterms, terms of payment, pelabuhan, perusahaan teratas, dan validasi estimasi pendapatan |
| **Celah_Pasokan** | 30 kode HS bernilai besar dengan keterwakilan rendah di direktori (alat skrining) |
| **Metode_Catatan** | Parameter yang bisa diubah, catatan pembersihan data, definisi, dan keterbatasan |
| **Data_HS** | Data nasional bersih + kolom hitung (USD/kg, bab, jumlah listing, segmen harga, status pasokan, skor peluang) |
| **Data_Eksportir** | Data listing bersih + kolom hitung (bab, ekspor nasional HS per bulan, status estimasi) |
| **Profil_Perusahaan** | Satu baris per perusahaan: jumlah listing, estimasi pendapatan mentah dan wajar |

---

## Cara menggunakan

1. Buka sheet **Dashboard** untuk gambaran umum, lalu **Ringkasan** untuk temuan dan rekomendasi.
2. Untuk menguji skenario, ubah sel kuning di sheet **Metode_Catatan**. Semua sheet akan menghitung ulang.

| Parameter | Nilai awal | Pengaruh |
|---|---|---|
| Jumlah bulan data ekspor nasional | 11 | Mengubah ekspor nasional per HS menjadi rata-rata per bulan (dipakai pada uji kewajaran estimasi) |
| Ambang nilai "HS bernilai besar" | US$ 10.000.000 | Menentukan status pasokan dan skor peluang |
| Batas "keterwakilan rendah" | 3 listing | Membedakan status "Keterwakilan rendah" dan "Terwakili" |

3. Pada **Data_HS** dan **Data_Eksportir** tersedia filter di baris header. Filter kolom *Status pasokan* atau *Status estimasi* untuk menelusuri baris tertentu.

> Kolom berlabel *helper* (abu-abu) dipakai untuk pengurutan Top-N dan tidak perlu diubah.

---

## Definisi dan metode

- **Bab HS**: dua digit pertama kode HS. "Bab 0" adalah listing berkode `00000000` (tidak terklasifikasi).
- **Segmen harga (USD/kg)**: Di bawah 1 · 1–5 · 5–20 · 20–100 · 100 ke atas (batas tetap).
- **Status pasokan** per kode HS:
  - *Di bawah ambang nilai*: nilai FOB di bawah ambang
  - *Belum terwakili*: 0 listing eksportir
  - *Keterwakilan rendah*: 1 sampai batas listing
  - *Terwakili*: lebih dari batas listing
- **Skor peluang** = Nilai FOB HS ÷ (1 + jumlah listing), hanya untuk HS di atas ambang nilai. Indikator skrining ukuran pasar per pemasok terdaftar, bukan prediksi laba.
- **Indeks keterwakilan bab** = pangsa listing eksportir ÷ pangsa nilai ekspor. Di atas 1: direktori lebih padat daripada porsi nilai ekspornya. Di bawah 1: kurang terwakili.
- **Status estimasi listing**: *Melebihi ekspor nasional HS* bila estimasi pendapatan bulanan satu listing lebih besar daripada (ekspor nasional HS ÷ jumlah bulan). Satu perusahaan tidak mungkin mengekspor lebih dari seluruh Indonesia untuk HS yang sama. Listing yang lolos uji ini berstatus *Wajar*.

---

## Pembersihan data

- File dibaca dengan encoding Windows-1252 dan desimal berkoma.
- Sel galat bawaan sumber dikosongkan: 122 sel galat pembagian nol (USD/kg) di file HS, 552 sel galat nilai (estimasi pendapatan) dan nilai "TIDAK DIKETAHUI" di file eksportir.
- USD/kg dihitung ulang dengan rumus (FOB ÷ berat). 68 kode HS tidak punya data berat sehingga harga per kg-nya kosong, sedangkan nilai FOB tetap dihitung.
- Nilai `-` pada jenis usaha, pelabuhan, terms of payment, dan incoterms diganti "Tidak diketahui".
- Nama provinsi diterjemahkan ke Bahasa Indonesia.
- 33 baris dengan nama perusahaan bermasalah (spasi ganda atau tanda `~`) dirapikan agar pencocokan rumus akurat.
- Kolom alamat, email, telepon, dan tautan produk tidak dibawa karena tidak diperlukan analisis.
- Simbol ≥/≤ pada deskripsi HS rusak di file sumber dan tampil sebagai `?`. Hanya pola "but ?" yang dipulihkan menjadi "≤".

---

## Ringkasan temuan

- **Ekspor sangat terkonsentrasi.** 10 kode HS teratas menyumbang 26,9% dari total US$ 234,6 miliar, dan 100 teratas 65,6%.
- **Nilai dan volume berbeda.** Barang di bawah US$ 1/kg sebesar 71,7% volume tetapi hanya 18,4% nilai.
- **Direktori hanya menutup sebagian kecil ekspor.** 524 dari 8.202 kode HS aktif (6,4%), setara 11,7% nilai ekspor. Sebanyak 1.187 HS bernilai ≥ US$ 10 juta belum punya listing.
- **Komposisi condong ke komoditas tertentu.** Kopi, teh dan rempah memuat 26,7% listing tetapi hanya 1,4% nilai ekspor. Nikel punya 3,6% nilai ekspor dengan nol listing.
- **Eksportir terpusat di Jawa.** 68,7% perusahaan berada di enam provinsi Jawa.
- **Estimasi pendapatan tidak bisa dipakai mentah.** 777 listing (27,3% dari yang berestimasi) mengklaim melebihi ekspor nasional HS-nya dan menyumbang 94,6% total estimasi. Total mentah US$ 12,97 miliar per bulan menjadi hanya US$ 0,69 miliar setelah disaring.

---

## Keterbatasan

- Harga FOB per kg pada file eksportir identik dengan rata-rata harga nasional per HS (terverifikasi pada seluruh baris yang cocok), **bukan** harga penawaran perusahaan. Estimasi pendapatan sangat sensitif terhadap kesalahan pemilihan kode HS.
- Estimasi volume umumnya rata-rata antara minimum order dan kapasitas bulanan. Bila salah satunya kosong, dipakai nilai yang tersedia.
- Direktori bukan sensus eksportir. Tidak adanya listing pada suatu HS tidak berarti tidak ada eksportir. Sebagian komoditas (batubara, nikel, LNG, kendaraan) didominasi pemain besar berizin khusus yang mungkin tidak memakai direktori. Gunakan **Celah_Pasokan** untuk skrining, lalu verifikasi di lapangan.
- Data ekspor nasional mencakup 11 bulan (Jan–Nov 2025), bukan setahun penuh.
- Tampilan dashboard diverifikasi lewat render LibreOffice. Tampilan grafik di Excel bisa sedikit berbeda, misalnya label sumbu pada kurva konsentrasi.

---

## Kompatibilitas

- Dirancang untuk Microsoft Excel (dan kompatibel dengan LibreOffice). Rumus hanya memakai fungsi yang tersedia luas (`SUMIFS`, `COUNTIFS`, `INDEX`, `MATCH`, `LARGE`, `SUMPRODUCT`), tanpa fungsi array dinamis.
- Pada file ±2 MB dengan puluhan ribu rumus, perhitungan ulang setelah mengubah parameter dapat memakan beberapa detik.
