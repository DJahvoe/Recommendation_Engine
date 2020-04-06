# **Apache Spark Workflow**

## _Workflow_

## _Perubahan Node Input dan Node Output_

### Mengubah input file location pada File Reader

### Mengubah input file location pada CSV to Spark

### Mengubah output file location pada CSV Writer

# **Cross-Industry Standard Process for Data Mining**

## _Business Understanding_

### Kemungkinan proses yang dapat dilakukan

- Memberikan recommendation untuk user
- Mencari kemiripan antara satu user dan lainnya
- Membentuk user profile

## _Data Understanding_

### 05111740000130_movies.csv

- jumlah data pada 05111740000130_movies.csv sebanyak **270.000++ baris**
- movieId: **id** dari film
- title: **judul** dari film
- genres: **genre** dari film

### 05111740000130_ratings.csv

- jumlah data pada 05111740000130_ratings.csv sebanyak **1.000.000++ baris**
- userId: **id user** yang memberikan rating terhadap film tertentu
- movieId: **id film** yang di-rating oleh user
- rating: **rating** yang diberikan user untuk film tersebut (0 sampai 5, -1 apabila belum menonton)
- timestamp: **waktu** saat pengambilan data rating dilakukan

## _Data Preparation_

### Proses cleaning data: _Tidak dilakukan cleaning data karena dapat dipastikan seluruh data memiliki kelengkapan sebelum diproses lebih lanjut_

### Proses construct required data:

- Pembuatan user profile custom: diperlukan pembuatan user baru untuk dilakukan uji coba recommendation system, yaitu dengan menambahkan kolom userID baru ke dalam list movie yang tersedia, lalu mengambil data rating movie yang telah ditonton oleh user dalam node **Ask User for Movie Ratings**, sedangkan sisa movie yang belum dirating akan masuk ke **no rating**

- Pembagian partisi untuk training dan test set: setelah dilakukan load _05111740000130_ratings.csv_ menggunakan **CSV to Spark**, dilakukan partitioning menggunakan **Spark Partitioning** memisahkan antara training set (sebanyak 80%) dan test set (sebanyak 20%)

### Proses integrasi data:

- Sebelum data user di-training, dilakukan concatenation antara rating user yang telah tersedia dengan rating user yang baru menggunakan **Spark Concatenate**.

## _Modelling_

### Metode permodelan yang dipilih untuk mengolah data yang telah di-load menggunakan **Collaborative Filtering**, dengan asumsi data rating yang telah di-load sebelumnya dapat memberikan suatu _recommendation_ terhadap user profile yang baru dengan cara mencari kemiripan antara user profile yang baru dengan user profile yang lainnya. di dalam KNIME Environment, node yang dapat dimanfaatkan untuk mendapatkan fungsional berikut adalah **Spark Collaborative Filtering Learner (MLlib)**

## _Evaluation_

Didapatkan hasil dari modelling menggunakan **Collaborative Filtering** sebagai berikut

## _Deployment_

### Penyimpanan hasil rekomendasi

1. Setelah mendapatkan model, dilakukan prediksi data movie user baru tersebut menggunakan **Spark Predictor**
2. Didapatkan hasil prediksi di dalam **Spark to Table**
3. Hasil prediksi tersebut selanjutnya di-extract untuk mendapatkan hasil prediksi rating yang tertinggi untuk direkomendasikan ke user (data dapat berupa CSV melalui **CSV Writer**, maupun Interactive View melalui **Display Recommendation**).

# **File Reader & CSV to Spark Comparison**

- Dengan menggunakan **Timer Info** Load yang dilakukan **CSV to Spark** lebih cepat dibandingan **File Reader**
- Kemungkinan penyebab dari hal tersebut adalah untuk data yang jumlahnya relatif besar (>1.000.000 baris) dan memiliki kolom yang sedikit (<1.000 kolom) maka kecepatan load **CSV to Spark akan lebih cepat**
- Sedangkan untuk data yang jumlahnya relatif kecil (kurang lebih 5000 baris) dan memiliki kolom yang banyak (>1000) maka kecepatan load **File Reader akan lebih cepat**
