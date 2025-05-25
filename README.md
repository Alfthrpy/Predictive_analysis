# Laporan Proyek Machine Learning - Muhammad Rizki Al-Fathir

## Domain Proyek

Perkembangan teknologi informasi yang sangat pesat telah membawa dampak signifikan terhadap berbagai aspek kehidupan, termasuk dalam transaksi keuangan, komunikasi, dan aktivitas belanja daring. Namun, kemajuan ini juga diiringi dengan peningkatan kejahatan siber, salah satunya adalah serangan phishing. Phishing merupakan upaya penipuan untuk memperoleh informasi sensitif seperti username, password, atau data kartu kredit, dengan menyamar sebagai entitas terpercaya melalui media elektronik.

Menurut laporan terbaru dari Anti-Phishing Working Group (APWG), jumlah serangan phishing yang dilaporkan telah mencapai lebih dari 1,3 juta kasus pada kuartal pertama tahun 2023 saja [1]. Fakta ini menunjukkan bahwa phishing masih menjadi ancaman serius yang membutuhkan solusi preventif dan detektif yang andal.

Salah satu pendekatan yang kini banyak digunakan untuk mengatasi permasalahan ini adalah penerapan machine learning. Dengan pendekatan ini, sistem dapat dilatih untuk mengenali pola-pola khas dari website phishing, sehingga mampu melakukan deteksi otomatis. Untuk keperluan tersebut, dataset dari UCI Machine Learning Repository yang berjudul Phishing Websites Dataset banyak digunakan oleh peneliti karena menyediakan berbagai fitur teknikal dari website yang dapat digunakan untuk klasifikasi [2].

Beberapa penelitian sebelumnya telah menunjukkan bahwa algoritma machine learning seperti Random Forest, Support Vector Machine (SVM), dan Neural Networks mampu memberikan akurasi tinggi dalam mendeteksi website phishing [3], [4]. Melalui penelitian ini, akan dibangun sebuah sistem analisis prediktif berbasis data Phishing Websites Dataset, dengan tujuan membandingkan kinerja berbagai model klasifikasi serta mengidentifikasi fitur yang paling berpengaruh terhadap prediksi phishing.

## Business Understanding

Saat ini, masih banyak sistem keamanan yang menggunakan metode blacklist atau rule-based yang hanya bisa mendeteksi website phishing yang sudah dikenal. Pendekatan tersebut lemah dalam menghadapi serangan phishing baru yang belum terdaftar atau memiliki pola baru (zero-day attack). Oleh karena itu, dibutuhkan pendekatan yang lebih adaptif seperti machine learning untuk mendeteksi phishing berdasarkan pola-pola umum dalam struktur dan karakteristik situs.

### Problem Statements

1. Masih banyak Korban Serangan Phising
    Banyak pengguna internet yang masih menjadi korban serangan phishing karena tidak mampu membedakan mana website yang aman dan mana yang berbahaya secara visual. Ini menunjukkan bahwa deteksi secara manual tidak efektif dan sangat berisiko terhadap keamanan data pengguna.

2. Metode Tradisional yang terbatas
    Metode deteksi phishing yang saat ini banyak digunakan oleh sistem keamanan masih berbasis aturan (rule-based) atau blacklist. Pendekatan ini memiliki keterbatasan dalam mendeteksi serangan phishing baru (zero-day phishing), karena belum ada dalam daftar atau pola yang dikenali sistem.

3. Pengembangan Model Prediktif
    Belum ada evaluasi komprehensif terkait algoritma machine learning mana yang paling optimal untuk mendeteksi phishing website berdasarkan dataset yang tersedia, khususnya pada Phishing Websites Dataset dari UCI.
### Goals

1. Mengembangkan sistem prediksi otomatis yang mampu membedakan website phishing dan non-phishing berdasarkan fitur teknikal tanpa perlu validasi visual dari pengguna.

2. Menerapkan pendekatan machine learning yang dapat mengenali pola baru dari data phishing, sehingga mampu mendeteksi website phishing meskipun belum pernah muncul sebelumnya.

3. Menganalisis dan membandingkan beberapa algoritma machine learning untuk menemukan model dengan performa terbaik berdasarkan metrik evaluasi akurasi


### Solution Statements
1. Menerapkan dan membandingkan berbagai algoritma klasifikasi
Beberapa algoritma machine learning yaitu Logistic Regression, Random Forest dan Xgboost akan diterapkan pada Phishing Websites Dataset. Setiap model akan diuji dan dibandingkan performanya menggunakan metrik seperti akurasi, precision, recall, dan F1-score untuk menemukan model terbaik.

2. Menelusuri feature importance untuk interpretabilitas model
Untuk menambah nilai interpretasi dari sistem yang dibangun, akan dilakukan analisis terhadap fitur mana yang paling berpengaruh dalam klasifikasi phishing, sehingga bisa menjadi insight tambahan bagi praktisi keamanan.


## Data Understanding
Dataset yang digunakan dalam proyek ini adalah [Phishing Websites Dataset](https://archive.ics.uci.edu/dataset/327/phishing+websites). yang diperoleh dari UCI Machine Learning Repository. Dataset ini disusun oleh Mohammad et al. dan terdiri dari kumpulan fitur yang mencerminkan karakteristik teknikal dan visual dari suatu website, yang dapat digunakan untuk membedakan antara situs phishing dan situs yang sah (legit).

Nilai -1, 0, dan 1 pada dataset ini tidak selalu merepresentasikan urutan (ordinal) atau level yang konsisten secara semantik. Beberapa fitur menggunakan nilai tersebut untuk menyatakan klasifikasi (legit/suspicious/phishing), sementara fitur lain hanya menyatakan kehadiran atau ketiadaan karakteristik teknis tertentu (boolean). Oleh karena itu, interpretasi nilai harus dilihat per fitur berdasarkan deskripsi asalnya.

Dalam Datasets ini, terdapat 30 kolom dengan value integer, namun merepresentasikan sebuah kategori atau kelas.

### Variabel-variabel pada Phising Websites Dataset
Berdasarkan [Introduction Paper](https://www.semanticscholar.org/paper/An-assessment-of-features-related-to-phishing-using-Mohammad-Thabtah/0c0ff58063f4e078714ea74f112bc709ba9fed06) dalam sumber dataset, berikut merupakan penjelasan tiap variable dalam dataset:


##### **Address Bar Based Features**

1. **`having_ip_address`**: Mengindikasikan apakah URL menggunakan alamat IP secara langsung (contoh: `http://125.98.3.123/...`).
2. **`url_length`**: Panjang dari URL. URL dengan ≥54 karakter dikategorikan sebagai phishing.
3. **`shortining_service`**: Mengindikasikan penggunaan layanan pemendek URL (seperti `bit.ly`, `tinyurl`).
4. **`having_at_symbol`**: Keberadaan simbol `@` di URL yang bisa digunakan untuk menyembunyikan alamat tujuan sebenarnya.
5. **`double_slash_redirecting`**: Penggunaan tanda `//` kedua dalam URL setelah protokol, dapat menyamarkan redireksi.
6. **`prefix_suffix`**: Penggunaan dash (`-`) sebagai prefiks atau sufiks pada domain utama.
7. **`having_sub_domain`**: Mengukur jumlah subdomain berdasarkan banyaknya titik (`.`) dalam domain; lebih dari 3 dianggap mencurigakan.

---

##### **SSL and Certificate Features**

8. **`sslfinal_state`**: Menilai apakah HTTPS digunakan dan apakah sertifikat SSL valid, termasuk siapa issuer-nya dan masa aktifnya.

---

##### **Domain Based Features**

9. **`domain_registration_length`**: Lamanya domain terdaftar. Domain yang berumur <1 tahun dianggap mencurigakan.
10. **`favicon`**: Apakah favicon berasal dari domain yang berbeda dengan domain utama.
11. **`port`**: Mengidentifikasi apakah port yang digunakan pada URL merupakan port umum atau tidak.
12. **`https_token`**: Keberadaan token ‘https’ di bagian domain URL (contoh: `https-login.com`).

---

##### **Abnormal Based Features**

13. **`request_url`**: Mengukur apakah sumber (seperti gambar) dalam halaman diambil dari domain berbeda.
14. **`url_of_anchor`**: Analisis pada tag `<a>`; anchor yang tidak mengarah ke halaman valid dianggap mencurigakan.
15. **`links_in_tags`**: Melihat link pada tag `<meta>`, `<script>`, dan `<link>`, apakah mengarah ke domain yang berbeda.
16. **`sfh` (Server Form Handler)**: Memeriksa apakah form dikirim ke domain berbeda atau kosong (`about:blank`).
17. **`submitting_to_email`**: Apakah form menggunakan metode pengiriman via email (mailto:).
18. **`abnormal_url`**: Ketidaksesuaian antara hostname URL dan identitas aslinya, dicek melalui WHOIS.

---

##### **HTML and JavaScript Based Features**

19. **`redirect`**: Adanya redirect otomatis dari satu URL ke URL lain.
20. **`on_mouseover`**: Adanya penggunaan JavaScript `onMouseOver` untuk memanipulasi link.
21. **`rightclick`**: Menilai apakah fungsi klik kanan dimatikan melalui JavaScript.
22. **`popupwindow`**: Apakah terdapat popup window yang meminta informasi pengguna.
23. **`iframe`**: Penggunaan `<iframe>` tersembunyi di halaman.

---

##### **Domain Popularity and Technical Records**

24. **`age_of_domain`**: Kondisi umur domain yang dihitung berdasarkan WHOIS.
25. **`dnsrecord`**: Ketersediaan DNS record berdasarkan query WHOIS.
26. **`web_traffic`**: Trafik website berdasarkan ranking dari Alexa. Website tanpa trafik dianggap mencurigakan.
27. **`page_rank`**: Ranking halaman dari mesin pencari.
28. **`google_index`**: Apakah halaman terindeks oleh Google.
29. **`links_pointing_to_page`**: Jumlah link dari domain lain yang mengarah ke halaman tersebut.
30. **`statistical_report`**: Berdasarkan blacklist dan data statistik keamanan, apakah domain terindikasi phishing.

Dalam datasets ini, tidak terdapat missing values dan outliers ataupun invalid data di tiap-tiap fitur. Di sumber dataset di sebutkan bahwa tidak ada missing values, dan ini benar adanya di buktikan dengan pengecekan manual.

Pengecekan outliers dan invalid data dilakukan dengan cara melihat apakah ada fitur yang memilik unique value lebih dari 3 karena semua fitur merupakan tipe kategori, tidak akan cocok jika digunakan teknik lain seperti IQR.

### Exploratory Data Analysis
#### Univariate Analysis
Berikut merupakan persebaran value dalam tiap fitur

##### Kolom: having_ip_address
    having_ip_address   jumlah sampel   persentase                         
     1                     7262            65.7
    -1                     3793            34.3
![image](https://github.com/user-attachments/assets/579bb553-e00a-4985-a41d-1656b4c30511)


##### Kolom: shortining_service
    shortining_service   jumlah sampel   persentase
           1                9611            86.9
          -1                1444            13.1
![image-1](https://github.com/user-attachments/assets/b111877f-c0e2-4b6c-95e3-34167d263818)


##### Kolom: having_at_symbol
    having_at_symbol   jumlah sampel   persentase
             1              9400            85.0
            -1              1655            15.0

![image-2](https://github.com/user-attachments/assets/ebf7e27b-59af-4940-8903-8822bfc78ab7)

##### Kolom: double_slash_redirecting
    double_slash_redirecting   jumlah sampel   persentase
             1                     9626            87.1
            -1                     1429            12.9

![image-3](https://github.com/user-attachments/assets/47c33893-9d1d-4592-8a31-7efa7eccc510)

##### Kolom: prefix_suffix
    prefix_suffix   jumlah sampel   persentase
         -1             9590            86.7
          1             1465            13.3

![image-4](https://github.com/user-attachments/assets/67f335f6-b953-403f-9cfc-12b82eebb670)

##### Kolom: domain_registration_length
    domain_registration_length   jumlah sampel   persentase
        -1                                7389        66.8
         1                                3666        33.2

![image-5](https://github.com/user-attachments/assets/451b385a-bb0b-4baa-88af-76ac3ef51f09)


##### Kolom: favicon
    favicon   jumlah sampel   persentase
         1            9002            81.4
        -1            2053            18.6
![image-6](https://github.com/user-attachments/assets/61faf8f4-c0c2-4eaf-a1a5-c99909b1f20f)

##### Kolom: port
    port   jumlah sampel   persentase
     1        9553            86.4
    -1        1502            13.6

![image-7](https://github.com/user-attachments/assets/9454f8df-4be8-49b2-8027-f8b439d02c70)

##### Kolom: https_token
    https_token   jumlah sampel   persentase
         1            9259            83.8
        -1            1796            16.2

![image-8](https://github.com/user-attachments/assets/87601ba8-9b17-4e69-90a7-2f9bb715166d)

##### Kolom: request_url
    request_url   jumlah sampel   persentase
         1            6560            59.3
        -1            4495            40.7

![image-9](https://github.com/user-attachments/assets/e15bf8c7-0491-45d6-a149-1ed262921fdc)

##### Kolom: submitting_to_email
    submitting_to_email   jumlah sampel   persentase
             1                 9041            81.8
            -1                 2014            18.2

![image-10](https://github.com/user-attachments/assets/a9cf69f3-e210-4088-90d5-e71f5de924a8)

##### Kolom: abnormal_url
    abnormal_url   jumlah sampel   persentase
         1             9426            85.3
        -1             1629            14.7

![image-11](https://github.com/user-attachments/assets/4d8c837d-5184-4c44-a761-a54aefb997bb)


##### Kolom: redirect
    redirect   jumlah sampel   persentase
        0          9776            88.4
        1          1279            11.6

![image-12](https://github.com/user-attachments/assets/7f3c41c6-6d78-4d47-98ce-ce80b5b34652)

##### Kolom: on_mouseover
    on_mouseover   jumlah sampel   persentase
         1             9740            88.1
        -1             1315            11.9

![image-13](https://github.com/user-attachments/assets/dfca1603-a7a2-4f9d-8c65-1ff4783b3c12)

##### Kolom: rightclick
    rightclick   jumlah sampel   persentase
         1           10579            95.7
        -1             476              4.3
![image-14](https://github.com/user-attachments/assets/c996f4e7-6c26-4d12-a95e-f7ec0ab9ad3d)



##### Kolom: popupwindow
    popupwindow   jumlah sampel   persentase
         1             8918            80.7
        -1             2137            19.3

![image-15](https://github.com/user-attachments/assets/99b6801a-0c44-464c-b143-9fa556fd68ce)

##### Kolom: iframe
    iframe   jumlah sampel   persentase
       1          10043            90.8
      -1           1012             9.2

![image-16](https://github.com/user-attachments/assets/d74b7064-2b72-4ab6-9418-d160b0c4e9a0)

##### Kolom: age_of_domain
    age_of_domain   jumlah sampel   persentase
           1              5866            53.1
          -1              5189            46.9

![image-17](https://github.com/user-attachments/assets/c7c303e8-f4bc-4bd7-84e2-02584d229859)

##### Kolom: dnsrecord
    dnsrecord   jumlah sampel   persentase
         1           7612            68.9
        -1           3443            31.1

![image-18](https://github.com/user-attachments/assets/91e5c235-5048-407e-8574-8365528c43ea)

##### Kolom: page_rank
    page_rank   jumlah sampel   persentase
        -1          8201            74.2
         1          2854            25.8

![image-19](https://github.com/user-attachments/assets/da8b00c7-8230-459a-aa9b-2ce5225cf9ff)

##### Kolom: google_index
    google_index   jumlah sampel   persentase
           1            9516            86.1
          -1            1539            13.9

![image-20](https://github.com/user-attachments/assets/fc9adbd2-55bc-4d85-8f9e-3ce71243071a)

##### Kolom: statistical_report
    statistical_report   jumlah sampel   persentase
             1               9505            86.0
            -1               1550            14.0

![image-21](https://github.com/user-attachments/assets/63099dde-d304-4e9c-ac9a-023b48b210a3)

##### Kolom: links_pointing_to_page
    links_pointing_to_page   jumlah sampel   persentase
                  0                6156            55.7
                  1                4351            39.4
                 -1                 548             5.0


![image-29](https://github.com/user-attachments/assets/fd264ba7-52af-402d-8beb-3b7f238f168d)

##### Kolom: web_traffic
    web_traffic   jumlah sampel   persentase
            1           5831            52.7
           -1           2655            24.0
            0           2569            23.2

![image-28](https://github.com/user-attachments/assets/5ad1562a-401e-4234-a745-232bdf04073d)

##### Kolom: sfh
    sfh   jumlah sampel   persentase
    -1          8440            76.3
     1          1854            16.8
     0           761             6.9

![image-27](https://github.com/user-attachments/assets/84df73bd-a38f-488e-aab8-1f2ed1925bb4)

##### Kolom: links_in_tags
    links_in_tags   jumlah sampel   persentase
            0             4449            40.2
           -1             3956            35.8
            1             2650            24.0

![image-26](https://github.com/user-attachments/assets/2fc4b8be-c438-40c8-b39f-3799bc9cc022)

##### Kolom: url_of_anchor
    url_of_anchor   jumlah sampel   persentase
            0             5337            48.3
           -1             3282            29.7
            1             2436            22.0

![image-25](https://github.com/user-attachments/assets/b8f1abe0-5fc5-4934-9cdb-48e96ec873ca)

##### Kolom: sslfinal_state
    sslfinal_state   jumlah sampel   persentase
              1            6331            57.3
             -1            3557            32.2
              0            1167            10.6

![image-24](https://github.com/user-attachments/assets/9756f410-744d-44a2-b7ae-d5981c012b6e)

##### Kolom: having_sub_domain
    having_sub_domain   jumlah sampel   persentase
                 1             4070            36.8
                 0             3622            32.8
                -1             3363            30.4

![image-23](https://github.com/user-attachments/assets/89d58d0c-4e87-4c90-94ae-04caa68d41b8)

##### Kolom: url_length
    url_length   jumlah sampel   persentase
          -1           8960            81.0
           1           1960            17.7
           0            135             1.2

![image-22](https://github.com/user-attachments/assets/603e518d-6683-4984-a7e1-6d464b76476d)

#### Multivariate Analysis
##### Multiple Correspondence Analysis

![image-30](https://github.com/user-attachments/assets/f4b3d554-b45e-4e6a-99f5-2a2c7ad99038)

Distribusi berlapis atau terpisah di area tertentu menunjukkan bahwa komponen hasil MCA mampu memetakan perbedaan antara kelas phishing dan non-phishing ke dalam dimensi lebih rendah.

Observasi dengan label yang sama cenderung terkonsentrasi di wilayah tertentu dalam ruang MCA.

Ini menunjukkan bahwa MCA berhasil menangkap struktur diskriminatif dari fitur kategorikal dalam data.

Overlap sebagian juga terlihat, artinya meskipun ada pemisahan, masih ada tumpang tindih antar kelas. Hal ini umum terjadi pada data kategorikal yang tidak sepenuhnya linier separable.


#### Chi-Square Test of Independence

![image-31](https://github.com/user-attachments/assets/f868247a-67e5-4431-a357-4b6080f1048c)

Untuk mengecek korelasi, pada datasets ini, digunakan teknik **Chi-Square Test of Independence**, yaitu uji statistik yang digunakan untuk menentukan apakah dua variabel kategorikal memiliki hubungan atau saling bebas (independen) satu sama lain.

Pada hasil perhitungan Chi-square, terdapat beberapa fitur yang memiliki nilai chi-square tinggi, seperti ssfinal_state dan fitur lain. Fitur yang memiliki chi-square besar, maka akan memiliki nilai `p-value` yang kecil. Semakin kecil nilai `p`, maka semakin memiliki hubungan yang signifikan dengan target.

## Data Preparation
Tahapan Data preparation dalam studi kasus ini adalah **Dimension Reduction** dan **Splitting Data**.

### Dimension Reduction
Multiple Correspondence Analysis (MCA) digunakan untuk mereduksi dimensi data yang seluruh fiturnya bertipe kategori karena metode ini dirancang khusus untuk menangkap pola dan hubungan antar kategori. Berbeda dengan PCA yang hanya cocok untuk data numerik, MCA mampu merepresentasikan data kategorikal ke dalam bentuk numerik berdimensi lebih rendah, sehingga memudahkan visualisasi dan analisis tanpa kehilangan informasi penting dari struktur asli data.

Reduksi MCA diperlukan dalam studi kasus ini karena melihat banyaknya fitur yang ada, yaitu 30 fitur. jadi untuk mengurangi beban komputasi dan menghindari Curse of Dimensionality, maka dilakukan reduksi dimensi dengan MCA

Data di reduksi menjadi 15 component, dimana 15 component ini sudah mewakili 70% variance dari data


![image-32](https://github.com/user-attachments/assets/97e8a25b-bad9-4fab-9a71-e5c633e05b82)

MCA dimulai dengan membuat tabel kontingensi yang menunjukkan frekuensi setiap kemungkinan kombinasi kategori. Hasil MCA tidak perlu standardisasi karena semua data input-nya sudah dalam bentuk kategori (dan MCA berbasis kontingensi, bukan magnitudo nilai).

|      |     0    |     1    |     2    |     3    |     4    |     5    |     6    |     7    |     8    |     9    |    10    |    11    |    12    |    13    |    14    |
|------|----------|----------|----------|----------|----------|----------|----------|----------|----------|----------|----------|----------|----------|----------|----------|
| 0    | 0.413792 | 0.592722 | 0.276242 | 0.290534 | -0.37062 | 0.036731 | -0.27249 | 0.024652 | 0.13217  | 0.049344 | 0.106787 | 0.256379 | 0.073947 | -0.40611 | 0.11311  |
| 1    | -0.12258 | 0.208008 | -0.0234  | 0.216761 | -0.13368 | 0.257708 | -0.03219 | -0.26835 | -0.16364 | -0.21098 | -0.15185 | 0.058636 | -0.06352 | 0.153469 | -0.1541  |
| 2    | 0.191027 | 0.264639 | 0.12057  | 0.108887 | -0.37632 | 0.713616 | 0.473802 | 0.712143 | 0.548942 | -0.14092 | -0.28271 | 0.542898 | 0.155323 | 0.273344 | 0.859573 |
| 3    | -0.12246 | 0.189967 | 0.272828 | -0.00789 | -0.14469 | 0.714824 | 0.522568 | 0.774958 | 0.818444 | -0.06302 | -0.18179 | 0.935621 | -0.17443 | 0.37513  | 0.18218  |
| 4    | 0.248219 | 0.093442 | -0.0744  | 0.074152 | -0.04063 | 0.582715 | 0.506819 | 0.474749 | 0.318223 | -0.21423 | -0.01464 | 0.445559 | -0.01548 | 0.709075 | 0.210749 |
| ...  | ...      | ...      | ...      | ...      | ...      | ...      | ...      | ...      | ...      | ...      | ...      | ...      | ...      | ...      | ...      |
| 11050| 1.140831 | -0.67315 | -0.47967 | -0.31236 | 0.034715 | -0.44842 | -0.05513 | 0.064994 | -0.24066 | 0.317483 | 0.102541 | 0.145976 | -0.03515 | -0.10381 | -0.03064 |
| 11051| 1.010727 | 0.163684 | -0.14266 | 0.711404 | 0.608659 | 0.058099 | 0.28917  | 0.108394 | 0.102585 | 0.159108 | -0.00359 | 0.389481 | 0.086155 | -0.38595 | -0.19520 |
| 11052| -0.32502 | -0.1162  | -0.11285 | 0.000981 | -0.00348 | 0.142939 | 0.182136 | -0.05859 | -0.1643  | -0.02456 | -0.00393 | -0.02745 | 0.019224 | -0.15852 | 0.108815 |
| 11053| 0.332252 | -0.25738 | 0.306381 | -0.21617 | 0.004726 | -0.1161  | 0.10296  | -0.2224  | 0.077887 | -0.0153  | -0.0095  | 0.243049 | 0.061624 | -0.29625 | 0.106965 |
| 11054| -0.12244 | -0.0525  | 0.604861 | -0.11056 | -0.15805 | -0.02131 | -0.15313 | -0.08825 | 0.240592 | 0.262746 | 0.040137 | 0.096132 | -0.07104 | -0.04182 | 0.074492 |


### Splitting Data
Rasio split 80:20 dipilih karena merupakan praktik umum dalam machine learning untuk membagi data menjadi data latih (train) dan data uji (test). Dengan 80% data digunakan untuk pelatihan, model dapat mempelajari pola secara optimal dari data yang cukup banyak. Sementara 20% sisanya digunakan untuk menguji performa model secara objektif pada data yang belum pernah dilihat sebelumnya, sehingga dapat mengukur generalisasi model dengan baik. Rasio ini juga menjaga agar data uji tetap cukup besar untuk menghasilkan evaluasi yang representatif, tanpa mengorbankan jumlah data latih yang dibutuhkan model.
```
Total # of sample in whole dataset: 11055
Total # of sample in train dataset: 8844
Total # of sample in test dataset: 2211
```


## Modeling

Pada tahap awal, tiga algoritma klasifikasi diterapkan sebagai baseline: **Logistic Regression**, **Random Forest**, dan **XGBoost**. Setiap model dijalankan dengan parameter dasar (default) yaitu
- `max_iter = 1000` untuk logistic regression
- `eval_metric = logloss`  di Xgboost 
- `random_state` di setiap model untuk memastikan reprodusibilitas hasil.

#### 1. Logistic Regression
Logistic Regression adalah model linear yang digunakan untuk klasifikasi biner. Model ini memprediksi probabilitas suatu sampel termasuk ke dalam kelas tertentu menggunakan fungsi logit (sigmoid). Logistic Regression mudah diinterpretasikan dan efisien untuk data dengan hubungan linier antara fitur dan target.

**Kelebihan:**  
- Sederhana, cepat, dan mudah diinterpretasikan  
- Tidak mudah overfitting pada dataset kecil  
- Cocok untuk baseline

**Kekurangan:**  
- Kurang efektif untuk data dengan hubungan non-linier  
- Sensitif terhadap multikolinearitas dan outlier

#### 2. Random Forest
Random Forest adalah algoritma ensemble berbasis pohon keputusan (decision tree) yang membangun banyak pohon secara acak dan menggabungkan hasilnya (voting) untuk meningkatkan akurasi dan stabilitas prediksi. Model ini mampu menangani data dengan fitur kategorikal dan non-linieritas.

**Kelebihan:**  
- Tahan terhadap overfitting  
- Dapat menangani data dengan banyak fitur dan interaksi kompleks  
- Memberikan estimasi feature importance

**Kekurangan:**  
- Interpretasi model lebih sulit dibandingkan model linear  
- Membutuhkan sumber daya komputasi lebih besar

#### 3. XGBoost
XGBoost (Extreme Gradient Boosting) adalah algoritma boosting berbasis pohon yang membangun model secara bertahap, di mana setiap pohon baru memperbaiki kesalahan pohon sebelumnya. XGBoost terkenal dengan performa tinggi dan efisiensi komputasi.

**Kelebihan:**  
- Akurasi tinggi pada banyak kasus  
- Mendukung regularisasi untuk mengurangi overfitting  
- Efisien dan scalable

**Kekurangan:**  
- Parameter tuning lebih kompleks  
- Interpretasi hasil lebih sulit

### Pemilihan Model Terbaik

Berdasarkan hasil baseline, **Random Forest** menunjukkan performa terbaik pada data test.

	                 LogReg	       RandomForest      XGBoost
    train_acc	0.930348	0.990615	0.990615
    test_acc	0.933062	0.963817	0.961556

Random Forest dipilih sebagai model terbaik karena memberikan akurasi tertinggi pada data uji (test set)dibandingkan dengan Logistic Regression dan XGBoost.Berdasarkan hasil evaluasi, Random Forest mencapaiakurasi 96.38% pada data test, sedikit lebih tinggi dari XGBoost (96.16%) dan secara signifikan lebih baik dari Logistic Regression (93.31%). Selain itu,Random Forest juga menunjukkan performa yang sangat baik pada data latih (train set), menandakan model mampu belajar pola data dengan baik tanpa overfitting yang berlebihan.


Tahapan ini membahas mengenai model machine learning yang digunakan untuk menyelesaikan permasalahan. Anda perlu menjelaskan tahapan dan parameter yang digunakan pada proses pemodelan.

## Evaluation

Pada proyek ini, metrik utama yang digunakan untuk mengevaluasi performa model adalah **akurasi**. Akurasi mengukur proporsi prediksi yang benar (baik positif maupun negatif) dibandingkan dengan seluruh jumlah prediksi yang dilakukan oleh model.

#### Formula Akurasi

Akurasi dihitung dengan rumus berikut:

$$
\text{Akurasi} = \frac{\text{Jumlah Prediksi Benar}}{\text{Total Jumlah Prediksi}}
$$

atau secara spesifik pada kasus klasifikasi biner:

$$
\text{Akurasi} = \frac{TP + TN}{TP + TN + FP + FN}
$$

di mana:
- **TP** = True Positive (prediksi positif yang benar)
- **TN** = True Negative (prediksi negatif yang benar)
- **FP** = False Positive (prediksi positif yang salah)
- **FN** = False Negative (prediksi negatif yang salah)


Berdasarkan hasil evaluasi pada data uji (test set), model **Random Forest** memberikan akurasi tertinggi yaitu **96.38%**. Artinya, dari seluruh data uji, sebanyak 96.38% prediksi yang dilakukan oleh model adalah benar (baik mendeteksi website phishing maupun non-phishing). Model XGBoost juga menunjukkan performa yang sangat baik dengan akurasi 96.16%, sedangkan Logistic Regression memperoleh akurasi 93.31%.

Akurasi yang tinggi ini menunjukkan bahwa model yang dibangun sangat efektif dalam membedakan antara website phishing dan non-phishing pada dataset yang digunakan. Dengan demikian, sistem prediksi otomatis yang dikembangkan dapat diandalkan untuk membantu mendeteksi website phishing secara akurat dan efisien.


Sebagai contoh, Anda memiih kasus klasifikasi dan menggunakan metrik **akurasi, precision, recall, dan F1 score**. Jelaskan mengenai beberapa hal berikut:
- Penjelasan mengenai metrik yang digunakan
- Menjelaskan hasil proyek berdasarkan metrik evaluasi

Ingatlah, metrik evaluasi yang digunakan harus sesuai dengan konteks data, problem statement, dan solusi yang diinginkan.

## Feature Importance

![image-33](https://github.com/user-attachments/assets/fa194ed4-1d8b-4169-b969-0ac31b3965ea)

Dari model terbaik yang dipilih, yaitu random forest. fitur yang paling berpengaruh itu adalah fitur `component 2`. `component 2` ini merupakan hasil dari reduksi fitur MCA.

    0     double_slash_redirecting__-1          2.106617
    1           shortining_service__-1          2.093770
    2                 abnormal_url__-1          1.715935
    3                  https_token__-1          1.713671
    4                      redirect__1          1.608653
    5                   rightclick__-1          1.303717
    6                       iframe__-1          1.178926
    7                         port__-1          1.013857

Lalu fitur fitur yang banyak berkontribusi untuk `component 2` ini yaitu double_slash_redirecting dan fitur fitur lain yang ada di tabel diatas. Data itu didapat dari `column_coordinates` dari hasil reduksi dengan MCA

Dapat dilihat bahwa double slash dan shortining service memiliki kontribusi besar di componen 2, sehingga dapat menjadi indikasi bahwa fitur-fitur tersebut berperan penting dalam membedakan antara URL phishing dan non-phishing. Oleh karena itu, fitur-fitur ini layak mendapat perhatian lebih dalam proses deteksi phishing, baik untuk analisis lanjutan maupun pengembangan model yang lebih akurat dan interpretatif.


## Referensi
[1] Anti-Phishing Working Group, “Phishing Activity Trends Report Q1 2023,” 2023. [Online]. Available: https://apwg.org/trendsreports/

[2] M. Aburrous, M. A. Hossain, K. Dahal, and F. Thabtah, “Phishing websites dataset,” UCI Machine Learning Repository, 2015. [Online]. Available: https://archive.ics.uci.edu/dataset/327/phishing+websites

[3] N. Abdelhamid, A. Ayesh, and F. Thabtah, “Phishing detection: A recent intelligent machine learning comparison based on models content and features,” Ain Shams Engineering Journal, vol. 5, no. 6, pp. 1211–1218, 2014. doi: 10.1016/j.asej.2014.11.010

[4] A. K. Jain and B. B. Gupta, “Phishing detection: Analysis of visual similarity-based approaches,” Security and Privacy, vol. 1, no. 1, 2018. doi: 10.1002/spy2.9

