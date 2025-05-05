# Laporan Proyek Machine Learning - Sistem Rekomendasi Film

## Domain Proyek

Industri hiburan, khususnya platform streaming film dan serial TV, telah mengalami pertumbuhan yang signifikan dalam beberapa tahun terakhir. Menurut laporan dari Grand View Research, pasar global video streaming diperkirakan mencapai nilai USD 223,98 miliar pada tahun 2028 [1]. Pertumbuhan ini didorong oleh meningkatnya penggunaan smartphone, ketersediaan internet berkecepatan tinggi, dan perubahan preferensi konsumen dari media tradisional ke platform digital.

Dengan jutaan konten yang tersedia di platform streaming seperti Netflix, Amazon Prime, dan Disney+, pengguna sering kali menghadapi tantangan yang disebut "choice overload" atau kelebihan pilihan. Menurut studi yang dilakukan oleh Nielsen, rata-rata pengguna menghabiskan 7,4 menit hanya untuk mencari konten yang ingin ditonton [2]. Fenomena ini dapat menyebabkan pengalaman pengguna yang buruk dan berpotensi mengakibatkan churn rate (tingkat pengguna yang berhenti berlangganan) yang tinggi.

Sistem rekomendasi film menjadi solusi penting untuk mengatasi masalah ini. Dengan memanfaatkan data historis pengguna dan teknik machine learning, sistem rekomendasi dapat memprediksi preferensi pengguna dan menyarankan konten yang relevan. Netflix melaporkan bahwa 80% dari streaming di platform mereka berasal dari rekomendasi personalisasi [3], yang menunjukkan pentingnya sistem rekomendasi yang efektif dalam meningkatkan engagement pengguna.

Proyek ini berfokus pada pengembangan sistem rekomendasi film menggunakan dataset MovieLens, yang berisi rating film dari pengguna nyata. Dengan mengimplementasikan teknik Content-based Filtering dan Collaborative Filtering, proyek ini bertujuan untuk memberikan rekomendasi film yang personal dan relevan kepada pengguna, sehingga meningkatkan pengalaman pengguna dan potensial engagement pada platform film.

**Referensi:**

1. Grand View Research, "Video Streaming Market Size Report, 2021-2028," 2021.
2. Nielsen, "The Nielsen Total Audience Report," Q2 2019.
3. C. A. Gomez-Uribe and N. Hunt, "The Netflix Recommender System: Algorithms, Business Value, and Innovation," ACM Transactions on Management Information Systems, vol. 6, no. 4, pp. 1-19, 2015.

## Business Understanding

### Problem Statements

Berdasarkan latar belakang di atas, berikut adalah beberapa permasalahan yang perlu diselesaikan dalam proyek ini:

1. Bagaimana cara mengatasi masalah "choice overload" yang dihadapi pengguna ketika mencari film untuk ditonton di antara ribuan pilihan yang tersedia?
2. Bagaimana mengembangkan sistem rekomendasi yang dapat memprediksi preferensi pengguna dengan akurat berdasarkan riwayat tontonan mereka?
3. Bagaimana meningkatkan pengalaman pengguna dengan memberikan rekomendasi film yang personal dan relevan?

### Goals

Tujuan dari proyek ini adalah:

1. Mengembangkan sistem rekomendasi film yang dapat membantu pengguna menemukan film yang sesuai dengan preferensi mereka di antara ribuan pilihan yang tersedia.
2. Membangun model yang dapat memprediksi preferensi pengguna berdasarkan riwayat rating film yang mereka berikan.
3. Meningkatkan pengalaman pengguna dengan memberikan rekomendasi film yang personal dan relevan, sehingga mengurangi waktu pencarian dan meningkatkan tingkat kepuasan.

### Solution Statements

Untuk mencapai tujuan di atas, berikut adalah solusi yang akan diimplementasikan:

1. **Content-based Filtering**:

- Mengembangkan sistem rekomendasi berbasis konten yang merekomendasikan film berdasarkan kemiripan fitur/atribut film (genre).
- Menggunakan TF-IDF Vectorizer untuk mengekstrak fitur dari genre film dan menghitung cosine similarity antar film.
- Solusi ini akan diukur menggunakan metrik Precision@K untuk mengevaluasi relevansi rekomendasi.

2. **Collaborative Filtering**:

- Mengembangkan sistem rekomendasi berbasis neural network yang mempelajari pola rating pengguna.
- Menggunakan model Neural Collaborative Filtering (NCF) dengan embedding layer untuk user dan item.
- Solusi ini akan diukur menggunakan metrik Root Mean Squared Error (RMSE) untuk mengevaluasi akurasi prediksi rating.

## Data Understanding

Dalam proyek ini, saya menggunakan dataset MovieLens Small yang dikembangkan oleh GroupLens Research. Dataset ini berisi 100,836 rating dan tag yang diberikan oleh 610 pengguna terhadap 9,742 film. Dataset ini dapat diunduh dari [situs resmi GroupLens](https://grouplens.org/datasets/movielens/latest/).

Dataset MovieLens Small terdiri dari beberapa file, namun dalam proyek ini saya fokus pada empat file utama:

### Variabel-variabel pada dataset MovieLens:

1. **ratings.csv**: Berisi rating yang diberikan pengguna terhadap film

- `userId`: ID unik untuk setiap pengguna
- `movieId`: ID unik untuk setiap film
- `rating`: Rating yang diberikan (skala 0.5-5.0)
- `timestamp`: Waktu rating diberikan (format unix timestamp)

2. **movies.csv**: Berisi informasi tentang film

- `movieId`: ID unik untuk setiap film
- `title`: Judul film (termasuk tahun rilis)
- `genres`: Genre film (dipisahkan dengan "|")

3. **tags.csv**: Berisi tag yang diberikan pengguna terhadap film

- `userId`: ID unik untuk setiap pengguna
- `movieId`: ID unik untuk setiap film
- `tag`: Tag teks yang diberikan oleh pengguna
- `timestamp`: Waktu tag diberikan (format unix timestamp)

4. **links.csv**: Berisi tautan ke sumber eksternal

- `movieId`: ID unik untuk setiap film
- `imdbId`: ID film di Internet Movie Database (IMDb)
- `tmdbId`: ID film di The Movie Database (TMDb)

### Exploratory Data Analysis

Untuk memahami data dengan lebih baik, saya melakukan beberapa analisis eksplorasi:

#### 1. Statistik Dasar Dataset

Dataset ratings memiliki 100,836 baris dengan 4 kolom (userId, movieId, rating, timestamp). Dataset movies memiliki 9,742 baris dengan 3 kolom (movieId, title, genres). Dari statistik deskriptif, kita dapat melihat bahwa rating rata-rata adalah 3.5 dengan standar deviasi 1.04, yang menunjukkan bahwa sebagian besar rating cenderung positif.

#### 2. Distribusi Rating

Dari analisis distribusi rating, terlihat bahwa distribusi rating cenderung ke arah positif, dengan mayoritas rating berada pada skala 3.0-4.0. Rating 4.0 adalah yang paling umum, diikuti oleh 3.0 dan 5.0. Ini menunjukkan bahwa pengguna cenderung memberikan rating yang baik untuk film yang mereka tonton.

![rating_distribution](https://github.com/user-attachments/assets/88582fa5-5b2d-4224-9748-c6e487158701)

#### 3. Distribusi Jumlah Rating per Film

Analisis menunjukkan bahwa sebagian besar film hanya memiliki sedikit rating (kurang dari 10), sementara hanya sedikit film yang memiliki banyak rating. Ini adalah contoh klasik dari distribusi long-tail yang umum dalam sistem rekomendasi, di mana sebagian kecil item sangat populer sementara sebagian besar item jarang diakses.

![ratings_per_movie](https://github.com/user-attachments/assets/a445fadb-4ed4-460b-85bd-09220dd8e853)

#### 4. Distribusi Jumlah Rating per Pengguna

Serupa dengan distribusi rating per film, distribusi rating per pengguna juga menunjukkan pola long-tail. Sebagian besar pengguna hanya memberikan sedikit rating, sementara hanya beberapa pengguna yang aktif memberikan banyak rating.

![ratings_per_user](https://github.com/user-attachments/assets/4f93496c-ea38-49fd-8243-48579981299f)

#### 5. Analisis Genre Film

Dari analisis genre film, terlihat bahwa genre Drama adalah yang paling umum dalam dataset, diikuti oleh Comedy dan Thriller. Ini memberikan insight tentang jenis film yang paling banyak tersedia dalam dataset.

![top_genres](https://github.com/user-attachments/assets/8be38295-439f-411b-b1b5-2a27a3663254)

#### 6. Film dengan Rating Tertinggi

Analisis film dengan rating tertinggi (dengan minimal 100 rating) menunjukkan bahwa film-film klasik dan kritikal sukses mendominasi daftar ini. Informasi ini dapat digunakan untuk memberikan rekomendasi populer kepada pengguna baru.

![top_rated_movies](https://github.com/user-attachments/assets/88537ef5-4af3-4a89-9adf-43c867c40455)

## Data Preparation

Sebelum membangun model, saya melakukan beberapa tahapan persiapan data untuk memastikan kualitas dan kesesuaian data dengan algoritma yang akan digunakan:

### 1. Filtering Data

Tahap pertama adalah memfilter data untuk menghapus film dan pengguna dengan sedikit rating (kurang dari 5 rating). Tahapan ini penting untuk mengatasi masalah sparsity dan cold-start yang umum dalam sistem rekomendasi. Dengan menghapus film dan pengguna dengan sedikit rating, model dapat fokus pada pola yang lebih kuat dan reliable dalam data.

Setelah filtering, jumlah rating berkurang dari 100,836 menjadi 90,274, menunjukkan bahwa sekitar 10% data dihapus karena tidak memenuhi kriteria minimum.

### 2. Ekstraksi Tahun dari Judul Film

Selanjutnya, saya mengekstrak tahun rilis film dari judul film menggunakan ekspresi reguler. Ekstraksi tahun dari judul film memungkinkan kita untuk menggunakan informasi temporal dalam analisis dan rekomendasi. Pengguna mungkin memiliki preferensi terhadap film dari era tertentu.

### 3. Persiapan Data untuk Content-Based Filtering

Untuk Content-Based Filtering, saya menggunakan TF-IDF (Term Frequency-Inverse Document Frequency) untuk mengekstrak fitur dari genre film. Langkah-langkah persiapan data meliputi:

- Mengisi nilai NaN pada kolom genres dengan string kosong
- Mengubah format genre dari pipe-separated (`|`) menjadi space-separated untuk diproses oleh TF-IDF Vectorizer
- Membuat matriks TF-IDF dari genre film
- Menghitung cosine similarity antar film berdasarkan matriks TF-IDF
- Membuat mapping dari movieId ke indeks dan sebaliknya untuk mempermudah pencarian film

### 4. Persiapan Data untuk Collaborative Filtering

Untuk Collaborative Filtering, data dibagi menjadi set training (80%) dan testing (20%) menggunakan fungsi train_test_split dari scikit-learn. Kemudian, userId dan movieId dipetakan ke indeks berurutan untuk digunakan dalam model neural network. Langkah-langkah persiapan data meliputi:

- Membuat mapping dari userId dan movieId ke indeks berurutan
- Mengkonversi data rating ke format yang sesuai untuk model, yaitu pasangan (user, movie, rating)
- Membuat fungsi untuk mengubah data menjadi format TensorFlow Dataset yang efisien untuk training

## Modeling

Dalam proyek ini, saya mengimplementasikan dua pendekatan untuk sistem rekomendasi: Content-Based Filtering dan Collaborative Filtering.

### 1. Content-Based Filtering

Content-Based Filtering merekomendasikan item berdasarkan kesamaan fitur atau atribut item. Dalam konteks film, fitur yang digunakan adalah genre film.

#### Implementasi Model:

Model Content-Based Filtering yang diimplementasikan menggunakan TF-IDF Vectorizer untuk mengekstrak fitur dari genre film, dan cosine similarity untuk menghitung kesamaan antar film. 

Langkah-langkah implementasi:

1. Membuat matriks TF-IDF dari genre film
2. Menghitung cosine similarity antar film
3. Membuat fungsi rekomendasi yang mengambil ID film sebagai input, mencari film-film yang paling mirip berdasarkan cosine similarity, dan mengembalikan daftar film yang direkomendasikan

Fungsi `get_recommendations_content_based` mengambil ID film sebagai input dan mengembalikan daftar film yang paling mirip berdasarkan genre. Fungsi ini bekerja dengan mencari indeks film dalam matriks cosine similarity, kemudian mengurutkan semua film berdasarkan nilai kesamaan, dan mengembalikan n film teratas (tidak termasuk film itu sendiri).

#### Hasil Rekomendasi:
Sebagai contoh, berikut adalah rekomendasi untuk film "Toy Story":

| movieId | title | genres |
|---------|-------|--------|
| 2294 | Antz (1998) | Adventure\Animation\Children\Comedy\Fantasy |
| 3114 | Toy Story 2 (1999) | Adventure\Animation\Children\Comedy\Fantasy |
| 3754 | Adventures of Rocky and Bullwinkle, The (2000) | Adventure\Animation\Children\Comedy\Fantasy |
| 4016 | Emperor's New Groove, The (2000) | Adventure\Animation\Children\Comedy\Fantasy |
| 4886 | Monsters, Inc. (2001) | Adventure\Animation\Children\Comedy\Fantasy |
| 45074 | Wild, The (2006) | Adventure\Animation\Children\Comedy\Fantasy |
| 53121 | Shrek the Third (2007) | Adventure\Animation\Children\Comedy\Fantasy |
| 65577 | Tale of Despereaux, The (2008) | Adventure\Animation\Children\Comedy\Fantasy |
| 91355 | Asterix and the Vikings (Astérix et les Viking...) | Adventure\Animation\Children\Comedy\Fantasy |
| 103755 | Turbo (2013) | Adventure\Animation\Children\Comedy\Fantasy |

Dari hasil rekomendasi, terlihat bahwa model berhasil merekomendasikan film-film dengan genre yang sama dengan "Toy Story", yaitu film animasi petualangan untuk anak-anak dengan unsur komedi dan fantasi.

#### Kelebihan Content-Based Filtering:

- Tidak memerlukan data dari pengguna lain, sehingga dapat mengatasi masalah cold-start untuk item baru
- Dapat memberikan rekomendasi untuk item yang belum populer (long-tail)
- Dapat memberikan penjelasan yang transparan tentang mengapa item tersebut direkomendasikan

#### Kekurangan Content-Based Filtering:

- Terbatas pada fitur yang tersedia (dalam kasus ini hanya genre)
- Tidak dapat mempelajari preferensi pengguna yang kompleks
- Cenderung merekomendasikan item yang serupa, sehingga kurang memberikan variasi (filter bubble)

### 2. Collaborative Filtering

Collaborative Filtering merekomendasikan item berdasarkan pola rating dari pengguna lain. Pendekatan ini mengasumsikan bahwa pengguna yang memiliki preferensi serupa di masa lalu akan memiliki preferensi serupa di masa depan.

#### Implementasi Model:

Model Neural Collaborative Filtering (NCF) yang diimplementasikan menggunakan TensorFlow dengan embedding layer untuk user dan item. Arsitektur model terdiri dari:

1. Input layer untuk user dan movie ID
2. Embedding layer untuk user dan movie (ukuran embedding = 50)
3. Flatten layer untuk mengubah embedding menjadi vektor
4. Concatenate layer untuk menggabungkan embedding user dan movie
5. Dense layer (128 unit) dengan aktivasi ReLU dan dropout (0.2)
6. Dense layer (64 unit) dengan aktivasi ReLU dan dropout (0.2)
7. Output layer (1 unit) untuk memprediksi rating

Model dilatih menggunakan Mean Squared Error sebagai loss function dan Adam optimizer dengan learning rate 0.001. Untuk mencegah overfitting, digunakan early stopping dengan memonitor validation loss dan patience 5 epochs.

#### Hasil Pelatihan:

Model dilatih selama 8 epoch (berhenti karena early stopping) dengan hasil akhir:

- Training loss: 0.5409
- Validation loss: 0.7529

Grafik loss selama training menunjukkan bahwa model konvergen dengan cepat dan mulai overfitting setelah epoch ke-4, yang ditandai dengan validation loss yang mulai meningkat sementara training loss terus menurun.

![model_loss](https://github.com/user-attachments/assets/d0ae9ebf-d415-48e8-a07e-7d8a821686eb)

#### Hasil Rekomendasi:
Berikut adalah contoh rekomendasi untuk pengguna dengan ID 414:

**Film yang sudah ditonton pengguna (dengan rating 5.0):**
| Judul Film | Tahun |
|------------|-------|
| Hell or High Water | 2016 |
| American President, The | 1995 |
| High Fidelity | 2000 |
| Usual Suspects, The | 1995 |
| Gladiator | 2000 |

**Rekomendasi berdasarkan Collaborative Filtering:**
| Judul Film | Genre |
|------------|-------|
| Persuasion (1995) | Drama\|Romance |
| Dead Alive (Braindead) (1992) | Comedy\|Fantasy\|Horror |
| Central Station (Central do Brasil) (1998) | Drama |
| Guess Who's Coming to Dinner (1967) | Drama |
| Hustler, The (1961) | Drama |
| Discreet Charm of the Bourgeoisie, The (Charme...) | Comedy\|Drama\|Fantasy |
| Gladiator (1992) | Action\|Drama |
| Neon Genesis Evangelion: The End of Evangelion... | Action\|Animation\|Drama\|Fantasy\|Sci-Fi |
| Louis C.K.: Live at the Beacon Theater (2011) | Comedy |
| Captain Fantastic (2016) | Drama |

Dari hasil rekomendasi, terlihat bahwa model merekomendasikan beragam film dengan genre yang bervariasi, tidak hanya terfokus pada satu genre seperti pada Content-Based Filtering. Ini menunjukkan bahwa model berhasil mempelajari preferensi pengguna yang lebih kompleks.

#### Kelebihan Collaborative Filtering:

- Dapat menemukan pola yang kompleks dan tidak eksplisit dalam preferensi pengguna
- Tidak memerlukan informasi tentang item (fitur)
- Dapat merekomendasikan item yang mungkin tidak mirip secara konten tetapi disukai oleh pengguna serupa

#### Kekurangan Collaborative Filtering:

- Menghadapi masalah cold-start untuk pengguna baru dan item baru
- Memerlukan jumlah data yang cukup besar untuk membuat rekomendasi yang akurat
- Sulit menjelaskan mengapa item tertentu direkomendasikan (black box)

## Evaluation

Untuk mengevaluasi kinerja model sistem rekomendasi, saya menggunakan metrik evaluasi yang berbeda untuk masing-masing pendekatan:

### 1. Content-Based Filtering - Precision@K

Untuk Content-Based Filtering, saya menggunakan metrik Precision@K untuk mengevaluasi relevansi rekomendasi. Precision@K mengukur proporsi item yang relevan di antara K item teratas yang direkomendasikan.

#### Formula Precision@K:

```javascript
Precision@K = (Jumlah item relevan di antara K rekomendasi) / K
```

Dalam implementasi evaluasi, saya menggunakan pendekatan berikut:

1. Pilih pengguna yang memiliki cukup banyak rating (minimal 20 rating)
2. Untuk setiap pengguna, pilih satu film yang disukai (rating ≥ 4) sebagai dasar rekomendasi
3. Dapatkan rekomendasi berdasarkan film tersebut
4. Hitung Precision@K sebagai proporsi film yang direkomendasikan yang juga disukai oleh pengguna

#### Hasil Evaluasi Content-Based Filtering:

```javascript
Content-Based Filtering - Precision@10: 0.0440
```

Hasil Precision@10 sebesar 0.0440 berarti bahwa sekitar 4.4% dari film yang direkomendasikan oleh model Content-Based Filtering relevan dengan preferensi pengguna. Nilai ini relatif rendah, yang menunjukkan bahwa rekomendasi berdasarkan kesamaan genre saja mungkin tidak cukup untuk menangkap preferensi pengguna yang kompleks.

### 2. Collaborative Filtering - RMSE

Untuk Collaborative Filtering, saya menggunakan Root Mean Squared Error (RMSE) untuk mengevaluasi akurasi prediksi rating. RMSE mengukur seberapa jauh prediksi rating menyimpang dari rating sebenarnya.

#### Formula RMSE:

```javascript
RMSE = sqrt(1/n * Σ(y_true - y_pred)²)
```

di mana:

- n adalah jumlah prediksi
- y_true adalah rating sebenarnya
- y_pred adalah rating yang diprediksi

#### Hasil Evaluasi Collaborative Filtering:

```javascript
Collaborative Filtering - RMSE: 0.8485
```

Hasil RMSE sebesar 0.8485 menunjukkan bahwa rata-rata prediksi rating menyimpang sekitar 0.85 poin dari rating sebenarnya. Mengingat skala rating adalah 0.5-5.0, error ini relatif kecil (sekitar 17% dari rentang skala), yang menunjukkan bahwa model Collaborative Filtering cukup akurat dalam memprediksi preferensi pengguna.

### Perbandingan dan Analisis Hasil

Dari hasil evaluasi, dapat disimpulkan bahwa model Collaborative Filtering memberikan kinerja yang lebih baik dalam memprediksi preferensi pengguna dibandingkan dengan model Content-Based Filtering. Hal ini dapat dijelaskan karena Collaborative Filtering dapat menangkap pola yang lebih kompleks dalam preferensi pengguna, tidak hanya berdasarkan kesamaan konten.

RMSE 0.8485 untuk Collaborative Filtering menunjukkan akurasi prediksi yang cukup baik, mengingat skala rating 0.5-5.0. Precision@10 sebesar 0.0440 untuk Content-Based Filtering menunjukkan bahwa meskipun rekomendasi berdasarkan genre dapat memberikan beberapa hasil yang relevan, namun pendekatan ini memiliki keterbatasan dalam menangkap preferensi pengguna yang kompleks.

Namun, kedua pendekatan memiliki kelebihan dan kekurangan masing-masing, dan dalam praktiknya, sistem rekomendasi yang baik sering menggabungkan kedua pendekatan ini (hybrid approach) untuk mendapatkan hasil yang optimal.

![evaluation_metrics](https://github.com/user-attachments/assets/ae448bfb-1054-47e2-b016-c17c6a2ed96b)

## Kesimpulan

Dalam proyek ini, saya telah berhasil mengimplementasikan dua pendekatan sistem rekomendasi film: Content-Based Filtering dan Collaborative Filtering.

Content-Based Filtering berhasil merekomendasikan film berdasarkan kesamaan genre, yang berguna untuk memberikan rekomendasi yang transparan dan mengatasi masalah cold-start untuk item baru. Namun, pendekatan ini memiliki keterbatasan dalam menangkap preferensi pengguna yang kompleks, yang tercermin dari nilai Precision@10 yang relatif rendah (0.0440).

Collaborative Filtering dengan model Neural Collaborative Filtering berhasil mempelajari pola preferensi pengguna yang kompleks dan memberikan rekomendasi yang lebih personal. Model ini mencapai RMSE 0.8485, yang menunjukkan akurasi prediksi yang cukup baik.

Untuk pengembangan lebih lanjut, beberapa saran yang dapat dipertimbangkan:

1. Mengembangkan sistem rekomendasi hybrid yang menggabungkan kelebihan dari kedua pendekatan
2. Memperkaya fitur film dengan menambahkan informasi seperti aktor, sutradara, atau sinopsis
3. Mengimplementasikan teknik deep learning yang lebih canggih seperti attention mechanism atau graph neural networks
4. Menambahkan fitur kontekstual seperti waktu, lokasi, atau perangkat yang digunakan pengguna
5. Mengembangkan strategi untuk mengatasi masalah cold-start, seperti content-boosted collaborative filtering

Dengan implementasi sistem rekomendasi film yang efektif, platform streaming dapat meningkatkan pengalaman pengguna, mengurangi waktu pencarian, dan pada akhirnya meningkatkan engagement dan retensi pengguna.
