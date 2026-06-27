# 🏛️ Sistem Case-Based Reasoning (CBR)
### Analisis Putusan Pengadilan — Pidana Khusus: Korupsi Kerugian Keuangan Negara

> **Mata Kuliah:** Penalaran Komputer  
> **Semester:** Genap 2025/2026  
> **Institusi:** Fakultas Teknik Informatika, Universitas Muhammadiyah Malang

---

## 📋 Daftar Isi
- [Deskripsi Proyek](#-deskripsi-proyek)
- [Siklus CBR](#-siklus-cbr)
- [Struktur Repository](#-struktur-repository)
- [Persyaratan Sistem](#-persyaratan-sistem)
- [Instalasi](#-instalasi)
- [Cara Menjalankan Pipeline](#-cara-menjalankan-pipeline)
- [Contoh Perintah](#-contoh-perintah)
- [Evaluasi Model](#-evaluasi-model)
- [Sumber Data](#-sumber-data)
- [Anggota Tim](#-anggota-tim)

---

## 📌 Deskripsi Proyek

Proyek ini mengimplementasikan sistem **Case-Based Reasoning (CBR)** berbasis Python untuk mendukung analisis putusan pengadilan pada domain:

> **Pidana Khusus — Korupsi Kerugian Keuangan Negara**

Sistem ini memanfaatkan data putusan yang dipublikasikan oleh [Direktori Putusan Mahkamah Agung Republik Indonesia](https://putusan3.mahkamahagung.go.id) dengan jumlah minimal **30 dokumen putusan** dalam format PDF.

Sistem CBR ini mampu:
- Mengambil dan memproses teks putusan secara otomatis
- Merepresentasikan setiap kasus dalam format terstruktur
- Mencari kasus lama yang paling mirip dengan kasus baru
- Memprediksi solusi (amar putusan) berdasarkan kasus serupa
- Mengevaluasi performa model retrieval secara kuantitatif

---

## 🔄 Siklus CBR

```
Kasus Baru
    │
    ▼
┌─────────────────────┐
│  1. Case Base       │  ← Scraping & Preprocessing ≥30 putusan PDF
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐
│  2. Case            │  ← Ekstraksi metadata, fitur teks → cases.csv
│     Representation  │
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐
│  3. Case Retrieval  │  ← TF-IDF + SVM → top-k kasus termirip
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐
│  4. Solution Reuse  │  ← Majority vote / weighted similarity
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐
│  5. Evaluation      │  ← Accuracy, Precision, Recall, F1-Score
└─────────────────────┘
```

---

## 📁 Struktur Repository

```
cbr-korupsi-keuangan-negara/
│
├── 📂 data/
│   ├── raw/                        # Teks putusan hasil scraping (*.txt)
│   ├── processed/
│   │   ├── cases.csv               # Dataset metadata terstruktur
│   │   ├── cases.json              # Dataset format JSON
│   │   └── cases_full.csv          # Dataset lengkap + teks penuh
│   ├── eval/
│   │   ├── queries.json            # Query uji + ground-truth
│   │   ├── retrieval_metrics.csv   # Metrik Recall@k & MRR
│   │   ├── prediction_metrics.csv  # Metrik Accuracy/Precision/Recall/F1
│   │   └── figures/                # Grafik hasil evaluasi
│   └── results/
│       └── predictions.csv         # Hasil prediksi kasus baru
│
├── 📂 models/                      # Model tersimpan
│   ├── tfidf_vectorizer.pkl
│   ├── tfidf_matrix.npy
│   ├── case_ids.json
│   ├── case_solutions.json
│   ├── classifier_svm.pkl
│   └── classifier_naive_bayes.pkl
│
├── 📂 notebooks/
│   ├── 01_case_base.ipynb          # Tahap 1: Scraping & Preprocessing
│   ├── 02_representation.ipynb     # Tahap 2: Case Representation
│   ├── 03_retrieval.ipynb          # Tahap 3: Case Retrieval
│   ├── 04_solution_reuse.ipynb     # Tahap 4: Case Solution Reuse
│   └── 05_evaluation.ipynb         # Tahap 5: Evaluasi Model
│
├── 📂 logs/
│   └── cleaning.log                # Log proses pembersihan teks
│
├── requirements.txt                # Daftar library Python
└── README.md                       # Dokumentasi proyek
```

---

## ⚙️ Persyaratan Sistem

| Komponen | Minimum |
|----------|---------|
| Python | 3.9 atau lebih baru |
| RAM | 4 GB (rekomendasi 8 GB) |
| Storage | 2 GB (untuk data & model) |
| Koneksi Internet | Diperlukan saat scraping |

---

## 🚀 Instalasi

### Langkah 1 — Clone Repository
```bash
git clone https://github.com/Fanyfauzah/cbr-korupsi-keuangan-negara.git
cd cbr-korupsi-keuangan-negara
```

### Langkah 2 — Buat Virtual Environment
```bash
# Buat environment
python -m venv venv

# Aktifkan (Windows)
venv\Scripts\activate

# Aktifkan (macOS/Linux)
source venv/bin/activate
```

### Langkah 3 — Install Dependensi
```bash
pip install -r requirements.txt
```

### Langkah 4 — Jalankan Jupyter Notebook
```bash
jupyter notebook
```

---

## ▶️ Cara Menjalankan Pipeline

Jalankan notebook secara **berurutan** dari Tahap 1 hingga Tahap 5:

| No | Notebook | Deskripsi | Output |
|----|----------|-----------|--------|
| 1 | `01_case_base.ipynb` | Scraping ≥30 putusan PDF dari MA RI, konversi ke teks, cleaning | `data/raw/*.txt` |
| 2 | `02_representation.ipynb` | Ekstraksi metadata & fitur teks, labeling otomatis | `data/processed/cases.csv` |
| 3 | `03_retrieval.ipynb` | TF-IDF vectorizer, training SVM & Naive Bayes, fungsi `retrieve()` | `models/`, `data/eval/queries.json` |
| 4 | `04_solution_reuse.ipynb` | Prediksi solusi dengan majority vote & weighted similarity | `data/results/predictions.csv` |
| 5 | `05_evaluation.ipynb` | Hitung metrik evaluasi, visualisasi, error analysis | `data/eval/retrieval_metrics.csv` |

> ⚠️ **Penting:** Pastikan setiap tahap selesai dijalankan sebelum melanjutkan ke tahap berikutnya.

---

## 💻 Contoh Perintah

### Retrieval Kasus Mirip
```python
# Notebook 03_retrieval.ipynb
hasil = retrieve("terdakwa korupsi dana desa sebesar 500 juta rupiah", k=5)

# Output:
# [('case_012', 0.8721), ('case_007', 0.8134), ('case_019', 0.7956), ...]
```

### Prediksi Solusi Kasus Baru
```python
# Notebook 04_solution_reuse.ipynb
solusi = predict_outcome("terdakwa mark-up anggaran jalan senilai 2 miliar")

print(solusi["predicted_label"])    # 'bersalah'
print(solusi["predicted_pidana"])   # '4 tahun'
print(solusi["top_k_case_ids"])     # ['case_003', 'case_017', 'case_021', ...]
print(solusi["similarity_max"])     # 0.8934
```

---

## 📊 Evaluasi Model

### Metrik yang Digunakan

| Metrik | Deskripsi |
|--------|-----------|
| **Accuracy** | Proporsi prediksi label yang benar dari keseluruhan data |
| **Precision** | Ketepatan model dalam memprediksi kelas positif |
| **Recall** | Kemampuan model menemukan semua kasus yang relevan |
| **F1-Score** | Harmonic mean antara Precision dan Recall |
| **Recall@k** | Proporsi ground-truth yang masuk dalam top-k hasil retrieval |
| **MRR** | Mean Reciprocal Rank — rata-rata posisi hasil yang relevan |

### Model yang Dibandingkan

| Model | Pendekatan |
|-------|------------|
| TF-IDF + Cosine Similarity | Retrieval berbasis statistik term |
| TF-IDF + SVM | Klasifikasi label putusan |
| TF-IDF + Naive Bayes | Klasifikasi probabilistik |
| CBR Majority Vote | Prediksi berdasarkan suara terbanyak top-k |
| CBR Weighted Similarity | Prediksi berbobot skor kemiripan |

---

## 🗃️ Sumber Data

| Keterangan | Detail |
|------------|--------|
| **Platform** | Direktori Putusan Mahkamah Agung RI |
| **URL** | https://putusan3.mahkamahagung.go.id |
| **Kategori** | Pidana Khusus → Korupsi Kerugian Keuangan Negara |
| **Format** | PDF |
| **Jumlah Dokumen** | ≥ 32 putusan |

---

## 👥 Anggota Tim

| Nama | NIM |
|------|-----|
| Fany Fauzah | 202310370311066 |

---
