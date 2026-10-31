# Global Retail & E-Commerce Product Price Intelligence

Analisis informasi strategis catatan harga produk retail global berbasis data *crowdsourced* Open Food Facts (Open Prices API). Analisis berfokus pada pola distribusi harga lintas mata uang utama (EUR dan USD), segmentasi kategori produk melalui ekstraksi kata kunci, evaluasi kualitas pencatatan data, serta rekomendasi keputusan bisnis yang aplikatif.

Notebook utama: [`notebook/retail_price_strategic_analysis.ipynb`](notebook/retail_price_strategic_analysis.ipynb)

---

## 🎯 Tujuan Analisis

Mengidentifikasi pola catatan harga dan karakteristik produk berdasarkan kelompok mata uang untuk menentukan aspek yang perlu diperhatikan lebih lanjut dalam pengelolaan produk dan strategi harga retail.

Pertanyaan kunci yang dijawab:
1. Berapa banyak catatan harga pada setiap mata uang dan bagaimana konsentrasinya?
2. Bagaimana karakteristik sebaran harga (median, rata-rata, skewness, pencilan)?
3. Kategori produk apa yang dominan muncul pada masing-masing mata uang?
4. Apakah terdapat perbedaan pola kategori produk antara EUR dan USD?
5. Bagaimana kualitas data produk (kelengkapan atribut merek, negara, kategori) dan informasi promosi/diskon?
6. Keputusan bisnis apa yang dapat diambil secara realistis tanpa menimbulkan salah tafsir?

---

## 👥 Untuk Siapa

| Pihak | Keputusan yang Terbantu |
|---|---|
| **Manajer Kategori & Pricing Retail** | Memetakan rentang harga kompetitif dan memantau klaster produk dominan per kelompok mata uang. |
| **Tim Data & Operasional Retail** | Mengidentifikasi *data quality gap* (atribut kosong, bias klasifikasi nama) untuk standarisasi input sistem katalog. |
| **Peneliti / Ekonom Perilaku Pasar** | Memahami disparitas penetapan harga rak lokal pada produk konsumsi sehari-hari. |

> **Catatan Batasan:** Dataset ini mencatat *observasi harga rak/struk*, bukan volume penjualan (*sales volume*) atau permintaan pasar (*consumer demand*). Jumlah catatan tidak merefleksikan tingkat kelarisan produk.

---

## 📁 Struktur Direktori

```text
info-strategis-1/
├── data/
│   └── global_retail_prices_dataset.csv
├── notebook/
│   └── retail_price_strategic_analysis.ipynb
└── README.md
```

---

## 📊 Dataset & Spesifikasi Teknis

* **Sumber Data:** Open Food Facts — Open Prices Global Initiative API (`https://prices.openfoodfacts.org/api/v1/prices`).
* **Metode Pengumpulan:** *Crowdsourced submission*, pemindaian struk belanja retail (*receipt scans*), dan kontribusi label rak toko oleh publik.
* **Lisensi Data:** Open Database License (ODbL) / Open Data Commons.
* **Ukuran Dataset:** 2.000 baris × 12 kolom.

### Kamus Kolom & Status Kelengkapan Riil

| Kolom | Tipe Data | Nullable | Kelengkapan Riil | Keterangan |
|---|---|---|---|---|
| `price_record_id` | Integer | No | 100% | Identifier unik setiap observasi harga. |
| `product_code` | String | Yes | 33,6% | Kode barcode produk (GTIN / UPC / EAN). |
| `product_name` | String | Yes | 76,9% | Nama/label produk yang tercatat pada struk/rak. |
| `brand` | String | Yes | 0% (Kosong) | Merek produk (tidak terisi pada snapshot ini). |
| `category` | String | Yes | 6,7% | Taksonomi kategori bawaan (sebagian besar kosong). |
| `price` | Float | No | 100% | Nominal harga tercatat dalam mata uang lokal. |
| `currency` | String | No | 100% | Kode mata uang ISO 4217 (EUR, USD, NOK, BAM, SEK, dll.). |
| `price_is_discounted` | Boolean | No | 100% | Penanda status harga promosi/diskon (100% bernilai `False` pada EUR). |
| `store_name` | String | Yes | 71,9% | Nama gerai/jaringan supermarket tempat harga diamati. |
| `city` | String | Yes | 29,1% | Kota lokasi gerai retail. |
| `country_code` | String | Yes | 0% (Kosong) | Kode ISO negara (tidak terisi pada snapshot ini). |
| `date_recorded` | Date | No | 100% | Tanggal pencatatan harga (format ISO-8601 `YYYY-MM-DD`). |

---

## 🔍 Alur Notebook

1. **Memuat Data & Pemeriksaan Kualitas Awal:** Verifikasi ukuran (2.000 baris), validasi nilai kosong pada harga dan mata uang (0 missing), serta validasi tidak adanya harga $\le 0$.
2. **Statistik Deskriptif & Analisis Distribusi EUR:** Menghitung sebaran harga EUR (median €2,50; rentang konsentrasi utama €1–€4; menceng ke kanan hingga maksimum €29,01).
3. **Analisis Gerai Retail (Toko Poslovitch):** Mengidentifikasi konsentrasi catatan toko tertentu; toko `poslovitch` menyumbang 68,2% dari seluruh catatan EUR rentang €1–€4.
4. **Rekayasa Fitur Klasifikasi Kategori:** Karena kolom `category` bawaan hanya terisi 6,7%, dibuat fungsi klasifikasi berbasis kata kunci nama produk (kedelai/nabati, buah/sayur, saus/pelengkap, daging, susu/olahan, roti/pastry).
5. **Analisis Mata Uang Pembanding (USD):** Sebaran harga USD (median $9,49; rata-rata $10,88; konsentrasi puncak $4–$6).
6. **Visualisasi Data Komprehensif:** Boxplot distribusi harga multi-mata uang, batang persentase kelengkapan kolom, diagram batang sebaran mata uang, dan komposisi kategori.
7. **Komparasi Kategori EUR vs USD:** Membandingkan proporsi kategori hasil klasifikasi antara kedua pasar mata uang.
8. **Analisis Dimensi Temporal & Status Diskon:** Pemeriksaan tren harga bulanan dan validasi kolom promosi (`price_is_discounted`).
9. **Fase Informasi Strategis & Keputusan Bisnis:** Sintesis temuan menjadi 6 pilar rekomendasi manajerial.

---

## 💡 Temuan Kunci & Rekomendasi Strategis

### 1. Fokus Pengamatan Berbasis Mata Uang (*Currency-Based Market Observation*)
* **Temuan:** Dataset didominasi oleh EUR (994 catatan / 49,7%) dan USD (407 catatan / 20,35%). Keduanya mencakup 70,05% data, sementara 6 mata uang teratas mencakup 95,2%.
* **Strategi & Keputusan Bisnis:**
  - Tetapkan EUR sebagai prioritas pengamatan utama dan USD sebagai kelompok pembanding.
  - Pisahkan analisis harga per mata uang secara independen. Jangan melakukan konversi nominal kurs langsung tanpa penyesuaian tanggal transaksi harian dan Purchasing Power Parity (PPP).

### 2. Prioritas Kategori Produk (*Product Category Focus*)
* **Temuan:** Pada kelompok EUR, kategori teridentifikasi terbesar adalah **produk berbasis kedelai/nabati (5,94%)**, **buah dan sayuran (5,03%)**, serta **saus dan pelengkap (4,93%)**. Namun, kategori *Lainnya* masih mencapai 51,81% dan produk tanpa nama 16,20%.
* **Strategi & Keputusan Bisnis:**
  - Jadikan produk kedelai/nabati dan saus sebagai *benchmark* pemantauan harga rutin pada pasar Eropa.
  - Tingkatkan aturan *keyword mapping* untuk menyaring 51,81% produk di kategori *Lainnya*.

### 3. Evaluasi Produk Lintas Pasar (*EUR vs USD Product Evaluation*)
* **Temuan:** Terdapat variasi komposisi yang tercatat:
  - Produk berbasis kedelai/nabati: EUR 5,94% vs USD 0,25%.
  - Buah dan sayuran: EUR 5,03% vs USD 0,49%.
  - Saus dan pelengkap: EUR 4,93% vs USD 5,65% (kategori spesifik tertinggi di USD).
  - Kategori *Lainnya* pada USD mencapai 83,54%.
* **Strategi & Keputusan Bisnis:**
  - Evaluasi pasar USD difokuskan pada kategori bumbu/saus kemasan.
  - Hindari menyimpulkan perbedaan selera konsumen semata-mata dari persentase ini, karena tingginya kategori *Lainnya* pada USD membatasi perbandingan langsung.

### 4. Peningkatan Kualitas Data Produk (*Data Quality Improvement*)
* **Temuan:** Kolom `brand` dan `country_code` 100% kosong. Nama produk kosong sebesar 23,1% (161 catatan pada EUR), dan `category` bawaan hanya 6,7%.
* **Strategi & Keputusan Bisnis:**
  - Wajibkan validasi input saat proses *scraping* / pengunggahan struk (validasi barcode GTIN dan nama toko).
  - Lakukan pengayaan (*data enrichment*) otomatis menghubungkan `product_code` ke database Open Food Facts global guna melengkapi merek dan taksonomi kategori resmi.

### 5. Peningkatan Pencatatan Promosi & Diskon (*Promotion Data Monitoring*)
* **Temuan:** Kolom `price_is_discounted` pada kelompok EUR bernilai `False` sebesar 100% (seluruh 994 catatan).
* **Strategi & Keputusan Bisnis:**
  - Efektivitas diskon belum dapat diukur dari snapshot ini.
  - Sistem pencatatan perlu mencatat harga acuan sebelum diskon (*strikethrough price*) dan durasi promosi agar elastisitas harga promosi dapat dimodelkan pada periode berikutnya.

### 6. Batasan Pengambilan Keputusan (*Business Decision Boundaries*)
* Rekomendasi yang **valid**: Pemantauan klaster harga rendah (€1–€4 pada EUR), identifikasi gerai dominan (`poslovitch`), standarisasi taksonomi katalog, dan perbaikan integritas data.
* Keputusan yang **belum dapat diambil dari dataset ini**: Menentukan volume produksi, alokasi kuota stok gudang, atau target laba kotor, karena ketiadaan data volume penjualan dan margin biaya produksi.

---

## 🛠️ Cara Menjalankan Notebook

Pastikan Python telah terpasang, lalu instal dependensi:

```bash
pip install pandas numpy matplotlib jupyter
```

Jalankan Jupyter Notebook:

```bash
jupyter notebook notebook/retail_price_strategic_analysis.ipynb
```

> Notebook telah dikonfigurasi untuk secara otomatis mendeteksi file dataset pada path `../data/global_retail_prices_dataset.csv`, `data/global_retail_prices_dataset.csv`, maupun saat diunggah pada sesi Google Colab.
