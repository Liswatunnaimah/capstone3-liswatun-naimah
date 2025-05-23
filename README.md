---

# **CAPSTONE PROJECT MODUL 3 - Predicting Hotel Booking Cancellation (Klasifikasi)**

### Dataset: Hotel Booking Demand  
### Author: Liswatun Naimah  

---

## Project Overview  
Proyek ini bertujuan membangun model klasifikasi untuk memprediksi apakah reservasi hotel akan dibatalkan (`is_canceled`). Dataset berisi data pemesanan hotel di Portugal. Model ini diharapkan membantu manajemen hotel mengambil tindakan preventif untuk mengurangi pembatalan dan kerugian finansial.

---

## Business Understanding  

### Business Problem  
- Tingginya *booking cancellation rate* menyebabkan kerugian pendapatan dan inefisiensi operasional.  
- Belum ada sistem prediktif otomatis untuk mengantisipasi pembatalan.

### Stakeholders  
- **Utama:** Tim Manajemen Hotel  
- **Pendukung:** Tim Revenue Management, Marketing, dan Customer Experience  

### Objective  
- Mengembangkan model klasifikasi untuk memprediksi `is_canceled`.  
- Mendukung keputusan berbasis data dalam promosi dan operasional.

---

## Problem Conversion  

- **Input:** Informasi pemesanan hotel  
- **Output:** Prediksi status pembatalan (`is_canceled`: 0 atau 1)  
- **Model Type:** Classification  
- **Learning Type:** Supervised Learning  

---

## Dataset Overview  

- **Jumlah Observasi:** 83.573  
- **Fitur:** 11 kolom  
- **Target:** `is_canceled`  
- **Link Dataset:** [Google Drive](https://drive.google.com/file/d/1YGCuluHZC8PvNAXNXF4ymu1jM6ejHSJv/view?usp=drive_link)

---

## Data Understanding  

- Kolom dengan missing value: hanya `country` (~0.4%)  
- Banyak data terindikasi duplikat → tidak dihapus untuk menjaga representasi perilaku pelanggan  
- Identifikasi fitur numerik dan kategorikal dilakukan untuk persiapan preprocessing  
- Distribusi target tidak seimbang (imbalanced): `63% not canceled`, `37% canceled`  

---

## Data Cleaning  

- Imputasi missing value `country` dengan `'Unknown'`  
- Duplikat dipertahankan karena tidak ada ID unik  
- Deteksi dan observasi outlier pada fitur numerik dilakukan  
- EDA dilakukan pada semua fitur untuk memahami distribusi data dan hubungan dengan target  

---

## Feature Engineering & Selection  

- Melakukan **outlier capping** pada fitur numerik: `previous_cancellations`, `booking_changes`, `days_in_waiting_list`, dan `required_car_parking_spaces`  
- Menyusun pipeline untuk **scaling** fitur numerik dan **encoding** fitur kategorikal  
- Tidak membuat fitur baru, fokus pada transformasi dan pemilihan fitur existing yang relevan secara bisnis  
- Seleksi fitur berdasarkan korelasi terhadap target dan uji statistik (Chi-Square, T-test)

---

## Modeling & Evaluation  

- Model baseline: Logistic Regression, KNN, Decision Tree  
- Model final: Random Forest, AdaBoost, XGBoost, LightGBM  
- Penanganan data imbalance: SMOTE  
- Evaluasi menggunakan:
  - **Recall** (utama karena False Negative lebih merugikan)
  - **F1 Score**
  - **PR Curve**
  - ROC hanya sebagai pelengkap  

- **Hyperparameter Tuning**: RandomizedSearchCV  

---

## Explainability  

- **SHAP (SHapley Additive Explanations)** digunakan untuk menjelaskan kontribusi setiap fitur terhadap output model  
- Visualisasi SHAP Summary Plot dan Force Plot menunjukkan bahwa fitur seperti `total_of_special_requests`, `deposit_type`, dan `country` merupakan faktor paling berpengaruh terhadap pembatalan reservasi  
- SHAP membantu meningkatkan transparansi model dan membangun kepercayaan dari stakeholder non-teknis  

---

## Final Result & Metrics  

- Model terbaik: **LightGBM**  
- Recall kelas cancel: **0.69**  
- Classification report menunjukkan performa baik secara keseluruhan  
- Model cukup akurat dan explainable melalui SHAP  

---

## Deployment Plan  

- Model LightGBM disimpan untuk deployment  
- Direkomendasikan integrasi via **Flask API** / **Streamlit App**  
- Dashboard tambahan dapat digunakan untuk tim marketing atau manajemen hotel  

---

## Business Simulation & Recommendation  

### Simulasi Biaya Promosi:
- Tanpa model: 654 pelanggan dikirim promo → biaya = 65.400  
- Dengan model: hanya 86 yang benar-benar perlu → biaya = 13.100  
- **Penghematan: Rp 52.300**  

### Rekomendasi Lanjutan:
1. Tambahkan fitur loyalitas dan spending behavior  
2. Lakukan A/B Testing implementasi model  
3. Gunakan F2 Score jika ingin lebih menekankan recall  
4. Tambahkan data musiman, tanggal, atau hari libur untuk prediksi yang lebih tajam  

---

## Model Limitation  

- Hanya valid untuk pola historis dalam dataset  
- Tidak bisa mengenali pola baru di luar observasi  
- Fitur `reserved_room_type` bersifat anonim  
- Performa model tergantung pada kualitas data input  

---

## Kesimpulan  

Model klasifikasi LightGBM berhasil memprediksi pembatalan hotel dengan cukup akurat dan explainable melalui SHAP. Model ini mampu memberikan insight actionable untuk manajemen hotel dalam mengurangi kerugian dan meningkatkan efisiensi.

---
