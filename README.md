---

# **CAPSTONE PROJECT MODUL 3 - Predicting Hotel Booking Cancellation (Klasifikasi)**

## Daftar Isi
- [Project Overview](#project-overview)
- [Business Understanding](#business-understanding)
- [Dataset](#dataset)
- [Data Understanding](#data-understanding)
- [Data Preparation](#data-preparation)
- [Modeling](#modeling)
- [Model Evaluation](#model-evaluation)
- [Interpretability](#interpretability)
- [Deployment Plan](#deployment-plan)
- [Business Impact & Recommendations](#business-impact--recommendations)
- [Conclusion](#conclusion)


### Dataset: Hotel Booking Demand

### Author: Liswatun Naimah

---

## Project Overview

Proyek ini bertujuan membangun model klasifikasi untuk memprediksi apakah reservasi hotel akan dibatalkan (`is_canceled`). Dataset berisi data pemesanan hotel di Portugal, mencakup informasi negara asal tamu, channel pemesanan, tipe pelanggan, hingga permintaan khusus. Model ini diharapkan membantu manajemen hotel dalam mengambil tindakan preventif untuk menurunkan potensi pembatalan dan kerugian operasional maupun finansial.

---

## Business Understanding

### Business Context

Industri perhotelan mengalami tantangan besar akibat tingginya angka pembatalan reservasi. Hal ini berdampak pada pendapatan, pengelolaan kamar, logistik staf, dan stok makanan. Model prediksi pembatalan pemesanan yang akurat dapat membantu hotel melakukan tindakan preventif untuk mengurangi kerugian.

### Masalah Bisnis

* Booking cancellation rate tinggi
* Tidak ada sistem prediktif otomatis

### Dampak Negatif Saat Ini

* Loss of Revenue
* Wasted Effort
* Penurunan Customer Experience

### Stakeholder

* Primary: Tim Manajemen Hotel
* Secondary: Tim Revenue Management, Tim Customer Experience, Tim Marketing

### Tujuan Proyek

* Memprediksi status pembatalan reservasi
* Menghasilkan insight yang actionable
* Mengoptimalkan strategi promosi dan efisiensi operasional

### Problem Conversion to ML

* Input: Informasi pemesanan
* Output: Prediksi label `is_canceled`
* Model Type: Classification
* Learning Type: Supervised Learning

### Evaluation Strategy & Metrics

* Fokus: Recall (kelas 1)
* Tambahan: F1 Score, PR Curve
* PR Curve dipilih karena data imbalance

---

## Dataset

* Jumlah Observasi: 83.573
* Jumlah Fitur: 11
* Target: `is_canceled`
* Link Dataset: [GDrive](https://drive.google.com/file/d/1YGCuluHZC8PvNAXNXF4ymu1jM6ejHSJv/view?usp=drive_link)

---

## Data Understanding

* Imputasi `country` = 'Unknown' (karena missing < 0.5%)
* Duplikat ditemukan banyak, namun **tidak dihapus** agar tidak terjadi data loss → dipertahankan karena tidak ada ID unik

---

## Data Preparation

* Feature capping dilakukan pada: `previous_cancellations`, `booking_changes`, `days_in_waiting_list`, `required_car_parking_spaces`
* Pembagian fitur numerik & kategorikal
* Train-Test Split 80:20 (stratified)
* Preprocessing pipeline dengan `ColumnTransformer` (numerik = scaling, kategorikal = OHE)

---

## Exploratory Data Analysis (EDA)

* Fitur `market_segment`, `deposit_type`, dan `total_of_special_requests` memiliki korelasi visual & statistik dengan target
* Dilakukan uji statistik (T-Test, Chi-Square)
* Semua fitur signifikan terhadap `is_canceled`

---

## Modeling

* Model baseline: Logistic Regression, Decision Tree, KNN
* Model akhir: Random Forest, AdaBoost, XGBoost, LightGBM (tuning)
* Penanganan imbalance dengan SMOTE
* Hyperparameter tuning dengan RandomizedSearchCV
* Metrik utama: Recall & F1 Score

---

## Explainability

* SHAP Summary & Force Plot untuk LightGBM
* Menunjukkan fitur penting: `total_of_special_requests`, `deposit_type`, `country`
* LIME digunakan untuk verifikasi interpretasi model pada sample individual

---

## Final Evaluation

* Classification Report: Recall = 0.69 untuk kelas cancel
* Confusion Matrix menunjukkan prediksi dominan benar pada kedua kelas
* Model cukup akurat dan explainable

---

## Deployment Plan

* Model LightGBM disimpan untuk deployment
* Akan diintegrasikan ke sistem reservasi hotel
* Model siap menerima input user untuk prediksi cancelation
* Implementasi disarankan via backend Flask/Streamlit + Dashboard untuk tim marketing

---

## Business Recommendation

### Simulasi Dampak Finansial

* Tanpa model: 654 pelanggan → promo ke semua = biaya 65.400
* Dengan model: hanya 86 yang benar-benar perlu → biaya hanya 13.100
* Penghematan: **52.300**

### Tambahan Dampak

* Jika promo ditingkatkan jadi 250 per pelanggan tetap lebih hemat dibanding promosi massal

### Future Recommendation

1. Tambahkan fitur seperti `loyalty_level`, `amount_spent`, `last_login`, dll
2. Tambahkan metrik seperti F2 jika ingin fokus ke recall
3. Gunakan data tambahan di musim tertentu untuk mempertajam prediksi
4. Lanjutkan A/B Testing pada hasil implementasi model

---

## Model Limitation

* Hanya berlaku untuk pola historis
* Tidak bisa menangkap perubahan perilaku booking baru
* Bergantung pada kualitas dan cakupan data historis
* Fitur `reserved_room_type` anonim → tidak transparan secara bisnis

---

## Conclusion

Model LightGBM yang dibangun berhasil memprediksi pembatalan reservasi dengan cukup baik dan memiliki interpretasi yang transparan melalui SHAP dan LIME. Model ini dapat memberikan dampak positif terhadap strategi pemasaran dan efisiensi operasional hotel.

---
