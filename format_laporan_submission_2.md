# Laporan Proyek Machine Learning - MC172D5X1392 Richelle Vania Thionanda

## Project Overview

Dalam era digital saat ini, pengguna dihadapkan pada banjir informasi yang dapat menyebabkan kelelahan dalam pengambilan keputusan. Sistem rekomendasi muncul sebagai solusi penting untuk menyaring informasi dan memberikan saran yang relevan, sehingga meningkatkan pengalaman pengguna dan keterlibatan mereka. Menurut Fan et al. (2022), sistem rekomendasi membantu pengguna membuat keputusan yang tepat secara efektif dan efisien dengan memberikan saran yang dipersonalisasi dalam berbagai aspek kehidupan, terutama untuk layanan online yang berorientasi pada manusia seperti platform e-commerce dan media sosial. ([arXiv][1])

Dalam industri hiburan, seperti layanan streaming film, sistem rekomendasi memainkan peran krusial dalam meningkatkan keterlibatan pengguna. Salunke dan Nichite (2022) menyatakan bahwa sistem rekomendasi dalam e-commerce menjadi semakin penting di dunia digital saat ini karena digunakan untuk mempersonalisasi pengalaman pengguna, membantu pelanggan menemukan apa yang mereka butuhkan dengan cepat dan efisien, serta meningkatkan pendapatan bisnis. ([arXiv][2])

Proyek ini bertujuan untuk membangun sistem rekomendasi film menggunakan dataset MovieLens 20M, yang berisi lebih dari 20 juta interaksi antara pengguna dan film. Dataset ini telah digunakan secara luas dalam penelitian sistem rekomendasi dan disediakan oleh GroupLens Research, University of Minnesota. Menurut Beel (2019), analisis empiris mengonfirmasi bahwa MovieLens adalah dataset standar de facto dalam penelitian sistem rekomendasi, dengan 40% dari makalah penuh dan pendek di Konferensi ACM RecSys 2017 dan 2018 menggunakan variasi dari dataset MovieLens. ([ISG Siegen][3])

Untuk mengatasi tantangan dalam memberikan rekomendasi yang relevan, proyek ini akan mengembangkan dua pendekatan utama:

1. **Content-Based Filtering**: Menggunakan TF-IDF vectorization pada genre film dan menghitung cosine similarity antar film untuk menghasilkan rekomendasi berdasarkan kemiripan konten.

2. **Collaborative Filtering**: Menggunakan matrix factorization (SVD) pada data rating pengguna untuk mengidentifikasi pola preferensi dan memberikan rekomendasi berbasis interaksi pengguna-item.

Dengan membandingkan performa kedua pendekatan ini, proyek ini bertujuan untuk mengevaluasi efektivitas masing-masing metode dalam konteks rekomendasi film, serta mengidentifikasi potensi kombinasi keduanya untuk meningkatkan akurasi dan relevansi rekomendasi.

---

**Referensi:**

* Fan, W., Zhao, X., Chen, X., Su, J., Gao, J., Wang, L., Liu, Q., Wang, Y., Xu, H., Chen, L., & Li, Q. (2022). A Comprehensive Survey on Trustworthy Recommender Systems. *arXiv preprint arXiv:2209.10117*. [https://arxiv.org/abs/2209.10117](https://arxiv.org/abs/2209.10117)

* Salunke, T., & Nichite, U. (2022). Recommender Systems in E-commerce. *arXiv preprint arXiv:2212.13910*. [https://arxiv.org/abs/2212.13910](https://arxiv.org/abs/2212.13910)

* Beel, J. (2019). On The Popularity of Recommender-System Datasets. *ISG Siegen*. [https://isg.beel.org/blog/2019/08/03/and-the-winner-is-movielens-on-the-popularity-of-recommender-system-datasets/](https://isg.beel.org/blog/2019/08/03/and-the-winner-is-movielens-on-the-popularity-of-recommender-system-datasets/)

---

## Business Understanding

### Problem Statements

Menjelaskan pernyataan masalah:
- Bagaimana memberikan rekomendasi film yang sesuai dengan preferensi konten pengguna?
- Bagaimana memanfaatkan data historis rating pengguna untuk menemukan kesamaan preferensi antar pengguna?
- Bagaimana membandingkan performa sistem rekomendasi berbasis konten dan berbasis kolaboratif?
  
### Goals

Menjelaskan tujuan proyek yang menjawab pernyataan masalah:
- Mengembangkan sistem rekomendasi berbasis content-based filtering berdasarkan genre film.
- Mengembangkan sistem rekomendasi berbasis collaborative filtering menggunakan matrix factorization.
- Mengevaluasi dan membandingkan performa kedua pendekatan menggunakan metrik evaluasi yang relevan.

    ### Solution statements
    - Content-Based Filtering: Menggunakan TF-IDF vectorization pada genre film, dan menghitung cosine similarity antar film untuk menghasilkan rekomendasi berdasarkan kemiripan konten.
    - Collaborative Filtering: Menggunakan matrix factorization (SVD) pada data rating pengguna untuk mengidentifikasi pola preferensi dan memberikan rekomendasi berbasis user-item interaction.

## Data Understanding

MovieLens 20M adalah dataset populer yang digunakan untuk riset sistem rekomendasi. Dataset ini dikelola oleh GroupLens Research Project di University of Minnesota.

- Ukuran Data: Dataset ini mengandung sekitar 20 juta rating dan 465.564 aplikasi tag.
- Pengguna dan Film: Meliputi interaksi dari 138.493 pengguna pada 27.278 film.
- Periode Waktu: Data ini dibuat oleh pengguna antara 9 Januari 1995 hingga 31 Maret 2015. Dataset ini sendiri dihasilkan pada 17 Oktober 2016 (dengan pembaruan tautan dan penambahan file genome).
- Kriteria Pengguna: Setiap pengguna yang disertakan dalam dataset ini telah memberikan rating setidaknya pada 20 film, memastikan adanya riwayat interaksi yang cukup.

Dataset MovieLens 20M memiliki beberapa file, di antaranya:

**1. Informasi Film (Item/Produk)**

Dalam dataset MovieLens, file movies.csv berfungsi sebagai pusat informasi utama mengenai film. File ini berisi data seperti movieId, title, dan genres. Data ini yang menyimpan informasi inti dari setiap Genre pada film, seperti “Action”, “Comedy”, dan “Drama”.

**2. Fitur Konten Film (Konten Item)**

Untuk mendukung sistem rekomendasi berbasis konten, file tags.csv menyediakan tag-tag yang diberikan oleh pengguna pada film tertentu, misalnya “time travel” atau “mind-bending”. Dalam MovieLens, genre dari movies.csv juga dapat diperlakukan sebagai fitur konten penting.

**3. Profil Pengguna (User/Consumer)**

Dataset MovieLens 20M tidak menyediakan informasi demografis atau preferensi eksplisit pengguna. Namun, preferensi ini dapat diturunkan secara tidak langsung melalui data rating pada file ratings.csv, dengan menganalisis pola perilaku menonton pengguna.

**4. Interaksi Pengguna dan Film (User-Item-Rating)**

File ratings.csv merupakan inti dari sistem rekomendasi dalam dataset ini. File ini mencatat interaksi pengguna dengan film, dalam bentuk rating numerik dari 0.5 hingga 5.0. Data inilah yang digunakan dalam pendekatan collaborative filtering untuk memahami hubungan antar pengguna dan film berdasarkan kesamaan pola rating.

**5. Informasi Tambahan**

Selain itu, file links.csv dalam MovieLens berisi relasi antara movieId dengan ID dari basis data film lain seperti IMDb dan TMDb. File ini berguna untuk integrasi eksternal atau penambahan metadata tambahan jika dibutuhkan.

[Kaggke Dataset](https://www.kaggle.com/datasets/grouplens/movielens-20m-dataset) 

**Fitur Dataset:**
1. **`movies.csv`** – Metadata Item (Film)
File ini berisi informasi dasar tentang film yang digunakan sebagai item dalam sistem rekomendasi.
    - `movieId` (int): ID unik untuk setiap film
    - `title` (string): Judul lengkap film, termasuk tahun rilis
    - `genres` (string): Kategori genre film, dipisahkan oleh simbol `|` (contoh: `Action|Comedy|Drama`)
     File ini penting dalam pendekatan *content-based filtering* karena genre digunakan sebagai fitur konten untuk menghitung kemiripan antar film.

2. **`ratings.csv`** – User-Item Interaction (Feedback Implisit/Eksplisit)
File ini menyimpan informasi interaksi antara pengguna dan film berupa rating numerik.
    - `userId` (int): ID unik pengguna
    - `movieId` (int): ID film yang dirating
    - `rating` (float): Skor rating (0.5 – 5.0 dalam kelipatan 0.5), mencerminkan preferensi eksplisit
    - `timestamp` (int): Waktu (format UNIX epoch) saat rating diberikan
     File ini merupakan inti dari pendekatan *collaborative filtering*, yang menggunakan pola rating untuk membangun model rekomendasi.

3. **`tags.csv`** – User-Generated Content (Fitur Tambahan Item)
File ini menyimpan tag atau label deskriptif yang diberikan pengguna kepada film, menambahkan informasi semantik tambahan.
    - `userId` (int): ID pengguna
    - `movieId` (int): ID film yang diberi tag
    - `tag` (string): Kata atau frasa yang diberikan pengguna, bisa berupa genre, tema, atau nama tokoh (misal: sci-fi, based on a book, christopher nolan)
    - `timestamp` (int): Waktu tag diberikan
      Informasi dari file ini dapat digunakan untuk memperkaya fitur konten dalam content-based filtering dan untuk analisis preferensi tematik.

4. **`links.csv`** – Eksternal Identifier Mapping
File ini menyediakan koneksi antara ID film dalam MovieLens dengan ID eksternal dari basis data lain seperti IMDb dan TMDb.
    - `movieId` (int): ID film dalam MovieLens
    - `imdbId` (int): ID film dalam IMDb
    - `tmdbId (float): ID film dalam The Movie Database (TMDb); sebagian nilai dapat hilang (NaN)
      File ini berguna jika pengguna ingin menambahkan metadata tambahan dari sumber luar, seperti poster film, sinopsis, atau rating global dari situs eksternal.

Jumlah film unik:  27278
Jumlah pengguna unik:  138493
Jumlah interaksi rating pengguna:  20000263
Jumlah tag unik:  38644
Jumlah film yang diberi tag:  19545
Jumlah koneksi ke IMDb/TMDb:  27278

## Univariate Exploratory Data Analysis

- Movies Variabel
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 27278 entries, 0 to 27277
Data columns (total 3 columns):
 #   Column   Non-Null Count  Dtype 
---  ------   --------------  ----- 
 0   movieId  27278 non-null  int64 
 1   title    27278 non-null  object
 2   genres   27278 non-null  object
dtypes: int64(1), object(2)
memory usage: 639.5+ KB
```
- Ratings Variabel
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 20000263 entries, 0 to 20000262
Data columns (total 4 columns):
 #   Column     Dtype  
---  ------     -----  
 0   userId     int64  
 1   movieId    int64  
 2   rating     float64
 3   timestamp  int64  
dtypes: float64(1), int64(3)
memory usage: 610.4 MB
```
```
	userId	movieId	rating	timestamp
count	2.000026e+07	2.000026e+07	2.000026e+07	2.000026e+07
mean	6.904587e+04	9.041567e+03	3.525529e+00	1.100918e+09
std	4.003863e+04	1.978948e+04	1.051989e+00	1.621694e+08
min	1.000000e+00	1.000000e+00	5.000000e-01	7.896520e+08
25%	3.439500e+04	9.020000e+02	3.000000e+00	9.667977e+08
50%	6.914100e+04	2.167000e+03	3.500000e+00	1.103556e+09
75%	1.036370e+05	4.770000e+03	4.000000e+00	1.225642e+09
max	1.384930e+05	1.312620e+05	5.000000e+00	1.427784e+09
```
- Jumlah genre unik: 20
- Genre yang tersedia: {'Adventure', 'Thriller', 'IMAX', 'Fantasy', 'War', 'Comedy', 'Film-Noir', 'Mystery', 'Romance', 'Drama', 'Crime', 'Sci-Fi', '(no genres listed)', 'Musical', 'Horror', 'Western', 'Documentary', 'Action', 'Animation', 'Children'}
- Jumlah userID: 138493
- Jumlah movieID: 26744
- Jumlah data rating: 20000263

- Tags Variable
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 465564 entries, 0 to 465563
Data columns (total 4 columns):
 #   Column     Non-Null Count   Dtype 
---  ------     --------------   ----- 
 0   userId     465564 non-null  int64 
 1   movieId    465564 non-null  int64 
 2   tag        465548 non-null  object
 3   timestamp  465564 non-null  int64 
dtypes: int64(3), object(1)
memory usage: 14.2+ MB
```
```
userId	movieId	tag	timestamp
0	18	4141	Mark Waters	1240597180
1	65	208	dark hero	1368150078
2	65	353	dark hero	1368150079
3	65	521	noir thriller	1368149983
4	65	592	dark hero	1368150078
```

- Likns Variable
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 27278 entries, 0 to 27277
Data columns (total 3 columns):
 #   Column   Non-Null Count  Dtype  
---  ------   --------------  -----  
 0   movieId  27278 non-null  int64  
 1   imdbId   27278 non-null  int64  
 2   tmdbId   27026 non-null  float64
dtypes: float64(1), int64(2)
memory usage: 639.5 KB
```
### Visualisasi
- Distribusi Genre Film

**Rubrik/Kriteria Tambahan (Opsional)**:
- Melakukan beberapa tahapan yang diperlukan untuk memahami data, contohnya teknik visualisasi data beserta insight atau exploratory data analysis.

## Data Preparation
Pada bagian ini Anda menerapkan dan menyebutkan teknik data preparation yang dilakukan. Teknik yang digunakan pada notebook dan laporan harus berurutan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan proses data preparation yang dilakukan
- Menjelaskan alasan mengapa diperlukan tahapan data preparation tersebut.

## Modeling
Tahapan ini membahas mengenai model sisten rekomendasi yang Anda buat untuk menyelesaikan permasalahan. Sajikan top-N recommendation sebagai output.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menyajikan dua solusi rekomendasi dengan algoritma yang berbeda.
- Menjelaskan kelebihan dan kekurangan dari solusi/pendekatan yang dipilih.

## Evaluation
Pada bagian ini Anda perlu menyebutkan metrik evaluasi yang digunakan. Kemudian, jelaskan hasil proyek berdasarkan metrik evaluasi tersebut.

Ingatlah, metrik evaluasi yang digunakan harus sesuai dengan konteks data, problem statement, dan solusi yang diinginkan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan formula metrik dan bagaimana metrik tersebut bekerja.

**---Ini adalah bagian akhir laporan---**

_Catatan:_
- _Anda dapat menambahkan gambar, kode, atau tabel ke dalam laporan jika diperlukan. Temukan caranya pada contoh dokumen markdown di situs editor [Dillinger](https://dillinger.io/), [Github Guides: Mastering markdown](https://guides.github.com/features/mastering-markdown/), atau sumber lain di internet. Semangat!_
- Jika terdapat penjelasan yang harus menyertakan code snippet, tuliskan dengan sewajarnya. Tidak perlu menuliskan keseluruhan kode project, cukup bagian yang ingin dijelaskan saja.
