# Analisis dan Dashboard Penjualan Ritel 2023

Analisis eksploratif atas 1.000 transaksi ritel, dengan dua dashboard interaktif: satu berbasis Excel dan satu berbasis web (HTML). Keduanya memakai data yang sama, memiliki tiga filter (kategori, gender, usia), dan menghasilkan angka yang identik.

## Isi proyek


## Dataset

Setiap baris adalah satu transaksi. Periode data 1 Januari 2023 sampai 1 Januari 2024, dengan tiga kategori produk.

| Kolom | Keterangan |
|---|---|
| `Transaction ID` | Nomor transaksi (1 sampai 1000) |
| `Date` | Tanggal transaksi (format bulan/hari/tahun) |
| `Customer ID` | ID pelanggan, unik di setiap baris |
| `Gender` | Female atau Male |
| `Age` | Usia pelanggan, 18 sampai 64 |
| `Product Category` | Beauty, Clothing, atau Electronics |
| `Quantity` | Jumlah unit, 1 sampai 4 |
| `Price per Unit` | Harga satuan: 25, 30, 50, 300, atau 500 |
| `Total Amount` | Quantity × Price per Unit |
| `Age Group`, `Month`, `Year`, `Month/Year` | Kolom turunan bawaan dataset |

Hasil pemeriksaan kualitas data: tidak ada nilai kosong, tidak ada baris duplikat, dan `Total Amount` sama dengan `Quantity × Price per Unit` di semua baris.

Dataset tidak menyebut mata uang, jadi semua angka ditampilkan apa adanya tanpa satuan.

## Dashboard Excel

### Cara memakai

1. Buka `dashboard_penjualan_ritel.xlsx` dan masuk ke sheet **Dashboard**.
2. Ubah tiga sel berwarna kuning di bagian atas: **Kategori**, **Gender**, dan **Usia**. Pilihan "Semua" berarti tanpa filter.
3. Kalimat ringkasan, empat angka utama, semua grafik, heatmap, dan porsi gender ikut berubah.

### Struktur sheet

| Sheet | Isi |
|---|---|
| `Dashboard` | Filter, ringkasan, enam grafik, heatmap kategori × usia, porsi gender, temuan, dan catatan data |
| `Analisis` | Seluruh tabel hitungan di balik grafik, dihitung dengan rumus dari sheet Data |
| `Data` | 1.000 transaksi asli, ditambah lima kolom bantu berisi rumus: Kelompok Usia, Bulan, Tahun, Hari (1 = Senin), dan Tingkat Harga |

### Cara kerja filter

Dropdown di Dashboard diterjemahkan menjadi tiga kriteria di sheet Analisis (sel B2 sampai B4). Pilihan "Semua" menjadi tanda `*` yang cocok dengan nilai apa pun. Setiap rumus `SUMIFS` dan `COUNTIFS` memakai ketiga kriteria itu, sehingga mengubah filter cukup untuk memperbarui seluruh dashboard.

Rumus yang dipakai hanya fungsi standar Excel (`SUMIFS`, `COUNTIFS`, `AVERAGEIF`, `INDEX`, `MATCH`, `IFERROR`, `FIXED`), jadi file bisa dibuka di Excel 2007 ke atas maupun LibreOffice. Angka pada kalimat ringkasan memakai `FIXED` agar pemisah ribuan mengikuti pengaturan regional komputer.

## Dashboard web

Buka `dashboard_penjualan_ritel.html` di browser. Pilih filter lewat tombol Kategori, Gender, dan Usia, lalu klik "Atur ulang filter" untuk kembali ke seluruh data. Tampilan mengikuti mode terang atau gelap perangkat.

Halaman memuat Chart.js dan font dari internet, jadi perlu koneksi saat dibuka pertama kali. Tanpa koneksi, angka dan tabel tetap tampil tetapi grafik tidak.

## Ringkasan temuan

Angka di bawah dihitung dari seluruh data tanpa filter.

**Harga satuan menentukan pendapatan.** Transaksi berharga 300 dan 500 hanya 39,6% dari jumlah transaksi, tetapi menyumbang 88,4% pendapatan.

| Tingkat harga | Porsi transaksi | Porsi pendapatan |
|---|---|---|
| 25 sampai 50 | 60,4% | 11,6% |
| 300 | 19,7% | 34,1% |
| 500 | 19,9% | 54,3% |

**Kategori, gender, dan usia tidak berbeda nyata.**

| Dimensi | Temuan | Uji statistik |
|---|---|---|
| Kategori | Electronics 156.905, Clothing 155.580, Beauty 143.515 | ANOVA, p = 0,85 |
| Gender | Perempuan 51,1% pendapatan, laki-laki 48,9%; rata-rata per transaksi 456,5 vs 455,4 | Uji t, p = 0,97 |
| Usia | Kelompok 25 sampai 54 menyumbang 63,9% pendapatan; rata-rata per transaksi Gen Z dan dewasa muda sekitar 500 vs 418 untuk senior | ANOVA, p = 0,24 |
| Kategori × gender | Tidak ada keterkaitan | Chi-kuadrat, p = 0,43 |
| Kategori × usia | Tidak ada keterkaitan | Chi-kuadrat, p = 0,62 |

Satu pola yang terlihat: kelompok senior condong membeli Electronics (38.210) dibanding Beauty (20.670).

**Waktu.**

- Bulan tertinggi Mei (53.150) dan terendah September (23.620). Rata-rata bulanan 37.872,5.
- Kuartal 4 terkuat (126.190) dan kuartal 3 terlemah (96.045).
- Hari terkuat Sabtu (78.815) dan terlemah Kamis (53.835).
- Kuantitas 3 dan 4 per transaksi menghasilkan sekitar 72% pendapatan.

## Keterbatasan

- **Kemungkinan data simulasi.** Kategori, harga satuan, dan kuantitas tersebar hampir merata. Perlakukan hasil sebagai latihan analisis, bukan kesimpulan bisnis nyata.
- **Hanya satu tahun data.** Pola bulanan belum bisa disebut musiman.
- **Januari 2024 tidak lengkap.** Bulan itu hanya berisi 2 transaksi (total 1.530). Keduanya tidak masuk grafik bulanan, tetapi tetap dihitung di total.
- **Tidak ada pelanggan berulang.** Setiap `Customer ID` hanya muncul sekali, sehingga analisis retensi atau nilai seumur hidup pelanggan tidak bisa dilakukan.
- **Teks temuan bersifat tetap.** Di kedua dashboard, bagian "Temuan dari seluruh data" tidak ikut berubah saat filter diubah.

## Verifikasi

- Seluruh rumus Excel (5.178 rumus) dihitung ulang tanpa galat.
- Total penjualan 456.000, 1.000 transaksi, dan 2.514 unit cocok dengan perhitungan pandas.
- Skenario filter Beauty, Perempuan, usia 25–39 menghasilkan 26.250 dari 50 transaksi, sama dengan pandas, termasuk rincian per bulan.
