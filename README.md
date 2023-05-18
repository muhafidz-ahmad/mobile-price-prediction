# Proyek 1 MLT Dicoding - Muhafidz Ahmad Halim

# Domain Proyek

<p>
  <img src="https://github.com/muhafidz-ahmad/mobile-price-prediction/assets/115754250/e6d6cd16-d9c8-4b78-92df-0f0b6e198af5" alt="ilustrasi smartphone" width="600">
	<figcaption>Gambar 1. Ilustrasi <i>smartphone</i></figcaption>
</p>

Di era modern ini, ponsel telah menjadi salah satu perangkat teknologi yang paling penting dan populer di dunia. Ponsel tidak lagi hanya digunakan sebagai alat komunikasi, tetapi juga sebagai perangkat serbaguna yang menggabungkan fungsi telepon, kamera, pemutar musik, internet, dan banyak lagi dalam satu perangkat.

Seiring dengan berkembangnya teknologi, industri ponsel terus berinovasi dengan meluncurkan berbagai model dan merek dengan harga yang berbeda-beda. Harga ponsel sangat bervariasi, mulai dari yang sangat mahal hingga yang sangat terjangkau, sesuai dengan spesifikasi dan fitur yang ditawarkan. Tidak jarang dengan banyaknya pilihan ponsel ini membuat konsumen bingung harus memilih ponsel mana yang harus mereka beli karena khawatir ponsel yang mereka beli *overprice*.

* Fokus proyek ini adalah mengembangkan model machine learning yang dapat mengklasifikasikan harga ponsel ke dalam 4 kategori berdasarkan kelas harganya. 
* Tujuan dari proyek ini adalah untuk memberikan panduan kepada konsumen dalam memilih ponsel yang sesuai dengan anggaran mereka.
* Terdapat beberapa penelitian yang dilakukan dalam pembuatan model machine learning untuk memprediksi harga ponsel ini ini. Beberapa diantaranya adalah:
	* Menggunakan algoritma *Decision Tree* dan *Naive Bayes Classifier* dan mendapatkan akurasi hingga 78% [1].
	* Menggunakan algoritma *Random Forest* dan mendapatkan akurasi hingga 90% [2].

# Business Understanding

## Problem Statement
* Banyaknya ponsel yang rilis tiap waktunya dengan beragam harga dan spesifikasi.
* Terkadang produsen ponsel memasang harga tidak masuk akan pada suatu ponsel yang baru dirilis.

## Goals
* Membantu konsumen dalam menentukan ponsel yang ingin dibelinya berdasarkan budget yang dimilikinya.
* Membuat model machine learning untuk memprediksi harga ponsel berdasarkan spesifikasi yang diberikan.

## Solution Statement
* Proyek ini akan menggunakan 2 algoritma *machine learning*, yaitu *Decision Tree* dan *Support Vector Machine* (SVM). *Decision Tree* adalah algoritma yang berbasis pada pohon keputusan, di mana setiap simpul dalam pohon mewakili keputusan atau aturan untuk mengklasifikasikan data. Sedangkan SVM adalah algoritma yang menggunakan pendekatan geometris untuk memisahkan dan mengklasifikasikan data.
* Akan digunakan dataset yang terdiri dari berbagai atribut atau fitur ponsel, seperti ukuran layar, resolusi kamera, kapasitas baterai, dan lainnya.

# Data Understanding
Data yang akan digunakan untuk project ini adalah data dari Kaggle tentang range harga suatu ponsel berdasarkan spesifikasi yang dimilikinya. Data dapat diunduh melalui [tautan berikut](https://www.kaggle.com/datasets/iabhishekofficial/mobile-price-classification?select=train.csv).

Dataset ini telah terbagi menjadi 2 bagian, yaitu data training dengan total 2000 baris dan data test dengan total 1000 baris. Hanya saja, data test yang disediakan tidak memiliki label harga.

## Variabel-variabel pada dataset
Masing-masing data terdiri dari 4 kolom, yaitu:
1. *battery_power: Total energy a battery can store in one time measured in mAh*
2. *blue: Has bluetooth or not*
3. *clock_speed: Speed at which microprocessor executes instructions*
4. *dual_sim: Has dual sim support or not*
5. *fc: Front Camera mega pixels*
6. *four_g: Has 4G or not*
7. *int_memory: Internal Memory in Gigabytes*
8. *m_dep: Mobile Depth in cm*
9. *mobile_wt: Weight of mobile phone*
10. *n_cores: Number of cores of processor*
11. *pc: Primary Camera mega pixels*
12. *px_height: Pixel Resolution Height*
13. *px_width: Pixel Resolution Width*
14. *ram: Random Access Memory in Megabytes*
15. *sc_h: Screen Height of mobile in cm*
16. *sc_w: Screen Width of mobile in cm*
17. *talk_time: Time that a single battery charge will last when you are on call in minutes*
18. *three_g: Has 3G or not*
19. *touch_screen: Has touch screen or not*
20. *wifi: Has wifi or not*
21. *price_range: This is the target variable with value of 0(low cost), 1(medium cost), 2(high cost) and 3(very high cost).*

## Kondisi Data
Baik data *training* maupun data *testing*, dataset ini sudah bersih dari data yang hilang maupun data duplikat.

Namun dengan menggunakan metode IQR untuk mendeteksi *outliers*, terdapat 18 *outliers* pada kolom *fc* dan 2 *outliers* pada kolom *px_height*. Data *outliers* ini akan dihapus.

## Exploratory Data Analysis
Pertama akan dibuat histogram pada kolom-kolom dengan tipe data numerik.

<p>
  <img src="https://github.com/muhafidz-ahmad/mobile-price-prediction/assets/115754250/885f6ea0-4168-485d-beec-9bed8c2321e8" alt="histogram semua kolom dengan tipe data numerik" width="1000">
  <figcaption>Gambar 2. Histogram semua kolom dengan tipe data numerik</figcaption>
</p>

* Pada histogram di atas terlihat bahwa label sudah seimbang.
* Terdapat beberapa kolom yang hanya terdiri dari 2 nilai yang menunjukan keberadaan fitur tertentu.

Selanjutnya akan dilihat korelasi antar fitur.

<p>
  <img src="https://github.com/muhafidz-ahmad/mobile-price-prediction/assets/115754250/a54a21e6-a1c8-4043-af07-f2e36965d58b" alt="Korelasi antar fitur" width="1000">
  <figcaption>Gambar 3. Korelasi antar fitur</figcaption>
</p>

Kebanyakan fitur tidak memiliki korelasi yang kuat satu sama lain. Hanya beberapa saja fitur yang memiliki korelasi yang cukup tinggi karena memang kedua fitur tersebut saling terkait. Seperti ukuran layar pada fitur *px_height* dan *px_width*, kemudian ketersediaan jaringan 3g dan 4g, serta resolusi kamera depan dan kamera belakang.

Untuk mempermudah melihat korelasi antara fitur dengan label, akan dibuat tabel yang telah diurutkan nilai korelasinya dari yang terbesar hingga yang terkecil.

| fitur         	| korelasi_dengan_price_range 	|
|---------------	|-----------------------------	|
| price_range   	| 1.0000                      	|
| ram           	| 0.9170                      	|
| battery_power 	| 0.2007                      	|
| px_width      	| 0.1658                      	|
| px_height     	| 0.1489                      	|
| int_memory    	| 0.0444                      	|
| sc_w          	| 0.0387                      	|
| pc            	| 0.0336                      	|
| touch_screen  	| 0.0304                      	|
| mobile_wt     	| 0.0303                      	|
| three_g       	| 0.0236                      	|
| sc_h          	| 0.0230                      	|
| fc            	| 0.0220                      	|
| talk_time     	| 0.0219                      	|
| blue          	| 0.0206                      	|
| wifi          	| 0.0187                      	|
| dual_sim      	| 0.0174                      	|
| four_g        	| 0.0148                      	|
| clock_speed   	| 0.0066                      	|
| n_cores       	| 0.0044                      	|
| m_dep         	| 0.0009                      	|

Terlihat bahwa RAM adalah fitur yang memiliki korelasi tertinggi dengan label harga ponsel.

Dari sini, akan diambil fitur dengan korelasi yang melewati *threshold* saja, dengan nilai *threshold* yang ditentukan adalah 0.01.

# Data Preparation
Pada bagian data preparation, project ini hanya menggunakan dua teknik, yaitu Standarisasi dan Split antara fitur target.

## Standarisasi
Standarisasi digunakan karena kolom-kolom pada data ini tidak terdistribusi normal. Kemudian standarisasi juga berguna untuk data menjadi lebih mudah dibandingkan karena range standar deviasi tiap kolom akan menjadi 1 dan mean 0.

Karena dataset yang digunakan ini terdapat beberapa kolom yang hanya memiliki 2 nilai, yaitu 0 dan 1, kolom tersebut akan dikecualikan.

## Feature and Target Splitting
Karena data masih berbentuk satu dataframe baik itu fitur maupun target, maka fitur dan target tersebut harus dipisahkan. Tujuan dari pemisahan antara fitur dan target ini agar data dapat dimasukan ke dalam model machine learning.

# Modelling
Algoritma yang akan digunakan pada project ini adalah *Decision Tree* dan *Support Vector Machine* (SVM).

## Decision Tree

<p>
  <img src="https://github.com/muhafidz-ahmad/mobile-price-prediction/assets/115754250/e07bbc0e-4af5-4de1-8720-973e6baaf2ae" alt="Ilustrasi pohon" width="600">
	<figcaption>Gambar 4. Ilustrasi <i>tree</i></figcaption>
</p>

Kelebihan dari algoritma *Decision Tree* adalah kemampuannya untuk memodelkan hubungan non-linier antara atribut dan label. Selain itu, *Decision Tree* juga dapat dengan mudah diinterpretasikan karena keputusan diambil berdasarkan aturan yang dapat dijelaskan dengan mudah oleh manusia. Namun, kelemahan dari *Decision Tree* adalah kecenderungannya untuk *overfitting* jika tidak dikendalikan dengan baik. Oleh karena itu, kami menggunakan *hyperparameter tuning* untuk mencari model *Decision Tree* yang memiliki kinerja yang optimal dan terhindar dari *overfitting*.

Pada algoritma *Decision Tree*, dilakukan *hyperparameter tuning* menggunakan *Grid Search* untuk mencari *hyperparameter* terbaik. *Hyperparameter* yang diatur adalah:
* criterion : 'gini', 'entropy'
* max_depth : 2, 3, 4

## SVM

<p>
  <img src="https://github.com/muhafidz-ahmad/mobile-price-prediction/assets/115754250/b594785d-89ce-414e-a965-66ee36b7a035" alt="Ilustrasi SVM" width="600">
  <figcaption>Gambar 5. Ilustrasi SVM</figcaption>
</p>

Kelebihan dari algoritma SVM adalah kemampuannya untuk bekerja dengan baik dalam dataset dengan dimensi tinggi dan kemampuannya untuk menangani data yang tidak linier secara efektif melalui penggunaan fungsi kernel. SVM juga cenderung lebih tahan terhadap *overfitting* jika parameter C diatur dengan benar. Namun, kelemahan dari SVM adalah komputasi yang memakan waktu lebih lama terutama pada dataset yang besar, dan sensitif terhadap pemilihan *hyperparameter* yang tepat.

Pada algoritma SVM, dilakukan juga *hyperparameter tuning* menggunakan *Grid Search* untuk mencari *hyperparameter* terbaik. *Hyperparameter* yang diatur adalah:
* kernel : 'linear', 'poly', 'rbf', 'sigmoid'
* C : 1, 3, 5

# Evaluation
Untuk mengevaluasi kinerja model klasifikasi harga ponsel, kami menggunakan beberapa metrik evaluasi yang umum digunakan dalam tugas klasifikasi. Metrik evaluasi yang kami gunakan adalah sebagai berikut:

## 1. Akurasi
Metrik ini mengukur proporsi jumlah prediksi yang benar dibandingkan dengan total jumlah prediksi. Akurasi memberikan gambaran umum tentang seberapa baik model dapat mengklasifikasikan harga ponsel secara benar. Akurasi dapat dihitung dengan membagi jumlah prediksi yang benar dibagi dengan jumlah total data. 

$$akurasi = {TP + TN \\over TP + TN + FP + FN}.$$

Dimana:
* TP (*True Positive*), jumlah data positif yang diklasifikasikan dengan benar.
* TN (*True Negative*), jumlah data negatif yang diklasifikasikan dengan benar.
* FP (*False Positive*), jumlah data negatif yang salah diklasifikasikan sebagai positif (*type 1 error*).
* FN (*False Negative*), jumlah data positif yang salah diklasifikasikan sebagai negatif (*type 2 error*).

## 2. Presisi
Presisi mengukur seberapa baik model dalam mengidentifikasi kelas yang positif secara akurat. Dalam konteks proyek ini, presisi mengukur seberapa baik model dalam mengklasifikasikan ponsel sebagai mahal, menengah, atau murah dengan benar.

$$presisi = {TP \\over TP + FP}.$$

## 3. Recall
Recall mengukur seberapa baik model dalam menemukan semua contoh dari kelas yang positif secara lengkap. Dalam konteks proyek ini, recall mengukur seberapa baik model dalam mengidentifikasi ponsel dengan harga yang sesuai dengan kelasnya.

$$recall = {TP \\over TP + FN}.$$

## 4. F1-Score
F1-score adalah ukuran yang menggabungkan presisi dan recall menjadi satu angka. F1-score memberikan gambaran holistik tentang kinerja model dalam mengklasifikasikan harga ponsel.

$$f1 score = 2 * {precision * recall \\over precision + recall}.$$

## Evaluasi Decision Tree
*Decision Tree* mendapatkan nilai evaluasi sebagai berikut:
* **Akurasi: 83%**
* **Presisi: 83%**
* **Recall: 83%**
* **F1-score: 83%**

## Evaluasi SVM
SVM mendapatkan nilai evaluasi sebagai berikut:
* **Akurasi: 98%**
* **Presisi: 98%**
* **Recall: 98%**
* **F1-score: 98%**

Dari kedua evaluasi model di atas, terlihat jelas bahwa SVM memberikan evaluasi yang lebih baik dibandingkan dengan Decision Tree dalam tugas prediksi harga ponsel. 

Model SVM menghasilkan akurasi yang lebih tinggi, presisi yang lebih baik, recall yang lebih tinggi, dan F1-score yang lebih baik dibandingkan dengan model Decision Tree. Hal ini menunjukkan bahwa SVM mampu secara efektif mengklasifikasikan harga ponsel ke dalam 4 kelas harga yang ditentukan. Dalam konteks proyek ini, tujuannya adalah memberikan panduan kepada konsumen dalam memilih ponsel yang sesuai dengan anggaran mereka, dan model SVM memberikan kinerja yang lebih baik dalam mencapai tujuan tersebut.

# Kesimpulan
Dari hasil project ini, dapat disimpulkan bahwa model SVM dengan parameter terbaik yang ditentukan melalui Grid Search merupakan pilihan terbaik untuk klasifikasi harga ponsel dalam proyek ini. SVM mampu menangani hubungan non-linier dengan baik dan memberikan kinerja yang lebih baik dalam hal akurasi klasifikasi.

# Referensi

[1]	[Asim, Muhammad & Khan, Zafar. (2018). Mobile Price Class prediction using Machine Learning Techniques. International Journal of Computer Applications. 179. 6-11. 10.5120/ijca2018916555.](https://www.researchgate.net/publication/323994340_Mobile_Price_Class_prediction_using_Machine_Learning_Techniques)

[2] 	[Ahsanul Hoque Sakib, Asif Khan Shakir, Shanjoy Sutradhar, MD.Abu Saleh, Washim Akram, and Khalid Been MD.Badruzzaman Biplop. 2022. A hybrid model for predicting Mobile Price Range using machine learning techniques. In 2022 The 8th International Conference on Computing and Data Engineering (ICCDE 2022). Association for Computing Machinery, New York, NY, USA, 86–91. https://doi.org/10.1145/3512850.3512860](https://dl.acm.org/doi/abs/10.1145/3512850.3512860)
