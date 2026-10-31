# 🔋 EV Battery Health & Degradation: Analisis Telemetri & Informasi Strategis

Repositori ini memuat analisis data telemetri komprehensif (*End-to-End Analytics*) untuk paket baterai kendaraan listrik (EV) berdasarkan dataset 50.000 observasi longitudinal melintasi 4 jenis kimia sel (`LFP`, `NMC`, `NCA`, `LMO`) dan 2.600 unit baterai fisik.

Notebook utama: [`notebook/ev_battery_strategic_analysis.ipynb`](notebook/ev_battery_strategic_analysis.ipynb).

---

## 🎯 Ringkasan Eksekutif & Keputusan Strategis

Analisis difokuskan pada pemecahan **3 Masalah Kritis** operasional dan investasi armada EV:

| # | Masalah Strategis | Untuk Siapa | Solusi Utama | Bukti Kunci dari Dataset |
|---|---|---|---|---|
| **1** | **Baterai rusak diam-diam, peringatan terlambat** | Tim Operasional Armada, Teknisi Perawatan (*Maintenance*), Produsen EV | Pasang alarm dini hambatan dalam (*internal resistance*) pada ambang batas **60–70 mΩ**; jadwalkan servis sebelum status risiko sistem berubah ke Medium/High. | Median sisa siklus (RUL) status **Medium hanya ~53 siklus** (~1–2 bulan operasi) dan **High sudah 0 siklus**. Risiko High melonjak dari **0,34%** (60–70 mΩ) ke **83,20%** (>90 mΩ). |
| **2** | **Salah memilih kimia sel untuk pola pemakaian & iklim** | Tim Pengadaan (*Procurement*), Pabrikan EV (R&D), Perusahaan Leasing & Asuransi | **Wajibkan LFP** untuk armada berat/komersial dan wilayah tropis lembap; batasi **NMC/NCA** hanya untuk kendaraan pribadi rute sedang; **hindari LMO** untuk operasional armada intensif. | Pada rute berat (>100 km/hari), risiko High LFP hanya **0,3%** vs NMC **12,9%**, NCA **16,2%**, dan LMO **26,9%**. Pada sel NCA, iklim tropis lembap menaikkan risiko High sebesar **+148%** (10,5% vs 4,2%). |
| **3** | **Pola pemakaian berat mempercepat degradasi tanpa disadari** | *Fleet Manager*, Operator SPKLU (CPO), Pengembang Software BMS | Terapkan manajemen rotasi unit (rute berat ⇄ rute ringan), proteksi otomatis via BMS (batasi frekuensi *fast charge* & cegah DoD >70%), dan terapkan program *smart charging*. | Jarak tempuh >100 km/hari menaikkan risiko High **~3,9x lipat** (3,1% ke 12,1%). Kombinasi 3 faktor beban berat sekaligus mendongkrak risiko High dari **4,5%** (0 faktor) menjadi **13,3%** (3 faktor). |

---

## 📌 Penjabaran 3 Masalah Strategis

### 1. Alarm Dini Hambatan Dalam (Early Warning System)
* **Masalah**: Label risiko kegagalan konvensional bereaksi terlambat. Ketika status risiko berubah menjadi *Medium*, sisa masa pakai efektif baterai tersisa sangat sedikit, sehingga tindakan perawatan berubah menjadi penanganan darurat dan memicu *downtime* mendadak.
* **Fakta Data**:
  * Median RUL saat status *Medium*: **~53 siklus** (~1–2 bulan operasional armada).
  * Median RUL saat status *High*: **0 siklus** (unit sudah tidak layak pakai).
  * Hambatan dalam (*internal resistance*) naik bertahap jauh sebelum kegagalan:
    * $<60\text{ m}\Omega$: risiko High **0,04%**
    * $60–70\text{ m}\Omega$: risiko High **0,34%** (jendela deteksi dini optimal)
    * $70–80\text{ m}\Omega$: risiko High **5,42%**
    * $80–90\text{ m}\Omega$: risiko High **28,71%**
    * $>90\text{ m}\Omega$: risiko High **83,20%**
* **Aksi Strategis**: Konfigurasikan telemetri BMS untuk memicu notifikasi servis preventif saat hambatan dalam menyentuh rentang **60–70 mΩ**.

### 2. Standarisasi Pengadaan Kimia Sel Sesuai Profil Rute & Iklim
* **Masalah**: Kesalahan penentuan jenis kimia baterai terhadap profil operasional memicu degradasi prematur, pembengkakan biaya penggantian, dan depresiasi nilai aset kendaraan (*residual value*).
* **Fakta Data**:
  * Daya tahan siklus menuju batas pensiun (80% SOH):
    * **LFP**: median ~3.216 siklus (baru 6 dari 784 unit menyentuh batas pensiun).
    * **NMC**: median ~1.723 siklus (216 dari 890 unit pensiun).
    * **NCA**: median ~1.576 siklus (177 dari 545 unit pensiun).
    * **LMO**: median ~1.165 siklus (175 dari 381 unit pensiun).
  * Ketahanan pada pemakaian intensif (>100 km/hari):
    * Risiko High LFP hanya **0,3%**, sedangkan LMO mencapai **26,9%**.
  * Sensitivitas lingkungan:
    * Sel NCA pada iklim tropis lembap mengalami lonjakan risiko High **+148%** dibanding iklim sedang (10,5% vs 4,2%).
* **Aksi Strategis**: 
  * Kontrak pengadaan armada komersial (taksi, logistik, bus) wajib menggunakan kimia **LFP**.
  * Alokasikan **NMC/NCA** hanya untuk kendaraan komuter rute sedang dengan protokol pendingin aktif.
  * Hentikan pengadaan sel **LMO** untuk armada dengan pemanfaatan tinggi.

### 3. Mitigasi Beban Kombinasi & Regulasi Pengisian Daya
* **Masalah**: Pengemudi dan operator sering mengabaikan dampak multiplikasi dari gabungan tiga faktor beban: jarak tempuh tinggi, kebiasaan *DC fast charging* terus-menerus, dan pengosongan baterai hingga batas bawah (*high depth of discharge*).
* **Fakta Data**:
  * Jarak tempuh harian: risiko High naik dari **3,1%** (<30 km/hari) menjadi **12,1%** (>100 km/hari).
  * Multiplikasi faktor beban (jarak jauh + *fast charge* >50% + DoD >70%):
    * 0 faktor: risiko High **4,5%**
    * 1 faktor: risiko High **6,3%**
    * 2 faktor: risiko High **9,2%**
    * 3 faktor: risiko High **13,3%**
  * Pengisian cepat (*DC fast charge*) menghasilkan penalti degradasi kapasitas (SOH) yang konsisten lebih curam di setiap kohor siklus pemakaian.
* **Aksi Strategis**:
  * Rotasi berkala unit kendaraan antara rute berat dan rute ringan.
  * Pembatasan via firmware BMS: prioritaskan *AC slow/normal charge*, batasi pengosongan baterai di bawah 20% SoC (DoD >80%).
  * Kemitraan dengan penyedia SPKLU (CPO) untuk program *smart charging* adaptif.

---

## 🗂️ Struktur Repositori

```text
.
├── README.md                                   # Dokumentasi proyek & ringkasan strategis
├── anu.txt                                     # Catatan ringkas temuan strategis
├── data/
│   ├── ev_battery_health.csv                   # Dataset utama telemetri baterai (50.000 baris)
│   └── data_dictionary .csv                    # Kamus data dan deskripsi 35 fitur
└── notebook/
    └── ev_battery_strategic_analysis.ipynb     # Notebook analisis end-to-end (Modul 0–6)
```

---

## 📊 Struktur Modul Analisis di Notebook

Notebook [`ev_battery_strategic_analysis.ipynb`](notebook/ev_battery_strategic_analysis.ipynb) terstruktur secara sistematis dalam 7 modul:

1. **Modul 0: Import Library & Setup Lingkungan Komputasi** — Konfigurasi pustaka analitik dan visualisasi standar publikasi.
2. **Modul 1: Pemuatan Data & Kamus Data (Data Ingestion)** — Pengecekan path dinamis dan validasi 35 kolom telemetri.
3. **Modul 2: Diagnosis Kualitas Data Mentah** — Audit integritas data, deteksi *missing values* (3–8%), dan anomali sensorik.
4. **Modul 3: Pembersihan Data Berbasis Domain Fisika Baterai** — Imputasi deret waktu longitudinal per unit (`battery_id`) dan *clipping* batas fisik.
5. **Modul 4: Pra-Processing & Feature Engineering** — Pembentukan *bracket* hambatan dalam, kelompok jarak harian, dan indeks beban berat.
6. **Modul 5: Visualisasi untuk Informasi Strategis** — Tiga panel visualisasi multi-subplot yang membuktikan masing-masing masalah secara kuantitatif.
7. **Modul 6: Penentuan Informasi Strategis** — Sintesis temuan, rekomendasi aksi untuk setiap stakeholder, dan tabel keputusan strategis.

---

## 📋 Deskripsi Kolom (Data Dictionary)

Dataset memuat **35 kolom** telemetri dari 2.600 unit baterai (`battery_id`):

| Kolom | Tipe | Satuan | Deskripsi |
|---|---|---|---|
| `battery_id` | string | - | Identifikator unik paket baterai |
| `observation_id` | string | - | Identifikator unik observasi telemetri |
| `chemistry` | categorical | - | Jenis kimia sel (`LFP`, `NMC`, `NCA`, `LMO`) |
| `manufacturer` | categorical | - | Pabrikan paket baterai |
| `vehicle_model` | categorical | - | Model/platform kendaraan listrik |
| `pack_capacity_kwh` | float | kWh | Kapasitas nominal terpasang |
| `manufacture_date` | date | - | Tanggal manufaktur unit baterai |
| `observation_date` | date | - | Tanggal pencatatan telemetri |
| `battery_age_days` | int | hari | Usia baterai saat pembacaan telemetri |
| `cycle_count` | int | siklus | Jumlah siklus setara penuh (*equivalent full cycles*) |
| `climate_zone` | categorical | - | Zona iklim operasional kendaraan |
| `avg_daily_distance_km` | float | km | Rata-rata jarak tempuh harian |
| `avg_charging_power_kw` | float | kW | Rata-rata daya pengisian |
| `fast_charge_ratio` | float | 0–1 | Rasio sesi pengisian cepat (*DC Fast Charging*) |
| `avg_depth_of_discharge_pct` | float | % | Rata-rata kedalaman pengosongan (*Depth of Discharge*) |
| `avg_state_of_charge_pct` | float | % | Rata-rata tingkat keterisian baterai (*State of Charge*) |
| `charge_cycles_per_month` | float | /bulan | Frekuensi pengisian daya per bulan |
| `total_energy_throughput_kwh` | float | kWh | Total energi kumulatif yang dilewatkan |
| `avg_ambient_temp_c` | float | °C | Rata-rata suhu lingkungan operasional |
| `humidity_pct` | float | % | Rata-rata kelembapan relatif |
| `altitude_m` | float | m | Ketinggian wilayah operasional di atas permukaan laut |
| `internal_resistance_mohm` | float | mΩ | Hambatan dalam baterai (meningkat seiring penuaan) |
| `voltage_avg_v` | float | V | Rata-rata tegangan paket baterai |
| `current_avg_a` | float | A | Rata-rata arus beban |
| `charge_time_avg_min` | float | menit | Rata-rata durasi per sesi pengisian |
| `peak_temp_during_fast_charge_c` | float | °C | Suhu puncak selama sesi pengisian cepat |
| `capacity_fade_pct` | float | % | Penurunan kapasitas kumulatif ($100 - \text{SOH}$) |
| `service_count` | int | count | Frekuensi servis/perawatan hingga saat ini |
| `last_service_date` | date | - | Tanggal servis terakhir |
| `bms_firmware_version` | categorical | - | Versi firmware BMS |
| `recall_flag` | binary | 0/1 | Indikator unit terdampak recall pabrikan |
| **`state_of_health_pct`** | float **[TARGET]** | % | Kapasitas aktual terhadap nominal (*State of Health*) |
| **`remaining_useful_life_cycles`** | int **[TARGET]** | siklus | Estimasi sisa siklus hingga mencapai ambang batas 80% SOH |
| **`failure_risk_label`** | categorical **[TARGET]** | - | Tingkat risiko kegagalan (`Low`, `Medium`, `High`) |
| **`degradation_category`** | categorical **[TARGET]** | - | Kategori degradasi (`Normal`, `Accelerated`, `Critical`) |

---

## ⚙️ Metodologi & Sifat Data

* **Physics-informed Synthetic Data**: Dibuat menggunakan parameter elektrokimia baterai yang merujuk pada literatur siklus hidup baterai (NASA Ames Prognostics Center of Excellence dan riset penuaan sel).
* **Efek Acak Konsisten per Baterai**: Setiap `battery_id` membawa faktor kualitas manufaktur, karakteristik pengemudi, dan degradasi non-linear yang terkorelasi antar-waktu.
* **Injeksi Noise Telemetri**: Diberikan *missingness* acak 3–8% serta *outlier* sensorik 1–2% untuk merefleksikan kondisi lapangan sesungguhnya.
* **Distribusi Kelas Riil**: Proporsi label risiko kegagalan dibuat imbalans (~85% Low, ~9% Medium, ~6% High) mencerminkan populasi armada aktual.

---

## ⚠️ Batasan Analisis (Limitations)

Dataset ini bersifat **sintetis** dan dirancang untuk keperluan analitik, edukasi, benchmark pemodelan prediktif, serta perancangan metodologi pemeliharaan. Angka dan rekomendasi yang dihasilkan merupakan inferensi matematis dari pola telemetri, bukan dasar hukum/teknis untuk klaim garansi, pengujian laboratorium elektrokimia, atau keputusan keselamatan operasional langsung di dunia nyata tanpa validasi empiris fisik.

---

## 📄 Lisensi

Dataset dilisensikan di bawah **Creative Commons Zero: Public Domain (CC0)**.
