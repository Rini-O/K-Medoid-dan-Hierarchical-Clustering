# Analisis Perbandingan Kinerja K-Medoids dan Hierarchical Clustering

## 📌 Deskripsi

Repository ini berisi implementasi penelitian **perbandingan kinerja algoritma K-Medoids dan Hierarchical Clustering dalam pengelompokan wilayah kabupaten/kota di Provinsi Jawa Timur berdasarkan indikator kesejahteraan rakyat**.

Penelitian ini bertujuan untuk mengelompokkan kabupaten/kota di Jawa Timur berdasarkan karakteristik indikator kesejahteraan rakyat, kemudian membandingkan kualitas hasil pengelompokan dari algoritma **K-Medoids** dan **Hierarchical Clustering**.

Kinerja kedua algoritma dibandingkan menggunakan **Silhouette Score**, di mana nilai yang semakin mendekati 1 menunjukkan kualitas cluster yang semakin baik.

---

## 🎯 Tujuan Penelitian

Penelitian ini memiliki beberapa tujuan utama:

1. Mengelompokkan kabupaten/kota di Provinsi Jawa Timur berdasarkan indikator kesejahteraan rakyat.
2. Menerapkan algoritma **K-Medoids** untuk melakukan pengelompokan wilayah.
3. Menerapkan **Hierarchical Clustering** dengan metode **Ward**.
4. Membandingkan kualitas cluster yang dihasilkan oleh kedua algoritma menggunakan **Silhouette Score**.
5. Menentukan algoritma yang menghasilkan kualitas pengelompokan terbaik berdasarkan nilai Silhouette Score.

---

## 📊 Dataset

Dataset yang digunakan merupakan data indikator kesejahteraan rakyat kabupaten/kota di Provinsi Jawa Timur.

### Karakteristik Dataset

| Informasi               | Keterangan                   |
| ----------------------- | ---------------------------- |
| Wilayah                 | Provinsi Jawa Timur          |
| Unit analisis           | Kabupaten/Kota               |
| Jumlah wilayah          | 38 Kabupaten/Kota            |
| Jumlah variabel numerik | 31 indikator                 |
| Data kategorikal        | Nama Kabupaten/Kota          |
| Missing value           | Tidak terdapat missing value |

Dataset mencakup berbagai aspek kesejahteraan rakyat, antara lain:

* Kependudukan
* Kesehatan
* Pendidikan
* Ketenagakerjaan
* Ekonomi
* Kemiskinan
* Perumahan
* Sanitasi
* Air minum
* Teknologi informasi dan komunikasi

Contoh indikator yang digunakan antara lain:

* Jumlah Penduduk
* Laju Pertumbuhan
* Rasio Beban Ketergantungan
* Kepadatan Penduduk
* Jumlah Tenaga Kesehatan
* Fasilitas Kesehatan
* Angka Kesakitan
* Umur Harapan Hidup
* Angka Harapan Sekolah
* Rata-rata Lama Sekolah
* TPAK
* TPT
* Gini Rasio
* Penduduk Miskin
* Sanitasi Layak
* Air Minum Layak
* Rumah Layak Huni
* Persentase Pengguna Internet

---

## 🔬 Metodologi

Tahapan penelitian yang dilakukan adalah:

```text
Dataset
   ↓
Data Cleaning
   ↓
Pengecekan Missing Value
   ↓
Standardisasi Data
   ↓
Uji Multikolinearitas (VIF)
   ↓
PCA
   ↓
Menentukan Jumlah Cluster
   ↓
┌───────────────────────┐
│                       │
│     K-Medoids         │
│                       │
└───────────┬───────────┘
            │
            ├──────→ Silhouette Score
            │
┌───────────┴───────────┐
│                       │
│ Hierarchical          │
│ Clustering - Ward     │
│                       │
└───────────┬───────────┘
            │
            ├──────→ Silhouette Score
            │
            ↓
    Perbandingan Kinerja
```

---

## 1. Data Cleaning

Data dibaca menggunakan Pandas kemudian dilakukan pembersihan nama kolom dan penghapusan kolom yang tidak diperlukan.

Kolom **Kabupaten/Kota** digunakan sebagai identitas wilayah, sedangkan variabel numerik digunakan sebagai atribut untuk proses clustering.

Hasil pengecekan menunjukkan bahwa dataset tidak memiliki missing value.

---

## 2. Standardisasi Data

Karena setiap indikator memiliki satuan dan skala yang berbeda, dilakukan standardisasi menggunakan **StandardScaler**.

```python
scaler = StandardScaler()

X_numeric = X.select_dtypes(include=np.number)

X_scaled = scaler.fit_transform(X_numeric)

X_scaled = pd.DataFrame(
    X_scaled,
    columns=X_numeric.columns
)
```

Standardisasi menghasilkan data dengan skala yang seragam sehingga setiap indikator dapat digunakan secara lebih seimbang dalam proses clustering.

---

## 3. Uji Multikolinearitas

Uji multikolinearitas dilakukan menggunakan **Variance Inflation Factor (VIF)** untuk mengidentifikasi hubungan linear yang tinggi antarvariabel.

Tahapan ini dilakukan sebelum PCA untuk membantu mengurangi permasalahan redundansi informasi pada indikator.

---

## 4. Principal Component Analysis (PCA)

PCA digunakan untuk mereduksi dimensi data sebelum proses clustering.

Berdasarkan hasil analisis, digunakan **6 Principal Components (PC)**. Keenam komponen tersebut menjelaskan variasi data dengan kontribusi:

| Komponen | Variansi |
| -------- | -------: |
| PC1      |   35.48% |
| PC2      |   14.66% |
| PC3      |   12.60% |
| PC4      |    8.60% |
| PC5      |    5.87% |
| PC6      |    3.65% |

Total variasi yang dijelaskan oleh keenam komponen tersebut adalah sekitar **80,86%**.

Data hasil PCA kemudian digunakan sebagai input untuk kedua algoritma clustering sehingga perbandingan dilakukan pada representasi data yang sama.

---

# 🤖 Algoritma Clustering

## 5. K-Medoids

K-Medoids merupakan algoritma clustering berbasis partisi yang menggunakan **medoid**, yaitu objek aktual dari dataset yang menjadi representasi suatu cluster.

Pada penelitian ini, jumlah cluster yang digunakan adalah:

```text
K = 3
```

Hasil pengelompokan K-Medoids menghasilkan:

| Cluster   | Jumlah Wilayah |
| --------- | -------------: |
| Cluster 1 |             21 |
| Cluster 2 |              8 |
| Cluster 3 |              9 |
| **Total** |         **38** |

---

## 6. Hierarchical Clustering

Hierarchical Clustering digunakan sebagai algoritma pembanding dengan pendekatan agglomerative.

Metode linkage yang digunakan adalah **Ward**.

```python
Z = linkage(
    X_for_clustering,
    method='ward'
)
```

Dendrogram digunakan untuk melihat struktur hierarki dan menentukan jumlah cluster.

Jumlah cluster yang digunakan:

```text
K = 3
```

Hasil pengelompokan Hierarchical Clustering:

| Cluster   | Jumlah Wilayah |
| --------- | -------------: |
| Cluster 1 |             11 |
| Cluster 2 |             19 |
| Cluster 3 |              8 |
| **Total** |         **38** |

---

# 📏 Evaluasi Clustering

Evaluasi kualitas cluster dilakukan menggunakan **Silhouette Score**.

Silhouette Score mengukur seberapa baik suatu objek berada di dalam cluster dibandingkan dengan cluster lainnya.

Nilai yang lebih tinggi menunjukkan bahwa objek dalam cluster memiliki kemiripan yang lebih baik dan pemisahan antarcluster lebih jelas.

---

## 📊 Hasil Perbandingan

| Algoritma               | Silhouette Score |
| ----------------------- | ---------------: |
| **K-Medoids**           |       **0.3608** |
| Hierarchical Clustering |           0.3353 |

Hasil penelitian menunjukkan bahwa **K-Medoids menghasilkan Silhouette Score sebesar 0.3608**, sedangkan **Hierarchical Clustering menghasilkan nilai 0.3353**.

Dengan demikian, berdasarkan evaluasi Silhouette Score, **K-Medoids menghasilkan kualitas pengelompokan yang lebih baik dibandingkan Hierarchical Clustering** pada dataset indikator kesejahteraan rakyat Provinsi Jawa Timur. Notebook juga mencatat K-Medoids sebagai algoritma terbaik berdasarkan nilai Silhouette Score.

### Kesimpulan Perbandingan

```text
K-Medoids
Silhouette Score = 0.3608
        ↑
        │
        │  lebih tinggi
        │
Hierarchical Clustering
Silhouette Score = 0.3353
```

Selisih Silhouette Score:

```text
0.3608 - 0.3353 = 0.0255
```

Artinya, pada penelitian ini **K-Medoids memiliki kualitas cluster yang sedikit lebih baik** berdasarkan Silhouette Score.

---

# 🗺️ Interpretasi Cluster

Hasil clustering dapat digunakan untuk melihat karakteristik masing-masing kelompok wilayah berdasarkan indikator kesejahteraan rakyat.

Interpretasi cluster dilakukan berdasarkan **rata-rata nilai variabel asli pada setiap cluster**, bukan berdasarkan data yang telah melalui standardisasi. Pendekatan ini digunakan agar karakteristik masing-masing cluster dapat diinterpretasikan kembali dalam satuan indikator aslinya.

Label seperti:

* **Tinggi**
* **Sedang**
* **Rendah**

ditentukan berdasarkan karakteristik rata-rata indikator pada masing-masing cluster.

> Catatan: nomor cluster dari algoritma tidak secara otomatis menunjukkan tingkat kesejahteraan. Penamaan cluster dilakukan setelah melihat karakteristik indikator pada masing-masing cluster.

---

# 📈 Visualisasi

Repository ini menyediakan beberapa bentuk visualisasi, antara lain:

* Dendrogram Hierarchical Clustering
* Visualisasi hasil clustering
* Visualisasi PCA
* Perbandingan Silhouette Score
* Peta persebaran cluster kabupaten/kota

Visualisasi digunakan untuk membantu melihat pola pengelompokan wilayah dan membandingkan hasil kedua algoritma.

---

# 🛠️ Teknologi yang Digunakan

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Scikit-learn
* Scikit-learn-extra
* SciPy
* Statsmodels
* Google Colab / Jupyter Notebook

Library utama yang digunakan dalam implementasi meliputi `StandardScaler`, `PCA`, `silhouette_score`, `KMedoids`, `dendrogram`, `linkage`, `fcluster`, dan `variance_inflation_factor`.

---

# 📁 Struktur Repository

```text
├── README.md
├── Kesejahteraan_Rakyat_Jatim_FIXED.ipynb
├── data/
│   └── Indikator_Kesejahteraan_Rakyat_Jatim.csv
│
├── hasil/
│   ├── rata_rata_kmedoids_per_cluster.xlsx
│   └── rata_rata_hierarchical_per_cluster.xlsx
│
└── visualisasi/
    ├── dendrogram.png
    ├── clustering_kmedoids.png
    ├── clustering_hierarchical.png
    └── silhouette_comparison.png
```

> Nama file pada bagian struktur repository dapat disesuaikan dengan file yang sebenarnya digunakan pada repository GitHub.

---

# 🚀 Cara Menjalankan

### 1. Clone Repository

```bash
git clone https://github.com/username/nama-repository.git
```

### 2. Masuk ke Folder Repository

```bash
cd nama-repository
```

### 3. Install Library

```bash
pip install numpy pandas matplotlib seaborn scikit-learn scipy statsmodels scikit-learn-extra
```

### 4. Jalankan Notebook

Buka:

```text
Kesejahteraan_Rakyat_Jatim_FIXED.ipynb
```

Notebook dapat dijalankan menggunakan:

* Google Colab
* Jupyter Notebook
* JupyterLab

Pastikan file dataset tersedia pada lokasi yang sesuai dengan path yang digunakan pada notebook.

---

# 📌 Ringkasan Penelitian

Penelitian ini membandingkan dua pendekatan clustering, yaitu **K-Medoids** dan **Hierarchical Clustering metode Ward**, untuk mengelompokkan 38 kabupaten/kota di Provinsi Jawa Timur berdasarkan indikator kesejahteraan rakyat.

Data diproses melalui standardisasi, pemeriksaan multikolinearitas, dan reduksi dimensi menggunakan PCA. Selanjutnya, kedua algoritma diterapkan menggunakan **3 cluster** pada data hasil PCA.

Berdasarkan evaluasi menggunakan Silhouette Score, K-Medoids memperoleh nilai **0.3608**, sedangkan Hierarchical Clustering memperoleh **0.3353**. Dengan demikian, **K-Medoids menjadi algoritma dengan kinerja lebih baik pada penelitian ini berdasarkan nilai Silhouette Score**.

---

# 👩‍💻 Author

**Rini Fatmawati**

Program Studi Sistem Informasi
Universitas Islam Negeri Sunan Ampel Surabaya

---

## 📚 Catatan

Repository ini dibuat sebagai bagian dari penelitian akademik mengenai **data mining dan clustering wilayah berdasarkan indikator kesejahteraan rakyat di Provinsi Jawa Timur**.
