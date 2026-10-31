# Apakah Apple Membagikan Lebih Banyak Uang daripada yang Dihasilkan Bisnisnya?

Analisis informasi strategis dari laporan keuangan resmi Apple Inc. (AAPL): membandingkan **uang yang dibagikan ke pemegang saham (buyback + dividen)** dengan **arus kas bebas (FCF)** tahun fiskal 2015–2025.

Notebook utama: [`notebook/final.ipynb`](notebook/final.ipynb)

## Pertanyaan

> Apakah Apple membagikan lebih banyak uang daripada yang dihasilkan bisnisnya?

Ukurannya:

```
Rasio = (Buyback + Dividen) ÷ FCF × 100%
FCF   = Kas dari aktivitas operasi − Belanja pabrik dan peralatan (capex)
```

Rasio di atas **100%** berarti selisihnya dibiayai dari simpanan kas atau pinjaman.

## Untuk siapa

| Pihak | Keputusan yang terbantu |
|---|---|
| Dewan direksi dan CFO Apple (utama) | Menetapkan batas buyback dan dividen supaya tidak menggerus simpanan |
| Pemberi pinjaman dan pembeli surat utang | Menilai risiko utang dan syarat pinjaman |
| Investor dividen dan buyback | Menilai apakah pembagian bisa berlanjut |

> Ini bukan saran investasi. Dataset tidak memuat harga saham.

## Dataset

Folder [`apple_stocks_dataset/`](apple_stocks_dataset/) — data resmi SEC EDGAR (CIK 0000320193), refresh terakhir 2026-10-19. Berisi 25.135 observasi keuangan dan 1.002 filing.

| File | Peran |
|---|---|
| `financial_facts.csv` | Semua angka keuangan (utama): `concept`, `value`, `unit`, `form`, `start`, `end`, `filed`, `accn` |
| `filings.csv` | Daftar surat SEC (penunjang): verifikasi sumber angka via `accessionNumber` dan waktu terbit surat utang (`424B2`) |
| `company_profile.json` | Profil perusahaan (CIK, ticker, bursa) |
| `data_dictionary.csv` | Kamus kolom |

Kode angka (XBRL) yang dipakai: `NetCashProvidedByUsedInOperatingActivities` (kas operasi), `PaymentsToAcquirePropertyPlantAndEquipment` (capex), `PaymentsForRepurchaseOfCommonStock` (buyback), `PaymentsOfDividends` (dividen), `StockholdersEquity` (ekuitas), `LongTermDebtNoncurrent` (utang jangka panjang).

## Alur notebook

1. **Membaca file data** — load 3 file dari `apple_stocks_dataset/`
2. **Mengenal isi data dan arti tiap kolom** — penjelasan bebas semua kolom
3. **Menyaring data mentah (preprocessing)** — 4 aturan:
   - hanya laporan tahunan 10-K, satuan USD
   - angka arus: hanya periode tepat 1 tahun (350–380 hari)
   - angka saldo: hanya posisi akhir tanggal
   - menangani duplikasi: satu angka per tahun, ambil dari laporan terbaru
4. **Grafik + cara membaca** — 4 grafik:
   - FCF vs total pembagian (batang)
   - Rasio pembagian ÷ FCF dengan batas 100% (bukti inti)
   - Ekuitas vs utang jangka panjang (dampak ke neraca)
   - Penerbitan surat utang 424B2 vs rasio (opsional)
5. **Jawaban** — ringkasan angka
6. **Solusi dan batasan**

## Temuan kunci

- Rasio melewati **100% pada 6 dari 11 tahun**: 2018, 2019, 2020, 2021, 2024, 2025
- Puncak **137,6% di 2019**
- Surplus 2015–2017: **40,9 miliar USD** (FCF > pembagian)
- Kelebihan pembagian 2018–2025: **59,5 miliar USD** (pembagian > FCF)
- Neto 2015–2025: **18,6 miliar USD** lebih banyak dibagikan daripada FCF
- Ekuitas turun dari puncak **134,0 miliar (2017)** ke **50,7 miliar (2022)**; utang jangka panjang memuncak **109,1 miliar (2021)**
- Buyback yang berayun (29,7–94,9 miliar; koefisien variasi 0,36), dividen stabil (11,6 → 15,4 miliar; koefisien variasi 0,09)
- Filing surat utang (424B2) **tidak** berkelompok di tahun rasio tinggi (korelasi −0,19)

## Menjalankan

```
pip install pandas numpy matplotlib jupyter
jupyter notebook notebook/final.ipynb
```

Notebook mencari data di `../apple_stocks_dataset`, `apple_stocks_dataset`, atau folder saat ini. Jika file CSV dipindah, sesuaikan `DATA_DIR` di cell pertama.

## Batasan

- Rasio > 100% tidak otomatis buruk: simpanan Apple dalam bentuk sekuritas tidak dihitung (hanya kas dan setara kas).
- Data menunjukkan **apa** yang terjadi, bukan **mengapa** — alasan keputusan manajemen perlu sumber lain (isi 10-K/10-Q).
- Tidak ada harga saham, jadi kualitas timing buyback tidak bisa dinilai.
- Korelasi dengan 424B2 hanya indikatif (11 titik data).
- Data 2014 tidak lengkap, analisis dimulai 2015.
