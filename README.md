# Koleksi Analisis Informasi Strategis Berbasis Data

Repositori komprehensif yang memuat 3 studi analitik informasi strategis lintas domain (Retail Pricing, Clean Energy / EV Battery Telemetry, dan Corporate Financial Capital Allocation). Setiap proyek menyertakan dataset lengkap, kamus data, notebook eksplorasi & visualisasi *end-to-end*, serta rekomendasi keputusan berbasis bukti.

---

## 📚 Daftar Modul Analisis

| Modul | Topik & Domain | Sumber Data | Ukuran Data | Notebook Utama | Dokumen Detail |
|---|---|---|---|---|---|
| **01** | **Global Retail & E-Commerce Product Price Intelligence** | Open Food Facts (Open Prices API) | ~194 KB (1 file CSV) | [`info-strategis-1/notebook/retail_price_strategic_analysis.ipynb`](info-strategis-1/notebook/retail_price_strategic_analysis.ipynb) | [`info-strategis-1/README.md`](info-strategis-1/README.md) |
| **02** | **EV Battery Health & Degradation Telemetry Analytics** | Telemetri Lapangan EV Fleet (50.000 records) | ~11.1 MB (2 file CSV) | [`info-strategis-2/notebook/ev_battery_strategic_analysis.ipynb`](info-strategis-2/notebook/ev_battery_strategic_analysis.ipynb) | [`info-strategis-2/README.md`](info-strategis-2/README.md) |
| **03** | **Apple Inc. Capital Allocation & Cash Flow Intelligence** | SEC EDGAR Form 10-K & 10-Q (FY2015–FY2025) | ~10.4 MB (6 file) | [`info-strategis-3/notebook/final.ipynb`](info-strategis-3/notebook/final.ipynb) | [`info-strategis-3/README.md`](info-strategis-3/README.md) |

---

## 🎯 Ringkasan Eksekutif Setiap Riset

### 1. Global Retail & E-Commerce Product Price Intelligence (`info-strategis-1`)
* **Tujuan**: Menganalisis pola distribusi harga produk retail global lintas mata uang utama (EUR vs. USD), mendeteksi disparitas harga, mengevaluasi kualitas kelengkapan metadata produk, serta merumuskan strategi penentuan harga rak.
* **Pertanyaan Kunci**: Bagaimana sebaran skewness harga dan outlier? Kategori apa yang mendominasi tiap mata uang? Bagaimana celah kualitas atribut produk katalog?
* **Pihak Terdampak**: Manajer Kategori & Pricing Retail, Tim Data Katalog, Peneliti Pasar Konsumsi.

### 2. EV Battery Health & Degradation Analytics (`info-strategis-2`)
* **Tujuan**: Memetakan dinamika degradasi dan risiko kegagalan paket baterai kendaraan listrik komersial/pribadi melintasi 4 kimia sel (`LFP`, `NMC`, `NCA`, `LMO`) dan kondisi iklim ekstrem.
* **3 Solusi Kritis**:
  1. **Early Warning System**: Mengatur alarm telemetri BMS di hambatan dalam (*internal resistance*) **60–70 mΩ** sebelum sisa siklus (RUL) habis mendadak.
  2. **Standardisasi Pengadaan Sel**: Wajibkan kimia **LFP** untuk armada rute berat & iklim tropis lembap; hindari **LMO** untuk operasional armada intensif.
  3. **Mitigasi Beban Kombinasi**: Rotasi armada rute berat-ringan dan cegah kombinasi *fast charge* >50% + Depth of Discharge (DoD) >70%.
* **Pihak Terdampak**: *Fleet Manager*, Tim Pengadaan (*Procurement*), Teknisi BMS, Operator SPKLU.

### 3. Apple Inc. Capital Allocation & Cash Flow Intelligence (`info-strategis-3`)
* **Tujuan**: Memverifikasi apakah Apple Inc. (AAPL) mendistribusikan lebih banyak kas kepada pemegang saham (via *share buyback* dan dividen) daripada kas riil yang dihasilkan dari kegiatan operasionalnya (*Free Cash Flow*).
* **Ukuran Strategis**:
  $$\text{Rasio Distribusi} = \frac{\text{Buyback} + \text{Dividen}}{\text{FCF}} \times 100\%$$
* **Temuan**: Analisis tren pembiayaan selisih rasio distribusi di atas 100% menggunakan cadangan kas kasir dan emisi surat utang korporasi (*bonds*).
* **Pihak Terdampak**: Dewan Direksi, CFO, Kreditur & Pembeli Obligasi, Investor Institusional.

---

## 📁 Struktur Direktori Repositori

```text
informasi-strategis-collection/
├── .gitignore
├── README.md                                     # Master portfolio overview
│
├── info-strategis-1/                             # Modul 1: Retail Price Intelligence
│   ├── data/
│   │   └── global_retail_prices_dataset.csv
│   ├── notebook/
│   │   └── retail_price_strategic_analysis.ipynb
│   └── README.md
│
├── info-strategis-2/                             # Modul 2: EV Battery Health
│   ├── data/
│   │   ├── ev_battery_health.csv
│   │   └── data_dictionary .csv
│   ├── notebook/
│   │   └── ev_battery_strategic_analysis.ipynb
│   ├── anu.txt
│   └── README.md
│
└── info-strategis-3/                             # Modul 3: Apple SEC Cash Flow
    ├── apple_stocks_dataset/
    │   ├── financial_facts.csv
    │   ├── filings.csv
    │   ├── company_profile.json
    │   ├── data_dictionary.csv
    │   ├── refresh_metadata.json
    │   └── README.md
    ├── notebook/
    │   └── final.ipynb
    ├── co-creation dengan ai.txt
    └── README.md
```

---

## 🚀 Cara Menjalankan Notebook

1. **Clone repositori:**
   ```bash
   git clone <URL_REPOSITORY_ANDA>
   cd informasi-strategis-collection
   ```

2. **Buat environment & install dependency:**
   ```bash
   python -m venv venv
   # Windows:
   .\venv\Scripts\activate
   # Linux/macOS:
   source venv/bin/activate

   pip install pandas numpy matplotlib seaborn jupyter
   ```

3. **Buka Jupyter Lab / Notebook:**
   ```bash
   jupyter notebook
   ```
   Buka file notebook di subfolder masing-masing (`info-strategis-1/notebook/`, `info-strategis-2/notebook/`, `info-strategis-3/notebook/`). Seluruh path pembacaan dataset telah dikonfigurasi secara adaptif/relatif sehingga langsung dapat dijalankan tanpa penyesuaian path.
