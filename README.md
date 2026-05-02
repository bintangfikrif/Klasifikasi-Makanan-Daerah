# Deep Learning (IF25-40401) Project
# Klasifikasi-Makanan-Daerah

Proyek ini dibuat sebagai bagian dari Tugas Besar Mata Kuliah Pembelajaran Mendalam (IF25-40401) di Program Studi Teknik Informatika, Institut Teknologi Sumatera. Topik utama tugas adalah membangun model yang dapat mengklasifikasi 5 jenis makanan khas Indonesia dengan baik menggunakan model *Deep Learning*.

## Daftar Anggota Kelompok

| Nama | NIM | Profil GitHub |
|------|------|----------------|
| Bintang Fikri Fauzan | 122140008 | https://github.com/bintangfikrif |
| Ferdana Al Hakim | 122140012 | https://github.com/luciferdana |
| Zidan Raihan | 122140100 | https://github.com/zidbytes |

## Dataset
Dataset Makanan Daerah terdiri dari 5 kelas masakan nusantara yaitu nasi goreng, rendang, soto ayam, bakso, dan gado gado. Dataset diperoleh dari sumber internal dan tidak dapat diperlihatkan karena sebagian memiliki potensi hak cipta pihak ketiga.

## Preprocessing & Data Augmentation
Untuk mempersiapkan data sebelum masuk ke model, dilakukan beberapa tahapan transformasi:
* **Resize**: Mengubah ukuran seluruh gambar menjadi `224x224` pixel[cite: 675, 681].
* **Data Augmentation (Training)**: Diterapkan untuk mencegah *overfitting* dan membuat model lebih kebal terhadap variasi data.
    * `RandomHorizontalFlip`: Membalik gambar secara horizontal dengan probabilitas 0.5 (50%).
    * `RandomRotation`: Memutar gambar secara acak hingga 10 derajat.
    * `ColorJitter`: Mengubah intensitas warna secara acak dengan parameter `brightness=0.2`, `contrast=0.2`, `saturation=0.2`, dan `hue=0.1`.
* **Tensor Conversion**: Mengonversi gambar menjadi PyTorch Tensor.
* **Normalization**: Menormalkan nilai pixel menggunakan parameter standar dari dataset ImageNet (`mean=[0.485, 0.456, 0.406]`, `std=[0.229, 0.224, 0.225]`).

## Model yang Digunakan
Proyek ini menguji dan membandingkan 3 arsitektur *pretrained* yang dilatih pada dataset ImageNet (`IMAGENET1K_V1`):
1.  **EfficientNet-V2-S**: Memiliki sekitar 20.18 juta parameter.
2.  **EfficientNet-B4**: Memiliki sekitar 17.56 juta parameter.
3.  **EfficientNet-B3**: Memiliki sekitar 10.70 juta parameter.

## Hyperparameters
* **Epochs**: 8.
* **Batch Size**: 32.
* **Optimizer**: AdamW dengan Initial Learning Rate `0.001` dan Weight Decay `0.01`
* **Loss Function**: Cross Entropy Loss (`nn.CrossEntropyLoss`). 
* **Learning Rate Scheduler**: Cosine Annealing LR dengan `T_max=10` dan `eta_min=1e-6`.

## Skema Evaluasi
* **Data Split**: Dataset dibagi menjadi himpunan *Training* (80%) dan *Validation* (20%).
* **Stratified Splitting**: Pembagian data menggunakan argumen `stratify` berdasarkan label. 
* **Metrik Penilaian**: Performa diukur menggunakan kalkulasi akurasi rata-rata dan loss epoch.
* **Model Checkpointing**: Selama masa *training*, model hanya akan disimpan apabilapatkan nilai *Validation Accuracy* tertinggi (`best_val_acc`)
* **Hasil Akhir**: Model terbaik diraih oleh **EfficientNet-B4** dengan *validation accuracy* sebesar 98.20%.