# Pizza Sales Dashboard 2015

Dashboard Excel interaktif untuk menganalisis penjualan restoran pizza sepanjang 2015: pendapatan, order, menu, kategori, ukuran, bahan baku, dan pola waktu. Dropdown filter menggerakkan seluruh KPI, grafik, dan peta panas. Seluruh angka dihitung dengan **rumus Excel** dari sheet data (±297 ribu rumus), bukan nilai statis.

**File utama:** `Dashboard_Pizza_Sales_2015.xlsx`

---

## Sumber data

| Item | Nilai |
|---|---|
| File | `pizza_sales.csv` |
| Baris item penjualan | 48.620 |
| Order unik | 21.350 |
| Menu pizza | 32 (4 kategori: Classic, Chicken, Supreme, Veggie) |
| Ukuran | S, M, L, XL, XXL |
| Periode | 1 Januari – 31 Desember 2015 (358 hari buka) |
| Jam operasional | 09:00 – 23:59 |
| Total pendapatan | US$ 817.860,05 |

Data tidak memiliki nilai kosong. Kolom sumber: `pizza_id`, `order_id`, `pizza_name_id`, `quantity`, `order_date`, `order_time`, `unit_price`, `total_price`, `pizza_size`, `pizza_category`, `pizza_ingredients`, `pizza_name`.

---

## Struktur workbook

| Sheet | Fungsi |
|---|---|
| **Dashboard** | Tampilan utama interaktif: 4 dropdown filter, 6 KPI, 3 kartu insight, 9 grafik, peta panas hari × jam, dan tabel peringkat menu |
| **Insight** | 8 temuan dan 6 rekomendasi (kalimat dihitung otomatis) plus tabel metrik kunci. Selalu memakai seluruh data |
| **Waktu** | Analisis per bulan, hari dalam seminggu, dan jam, plus peta panas jumlah order dan tabel harian. Seluruh data |
| **Menu** | Kinerja 32 menu, kuadran menu, matriks popularitas vs harga, per kategori, per ukuran, ukuran × kategori, dan 15 bahan terpopuler. Seluruh data |
| **Metode** | Panduan pemakaian, definisi, pembersihan data, dan keterbatasan |
| **Kalkulasi** | Mesin perhitungan dashboard; tabel di sini mengikuti dropdown filter. Tidak perlu diubah manual |
| **Bahan** | Matriks pizza × bahan baku (1 = pizza memuat bahan) |
| **Data_Pizza** | Data transaksi bersih + kolom turunan + kolom rumus penerap filter |

---

## Cara menggunakan dashboard

1. Buka sheet **Dashboard**.
2. Pilih nilai pada empat kotak putih bergaris merah di bagian atas (klik sel, lalu pilih dari panah dropdown):

| Filter | Pilihan |
|---|---|
| Bulan | Semua, Januari … Desember |
| Kategori | Semua, Classic, Chicken, Supreme, Veggie |
| Ukuran | Semua, S, M, L, XL, XXL |
| Metrik grafik tren | Pendapatan, Pizza terjual, Order |

3. Seluruh KPI, kartu insight, grafik, peta panas, dan tabel peringkat menghitung ulang otomatis. Pilih **Semua** untuk kembali ke seluruh data.
4. Tautan di baris atas (Insight, Waktu, Menu, Metode) membawa ke sheet terkait.

**Perilaku filter pada grafik tertentu** (agar perbandingan tetap terlihat):

| Visual | Filter yang diikuti |
|---|---|
| KPI, kartu insight, grafik per hari, grafik per jam, peta panas, grafik harian, tabel peringkat, top 10 menu dan bahan | Semua filter |
| Tren bulanan | Kategori dan Ukuran. Filter Bulan diabaikan; bulan terpilih diberi warna |
| Pendapatan per kategori | Bulan dan Ukuran (mengabaikan filter Kategori) |
| Pizza terjual per ukuran | Bulan dan Kategori (mengabaikan filter Ukuran) |
| Distribusi pizza per order | Hanya Bulan |

Metrik grafik tren hanya memengaruhi grafik bulanan, harian, per hari, per jam, dan peta panas.

---

## Definisi

- **Pendapatan** = jumlah `total_price` (US$). **Pizza terjual** = jumlah `quantity`. **Order** = jumlah `order_id` unik.
- **Dalam filter Kategori/Ukuran**, sebuah order dihitung bila memuat minimal satu baris yang lolos filter. Nilai order dan pizza per order dihitung dari baris yang lolos saja.
- **Hari buka** = jumlah tanggal yang memiliki minimal satu order. Terdapat 7 hari tanpa transaksi (24–25 Sep, 5/12/19/26 Okt, dan 25 Des) yang tidak dihitung.
- **Kuadran menu**: popularitas (jumlah pizza terjual) dan harga (harga rata-rata per pizza) dibandingkan dengan median seluruh menu.
  - *Bintang*: laris dan berharga tinggi
  - *Pekerja keras*: laris tetapi berharga rendah
  - *Berpotensi*: berharga tinggi tetapi kurang laris
  - *Perlu evaluasi*: kurang laris dan berharga rendah
- **Bahan baku**: jumlah pizza terjual yang memuat bahan tersebut (pizza yang memuat 5 bahan dihitung sekali untuk tiap bahan).
- **Rata-rata 7 hari** (grafik harian) = rata-rata nilai 7 hari terakhir.

---

## Pembersihan data dan kolom turunan

- `order_date` (format dd-mm-yyyy) dan `order_time` diubah menjadi nilai tanggal dan waktu Excel. Total harga = harga satuan × jumlah pada seluruh baris (terverifikasi).
- **Kolom turunan** (nilai, dihitung dari data sumber): Bulan, Hari (1 = Senin), Jam, Qty dalam order, Nilai order, dan Baris pertama order.
- **Kolom rumus** (berwarna merah di `Data_Pizza`): Lolos filter, Kum. lolos/order, Order baru, dan padanannya tanpa filter bulan. Kolom ini menerapkan dropdown dashboard ke setiap baris dan menghitung order unik.
- `pizza_name_id` dan `pizza_ingredients` tidak dibawa ke `Data_Pizza`; bahan baku dipetakan per menu di sheet `Bahan`.
- Nama bahan dirapikan: `Artichoke` disatukan dengan `Artichokes`, dan `?duja Salami` (karakter rusak di sumber) ditulis `'Nduja Salami`.

---

## Ringkasan temuan

- **Skala bisnis:** pendapatan US$ 817.860 dari 21.350 order dan 49.574 pizza. Rata-rata nilai order US$ 38,31, dengan 2,32 pizza per order dan rata-rata US$ 2.285 per hari buka.
- **Pola mingguan:** Jumat terbaik (US$ 2.721 per hari buka), Minggu terlemah (US$ 1.908), selisih 43%.
- **Dua gelombang jam sibuk:** makan siang 12:00–13:59 memuat 23,3% order dan makan malam 17:00–18:59 memuat 22,2%. Jam tersibuk 12:00 (2.520 order).
- **Musiman relatif datar:** selisih bulan terkuat dan terlemah (per hari buka) kurang dari 15%.
- **Menu:** pendapatan terbesar The Thai Chicken Pizza (5,3%); terlaris The Classic Deluxe Pizza (2.453 pizza). 12 dari 32 menu menghasilkan 50% pendapatan. Menu paling lemah The Brie Carre Pizza (490 pizza).
- **Kategori dan ukuran:** Classic terbesar menurut jumlah pizza (30,0%) dan pendapatan (26,9%). Ukuran L menyumbang 45,9% pendapatan, sedangkan XL + XXL hanya 1,2% pizza.
- **Kuadran menu:** 7 Bintang, 9 Pekerja keras, 9 Berpotensi, 7 Perlu evaluasi.
- **Perilaku order:** 38% order hanya berisi 1 pizza. Order besar (≥ 5 pizza) hanya 3,6% dari order tetapi menyumbang 14,2% pendapatan.
- **Bahan:** Garlic paling banyak dipakai (56,3% pizza terjual memuatnya), disusul Tomatoes dan Red Onions.

Angka di atas adalah kondisi data saat ini; sheet **Insight** menghitungnya ulang otomatis.

---

## Keterbatasan

- Data hanya satu tahun (2015), sehingga pertumbuhan tahunan dan musiman multi-tahun tidak dapat dinilai.
- Tidak ada data biaya, margin, kanal penjualan, atau pelanggan. Kuadran menu memakai harga rata-rata (dipengaruhi campuran ukuran), bukan margin keuntungan; analisis profitabilitas dan retensi belum dapat dilakukan.
- Order sangat besar (hingga 28 pizza) ada tetapi jarang; rata-rata nilai order dipengaruhi order tersebut.
- Warna per-kuadran pada titik grafik matriks menu (sheet Menu) bergantung pada dukungan aplikasi; kuadran pasti tersedia di kolom *Kuadran menu*.
- Tampilan dashboard diverifikasi lewat render LibreOffice. Tampilan grafik di Excel bisa sedikit berbeda (mis. label sumbu tanggal pada grafik harian).

---

## Kompatibilitas dan kinerja

- Dirancang untuk Microsoft Excel (kompatibel dengan LibreOffice). Rumus memakai fungsi yang tersedia luas (`SUMIFS`, `COUNTIFS`, `INDEX`, `MATCH`, `LARGE`, `SUMPRODUCT`, `CHOOSE`) tanpa fungsi array dinamis, dan rentang data dipakai lewat nama terdefinisi (`d_rev`, `d_qty`, dll.).
- File ±7 MB dengan ±297 ribu rumus. Setelah mengganti filter, perhitungan ulang dapat memakan beberapa detik.
- Jangan mengubah isi sheet `Kalkulasi`, kolom rumus di `Data_Pizza`, dan sheet `Bahan`, karena dashboard bergantung padanya.
