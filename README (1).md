# 🏛️ Sistem Case-Based Reasoning (CBR)
### Analisis Putusan Pengadilan — Pidana Khusus: Korupsi Kerugian Keuangan Negara

> **Mata Kuliah:** Penalaran Komputer | **Semester:** Genap 2025/2026  
> **Institusi:** Fakultas Teknik Informatika, Universitas Muhammadiyah Malang

---

## 📋 Daftar Isi
- [Deskripsi Proyek](#-deskripsi-proyek)
- [Siklus CBR](#-siklus-cbr)
- [Struktur Repository](#-struktur-repository)
- [Instalasi](#-instalasi)
- [Cara Menjalankan Pipeline](#-cara-menjalankan-pipeline)
- [Contoh Perintah](#-contoh-perintah)
- [Evaluasi Model](#-evaluasi-model)
- [Anggota Tim](#-anggota-tim)

---

## 📌 Deskripsi Proyek

Proyek ini mengimplementasikan sistem **Case-Based Reasoning (CBR)** berbasis Python
untuk mendukung analisis putusan pengadilan domain **Pidana Khusus — Korupsi Kerugian Keuangan Negara**.

Data bersumber dari [Direktori Putusan MA RI](https://putusan3.mahkamahagung.go.id) (≥ 32 dokumen PDF).

---

## 🔄 Siklus CBR

```
Kasus Baru
    │
    ▼
┌─────────────────────┐
│  1. Case Base       │  ← Preprocessing ≥30 putusan PDF
└─────────┬───────────┘
          ▼
┌─────────────────────┐
│  2. Representation  │  ← Ekstraksi metadata → cases.csv
└─────────┬───────────┘
          ▼
┌─────────────────────┐
│  3. Case Retrieval  │  ← TF-IDF + SVM → top-k kasus mirip
└─────────┬───────────┘
          ▼
┌─────────────────────┐
│  4. Solution Reuse  │  ← Majority vote / weighted similarity
└─────────┬───────────┘
          ▼
┌─────────────────────┐
│  5. Evaluation      │  ← Accuracy, Precision, Recall, F1
└─────────────────────┘
```

---

## 📁 Struktur Repository

```
cbr-korupsi-keuangan-negara/
├── data/
│   ├── raw/                        # Teks putusan hasil scraping (*.txt)
│   ├── processed/
│   │   ├── cases.csv               # Dataset metadata terstruktur
│   │   ├── cases.json              # Dataset format JSON
│   │   └── cases_full.csv          # Dataset + teks penuh
│   ├── eval/
│   │   ├── queries.json            # Query uji + ground-truth
│   │   ├── retrieval_metrics.csv   # Metrik Recall@k & MRR
│   │   ├── prediction_metrics.csv  # Metrik Accuracy/Precision/Recall/F1
│   │   └── figures/                # Grafik evaluasi
│   └── results/
│       └── predictions.csv         # Hasil prediksi kasus baru
├── models/                         # Model tersimpan (pickle)
│   ├── tfidf_vectorizer.pkl
│   ├── tfidf_matrix.npy
│   ├── case_ids.json
│   ├── case_solutions.json
│   ├── classifier_svm.pkl
│   └── classifier_naive_bayes.pkl
├── notebooks/
│   ├── 01_case_base.ipynb          # Tahap 1: Scraping & Preprocessing
│   ├── 02_representation.ipynb     # Tahap 2: Case Representation
│   ├── 03_retrieval.ipynb          # Tahap 3: Case Retrieval
│   ├── 04_solution_reuse.ipynb     # Tahap 4: Case Solution Reuse
│   └── 05_evaluation.ipynb         # Tahap 5: Evaluasi Model
├── logs/
│   └── cleaning.log
├── requirements.txt
└── README.md
```

---

## 🚀 Instalasi

```bash
git clone https://github.com/Fanyfauzah/cbr-korupsi-keuangan-negara.git
cd cbr-korupsi-keuangan-negara
python -m venv venv
venv\Scripts\activate        # Windows
pip install -r requirements.txt
jupyter notebook
```

---

## ▶️ Cara Menjalankan Pipeline

| No | Notebook | Fungsi | Output |
|----|----------|--------|--------|
| 1 | `01_case_base.ipynb` | Konversi PDF → teks bersih | `data/raw/*.txt` |
| 2 | `02_representation.ipynb` | Ekstrak metadata → dataset | `data/processed/cases.csv` |
| 3 | `03_retrieval.ipynb` | TF-IDF + SVM + fungsi retrieve() | `models/` |
| 4 | `04_solution_reuse.ipynb` | Prediksi solusi kasus baru | `data/results/predictions.csv` |
| 5 | `05_evaluation.ipynb` | Evaluasi metrik + grafik | `data/eval/` |

> ⚠️ Jalankan secara **berurutan** dari Tahap 1 → Tahap 5.

---

## 💻 Contoh Perintah

```python
# Tahap 3 — Retrieval
hasil = retrieve("terdakwa korupsi dana desa 500 juta rupiah", k=5)
# Output: [('case_012', 0.8721), ('case_007', 0.8134), ...]

# Tahap 4 — Prediksi solusi
solusi = predict_outcome("terdakwa mark-up anggaran jalan 2 miliar")
print(solusi["predicted_label"])   # 'bersalah'
print(solusi["predicted_pidana"])  # '4 tahun'
```

---

## 📊 Evaluasi Model

| Metrik | Deskripsi |
|--------|-----------|
| Accuracy | Proporsi prediksi benar |
| Precision | Ketepatan prediksi positif |
| Recall | Kemampuan menemukan kasus relevan |
| F1-Score | Harmonic mean Precision & Recall |
| Recall@k | Ground-truth masuk top-k retrieval |
| MRR | Mean Reciprocal Rank |

---

## 🗃️ Sumber Data

| | |
|--|--|
| **Platform** | Direktori Putusan Mahkamah Agung RI |
| **URL** | https://putusan3.mahkamahagung.go.id |
| **Kategori** | Pidana Khusus → Korupsi Kerugian Keuangan Negara |
| **Jumlah** | ≥ 32 putusan PDF |

---

## 👥 Anggota Tim

| Nama | NIM |
|------|-----|
| Fany Fauzah | 202310370311066 |

---

*Proyek Akademik — Penalaran Komputer, Informatika UMM, 2025/2026*
