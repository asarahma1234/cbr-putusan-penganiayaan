# CBR Putusan Penganiayaan PN Rantau Prapat

## Identitas

Nama: Asa Saila Rahma

NIM: 202310370311201

Mata Kuliah: Penalaran Komputer

Topik: Case-Based Reasoning pada Putusan Pengadilan

## Deskripsi Project

Project ini merupakan implementasi sederhana metode Case-Based Reasoning (CBR) menggunakan data putusan pengadilan dari Direktori Putusan Mahkamah Agung Republik Indonesia.

Data yang digunakan berfokus pada satu domain perkara, yaitu:

* Jenis perkara: Pidana Umum – Penganiayaan
* Pengadilan: PN Rantau Prapat
* Pasal utama: Pasal 351 ayat (1) KUHP
* Jumlah dokumen: 30 putusan

Sistem ini dibangun untuk mencari kasus lama yang paling mirip dengan kasus baru, kemudian menggunakan solusi dari kasus lama sebagai rekomendasi penyelesaian kasus baru.

## Metode yang Digunakan

Metode utama yang digunakan dalam project ini adalah:

1. Case Base Construction
   Mengumpulkan 30 dokumen putusan pengadilan dalam format PDF.

2. Case Representation
   Mengekstrak metadata dan isi penting dari setiap putusan, seperti nomor perkara, tanggal putusan, terdakwa, pasal, amar putusan, dan ringkasan fakta.

3. Case Retrieval
   Menggunakan TF-IDF Vectorization dan Cosine Similarity untuk mencari kasus lama yang paling mirip dengan query kasus baru.

4. Case/Solution Reuse
   Mengambil solusi dari kasus paling mirip berdasarkan amar putusan. Sistem juga menggunakan majority vote dari top-k kasus untuk menentukan label solusi.

5. Evaluation
   Mengevaluasi sistem menggunakan metrik Accuracy, Precision, Recall, F1-score, dan Mean Reciprocal Rank (MRR).

## Struktur Folder

Struktur folder project ini adalah sebagai berikut:

cbr-putusan-penganiayaan/
│
├── data/
│   ├── raw/
│   ├── text/
│   ├── processed/
│   ├── eval/
│   └── results/
│
├── notebooks/
│   ├── 01_build_case_base.ipynb
│   ├── 02_case_representation.ipynb
│   ├── 03_case_retrieval.ipynb
│   ├── 04_solution_reuse.ipynb
│   └── 05_model_evaluation.ipynb
│
├── README.md
└── requirements.txt

## Penjelasan Folder

### data/raw/

Folder ini berisi 30 file PDF putusan pengadilan yang digunakan sebagai data mentah.

### data/text/

Folder ini berisi hasil ekstraksi teks dari setiap file PDF.

### data/processed/

Folder ini berisi data hasil preprocessing dan representasi kasus, seperti:

* cases.csv
* cases.json
* cases.xlsx
* tfidf_vectorizer.pkl
* tfidf_matrix.npz
* case_index.csv
* similarity_matrix.csv
* summary setiap tahap

### data/eval/

Folder ini berisi query evaluasi, yaitu:

* queries.json

### data/results/

Folder ini berisi hasil retrieval, hasil prediksi, dan hasil evaluasi, seperti:

* retrieval_results.csv
* retrieval_metrics.csv
* retrieval_eval_details.csv
* predictions.csv
* prediction_details.csv
* prediction_metrics.csv
* prediction_eval_details.csv
* classification_report.txt
* evaluation_analysis.txt

### notebooks/

Folder ini berisi notebook pengerjaan dari tahap awal sampai evaluasi.

## Cara Instalasi

Install library yang dibutuhkan dengan menjalankan perintah berikut:

pip install -r requirements.txt

## Cara Menjalankan Project

Notebook dijalankan secara berurutan dari tahap pertama sampai tahap kelima:

1. 01_build_case_base.ipynb
   Digunakan untuk membaca 30 PDF, mengekstrak teks, membersihkan teks, dan menyimpan hasil ekstraksi.

2. 02_case_representation.ipynb
   Digunakan untuk mengambil metadata penting dari teks putusan dan membuat file cases.csv.

3. 03_case_retrieval.ipynb
   Digunakan untuk membangun model retrieval menggunakan TF-IDF dan Cosine Similarity.

4. 04_solution_reuse.ipynb
   Digunakan untuk mengambil solusi dari kasus lama yang paling mirip dan membuat prediksi outcome.

5. 05_model_evaluation.ipynb
   Digunakan untuk mengevaluasi performa retrieval dan prediksi solusi.

## Fungsi Utama

Fungsi utama pada tahap retrieval adalah:

retrieve(query, k=5)

Fungsi ini menerima teks kasus baru, kemudian mengembalikan 5 kasus lama yang paling mirip berdasarkan nilai cosine similarity.

Fungsi utama pada tahap solution reuse adalah:

predict_outcome(query, k=5)

Fungsi ini menerima teks kasus baru, mencari top-5 kasus termirip, lalu memberikan rekomendasi solusi berdasarkan amar putusan dari kasus lama.

## Hasil Evaluasi

Berdasarkan hasil evaluasi internal terhadap 6 query evaluasi, diperoleh hasil:

### Retrieval Evaluation

* Accuracy@1: 1.0000
* Accuracy@3: 1.0000
* Accuracy@5: 1.0000
* Precision@5: 0.2000
* Recall@5: 1.0000
* F1@5: 0.3333
* MRR: 1.0000

### Prediction Evaluation

* Accuracy: 1.0000
* Precision Macro: 1.0000
* Recall Macro: 1.0000
* F1 Macro: 1.0000

## Analisis Singkat

Hasil evaluasi menunjukkan bahwa sistem mampu menemukan kembali kasus yang relevan berdasarkan query evaluasi. Nilai Accuracy@5 dan Recall@5 yang tinggi menunjukkan bahwa ground truth case berhasil ditemukan pada daftar top-5 hasil retrieval.

Nilai Precision@5 sebesar 0.2000 terjadi karena hanya terdapat satu ground truth case yang dianggap relevan pada setiap query, sedangkan sistem mengembalikan 5 hasil teratas. Oleh karena itu, jika ground truth ditemukan dalam top-5, precision bernilai 1/5 atau 0.2000.

Evaluasi prediksi solusi menunjukkan hasil yang tinggi karena label amar pada dataset cenderung seragam, yaitu berkaitan dengan pidana penjara pada perkara penganiayaan.

## Kesimpulan

Project ini berhasil membangun sistem Case-Based Reasoning sederhana untuk perkara Pidana Umum – Penganiayaan. Sistem mampu membangun case base dari 30 putusan, merepresentasikan kasus menjadi data terstruktur, melakukan retrieval menggunakan TF-IDF dan Cosine Similarity, menggunakan kembali solusi dari kasus lama, serta mengevaluasi hasil retrieval dan prediksi dengan metrik yang terukur.
