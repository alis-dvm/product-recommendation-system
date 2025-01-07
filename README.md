# product-recommendation-system
Sistem rekomendasi produk

## **Project Overview**
Dalam proyek ini, kami membangun sistem rekomendasi produk, dari situs web e-commerce, menggunakan pendekatan *Content-Based Filtering* dan *Collaborative Filtering* untuk membantu pengguna menemukan produk yang relevan berdasarkan preferensi/ riwayat pembelian dan perilaku penelusuran mereka

1. Content-Based Filtering yaitu menganalisis karakteristik produk untuk memberikan rekomendasi berdasarkan kesamaan dengan produk lain.
2. Collaborative Filtering yaitu menggunakan data interaksi pengguna untuk merekomendasikan produk yang diminati oleh pengguna serupa.

## **Business Understanding**
**Problem Statements**<br>
Pengguna sering menghadapi tantangan dalam memilih produk yang tepat karena banyaknya pilihan yang ada, yang sesuai untuk kondisi mereka. Oleh karena itu, penting untuk menciptakan sistem rekomendasi yang dapat membantu pengguna dalam memilih produk yang tepat dan relevan, dimana pengguna puas, sekaligus dapat meningkatkan penjualan & profit perusahaan.

**Goals**<br>
1. Mengembangkan sistem rekomendasi produk yang dapat memberikan saran produk yang lebih tepat berdasarkan data produk, konsumen dan pengalaman pengguna lain.
2. Membantu konsumen dalam memilih produk yang lebih sesuai dan memuaskan, sekaligus meningkatkan penjualan produk dari perusahaan, dengan memanfaatkan pendekatan algoritma Content-Based Filtering dan Collaborative Filtering.

**Solution Approach**<br>
* Content-Based Filtering digunakan untuk memberikan rekomendasi item yang serupa dengan apa yang pernah dibeli atau dilihat pengguna sebelumnya berdasarkan detail karakteristik/ atribut produk, seperti kategori, merek, harga dan diskripsi produk.
* Collaborative Filtering digunakan untuk memberikan rekomendasi dari data perilaku konsumen untuk mengidentifikasi pola berdasarkan rating dan preferensi pengguna lain yang memiliki riwayat pembelian sebelumnya.

## **Data Understanding**
**Exploratory Data Analysis (EDA) & Data Visualization**<br>

Sebelum membangun model, perlu dilakukan eksplorasi data untuk memahami struktur dataset dan kualitasnya.

Dalam proyek ini, kami menggunakan dataset yang berasal dari database "Bigbasket", supermarket grosir online terbesar di India, yang dapat di akses pada link [ini](https://www.kaggle.com/datasets/amrit0611/big-basket-product-analysis/data).

**1. Output 5 baris pertama dataset "BigBasketProducts.csv" dengan fungsi ```head```**
index 	product 	category 	sub_category 	brand 	sale_price 	market_price 	type 	rating 	description
0 	1 	Garlic Oil - Vegetarian Capsule 500 mg 	Beauty & Hygiene 	Hair Care 	Sri Sri Ayurveda 	220.0 	220.0 	Hair Oil & Serum 	4.1 	This Product contains Garlic Oil that is known...
1 	2 	Water Bottle - Orange 	Kitchen, Garden & Pets 	Storage & Accessories 	Mastercook 	180.0 	180.0 	Water & Fridge Bottles 	2.3 	Each product is microwave safe (without lid), ...
2 	3 	Brass Angle Deep - Plain, No.2 	Cleaning & Household 	Pooja Needs 	Trm 	119.0 	250.0 	Lamp & Lamp Oil 	3.4 	A perfect gift for all occasions, be it your m...
3 	4 	Cereal Flip Lid Container/Storage Jar - Assort... 	Cleaning & Household 	Bins & Bathroom Ware 	Nakoda 	149.0 	176.0 	Laundry, Storage Baskets 	3.7 	Multipurpose container with an attractive desi...
4 	5 	Creme Soft Soap - For Hands & Body 	Beauty & Hygiene 	Bath & Hand Wash 	Nivea 	162.0 	162.0 	Bathing Bars & Soaps 	4.4 	Nivea Creme Soft Soap gives your skin the best...

**2. Struktur dataset**<br>
Dataset terdiri dari 10 kolom dengan 27555 baris. Mayoritas kolom bertipe ```object``` dan sebagian bertipe ```float64``` kecuali kolom *index* bertipe ```int64```. Berikut detail variabel fitur dataset ini:<br>

        index: Nama obat yang digunakan.
        product: Nama produk (dapat digunakan untuk deskripsi dalam Content-Based Filtering).
        category: Kategori utama produk.
        sub_category: Sub-kategori produk.
        brand: Merek produk.
        sale_price: Harga jual produk diplatform e-commerce.
        market_price: Harga produk dipasaran.
        type: Tipe produk (lebih spesifik dibanding kategori/sub-kategori).
        rating: Rating pengguna (berguna untuk evaluasi Collaborative Filtering).
        description: Deskripsi produk (fitur utama untuk Content-Based Filtering).

RangeIndex: 27555 entries, 0 to 27554
Data columns (total 10 columns):
 #   Column        Non-Null Count  Dtype  
---  ------        --------------  -----  
 0   index         27555 non-null  int64  
 1   product       27554 non-null  object 
 2   category      27555 non-null  object 
 3   sub_category  27555 non-null  object 
 4   brand         27554 non-null  object 
 5   sale_price    27555 non-null  float64
 6   market_price  27555 non-null  float64
 7   type          27555 non-null  object 
 8   rating        18929 non-null  float64
 9   description   27440 non-null  object 
dtypes: float64(3), int64(1), object(6)

**3. Statistik Deskriptif (untuk kolom Age)**:<br>
Kolom yang dapat diproses menggunakan fungsi ```describe()``` adalah kolom  bertipe numeric, yaitu data ```int64``` & ```float64```. Berdasarkan output dibawah, rentang harga jual di platform, dari terendah (```min```) adalah 2,45 Rupee hingga harga tertinggi (```max```) 12.500 Rupee, sedangkan rata-rata rating (```mean```) adalah 3,94.
 	index 	sale_price 	market_price 	rating
count 	27555.00000 	27555.000000 	27555.000000 	18929.000000
mean 	13778.00000 	322.514808 	382.056664 	3.943410
std 	7954.58767 	486.263116 	581.730717 	0.739063
min 	1.00000 	2.450000 	3.000000 	1.000000
25% 	6889.50000 	95.000000 	100.000000 	3.700000
50% 	13778.00000 	190.000000 	220.000000 	4.100000
75% 	20666.50000 	359.000000 	425.000000 	4.300000
max 	27555.00000 	12500.000000 	12500.000000 	5.000000

**4. Data Kosong (Missing Values)**<br>
Kolom *rating* memiliki nilai yang hilang yang paling tinggi (8626 nilai kosong, dari total 27555 entri), yang bisa jadi mempengaruhi analisis lebih lanjut.
 	0
index 	0
product 	1
category 	0
sub_category 	0
brand 	1
sale_price 	0
market_price 	0
type 	0
rating 	8626
description 	115

**5. Jumlah nilai unik setiap kolom**
index mempunyai 27555 nilai yang unik
product mempunyai 23540 nilai yang unik
category mempunyai 11 nilai yang unik
sub_category mempunyai 90 nilai yang unik
brand mempunyai 2313 nilai yang unik
sale_price mempunyai 3256 nilai yang unik
market_price mempunyai 1348 nilai yang unik
type mempunyai 426 nilai yang unik
rating mempunyai 40 nilai yang unik
description mempunyai 21944 nilai yang unik

**6. Visualisasi jumlah item disetiap kategori**
![vis kategori](https://github.com/user-attachments/assets/11780fc7-c6ca-4aa8-bf3a-a3dba16c05af)

**7. Visualisasi 10 produk terlaris**
produk 	kategori 	jumlah
0 	Turmeric Powder/Arisina Pudi 	Foodgrains, Oil & Masala 	26
1 	Extra Virgin Olive Oil 	Gourmet & World Food 	14
2 	Cow Ghee/Tuppa 	Foodgrains, Oil & Masala 	14
3 	Soft Drink 	Beverages 	12
4 	Colorsilk Hair Colour With Keratin 	Beauty & Hygiene 	12
5 	Coriander Powder 	Foodgrains, Oil & Masala 	11
6 	Ghee/Tuppa 	Foodgrains, Oil & Masala 	11
7 	Powder - Coriander 	Foodgrains, Oil & Masala 	11
8 	Olive Oil - Extra Virgin 	Gourmet & World Food 	11
9 	Hand Sanitizer 	Beauty & Hygiene 	10

![vis produk laris](https://github.com/user-attachments/assets/87cd61a7-732b-4b27-83ca-8d27c5b2ae7f)
























  
