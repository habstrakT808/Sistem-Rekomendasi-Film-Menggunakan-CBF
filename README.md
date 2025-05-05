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

Dalam proyek ini, saya menggunakan dataset MovieLens Small yang dikembangkan oleh GroupLens Research. Dataset ini berisi 100,000 rating dan 3,600 tag yang diberikan oleh 600 pengguna terhadap 9,000 film. Dataset ini dapat diunduh dari [situs resmi GroupLens](https://grouplens.org/datasets/movielens/latest/).

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

Dataset ini memiliki lebih dari 100 ribu rating yang diberikan oleh 610 pengguna terhadap 9742 film. Ini menunjukkan dataset yang cukup besar dan beragam untuk mengembangkan sistem rekomendasi.

#### 2. Distribusi Rating

![rating_distribution](https://github.com/user-attachments/assets/e469416e-e2b6-4320-b177-9c0a1479826c)

Dari visualisasi di atas, terlihat bahwa distribusi rating cenderung ke arah positif, dengan mayoritas rating berada pada skala 3.0-4.0. Rating 4.0 adalah yang paling umum, diikuti oleh 3.0 dan 5.0. Ini menunjukkan bahwa pengguna cenderung memberikan rating yang baik untuk film yang mereka tonton.

#### 3. Distribusi Jumlah Rating per Film

![ratings_per_movie](https://github.com/user-attachments/assets/be752a0d-7a93-4476-9fac-111cef4aeb4e)

Grafik di atas menunjukkan bahwa sebagian besar film hanya memiliki sedikit rating (kurang dari 10), sementara hanya sedikit film yang memiliki banyak rating. Ini adalah contoh klasik dari distribusi long-tail yang umum dalam sistem rekomendasi, di mana sebagian kecil item sangat populer sementara sebagian besar item jarang diakses.

#### 4. Distribusi Jumlah Rating per Pengguna

![ratings_per_user](https://github.com/user-attachments/assets/c6145888-31db-4200-bad3-57125138080f)

Serupa dengan distribusi rating per film, distribusi rating per pengguna juga menunjukkan pola long-tail. Sebagian besar pengguna hanya memberikan sedikit rating, sementara hanya beberapa pengguna yang aktif memberikan banyak rating.

#### 5. Analisis Genre Film

![top_genres](https://github.com/user-attachments/assets/f92fddbc-d161-4c3e-a60d-87ef2c4a9578)

Genre Drama adalah yang paling umum dalam dataset, diikuti oleh Comedy dan Thriller. Ini memberikan insight tentang jenis film yang paling banyak tersedia dalam dataset.

#### 6. Film dengan Rating Tertinggi

![top_rated_movies](https://github.com/user-attachments/assets/390e4de1-a5f9-4eb0-bb85-6f530592488d)

Visualisasi di atas menunjukkan 15 film dengan rating tertinggi (dengan minimal 100 rating). Film-film klasik dan kritikal sukses seperti "The Shawshank Redemption" dan "The Godfather" mendominasi daftar ini.

## Data Preparation

Sebelum membangun model, saya melakukan beberapa tahapan persiapan data untuk memastikan kualitas dan kesesuaian data dengan algoritma yang akan digunakan:

### 1. Filtering Data

Tahap pertama adalah memfilter data untuk menghapus film dan pengguna dengan sedikit rating (kurang dari 5 rating). Tahapan ini penting untuk mengatasi masalah sparsity dan cold-start yang umum dalam sistem rekomendasi. Dengan menghapus film dan pengguna dengan sedikit rating, model dapat fokus pada pola yang lebih kuat dan reliable dalam data.

### 2. Ekstraksi Tahun dari Judul Film

Selanjutnya, saya mengekstrak tahun rilis film dari judul film. Ekstraksi tahun dari judul film memungkinkan kita untuk menggunakan informasi temporal dalam analisis dan rekomendasi. Pengguna mungkin memiliki preferensi terhadap film dari era tertentu.

### 3. Persiapan Data untuk Content-Based Filtering

Untuk Content-Based Filtering, saya menggunakan TF-IDF (Term Frequency-Inverse Document Frequency) untuk mengekstrak fitur dari genre film. Setiap film direpresentasikan sebagai vektor TF-IDF, dan kesamaan antar film dihitung menggunakan cosine similarity. Semakin tinggi nilai cosine similarity, semakin mirip kedua film tersebut berdasarkan genre.

### 4. Persiapan Data untuk Collaborative Filtering

Untuk Collaborative Filtering, data dibagi menjadi set training (80%) dan testing (20%). Kemudian, userId dan movieId dipetakan ke indeks berurutan untuk digunakan dalam model neural network. Data kemudian dikonversi ke format yang sesuai untuk TensorFlow Dataset API, yang memungkinkan loading data yang efisien selama training model.

## Modeling

Dalam proyek ini, saya mengimplementasikan dua pendekatan untuk sistem rekomendasi: Content-Based Filtering dan Collaborative Filtering.

### 1. Content-Based Filtering

Content-Based Filtering merekomendasikan item berdasarkan kesamaan fitur atau atribut item. Dalam konteks film, fitur yang digunakan adalah genre film.

#### Implementasi Model:

Model Content-Based Filtering yang diimplementasikan menggunakan TF-IDF Vectorizer untuk mengekstrak fitur dari genre film, dan cosine similarity untuk menghitung kesamaan antar film. Fungsi rekomendasi mengambil ID film sebagai input, mencari film-film yang paling mirip berdasarkan cosine similarity, dan mengembalikan daftar film yang direkomendasikan.

#### Hasil Rekomendasi:

Sebagai contoh, berikut adalah rekomendasi untuk film "Toy Story":

| movieId | title | genres |
|---------|-------|--------|
| 2294 | Antz (1998) | Adventure\|Animation\|Children\|Comedy\|Fantasy |
| 3114 | Toy Story 2 (1999) | Adventure\|Animation\|Children\|Comedy\|Fantasy |
| 3754 | Adventures of Rocky and Bullwinkle, The (2000) | Adventure\|Animation\|Children\|Comedy\|Fantasy |
| 4016 | Emperor's New Groove, The (2000) | Adventure\|Animation\|Children\|Comedy\|Fantasy |
| 4886 | Monsters, Inc. (2001) | Adventure\|Animation\|Children\|Comedy\|Fantasy |
| 45074 | Wild, The (2006) | Adventure\|Animation\|Children\|Comedy\|Fantasy |
| 53121 | Shrek the Third (2007) | Adventure\|Animation\|Children\|Comedy\|Fantasy |
| 65577 | Tale of Despereaux, The (2008) | Adventure\|Animation\|Children\|Comedy\|Fantasy |
| 91355 | Asterix and the Vikings (Astérix et les Viking...) | Adventure\|Animation\|Children\|Comedy\|Fantasy |
| 103755 | Turbo (2013) | Adventure\|Animation\|Children\|Comedy\|Fantasy |

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

Model Neural Collaborative Filtering (NCF) yang diimplementasikan menggunakan TensorFlow dengan embedding layer untuk user dan item. Model ini memiliki beberapa layer dense dengan aktivasi ReLU dan dropout untuk mencegah overfitting. Model dilatih menggunakan Mean Squared Error sebagai loss function dan Adam optimizer.

#### Hasil Rekomendasi:

Berikut adalah contoh rekomendasi untuk pengguna dengan ID 414:

## Film yang Sudah Ditonton Pengguna:

| ID | Title | Rating |
|----|-------|--------|
| 2112 | Hell or High Water (2016) | 5.0 |
| 8 | American President, The (1995) | 5.0 |
| 1030 | High Fidelity (2000) | 5.0 |
| 30 | Usual Suspects, The (1995) | 5.0 |
| 1056 | Gladiator (2000) | 5.0 |

## Rekomendasi berdasarkan Collaborative Filtering:

| Index | movieId | Title | Genres |
|-------|---------|-------|--------|
| 27 | 28 | Persuasion (1995) | *tidak tersedia* |
| 940 | 1241 | Dead Alive (Braindead) (1992) | *tidak tersedia* |
| 1759 | 2357 | Central Station (Central do Brasil) (1998) | *tidak tersedia* |
| 2582 | 3451 | Guess Who's Coming to Dinner (1967) | *tidak tersedia* |
| 2593 | 3468 | Hustler, The (1961) | *tidak tersedia* |
| 4504 | 6666 | Discreet Charm of the Bourgeoisie, The (Charme...) | *tidak tersedia* |
| 5110 | 8132 | Gladiator (1992) | Action\|Drama |
| 5621 | 27156 | Neon Genesis Evangelion: The End of Evangelion... | Action\|Animation\|Drama\|Fantasy\|Sci-Fi |
| 7815 | 92535 | Louis C.K.: Live at the Beacon Theater (2011) | Comedy |
| 9301 | 158966 | Captain Fantastic (2016) | Drama |

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
Content-Based Filtering - Precision@10: 0.0290
```

Hasil Precision@10 sebesar 0.0290 berarti bahwa sekitar 2.9% dari film yang direkomendasikan oleh model Content-Based Filtering relevan dengan preferensi pengguna. Nilai ini relatif rendah, yang menunjukkan bahwa rekomendasi berdasarkan kesamaan genre saja mungkin tidak cukup untuk menangkap preferensi pengguna yang kompleks.

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

### Visualisasi Hasil Evaluasi

![evaluation_metrics](https://github.com/user-attachments/assets/f5025920-00f3-4321-b03c-1badcfcb4214)

Dari hasil evaluasi, dapat disimpulkan bahwa model Collaborative Filtering memberikan kinerja yang lebih baik dalam memprediksi preferensi pengguna dibandingkan dengan model Content-Based Filtering. Hal ini dapat dijelaskan karena Collaborative Filtering dapat menangkap pola yang lebih kompleks dalam preferensi pengguna, tidak hanya berdasarkan kesamaan konten.

Namun, kedua pendekatan memiliki kelebihan dan kekurangan masing-masing, dan dalam praktiknya, sistem rekomendasi yang baik sering menggabungkan kedua pendekatan ini (hybrid approach) untuk mendapatkan hasil yang optimal.
