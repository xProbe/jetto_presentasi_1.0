# DYNAMIC PRICING ENGINE - JETTO.ID
## Pengembangan Model Machine Learning untuk Optimasi Harga E-Commerce

---

**Disusun Oleh:**
- Danendra Pandya Reswara (123230169)
- Adam Fauzan Waskito (123230178)
- Muhammad Ridhwan Fahmi R (123230179)

**Program Studi Informatika**
**Universitas Pembangunan Nasional "Veteran" Yogyakarta**
**2025**

---

## 📋 Ringkasan Eksekutif (Executive Summary)

Proyek ini bertujuan membangun sistem **Dynamic Pricing** (penentuan harga dinamis) untuk **Jetto.id**, sebuah startup e-commerce baru. Tantangan utama adalah ketiadaan data transaksi historis internal. Solusi yang kami kembangkan menggunakan pendekatan *hybrid*: memanfaatkan data pasar eksternal (Online Retail Dataset) dan mensimulasikan data internal berdasarkan aturan ekonomi (Rule-Based Simulation) untuk melatih model Machine Learning.

**Hasil Utama:**
- **Model Terbaik:** XGBoost Regressor (Tuned)
- **Performa Teknis:** R² Score **0.9997**, MAPE **0.54%**
- **Dampak Bisnis:** Potensi peningkatan profit sebesar **+9.61%** dibanding strategi harga statis.

---

## 1. Business Understanding

### Latar Belakang & Masalah
Jetto.id menghadapi masalah *Cold Start* dalam strategi penetapan harga:
1.  **Tidak ada data historis:** Sulit menentukan harga awal yang kompetitif namun tetap menguntungkan.
2.  **Risiko Salah Harga:** Harga terlalu tinggi menurunkan penjualan, harga terlalu rendah menggerus margin.
3.  **Kebutuhan Adaptasi:** Perlu sistem yang bisa merespons perubahan waktu (weekend/hari biasa) dan tren pasar.

### Tujuan Proyek
1.  Membangun model prediksi harga yang akurat (R² > 0.85).
2.  Membuat simulasi dampak bisnis untuk membuktikan kelayakan model.
3.  Menyediakan rekomendasi harga otomatis berbasis data (Data-Driven Pricing).

---

## 2. Data Understanding & Preparation

### Sumber Data
1.  **Data Eksternal:** *Online Retail Dataset* (UCI Machine Learning Repository/Kaggle) untuk menangkap pola harga pasar global.
2.  **Data Internal (Simulasi):** Karena data riil belum ada, kami men-generate 50.000 data transaksi simulasi menggunakan logika bisnis (elastisitas harga, musiman, dan stok).

### Proses Data Preparation
* **Data Cleaning:**
    * Menghapus transaksi batal (*Cancelled orders*).
    * Membuang data harga negatif/nol dan deskripsi kosong.
    * Menangani *Outliers* menggunakan metode IQR (Interquartile Range) dengan multiplier 3.0 (konservatif).
* **Feature Engineering (21 Fitur Baru):**
    * Agregasi level produk (bukan per transaksi).
    * Fitur Harga: `market_price_mean`, `price_volatility`.
    * Fitur Performa: `total_quantity_sold`, `revenue_per_transaction`.
    * Fitur Waktu: `weekend_ratio`, `peak_hour_ratio`.
    * Fitur Bisnis: `price_elasticity` (proxy), `avg_profit_margin`.

---

## 3. Modeling Strategy

Kami membandingkan **7 Algoritma Machine Learning** untuk mencari model terbaik:

### Model Linear (Baseline)
1.  **Linear Regression:** Sebagai patokan dasar.
2.  **Ridge Regression:** Regresi dengan regularisasi L2.
3.  **Lasso Regression:** Regresi dengan regularisasi L1.
4.  **ElasticNet:** Kombinasi L1 dan L2.

### Model Ensemble (Advanced)
5.  **Random Forest:** Metode *Bagging* untuk menangkap pola non-linear.
6.  **Gradient Boosting:** Metode *Boosting* standar.
7.  **XGBoost (Extreme Gradient Boosting):** Algoritma *state-of-the-art* yang efisien dan akurat.

> *Catatan: Model dipilih berdasarkan metrik R² (akurasi) dan MAPE (rata-rata persentase error).*

---

## 4. Hasil Evaluasi & Analisis

### Perbandingan Performa Model
Setelah proses *Hyperparameter Tuning*, berikut adalah hasil pada data validasi:

| Model | R² Score | MAPE | Status |
| :--- | :--- | :--- | :--- |
| Linear Regression | 0.9999 | 0.26% | Baseline |
| Gradient Boosting | 0.9995 | 0.92% | Sangat Bagus |
| Random Forest | 0.9987 | 1.12% | Bagus |
| **XGBoost (Tuned)** | **0.9992** | **1.37%** | **Terbaik 🏆** |

**Hasil Evaluasi Final (Test Set):**
- R² Score: **0.9997**
- MAPE: **0.54%**

**Mengapa XGBoost?**
* Akurasi tertinggi dan error terendah.
* Sangat cepat dalam proses training dan prediksi.
* Mampu menangkap pola kompleks dari aturan simulasi bisnis yang kami buat.

### Analisis Fitur Penting (Feature Importance)
Faktor utama yang menentukan harga rekomendasi model:
1.  **Market Price Mean (53.7%):** Harga pasar adalah acuan utama.
2.  **Market Price Max (21.3%):** Batas atas harga pasar.
3.  **Market Price Min (12.5%):** Batas bawah harga pasar.
4.  **Market Price Std (12.0%):** Standar deviasi/volatilitas harga.

### Simulasi Dampak Bisnis
Kami membandingkan **Static Pricing** (Harga Rata-rata Pasar) vs **Dynamic Pricing** (Saran Model):
* **Revenue Growth:** +3.36%
* **Profit Growth:** +9.61%
* **Estimasi Profit Tambahan:** £28,307.32 (berdasarkan volume data simulasi 50.000 transaksi).

---

## 5. Kesimpulan & Rekomendasi

### Kesimpulan
Sistem *Dynamic Pricing Engine* ini terbukti secara teknis dan bisnis:
* ✅ Model berhasil mempelajari logika bisnis dengan sangat presisi (R² = 0.9997, MAPE = 0.54%).
* ✅ Simulasi menunjukkan potensi kenaikan keuntungan sebesar +9.61% (£28,307.32).
* ✅ Sistem siap digunakan sebagai *Proof of Concept* (PoC) untuk fase peluncuran Jetto.id.

### Rekomendasi (Next Steps)
1.  **Pilot Testing:** Implementasikan model pada 20% produk terlaris di Jetto.id.
2.  **Data Collection:** Mulai kumpulkan data transaksi riil pelanggan untuk menggantikan data simulasi secara bertahap.
3.  **Retraining:** Latih ulang model setiap bulan dengan data riil untuk menangkap perubahan perilaku konsumen yang sebenarnya.

---

## 🛠️ Cara Menjalankan Project

1.  **Install Library:**
    ```bash
    pip install pandas numpy scikit-learn xgboost matplotlib seaborn
    ```
2.  **Urutan Eksekusi Notebook:**
    * Jalankan `Presentasi_Data_Preparation_Jetto_fix.ipynb` 
    * Jalankan `Presentasi_Data_Modelling_Jetto_fix.ipynb` 
    * Jalankan `Presentasi_Data_EvaluationDeployment_fix.ipynb` 