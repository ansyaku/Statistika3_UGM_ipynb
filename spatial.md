# Data Spasial: Konsep Dasar dan Lanjutan

Data spasial adalah jenis data yang memperhatikan **lokasi dalam ruang**. Dalam konteks analisis ekonomi, sosial, atau lingkungan, data ini penting karena **karakteristik suatu lokasi sering dipengaruhi oleh lokasi sekitarnya**.

Tiga konsep utama:

1. **Data Spasial**
2. **Ketergantungan Spasial**
3. **Autokorelasi Spasial**
4. **Heterogenitas Spasial**

---

## 1. Data Spasial

**Definisi**:  
Data *cross-sectional* yang memiliki referensi lokasi (titik atau wilayah) dalam ruang.

### Jenis:
- **Titik (Point-based)**: Lokasi dengan koordinat spesifik.  
  _Contoh_: Harga rumah di koordinat X,Y.
- **Wilayah (Areal-based)**: Lokasi diwakili area administratif, biasanya pakai _centroid_.  
  _Contoh_: PDRB per kabupaten, kepadatan penduduk per kecamatan.

> **Implikasi**: Observasi membawa informasi spasial → tidak bisa dianggap independen.
## Contoh **Point-based** dan **Areal-based**

| Tipe Spasial   | Ciri Utama                         | Contoh Ekonomi/Sosial                                   |
|----------------|-------------------------------------|----------------------------------------------------------|
| **Point-based** | Setiap titik = 1 koordinat (x, y)   | - Harga rumah (per titik koordinat) <br> - Lokasi kecelakaan lalu lintas <br> - Lokasi ATM atau sekolah |
| **Areal-based** | Setiap observasi = 1 wilayah aglomerasi | - PDRB per kabupaten <br> - Tingkat kemiskinan per kecamatan <br> - Hasil panen per desa |

---

## Contoh Data Nyata (Simplified):

### *Point-based*:

| ID Rumah | Latitude | Longitude | Harga Rumah (juta) |
|----------|----------|-----------|---------------------|
| R1       | -6.201   | 106.845   | 750                 |
| R2       | -6.215   | 106.810   | 920                 |
| R3       | -6.205   | 106.825   | 870                 |

> Di sini setiap rumah punya **lokasi unik (x, y)** → bisa dipetakan secara presisi.

---

### *Areal-based*:

| Kabupaten | PDRB per Kapita (juta) | Koordinat Centroid |
|-----------|-------------------------|---------------------|
| Bandung   | 50                      | (-6.9, 107.6)       |
| Garut     | 35                      | (-7.4, 107.9)       |
| Cianjur   | 40                      | (-6.8, 107.2)       |

> Lokasi ditentukan berdasarkan **wilayah administratif**, direpresentasikan dengan titik **centroid**-nya (pusat area). Tapi secara spasial yang lebih lengkap dan realistis, kamu bisa (dan sebaiknya) mengganti atau melengkapi ini dengan batas-batas wilayah dari shapefile (polygon area).

---

## Implikasi Analisis:

- **Point-based** → lebih detail → cocok untuk interpolasi, kernel density, dsb.
- **Areal-based** → lebih umum dalam ekonomi regional dan sosial → cocok untuk regresi spasial (SAR, SEM), agregasi kebijakan.

---





## Konsep Kontiguitas
**Kontiguitas** adalah konsep spasial yang merujuk pada **ketetanggaan antar unit geografis**, seperti desa, kecamatan, atau kabupaten. Konsep ini sangat penting dalam analisis spasial karena menyatakan bahwa unit yang **berbatasan secara langsung** kemungkinan memiliki hubungan atau kemiripan yang lebih kuat dibandingkan unit yang berjauhan.

### Inti Konsep Kontiguitas:

1. **Mencerminkan Konfigurasi Geografis**  
   Kontiguitas menggambarkan bagaimana satu lokasi (unit spasial) **berhubungan secara posisi geografis** dengan lokasi-lokasi lain di sekitarnya dalam suatu sistem $S$.

2. **Berdasarkan Peta Wilayah**  
   Matriks kontiguitas dibentuk dari peta wilayah aktual, dan struktur spasialnya mencerminkan **siapa bertetangga dengan siapa** (adjacency).  
   Contoh: Kecamatan A berbatasan dengan Kecamatan B dan C → berarti A bertetangga dengan B dan C.

3. **Asumsi Ketergantungan Lebih Tinggi**  
   Unit-unit yang bertetangga diasumsikan punya **ketergantungan spasial lebih tinggi**. Misalnya:
   - Harga rumah di dua kecamatan yang bertetangga lebih mungkin mirip
   - Tingkat kemiskinan antar desa berdekatan mungkin saling memengaruhi

4. **Keragaman Lebih Rendah antar Tetangga**  
   Karena berbagi karakteristik geografis dan sosial yang sama (akses jalan, infrastruktur, budaya, dll), unit yang bertetangga cenderung punya nilai variabel yang **relatif sama** → disebut juga **positive spatial autocorrelation**.


## 2. Ketergantungan Spasial

**Definisi**:  
Nilai suatu variabel di satu lokasi **tergantung** pada nilai variabel yang sama di lokasi lain, terutama yang berdekatan.


### Ciri-ciri:
- Tidak independen antar lokasi
- Terkait **jarak antar lokasi**
- **Semakin dekat → semakin tergantung**
- Ketergantungan merupakan **fungsi negatif dari jarak**


## Contoh Nyata Ketergantungan Spasial

### 1. Harga Tanah atau Properti
## Contoh 1: Harga Rumah dan Lokasi

- Terlepas dari desain dan ukuran, **harga rumah tergantung pada lokasinya**.
  - Semakin jauh dari pusat kota, harga rumah cenderung menurun secara rata-rata.
- Harga rumah juga tergantung dari **situasi di sekeliling rumah tersebut**, misalnya keberadaan fasilitas umum dalam radius tertentu: sekolah, pasar, jalan utama, dsb.
- Karena rumah-rumah tersebut **memanfaatkan fasilitas yang sama**, maka rumah-rumah yang **berdekatan** cenderung memiliki harga yang mirip.
- **Kemiripan harga** di antara rumah-rumah yang berdekatan adalah indikasi adanya **ketergantungan spasial**.
- Di sisi lain, keberadaan fasilitas adalah karakteristik yang bisa **berbeda-beda antar lokasi**, sehingga memberikan dimensi **heterogenitas spasial** juga.


### 2. Polusi Udara (PM2.5)
- Jika satu kecamatan memiliki tingkat polusi udara tinggi, kecamatan tetangganya juga cenderung terpengaruh karena angin dan sebaran atmosfer.
- Artinya: nilai PM2.5 di suatu lokasi **dipengaruhi oleh tetangganya**.

### 3. Kasus Penyakit Menular
- Ketika terjadi wabah demam berdarah di Kecamatan X, kecamatan tetangga (Y dan Z) juga menunjukkan peningkatan kasus dalam waktu dekat.
- Hal ini menunjukkan **ketergantungan spasial dalam penyebaran penyakit.**

---

### Ringkasan:
> Ketergantungan spasial membuat nilai di suatu lokasi **tidak bisa dianggap independen** dari lokasi-lokasi di sekitarnya, dan **jarak geografis** menjadi faktor utama dalam mengukur kekuatan pengaruh ini.

---

## 3. Autokorelasi Spasial Positif

**Definisi**:  
Lokasi yang berdekatan menunjukkan **kemiripan karakteristik**.

### Ciri-ciri:
- Terjadi **clustering** nilai serupa
- Warna/pola yang mirip di peta menunjukkan **autokorelasi positif**
- Mengindikasikan adanya hubungan spasial

*Spatial autocorrelation* dapat dituliskan dalam persamaan:

$$
\text{cov}(y_i, y_j) = \mathbb{E}[y_i y_j] - \mathbb{E}[y_i] \cdot \mathbb{E}[y_j] \neq 0 \quad \text{untuk } i \ne j
$$

Dimana:
- $i, j$ mengacu kepada observasi (lokasi)
- $y_i$ dan $y_j$ adalah nilai variabel acak pada lokasi tersebut

## Contoh Autokorelasi Spasial Positif (Nyata)

### 1. Harga Rumah di Perkotaan
- Rumah-rumah di kawasan elit (misalnya Pondok Indah) punya harga tinggi dan berdekatan satu sama lain.
- Di sisi lain, rumah di pinggiran kota punya harga rendah, dan juga berkelompok.

> → Harga rumah tinggi berkelompok dengan tinggi, harga rendah dengan rendah  
> → Ini autokorelasi spasial positif

---

### 2. Tingkat Kemiskinan antar Kecamatan
- Kecamatan A, B, C saling berbatasan dan semuanya memiliki tingkat kemiskinan tinggi.
- Kecamatan D, E, F di daerah pusat kota memiliki tingkat kemiskinan rendah.

> → Nilai mirip (kemiskinan tinggi atau rendah) terkonsentrasi secara spasial

---

### 3. NDVI (Vegetasi) di Citra Satelit
- Di peta satelit, daerah hutan lebat memiliki nilai NDVI tinggi, dan lokasinya saling berdempetan.
- Lahan gundul atau area industri punya NDVI rendah dan juga berdekatan.

---

## Hubungan Statistik

Diukur menggunakan:

- **Moran’s I > 0** → korelasi positif antar nilai tetangga
- **Geary’s C < 1** → mendukung korelasi positif


---

## 4. Heterogenitas Spasial

**Definisi**:  
Heterogenitas spasial terjadi saat hubungan antara variabel X dan Y berbeda-beda di tiap lokasi.
Artinya, koefisien regresi tidak sama di semua tempat.

### Ciri-ciri:
- Hubungan antar variabel berbeda per lokasi
- Menunjukkan **ketidakstabilan struktural**
- **Tidak cocok** jika menggunakan model global (OLS)

### Solusi:
- Gunakan model dengan parameter berbeda per lokasi, seperti:
  - **Geographically Weighted Regression (GWR)**
- Uji **Breusch-Pagan** untuk heteroskedastisitas spasial
## Contoh Heterogenitas Spasial

### Contoh 1: Efek Pendidikan terhadap Pendapatan

**Di kota besar:**
- Tambahan 1 tahun pendidikan → kenaikan pendapatan bisa besar (karena banyak lapangan kerja formal).
- Koefisien β untuk pendidikan = besar dan signifikan.

**Di daerah rural:**
- Tambahan 1 tahun pendidikan → mungkin tidak terlalu meningkatkan pendapatan (karena sebagian besar pekerjaan berbasis pertanian informal).
- Koefisien β = rendah atau tidak signifikan.

> → Hubungan antara pendidikan dan pendapatan berbeda secara spasial.

---

### Contoh 2: Pengaruh Akses Jalan terhadap Kemiskinan

**Di kota:**
- Jalan bagus → tidak banyak pengaruh, karena semua wilayah relatif mudah diakses.
- Koefisien negatif tapi kecil.

**Di daerah terpencil:**
- Jalan bagus → sangat penting untuk akses pasar, pendidikan, dan layanan.
- Koefisien negatif besar (akses jalan sangat mengurangi kemiskinan).

> → Model OLS global tidak bisa menangkap perbedaan ini. Butuh model lokal (seperti GWR).

---

### Contoh 3: Efek Urbanisasi terhadap Harga Lahan

**Di daerah industri:**
- Urbanisasi → langsung menaikkan harga lahan karena permintaan tinggi dari perusahaan.
- Koefisien urbanisasi tinggi dan positif.

**Di kota wisata:**
- Urbanisasi → tidak selalu menaikkan harga (karena preferensi wisatawan bisa berbeda).
- Koefisien bisa lebih kecil atau bahkan tidak signifikan.

> → Efek urbanisasi terhadap harga lahan tidak seragam.

---

## Ringkasan

| Konsep                    | Inti                                                        | Implikasi                                                   |
|--------------------------|-------------------------------------------------------------|--------------------------------------------------------------|
| **Data Spasial**         | Observasi punya referensi lokasi                            | Lokasi penting, bisa jadi sumber ketergantungan              |
| **Ketergantungan Spasial** | Lokasi saling memengaruhi                                  | Model harus memperhatikan hubungan geografis                 |
| **Autokorelasi Spasial** | Lokasi berdekatan cenderung mirip                           | Bisa diuji statistik, penting untuk validitas model           |
| **Heterogenitas Spasial**| Hubungan antar variabel berbeda antar lokasi                | Butuh model fleksibel seperti GWR                            |


## Beberapa Hasil Penelitian dengan Konsep Ketergantungan Spasial

- **Fitriani, R., Diartho, H. C., dan Hadiningrum, S.** (2020).  
  *Growth externalities on the environmental quality index of East Java Indonesia, spatial econometrics model of STIRPAT*.  
  *Indonesian Journal of Statistics and Its Applications*, 6(1), hal. 216–233.  
  doi: [10.29244/ijsa.v4i1.628](https://doi.org/10.29244/ijsa.v4i1.628)  
  - **Topik**: Memodelkan kualitas lingkungan

- **Fitriani, R., Pusdiktasari, Z. F., dan Diartho, H. C.** (2020).  
  *The dynamic of 2011–2016 East Java’s regional spatial growth: An exploratory spatial data analysis*.  
  *Journal of Economics and Business*, 3(2).  
  doi: [10.31014/aior.1992.03.02.252](https://doi.org/10.31014/aior.1992.03.02.252)  
  - **Topik**: Memodelkan pertumbuhan ekonomi regional

- **Fitriani, R. dan Sumarminingsih, E.** (2015).  
  *Spatial extent of land use externalities in the Jakarta fringe: Spatial econometric analysis*.  
  *Review of Urban & Regional Development Studies*, 27(3), hal. 230–242.  
  doi: [10.1111/rurd.12041](https://doi.org/10.1111/rurd.12041)  
  - **Topik**: Analisis eksternalitas penggunaan lahan di kawasan pinggiran Jakarta
---


# Dasar Analisis Spasial: Matriks Pembobot dan Tiga Konsep Utama

---

## 1. Matriks Pembobot Spasial (Spatial Weight Matrix)

### Apa itu Matriks Pembobot Spasial (W)?

Matriks pembobot spasial $W$ adalah representasi numerik dari **struktur hubungan antar lokasi**. Matriks ini menunjukkan siapa yang memengaruhi siapa dalam konteks spasial.

- Ukuran: $N \times N$, dengan $N$ adalah jumlah unit spasial (misal: kabupaten, desa, atau titik koordinat)
- Elemen $w_{ij}$: menyatakan seberapa besar lokasi $j$ memengaruhi lokasi $i$

### Jenis Matriks Pembobot:

1. **Contiguity-based (berdasarkan ketetanggaan)**
   - $w_{ij} = 1$ jika i dan j bertetangga, 0 jika tidak
   - Cocok untuk data areal seperti kabupaten/kecamatan
   - Contoh metode:

### Queen Contiguity: Lokasi dianggap bertetangga jika berbagi sisi atau sudut.
   		- **Queen contiguity** adalah metode penentuan hubungan ketetanggaan spasial di mana dua lokasi dianggap bertetangga jika **berbagi sisi _atau_ sudut**.
   		- Ciri-ciri:
   		- Jika dua unit spasial memiliki **minimal satu sisi atau titik sudut yang sama**, maka mereka bertetangga.
   		- Dalam grid 3x3, satu lokasi pusat (misalnya A) bisa memiliki **maksimum 8 tetangga**.

#### Contoh visual:
Lokasi **A** bertetangga dengan:
- **B1, B2, B3, B4** (atas, bawah, kiri, kanan)
- **C1, C2, C3, C4** (diagonal)

### Operator Spatial Lag: $WX$

Matriks pembobot spasial $W$ bisa digunakan untuk **memboboti variabel** spasial $X$ yang diasumsikan memiliki ketergantungan spasial. Operasi ini disebut sebagai **spatial lag**.

#### Notasi:
- $W$: Matriks pembobot
- $X$: Vektor variabel spasial
- \$WX$: Hasil pembobotan — nilai tertimbang dari tetangga

#### Interpretasi:
$$
WX_i = \sum_j w_{ij} X_j
$$
Artinya, nilai $WX_i$ adalah rata-rata tertimbang dari tetangga-tetangga unit $i$.

#### Contoh:

$$
W = \begin{bmatrix}
0 & \frac{1}{2} & \frac{1}{2} & 0 & 0 \\
\frac{1}{3} & 0 & \frac{1}{3} & \frac{1}{3} & 0 \\
\frac{1}{3} & \frac{1}{3} & 0 & \frac{1}{3} & 0 \\
0 & \frac{1}{3} & \frac{1}{3} & 0 & \frac{1}{3} \\
0 & 0 & 0 & \frac{1}{2} & \frac{1}{2}
\end{bmatrix}, \quad
X = \begin{bmatrix}
X_1 \\ X_2 \\ X_3 \\ X_4 \\ X_5
\end{bmatrix}
$$

Maka hasil:

$$
WX = \begin{bmatrix}
\frac{X_2 + X_3}{2} \\
\frac{X_1 + X_3 + X_4}{3} \\
\frac{X_1 + X_2 + X_4}{3} \\
\frac{X_2 + X_3 + X_5}{3} \\
\frac{X_4 + X_5}{2}
\end{bmatrix}
$$

---

### Hubungan dengan Time Lag

Operator $WX$ dalam analisis spasial berperan serupa dengan **time lag** $X_{t-k}$ dalam analisis deret waktu:

- **Time series**: $X_{t-k}$ → informasi masa lalu
- **Spatial**: $WX$ → informasi dari tetangga spasial

---

> Spatial lag sangat penting dalam model-model spasial seperti:
> - Spatial Autoregressive Model (SAR)
> - Spatial Durbin Model (SDM)
> - Analisis Moran’s I dan deteksi autokorelasi spasial
---

### Rook Contiguity: Lokasi dianggap bertetangga hanya jika berbagi sisi.
   		- Untuk **grid yang tidak teratur**, dua lokasi dikatakan **bertetangga** jika **berbagi batas wilayah** (sisi langsung).
   		- Sebagai contoh, perhatikan susunan lokasi berikut:
   		- Konsep ketetanggaan antar lokasi 1 sampai 5 tersebut dapat dinyatakan dalam bentuk **matriks kontiguitas spasial** sebagai berikut:

---

$$
C =
\begin{bmatrix}
0 & 1 & 1 & 0 & 0 \\
1 & 0 & 1 & 1 & 0 \\
1 & 1 & 0 & 1 & 0 \\
0 & 1 & 1 & 0 & 1 \\
0 & 0 & 0 & 1 & 0 \\
\end{bmatrix}
$$

> Matriks ini menunjukkan hubungan "tetangga langsung" antar lokasi (berbagi sisi). Nilai `1` berarti bertetangga, sedangkan `0` berarti tidak bertetangga.

2. **Distance-based**
   - Bobot dihitung dari jarak antar lokasi, contoh:
     $$
     w_{ij} =
     \begin{cases}
     \frac{1}{d_{ij}} & \text{jika } d_{ij} < d_{\text{band}} \\
     0 & \text{jika } d_{ij} \ge d_{\text{band}}
     \end{cases}
     $$
   - Cocok untuk data titik koordinat (point-based)

---

## 2. Tujuan Penggunaan Matriks Pembobot

Matriks $W$ digunakan untuk mendefinisikan struktur spasial yang diperlukan dalam berbagai analisis:

| Tujuan / Analisis                      | Perlu $W$? | Keterangan                                         |
|----------------------------------------|----------------|----------------------------------------------------|
| Representasi hubungan spasial          | Ya             | Untuk mendefinisikan siapa bertetangga             |
| Model regresi dengan efek spasial      | Ya             | Seperti SAR, SEM, atau SDM                        |
| Uji autokorelasi spasial               | Ya             | Digunakan dalam perhitungan Moran’s I, Geary’s C  |
| Model lokal seperti GWR                | Kadang         | Biasanya memakai fungsi bobot berbasis jarak      |

---

## 3. Tiga Konsep Utama dalam Analisis Spasial

### 3.1. Ketergantungan Spasial (Spatial Dependence)

- **Definisi**: Nilai suatu variabel di satu lokasi dipengaruhi oleh nilai yang sama di lokasi lain yang berdekatan.
- **Contoh**: Harga tanah di suatu wilayah dipengaruhi oleh harga tanah di wilayah tetangga.
- **Model Terkait**: Spatial Autoregressive Model (SAR), Spatial Error Model (SEM)
- **Penggunaan $W$**: Wajib, sebagai dasar struktur pengaruh antar lokasi

### 3.2. Autokorelasi Spasial (Spatial Autocorrelation)

- **Definisi**: Kemiripan nilai antar lokasi yang berdekatan; lokasi yang dekat cenderung memiliki nilai yang mirip.
- **Contoh**: Kecamatan dengan tingkat kemiskinan tinggi cenderung berdekatan dengan kecamatan lain yang juga miskin.
- **Indikator Statistik**: Moran’s I, Geary’s C
- **Penggunaan $W$**: Wajib, karena diperlukan untuk mengukur pola spasial

### 3.3. Heterogenitas Spasial (Spatial Heterogeneity)

- **Definisi**: Hubungan antara variabel (misalnya X dan Y) berbeda-beda tergantung lokasi.
- **Contoh**: Pengaruh pendidikan terhadap pendapatan besar di kota tapi kecil di desa.
- **Model Terkait**: Geographically Weighted Regression (GWR), Multiscale GWR
- **Penggunaan $W$**: Tidak selalu dalam bentuk matriks klasik, tetapi bobot berbasis jarak (misalnya kernel) tetap digunakan

---

## 4. Hubungan Antara Ketiga Konsep

| Konsep                      | Tujuan Analisis                    | Gunakan $W$? |
|-----------------------------|------------------------------------|------------------|
| Ketergantungan Spasial      | Menyatakan pengaruh antar lokasi   | Ya               |
| Autokorelasi Spasial        | Mengukur kemiripan antar lokasi    | Ya               |
| Heterogenitas Spasial       | Menyadari variasi lokal dalam hubungan antar variabel | Kadang            |

---

## 5. Penutup

Matriks pembobot spasial $W$ adalah **pondasi** dalam analisis spasial. Pemahaman terhadap $W$ sangat penting untuk melangkah ke:
- Uji statistik spasial
- Model regresi spasial

## Struktur Umum Matriks Pembobot

### 1. Matriks Awal $C$

Berisi nilai kedekatan atau hubungan antar unit:

$$
C =
\begin{bmatrix}
c_{11} & c_{12} & \cdots & c_{1N} \\
c_{21} & c_{22} & \cdots & c_{2N} \\
\vdots & \vdots & \ddots & \vdots \\
c_{N1} & c_{N2} & \cdots & c_{NN}
\end{bmatrix}
$$

### 2. Dikonversi ke $W$ dengan standar baris (row-standardized)

$$
w_{ij} = \frac{c_{ij}}{\sum_{j=1}^{n} c_{ij}} \quad \text{sehingga } \sum_j w_{ij} = 1
$$


## Normalisasi Matriks Pembobot Spasial (Row-Standardized)

Matriks pembobot spasial $W$ diperoleh dari matriks kedekatan $C$ dengan membagi setiap elemen baris dengan total elemen pada baris tersebut:

$$
\mathbf{W} =
\begin{bmatrix}
\frac{c_{11}}{c_1.} & \frac{c_{12}}{c_1.} & \cdots & \frac{c_{1N}}{c_1.} \\
\frac{c_{21}}{c_2.} & \frac{c_{22}}{c_2.} & \cdots & \frac{c_{2N}}{c_2.} \\
\vdots & \vdots & \ddots & \vdots \\
\frac{c_{N1}}{c_N.} & \frac{c_{N2}}{c_N.} & \cdots & \frac{c_{NN}}{c_N.}
\end{bmatrix}
$$

$$=
\mathbf{W} =
\begin{bmatrix}
w_{11} & w_{12} & \cdots & w_{1N} \\
w_{21} & w_{22} & \cdots & w_{2N} \\
\vdots & \vdots & \ddots & \vdots \\
w_{N1} & w_{N2} & \cdots & w_{NN}
\end{bmatrix}
$$

Setiap baris pada $W$ dijamin menjumlahkan ke 1:

$$
\sum_{j=1}^{n} w_{ij} = 1
$$



---

## Dua Jenis Matriks Pembobot Spasial

Bentuk Matriks Awal $C$: Sama-sama Matriks $N \times N$.
Semua pendekatan (baik kontiguitas maupun jarak) akan menghasilkan matriks persegi $C$ yang berukuran $N \times N$, di mana:

- Baris = lokasi target (unit spasial ke-$i$)
- Kolom = tetangga/pengaruh (unit spasial ke-$j$)
- Isi matriks = ukuran kedekatan antara lokasi $i$ dan $j$. Isi Matriks $C$ berbeda tergantung jenis. 

### 1. Kontiguitas (Contiguity-Based)

- Matriks $C$ berisi nilai biner:
- Berdasarkan batas geografis (siapa bertetangga)
- Adjacency 1 = bertetangga  
- Adjacency 0 = tidak bertetangga  
- Contoh metode: Queen Contiguity, Rook Contiguity
- Cocok untuk data wilayah administratif (misalnya antar kabupaten)
$$
c_{ij} =
\begin{cases}
1 & \text{jika lokasi } i \text{ dan } j \text{ bertetangga} \\
0 & \text{jika tidak bertetangga}
\end{cases}
$$

- Sumbernya: dari adjacency (batas wilayah)


## Contoh Matriks Pembobot: Contiguity (1)

Contoh: $7 \times 7$ matriks pembobot spasial $W$ menggunakan *first order contiguity relations* untuk 7 wilayah:

$$
C =
\begin{bmatrix}
0 & 1 & 0 & 0 & 0 & 0 & 0 \\
1 & 0 & 1 & 0 & 0 & 0 & 0 \\
0 & 1 & 0 & 1 & 0 & 0 & 0 \\
0 & 0 & 1 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 1 & 0 & 1 & 0 \\
0 & 0 & 0 & 0 & 1 & 0 & 1 \\
0 & 0 & 0 & 0 & 0 & 1 & 0 \\
\end{bmatrix}
$$

- Baris dan kolom merepresentasikan unit spasial: R1 hingga R7
- Nilai 1 menunjukkan bahwa dua wilayah bertetangga (adjacent)
- Nilai 0 menunjukkan tidak ada keterhubungan langsung

Matriks ini disebut **contiguity matrix**, dan digunakan sebagai input awal dalam membentuk matriks pembobot spasial $W$ melalui normalisasi baris.

### 2. Jarak (Distance-Based) - Inverse Distance Matrix

- Matriks $C$ berisi nilai berbobot berdasarkan jarak, misal:

  
$$
w_{ij} =
\begin{cases}
\frac{1}{d_{ij}} & \text{jika } d_{ij} < d_{\text{band}} \\
0 & \text{jika } d_{ij} \ge d_{\text{band}}
\end{cases}
$$
- Perlu ditentukan band (jarak maksimum) untuk mengelompokkan apakah dua lokasi saling berhubungan spasial atau tidak
- $d_{ij}$: jarak antar lokasi $i$ dan $j$
- $d_{\text{band}}$: batas maksimum untuk dianggap saling memengaruhi

Atau bisa juga menggunakan formulasi:

$$
c_{ij} = r_{ij}^{-1}
$$

- Sumbernya: dari koordinat (latitude/longitude) antar lokasi
- Cocok untuk data titik koordinat (point-based), misalnya lokasi rumah, ATM, sekolah


### Asumsi Efek Global: Inverse Distance

Jika diasumsikan bahwa perubahan karakter atau sifat tertentu di suatu lokasi memberikan **efek global** kepada lokasi-lokasi lain (misalnya polusi, iklim, atau informasi), maka kita bisa gunakan pendekatan berbasis **inverse distance**.

Formulasi yang digunakan:

$$
c_{ij} = r_{ij}^{-1}
$$

**Keterangan:**

- $i$: indeks lokasi ke-$i$  
- $j$: indeks lokasi ke-$j$  
- $r_{ij}$: jarak antara lokasi $i$ dan $j$  
- Perhitungan jarak berdasarkan koordinat **easting** dan **northing** dari masing-masing lokasi.

---

### Asumsi Efek Lokal: Batas Jarak (Cut-Off Distance)

Jika efek dari suatu lokasi **hanya berlaku secara lokal** (misal hanya berpengaruh dalam radius tertentu), maka digunakan pendekatan binary cut-off berdasarkan jarak maksimum $r_0$.

Formulasi:

$$
c_{ij} = 
\begin{cases}
1, & \text{jika } r_{ij} \leq r_0 \\
0, & \text{selainnya}
\end{cases}
$$

**Keterangan:**

- $r_0$: adalah **jarak kritis maksimum** di mana pengaruh spasial masih dianggap ada  
- Cocok untuk membatasi efek spasial hanya pada **wilayah lokal** di sekitar lokasi  



## Autokorelasi Spasial

Statistik uji yang paling sering digunakan untuk menguji keberartian autokorelasi spasial adalah **Moran’s I**. Hipotesis yang diuji terhadap variabel $X$ adalah:

- $H_0$: Variabel $X$ **tidak** berautokorelasi spasial (bersifat random secara spasial)
- $H_1$: Variabel $X$ **ber**autokorelasi spasial

---
### Mengestimasi Tingkat Keterkaitan Spasial Untuk Variabel Ekonomi yang Diteliti

Rumus Moran’s I (Degree of Spatial Autocorrelation)

$$
I = \frac{n \sum_{i=1}^n \sum_{p=1}^n w_{ip}(x_i - \bar{x})(x_p - \bar{x})}
{\sum_{i=1}^n (x_i - \bar{x})^2}, \quad -1 \leq I \leq 1
$$

**Keterangan:**
- $I$: Nilai Moran’s I (degree of spatial autocorrelation)
- $n$: Jumlah observasi atau lokasi penelitian
- $x_i$: Nilai variabel ekonomi pada lokasi $i$
- $x_p$: Nilai variabel ekonomi pada lokasi $p$, di mana $p = i+1$ (daerah sekitar lokasi $i$)
- $\bar{x}$: Rata-rata dari semua $x_i$
- $w_{ip}$: Elemen dari matriks pembobot spasial antara lokasi $i$ dan $p$

---

### Menguji Tingkat Signifikasi Keterkaitan Spasial
Karena distribusi nilai Moran’s I dapat diasumsikan mendekati distribusi normal, maka dilakukan transformasi Z sebagai berikut:
Untuk menilai apakah nilai $I$ signifikan secara statistik, dilakukan transformasi ke skor Z berikut:

$$
Z_{\text{hitung}} = \frac{I - \mathbb{E}(I)}{\sqrt{\text{Var}(I)}} \sim \mathcal{N}(0,1)
$$


### Komponen Tambahan:

- **Ekspektasi (expected value):**

$$
\mathbb{E}(I) = -\frac{1}{n-1}
$$

- **Variansi Moran’s I:**

$$
\text{Var}(I) = \frac{n^2 S_1 - n S_2 + 3S_0^2}{(n^2 - 1) S_0^2} - \left[\mathbb{E}(I)\right]^2
$$

- **Komponen-komponen pendukung:**

$$
S_0 = \sum_{i=1}^n \sum_{p=1}^n w_{ip}
$$

$$
S_1 = \frac{1}{2} \sum_{i=1}^n \sum_{p=1}^n (w_{ip} + w_{pi})^2
$$

$$
S_2 = \sum_{i=1}^n \left( \sum_{p=1}^n w_{ip} + \sum_{p=1}^n w_{pi} \right)^2
$$

---

### Interpretasi:
- Nilai $I > 0$: Indikasi adanya **autokorelasi spasial positif** (lokasi yang berdekatan cenderung memiliki nilai yang mirip)
- Nilai $I < 0$: Indikasi adanya **autokorelasi spasial negatif**
- Nilai $I \approx 0$: Tidak ada keterkaitan spasial (random)
- Nilai statistik $Z$ dibandingkan terhadap distribusi normal standar.
- Keberartian autokorelasi ditentukan dari nilai-**p** yang diperoleh.

---

### Tujuan Uji
Pengujian Moran’s I bertujuan untuk melihat apakah **spatial weights** $W$ menghasilkan nilai yang secara signifikan berbeda dari nol (tidak ada hubungan spasial).

## Pengujian Spatial Autocorrelation

### Tujuan:
- **Mengestimasi** tingkat keterkaitan spasial pada variabel ekonomi (misalnya: kemiskinan, pertumbuhan ekonomi)
- **Mengukur signifikansi statistik** dari keterkaitan spasial tersebut


# Spatial Detection: Global dan Local Spatial Autocorrelation

Deteksi spasial dilakukan untuk mengetahui **apakah terdapat pola keterkaitan spasial antar wilayah**. Pola ini penting untuk:
- Mengidentifikasi **spatial clustering** (kumpulan wilayah dengan nilai serupa)
- Menentukan apakah suatu wilayah **berpengaruh terhadap wilayah lain secara spasial**

Terdapat dua pendekatan utama dalam deteksi spasial:

---

## 1. Global Spatial Autocorrelation Test

###  Tujuan:
Mengukur **derajat keterkaitan spasial secara keseluruhan** dari seluruh wilayah dalam studi.

###  Indikator yang umum digunakan:
- **Moran's I**  
  Nilai antara -1 (negatif) hingga +1 (positif).  
  - Positif → wilayah dengan nilai tinggi (rendah) cenderung bertetangga dengan wilayah bernilai tinggi (rendah).
  - Negatif → wilayah dengan nilai tinggi cenderung bertetangga dengan nilai rendah (dan sebaliknya).

- **Geary’s C**  
  Lebih sensitif terhadap perbedaan lokal.  
  Nilai < 1 menunjukkan autokorelasi positif, nilai > 1 menunjukkan autokorelasi negatif.

###  Interpretasi:
Jika nilai uji signifikan, berarti **nilai suatu variabel tidak tersebar acak secara spasial**, dan ada indikasi klasterisasi.

---

## 2. Local Spatial Autocorrelation Test

###  Tujuan:
Mengidentifikasi **lokasi spesifik** (wilayah tertentu) yang memiliki **pengaruh spasial signifikan terhadap tetangganya**.

###  Indikator yang umum digunakan:
- **Local Moran’s I (LISA - Local Indicators of Spatial Association)**  
  Digunakan untuk:
  - Mendeteksi **hotspots (nilai tinggi dikelilingi tinggi)**
  - Mendeteksi **coldspots (nilai rendah dikelilingi rendah)**
  - Mendeteksi **outlier spasial (nilai tinggi dikelilingi rendah, atau sebaliknya)**

- **Local Geary’s C**  
  Sama seperti Local Moran’s I, tapi lebih sensitif terhadap perbedaan ekstrem lokal.

### Visualisasi:
Biasanya divisualisasikan dalam **peta klaster spasial (LISA cluster map)** yang menunjukkan:
- High-High
- Low-Low
- High-Low
- Low-High

---

## Ringkasan Tabel

| Jenis Uji               | Skala Analisis | Tujuan Utama                        | Indikator Umum         |
|-------------------------|----------------|-------------------------------------|-------------------------|
| Global Autocorrelation  | Seluruh wilayah| Mengukur apakah data tersebar acak | Moran’s I, Geary’s C    |
| Local Autocorrelation   | Per wilayah     | Menemukan klaster & outlier lokal  | Local Moran’s I, LISA   |

---

## Kapan Menggunakan Masing-Masing?

- Gunakan **Global Test** saat ingin tahu **apakah ada pola spasial secara umum**.
- Gunakan **Local Test** saat ingin tahu **dimana pola tersebut terjadi secara spesifik**.


---






# Klasifikasi Model Spasial Berdasarkan Jenis Interaksi

Model-model spasial dapat dikelompokkan berdasarkan **sumber interaksi spasial** yang dimodelkan:

---
## 1. Model Lengkap: Semua Interaksi (termasuk ke Gabungan Interaksi: WY dan WX)

> Menangkap **semua sumber interaksi spasial**: $WY, WX, Wu$

###  Model GNS (General Nesting Spatial)

$$
Y = \rho WY + X\beta + \theta WX + u \\
u = \lambda W\mu + \varepsilon
$$

- Model paling umum dan fleksibel
- Mengandung **semua kemungkinan interaksi spasial**

## 2. Interaksi Antar Respons (Y)

> Respons di suatu lokasi dipengaruhi oleh respons di lokasi tetangganya  
> Ditandai dengan keberadaan term **`WY`** pada persamaan

###  Model SAR (Spatial Autoregressive Model)

$$
Y = \rho WY + X\beta + \varepsilon
$$

- Efek spasial hanya berasal dari **variabel terikat (Y)**
- Tidak memperhitungkan pengaruh X dari tetangga

---

## 3. Interaksi Antar Variabel Penjelas (X)

> Respons suatu lokasi dipengaruhi oleh variabel penjelas dari lokasi lain  
> Ditandai dengan keberadaan term **`WX`**

###  Model SLX (Spatial Lag of X)

$$
Y = X\beta + \theta WX + \varepsilon
$$

- Tidak ada WY → **tidak ada interaksi antar Y**
- Tidak ada Wu → **tidak ada autokorelasi galat**
- Efek X dari tetangga ditangkap lewat `WX`

---

## 4. Interaksi Antar Galat (u)

> Galat pada suatu lokasi berkorelasi dengan galat di lokasi tetangganya  
> Ditandai dengan keberadaan **`Wu`** dalam galat

###  Model SEM (Spatial Error Model)

$$
Y = X\beta + \lambda W\mu + \varepsilon
$$

- Tidak ada WY → **tidak ada efek dari tetangga terhadap Y**
- Tidak ada WX → **tidak ada efek dari X tetangga**
- Tapi galat punya pola spasial → **korelasi galat antar wilayah**

---

## 5. Gabungan Interaksi: WY dan WX

> Menangkap baik **efek spasial dalam Y** maupun **X dari tetangga**

### Model SDM (Spatial Durbin Model)

$$
Y = \rho WY + X\beta + \theta WX + \varepsilon
$$

- Gabungan SAR + SLX
- Cocok jika Y dan X sama-sama punya efek spasial

---

## 6. Gabungan Interaksi: WX dan Wu

> Menangkap efek dari X tetangga dan korelasi galat antar lokasi

### Model SDEM (Spatial Durbin Error Model)

$$
Y = X\beta + \theta WX + u \\
u = \lambda W\mu + \varepsilon
$$

- Tidak ada WY → **Y tidak saling memengaruhi**
- Tapi X tetangga dan korelasi galat tetap dimodelkan

---

## 7. Gabungan Interaksi: WY dan Wu

> Respons dipengaruhi oleh respons tetangga dan juga ada korelasi galat

### Model SAC (Spatial Autoregressive Confused)

$$
Y = \rho WY + X\beta + u \\
u = \lambda W\mu + \varepsilon
$$

- Tidak ada WX → **X dari tetangga tidak ikut**
- Tetapi respons dan galat saling terhubung secara spasial

---



---
## Ringkasan Tabel Interaksi Spasial

| Model | WY (Interaksi Respons) | WX (Interaksi Penjelas) | Wu (Interaksi Galat) | Keterangan                    |
|-------|------------------------|--------------------------|-----------------------|-------------------------------|
| SAR   | Yes                    | No                       | No                    | Hanya Y tetangga              |
| SLX   | No                     | Yes                      | No                    | Hanya X tetangga              |
| SEM   | No                     | No                       | Yes                   | Hanya error spasial           |
| SDM   | Yes                    | Yes                      | No                    | Gabungan Y dan X tetangga     |
| SDEM  | No                     | Yes                      | Yes                   | Gabungan X tetangga dan error |
| SAC   | Yes                    | No                       | Yes                   | Gabungan Y tetangga dan error |
| GNS   | Yes                    | Yes                      | Yes                   | Semua jenis interaksi         |


- Konversi dari DMS (longitude-latitude) ke Easting Northing  
  [https://www.latlong.net/lat-long-utm.html](https://www.latlong.net/lat-long-utm.html)