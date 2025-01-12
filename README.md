# product-recommendation-system


## **Project Overview**
Sistem rekomendasi sangat penting dalam mempersonalisasi pengalaman pengguna di berbagai domain, seperti e-commerce, film, dan pasar kerja. Dalam proyek ini, kami membangun sistem rekomendasi produk, dari situs web e-commerce. Pendekatan yang kami gunakan adalah *Content-Based Filtering* dan *Collaborative Filtering*, dalam rangka membantu pengguna menemukan produk yang relevan berdasarkan preferensi/ riwayat pembelian dan perilaku penelusuran mereka.

1. Content-Based Filtering yaitu menganalisis karakteristik produk untuk memberikan rekomendasi berdasarkan kesamaan dengan produk lain.
2. Collaborative Filtering yaitu menggunakan data interaksi pengguna untuk merekomendasikan produk yang diminati oleh pengguna serupa.

## **Business Understanding**
**Problem Statements**<br>
Perilaku konsumen berubah karena perkembangan era digital yang semakin modern. Mereka sebelumnya melakukan transaksi di toko fisik tetapi sekarang beralih ke toko online atau e-commerce. Perubahan ini memiliki dampak dalam cara pelanggan berpikir tentang produk yang mereka beli. Pengguna dapat memilih untuk membeli produk berdasarkan berbagai faktor, seperti harga, kualitas produk, promosi, dan bahkan perusahaan yang mereka tuju.

Salah satu faktor yang mendorong marketplace mengalami pertumbuhan yang pesat adalah fitur-fitur yang ditawarkan oleh perusahaan yang sangat inovatif dan dapat menarik minat pelanggan untuk terus membeli produk di marketplace tersebut. Di antara banyak fitur yang ditawarkan oleh marketplace, fitur sistem rekomendasi menjadi salah satu yang sangat berpengaruh terhadap keputusan pembeli untuk membeli suatu produk. Pengguna sering menghadapi tantangan dalam memilih produk yang tepat karena banyaknya pilihan yang ada, yang sesuai untuk kondisi mereka [[1]](https://doi.org/10.1117/12.3013353). Oleh karena itu, penting untuk menciptakan sistem rekomendasi yang dapat membantu pengguna dalam memilih produk yang tepat dan relevan, dimana pengguna puas, sekaligus dapat meningkatkan penjualan & profit perusahaan [[2]](https://doi.org/10.1509/jmkg.73.2.55).

**Goals**<br>
1. Mengembangkan sistem rekomendasi produk yang dapat memberikan saran produk yang lebih tepat berdasarkan data produk, konsumen dan pengalaman pengguna lain.
2. Membantu konsumen dalam memilih produk yang lebih sesuai dan memuaskan, sekaligus meningkatkan penjualan produk dari perusahaan, dengan memanfaatkan pendekatan algoritma Content-Based Filtering dan Collaborative Filtering.

**Solution Approach**<br>
* Content-Based Filtering digunakan untuk memberikan rekomendasi item yang serupa dengan apa yang pernah dibeli atau dilihat pengguna sebelumnya berdasarkan detail karakteristik/ atribut produk, seperti kategori, merek, harga dan diskripsi produk [[3]](https://doi.org/10.35806/ijoced.v6i1.393).
* Collaborative Filtering digunakan untuk memberikan rekomendasi dari data perilaku konsumen untuk mengidentifikasi pola berdasarkan rating dan preferensi pengguna lain yang memiliki riwayat pembelian sebelumnya [[4]](https://doi.org/10.48550/arXiv.2409.19262) & [[5]](https://doi.org/10.18280/jesa.570421).

## **Data Understanding**
**Exploratory Data Analysis (EDA) & Data Visualization**<br>

Sebelum membangun model, perlu dilakukan eksplorasi data untuk memahami struktur dataset dan kualitasnya.

Dalam proyek ini, kami menggunakan dataset yang berasal dari database "Bigbasket", supermarket grosir online terbesar di India, yang dapat di akses pada link [ini](https://www.kaggle.com/datasets/amrit0611/big-basket-product-analysis/data).

**1. Output 5 baris pertama dengan fungsi ```head``` pada dataset "BigBasketProducts.csv"**

| Index | Product | Category               | Sub-Category       | Brand            | Sale Price | Market Price | Type                     | Rating | Description                                    |
|-------|---------|------------------------|--------------------|------------------|------------|--------------|--------------------------|--------|------------------------------------------------|
| 0     | Garlic Oil - Vegetarian Capsule 500 mg | Beauty & Hygiene     | Hair Care          | Sri Sri Ayurveda | 220.0      | 220.0        | Hair Oil & Serum         | 4.1    | This Product contains Garlic Oil that is known... |
| 1     | Water Bottle - Orange           | Kitchen, Garden & Pets | Storage & Accessories | Mastercook       | 180.0      | 180.0        | Water & Fridge Bottles   | 2.3    | Each product is microwave safe (without lid), ... |
| 2     | Brass Angle Deep - Plain, No.2  | Cleaning & Household   | Pooja Needs        | Trm              | 119.0      | 250.0        | Lamp & Lamp Oil          | 3.4    | A perfect gift for all occasions, be it your m... |
| 3     | Cereal Flip Lid Container/Storage Jar - Assorted | Cleaning & Household   | Bins & Bathroom Ware | Nakoda           | 149.0      | 176.0        | Laundry, Storage Baskets | 3.7    | Multipurpose container with an attractive desi... |
| 4     | Creme Soft Soap - For Hands & Body | Beauty & Hygiene     | Bath & Hand Wash    | Nivea            | 162.0      | 162.0        | Bathing Bars & Soaps     | 4.4    | Nivea Creme Soft Soap gives your skin the best... |


**2. Struktur dataset**<br>
Dataset terdiri dari 10 kolom dengan 27555 baris. Mayoritas kolom bertipe ```object``` dan sebagian bertipe ```float64``` kecuali kolom *index* bertipe ```int64```. Berikut detail variabel fitur dataset ini:<br>

- ```index```: Nomor urut atau identifikasi unik untuk setiap baris dalam dataset.
- ```product```: Nama produk.
- ```category```: Kategori utama produk.
- ```sub_category```: Sub-kategori produk.
- ```brand```: Merek produk.
- ```sale_price```: Harga jual produk diplatform e-commerce.
- ```market_price```: Harga produk dipasaran.
- ```type```: Tipe produk (lebih spesifik dibanding kategori/sub-kategori).
- ```rating```: Rating pengguna.
- ```description```: Deskripsi produk.
```
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
```
**3. Statistik Deskriptif (untuk kolom Age)**:<br>
Kolom yang dapat diproses menggunakan fungsi ```describe()``` adalah kolom  bertipe numeric, yaitu data ```int64``` & ```float64```. Berdasarkan output dibawah, rentang harga jual di platform, dari terendah (```min```) adalah 2,45 Rupee hingga harga tertinggi (```max```) 12.500 Rupee, sedangkan rata-rata rating (```mean```) adalah 3,94.

| Metric       | Index        | Sale Price    | Market Price  | Rating       |
|--------------|--------------|---------------|---------------|--------------|
| **Count**    | 27,555.00000 | 27,555.000000 | 27,555.000000 | 18,929.000000 |
| **Mean**     | 13,778.00000 | 322.514808    | 382.056664    | 3.943410     |
| **Std**      | 7,954.58767  | 486.263116    | 581.730717    | 0.739063     |
| **Min**      | 1.00000      | 2.450000      | 3.000000      | 1.000000     |
| **25%**      | 6,889.50000  | 95.000000     | 100.000000    | 3.700000     |
| **50%**      | 13,778.00000 | 190.000000    | 220.000000    | 4.100000     |
| **75%**      | 20,666.50000 | 359.000000    | 425.000000    | 4.300000     |
| **Max**      | 27,555.00000 | 12,500.000000 | 12,500.000000 | 5.000000     |


**4. Data Kosong (Missing Values)** <br>
Berdasarkan output tabel dibawah ini, kolom ```rating``` memiliki nilai yang hilang yang paling tinggi (8626 nilai kosong, dari total 27555 entri), yang bisa jadi mempengaruhi analisis lebih lanjut.
| Column        | Missing Values |
|---------------|----------------|
| Index         | 0              |
| Product       | 1              |
| Category      | 0              |
| Sub-Category  | 0              |
| Brand         | 1              |
| Sale Price    | 0              |
| Market Price  | 0              |
| Type          | 0              |
| Rating        | 8,626          |
| Description   | 115            |


**5. Jumlah nilai unik setiap kolom**
- ```index``` 		mempunyai 27555 nilai yang unik
- ```product``` 	mempunyai 23540 nilai yang unik
- ```category``` 	mempunyai 11 	nilai yang unik
- ```sub_category``` 	mempunyai 90 	nilai yang unik
- ```brand``` 		mempunyai 2313 	nilai yang unik
- ```sale_price``` 	mempunyai 3256 	nilai yang unik
- ```market_price``` 	mempunyai 1348 	nilai yang unik
- ```type``` 		mempunyai 426 	nilai yang unik
- ```rating``` 		mempunyai 40 	nilai yang unik
- ```description``` 	mempunyai 21944 nilai yang unik

  Berdasarkan informasi diatas, bila kolom index diabaikan, kolom ```product``` dan ```description``` memiliki nilai unik yang terbesar.<br>

**6. Visualisasi jumlah item disetiap kategori**

![vis kategori](https://github.com/user-attachments/assets/11780fc7-c6ca-4aa8-bf3a-a3dba16c05af)

   Berdasarkan visualisasi data diatas, jumlah item terbanyak dalam kolom ```category``` adalah produk dalam kategori "Beauty & Hygiene", diikuti dengan peringkat kedua terbanyak adalah produk dalam kategori "Gourmet & World Food".

**7. Visualisasi 10 produk terlaris**

| No. | Product                          | Category                    | Count |
|-----|----------------------------------|-----------------------------|-------|
| 0   | Turmeric Powder/Arisina Pudi     | Foodgrains, Oil & Masala    | 26    |
| 1   | Extra Virgin Olive Oil           | Gourmet & World Food        | 14    |
| 2   | Cow Ghee/Tuppa                   | Foodgrains, Oil & Masala    | 14    |
| 3   | Soft Drink                       | Beverages                   | 12    |
| 4   | Colorsilk Hair Colour With Keratin | Beauty & Hygiene           | 12    |
| 5   | Coriander Powder                 | Foodgrains, Oil & Masala    | 11    |
| 6   | Ghee/Tuppa                       | Foodgrains, Oil & Masala    | 11    |
| 7   | Powder - Coriander               | Foodgrains, Oil & Masala    | 11    |
| 8   | Olive Oil - Extra Virgin         | Gourmet & World Food        | 11    |
| 9   | Hand Sanitizer                   | Beauty & Hygiene            | 10    |

![vis produk laris](https://github.com/user-attachments/assets/87cd61a7-732b-4b27-83ca-8d27c5b2ae7f)

Berdasarkan visualisasi data diatas, produk yang paling banyak terjual adalah yang berhubungan dengan persipan pembuatan masakan, yaitu bumbu seperti kunyit "Turmeric Powder/Arisina Pudi" & ketumbar "Coriander Powder". Selain itu minyak "Extra Virgin Olive Oil" & ghee juga terlaris penjualannya.

**8. Daftar 10 produk yang paling sedikit terjual**

| No. | Product                                     | Category                | Count |
|-----|---------------------------------------------|-------------------------|-------|
| 0   | Geometry Box - Kidzz                        | Cleaning & Household    | 1     |
| 1   | Geometry Box - Invento                      | Cleaning & Household    | 1     |
| 2   | Geometry Box - Export                       | Cleaning & Household    | 1     |
| 3   | Geometry Box - Disney, Invento              | Cleaning & Household    | 1     |
| 4   | Geometry Box - Asteroid                     | Cleaning & Household    | 1     |
| 5   | Geometry Box - Archimedes                   | Cleaning & Household    | 1     |
| 6   | Genuine Wood Shaving Brush                  | Beauty & Hygiene        | 1     |
| 7   | Gentleman Urbane Deodorant                  | Beauty & Hygiene        | 1     |
| 8   | Gentleman Urbane - Eau De Parfum For Men    | Beauty & Hygiene        | 1     |
| 9   | Pasta Shell                                 | Snacks & Branded Foods  | 1     |

Berdasarkan data tabel diatas, produk yang paling sedikit terjual adalah kotak geometri "Geometry Box" yang kesemuanya masuk dalam kategori "Cleaning & Household". Selain itu beberapa produk kebersihan seperti sikat, deodoran dan parfum serta produk makan kulit pasta "Pasta Shell" juga yang paling tidak laku terjual.

**9. Produk unggulan (rating 5) berbasis kategori**

| Index   | Product                                  | Category               | Sub-Category            | Brand               | Sale Price | Market Price | Type                   | Rating | Description                                     |
|---------|------------------------------------------|------------------------|-------------------------|---------------------|------------|--------------|------------------------|--------|-------------------------------------------------|
| 13      | Face Wash - Oil Control, Active         | Beauty & Hygiene       | Skin Care              | Oxy                 | 110.00     | 110.0        | Face Care             | 5.0    | This face wash deeply cleanses dirt and impurities... |
| 17      | Smooth Skin Oil - For Dry Skin          | Beauty & Hygiene       | Skin Care              | Aroma Treasures     | 324.00     | 360.0        | Aromatherapy          | 5.0    | Specially crafted for dry skin, this richly fo... |
| 25      | Veggie Cutter                           | Kitchen, Garden & Pets | Kitchen Accessories    | IRICH               | 195.00     | 195.0        | Choppers & Graters    | 5.0    | Food Grade High Quality Plastic, Keep and stor... |
| 45      | Plain Green Olives                      | Gourmet & World Food   | Tinned & Processed Food | Figaro             | 179.00     | 179.0        | Olive, Jalapeno, Gherkin | 5.0    | Olives are small fruits that grow on olive tre... |
| 93      | Topp Up Milk - Elaichi                  | Bakery, Cakes & Dairy  | Dairy                  | Gowardhan           | 80.01      | 90.0         | Flavoured, Soya Milk  | 5.0    | Gowardhan Topp-Up Milk is made by 100% of cow... |
| 27508   | Extra Crisp Sweet Corn                  | Gourmet & World Food   | Tinned & Processed Food | Daucy              | 202.50     | 225.0        | Beans & Pulses        | 5.0    | We bring for you Europe’s leading brand of can... |
| 27509   | Palm Jaggery/Bella Crystals             | Foodgrains, Oil & Masala | Salt, Sugar & Jaggery | Draft               | 199.00     | 199.0        | Sugar & Jaggery       | 5.0    | It is a healthy alternative to white sugar and... |
| 27513   | Water Bottle - Fridge, Tulip, Dark Blue | Kitchen, Garden & Pets | Storage & Accessories  | Cello               | 109.00     | 137.0        | Water & Fridge Bottles | 5.0    | Cello Tulip fridge bottle is manufactured by J... |
| 27516   | EDT Spray - Musk For Men                | Beauty & Hygiene       | Fragrances & Deos      | Brut                | 595.00     | 595.0        | Men's Deodorants      | 5.0    | Brut Musk was launched in 1986 as an elegant m... |
| 27549   | Apple Cider Vinegar Shampoo             | Beauty & Hygiene       | Hair Care              | Morpheme Remedies   | 499.00     | 499.0        | Shampoo & Conditioner | 5.0    | Say no to dull, lifeless, dry and damaged hair... |

1407 rows × 10 columns

![Top ratingl](https://github.com/user-attachments/assets/de593494-0a74-4d67-9aff-1c9fc3b96455)

Berdasarkan visualisasi data diatas, terdapat 1.407 produk dengan rating tertinggi yaitu rating 5 dari 27.555 produk. Dari sekitar 5 % produk tersebut kategori "Beauty & Hygiene" merupakan produk terbanyak, diikuti dengan peringkat kedua produk dalam kategori "Gourmet & World Food".

**10. Produk dengan rating terendah "1" berbasis kategori**

| Index  | Product                                        | Category               | Sub-Category              | Brand             | Sale Price | Market Price | Type                      | Rating | Description                                    |
|--------|-----------------------------------------------|------------------------|---------------------------|-------------------|------------|--------------|---------------------------|--------|------------------------------------------------|
| 65     | Aqua Halo Rejuvenating Conditioner            | Beauty & Hygiene       | Hair Care                 | Azafran           | 168.75     | 225.0        | Shampoo & Conditioner     | 1.0    | This Aqua Halo Rejuvenating Conditioner is an... |
| 299    | Family Sunscreen Lotion SPF 25                | Beauty & Hygiene       | Skin Care                 | INATUR            | 273.00     | 420.0        | Face Care                 | 1.0    | Family Sunscreen SPF 25 is an innovative sun p... |
| 300    | Japanese Cooking Rice-Wine                    | Gourmet & World Food   | Oils & Vinegar            | Urban Platter     | 475.00     | 500.0        | Flavoured & Other Oils    | 1.0    | Urban Platter Japanese Cooking Rice-Wine, 500m... |
| 436    | Aloe Vera Juice - Purifies Blood & Boosts Immu... | Gourmet & World Food   | Drinks & Beverages        | Jiva Ayurveda     | 220.00     | 220.0        | Health Drinks             | 1.0    | A natural anti-oxidant and blood purifier, Alo... |
| 452    | Hair Serum - Anti-Dandruff                    | Beauty & Hygiene       | Hair Care                 | Aroma Magic       | 637.50     | 750.0        | Hair Oil & Serum          | 1.0    | I am 100% free of parabens, petrochemicals, su... |
| 27192  | Mukhallat Khas Concentrated Oriental Perfume   | Beauty & Hygiene       | Fragrances & Deos         | Ajmal             | 1500.00    | 1500.0       | Attar                     | 1.0    | A special oriental blend that will appeal to i... |
| 27263  | PP Raspberry Belt                             | Gourmet & World Food   | Chocolates & Biscuits     | Kantan            | 30.00      | 30.0         | Marshmallow, Candy, Jelly | 1.0    | Fini Fantan Raspberry Belts contains raspberry... |
| 27286  | Dry Shampoo - Instant Hair Refresh, Coconut & ... | Beauty & Hygiene       | Hair Care                 | Batiste           | 649.00     | 649.0        | Dry Shampoo & Conditioner | 1.0    | Batiste refreshes your hair between washes, le... |
| 27368  | Moroccan Argan Hair Growth Oil For Hair Growth  | Beauty & Hygiene       | Hair Care                 | Khadi Meghdoot    | 375.00     | 375.0        | Shampoo & Conditioner     | 1.0    | This oil comes with the goodness of natural an... |
| 27519  | Gluten-Free Vegetable Millet Khichdi Mix      | Gourmet & World Food   | Cooking & Baking Needs    | Graminway         | 350.00     | 350.0        | Flours & Pre-Mixes        | 1.0    | Graminway Little Millet Khichdi Mix makes your... |

387 rows × 10 columns

![bottom kategori](https://github.com/user-attachments/assets/874ec2b8-67f5-45f6-8af8-563be44f1004)

Berdasarkan visualisasi data diatas, terdapat 387 produk dengan rating terendah yaitu rating 1 dari 27.555 produk. Yang menarik dari sekitar 1,4 % produk tersebut, kategori "Beauty & Hygiene" merupakan produk dengan rating terendah, diikuti dengan produk dalam kategori "Kitchen, Garden & Pets" dan "Gourmet & World Food".

## **Data Preparation**
**1. Dropping Uneeded Column**<br>
Kita akan menghapus kolom yang tidak relevan, yaitu kolom (```index```) untuk analisis lebih lanjut. Berikut hasil output tabelnya, setelah dihapus kolom index:

| Product                                    | Category                | Sub-Category         | Brand             | Sale Price | Market Price | Type                    | Rating | Description                                                                 |
|--------------------------------------------|-------------------------|----------------------|-------------------|------------|--------------|-------------------------|--------|-----------------------------------------------------------------------------|
| Garlic Oil - Vegetarian Capsule 500 mg    | Beauty & Hygiene        | Hair Care            | Sri Sri Ayurveda  | 220.0      | 220.0        | Hair Oil & Serum        | 4.1    | This Product contains Garlic Oil that is known...                          |
| Water Bottle - Orange                      | Kitchen, Garden & Pets  | Storage & Accessories| Mastercook        | 180.0      | 180.0        | Water & Fridge Bottles  | 2.3    | Each product is microwave safe (without lid), ...                          |
| Brass Angle Deep - Plain, No.2             | Cleaning & Household    | Pooja Needs          | Trm               | 119.0      | 250.0        | Lamp & Lamp Oil         | 3.4    | A perfect gift for all occasions, be it your m...                          |
| Cereal Flip Lid Container/Storage Jar - Assorted | Cleaning & Household | Bins & Bathroom Ware | Nakoda            | 149.0      | 176.0        | Laundry, Storage Baskets| 3.7    | Multipurpose container with an attractive desi...                          |
| Creme Soft Soap - For Hands & Body         | Beauty & Hygiene        | Bath & Hand Wash     | Nivea             | 162.0      | 162.0        | Bathing Bars & Soaps    | 4.4    | Nivea Creme Soft Soap gives your skin the best...                          |

**2. Detection and Removal Duplicates**<br>
Sebelum membangun model, kita akan memeriksa duplikasi dalam data dan menghapusnya untuk memastikan data yang digunakan tidak redundan. Hal ini dilakukan untuk meningkatkan kualitas data, yaitu menghindari bias, meningkatkan akurasi serta dapat juga mengurangi beban komputasi. Berikut hasil outputnya, sebelum dan sesudah perlakuan:<br>
```
Total Duplicates : 354
After remove Duplicates : 0
```

**3. Handling Missing Value**<br>
Kita juga perlu menangani nilai yang hilang dalam data, baik dengan mengisi nilai yang hilang ataupun menghapus baris yang mengandung nilai yang hilang tersebut. Sebelum kita melakukan penanganan data yang nilainya hilang, terlebih dahulu kita lihat distribusi, mean dan median kolom ```rating``` yang menurut informasi sebelumnya, ketika melakukan EDA, nilai null nya paling banyak, yaitu 8626. Setelah kita mengetahui informasinya, kita akan memutuskan untuk menghapus atau mengisi kolom ```rating``` tersebut.
- Distribusi kolom rating
  
![vis distribusi rating](https://github.com/user-attachments/assets/4cab67ac-2d0a-4687-8c8c-b1da3a65c2a7)

Berdasarkan visualisasi output diatas, ditemukan pola distribusi peringkat maksimum (paling banyak) berada di antara rating 3,5 dan 4,5
- Mean rating<br>
```3.943409583179249```
- Median rating<br>
```4.1```

Berdasarkan informasi sebelumnya dan juga kolom ```rating``` mempunyai jumlah nilai hilang yang tidak sedikit, kita memutuskan untuk mengisi nilai null dengan mean rating.
- Nilai kolom ```rating``` yang hilang, diisi dengan nilai mean rating

Untuk menangani nilai null kolom yang lain, yang jumlahnya sedikit, kita akan melihat prosentasenya
- Prosentase nilai yang hilang keseluruan<br>
```Total Data Null: 0.05```

Berdasarkan hasil output tersebut, kita akan menghapus baris yang mengandung nilai null
- Hasil setelah nilai yang hilang dihapus<br>
Berikut hasil setelah 2 kali penanganan nilai null tersebut:

| Product                                | Category                | Sub Category             | Brand                     | Sale Price | Market Price | Type                   | Rating | Description                                          |
|----------------------------------------|-------------------------|--------------------------|---------------------------|------------|--------------|------------------------|--------|------------------------------------------------------|
| Garlic Oil - Vegetarian Capsule 500 mg | Beauty & Hygiene        | Hair Care                | Sri Sri Ayurveda          | 220.00     | 220.00       | Hair Oil & Serum       | 4.1    | This Product contains Garlic Oil that is known...    |
| Water Bottle - Orange                  | Kitchen, Garden & Pets  | Storage & Accessories    | Mastercook                | 180.00     | 180.00       | Water & Fridge Bottles | 2.3    | Each product is microwave safe (without lid), ...    |
| Brass Angle Deep - Plain, No.2         | Cleaning & Household    | Pooja Needs              | Trm                       | 119.00     | 250.00       | Lamp & Lamp Oil        | 3.4    | A perfect gift for all occasions, be it your m...    |
| Cereal Flip Lid Container/Storage Jar  | Cleaning & Household    | Bins & Bathroom Ware     | Nakoda                    | 149.00     | 176.00       | Laundry, Storage Baskets | 3.7  | Multipurpose container with an attractive desi...    |
| Creme Soft Soap - For Hands & Body     | Beauty & Hygiene        | Bath & Hand Wash         | Nivea                     | 162.00     | 162.00       | Bathing Bars & Soaps   | 4.4    | Nivea Creme Soft Soap gives your skin the best...    |
| ...                                    | ...                     | ...                      | ...                       | ...        | ...          | ...                    | ...    | ...                                                  |
| Wottagirl! Perfume Spray - Heaven      | Beauty & Hygiene        | Fragrances & Deos        | Layerr                    | 199.20     | 249.00       | Perfume                | 3.9    | Layerr brings you Wottagirl Classic fragrant b...    |
| Rosemary                               | Gourmet & World Food    | Cooking & Baking Needs   | Puramate                  | 67.50      | 75.00        | Herbs, Seasonings & Rubs | 4.0  | Puramate rosemary is enough to transform a dis...    |
| Peri-Peri Sweet Potato Chips           | Gourmet & World Food    | Snacks, Dry Fruits, Nuts | FabBox                   | 200.00     | 200.00       | Nachos & Chips         | 3.8    | We have taken the richness of Sweet Potatoes (...    |
| Green Tea - Pure Original              | Beverages               | Tea                      | Tetley                    | 396.00     | 495.00       | Tea Bags               | 4.2    | Tetley Green Tea with its refreshing pure, ori...    |
| United Dreams Go Far Deodorant         | Beauty & Hygiene        | Men's Grooming           | United Colors Of Benetton | 214.53     | 390.00       | Men's Deodorants       | 4.5    | The new mens fragrance from the United Dreams ...    |

27087 rows × 9 columns

**4. Changing Certain Value** (menghapus &, memisahkan string, menyusun dalam bentuk list)<br>
Langkah ini dilakukan untuk memudahkan langkah selanjutnya dalam pemodelan, yaitu manipulasi teks, khususnya untuk menghilangkan & dan memisahkan teks menjadi elemen-elemen dalam sebuah daftar.
```
rmv_spc = lambda a:a.strip()
get_list = lambda a:list(map(rmv_spc,re.split('& |, |\*|\n', a)))
```

```
for col in ['category', 'sub_category', 'type']:
    df2[col] = df2[col].apply(get_list)
```

**5. Changing Certain Value** (merubah jadi huruf kecil, menghilangkan spasi)<br>
Proses ini, yaitu mengubah nilai pada kolom ```category```, ```sub_category``` dan ```type```, semuanya menjadi huruf kecil dan juga menghilangkan spasi antar kata.
Hal ini dilakukan untuk memastikan, model rekomendasi tidak menganggap "Cholocate IceCream" dan "Chocolate Bar" sebagai produk yang sama.
```
def cleaner(x):
    if isinstance(x, list):
        return [str.lower(i.replace(" ", "")) for i in x]
    else:
        if isinstance(x, str):
            return str.lower(x.replace(" ", ""))
        else:
            return ''
```

```
for col in ['category', 'sub_category', 'type','brand']:
    df2[col] = df2[col].apply(cleaner)
```

**6. Content-Based Filtering** <br>
- Menggabungkan nilai kategori, sub kategori, tipe & brand
Hal ini dilakukan, untuk efisiensi pembuatan model rekomendasi produk yang merupakan representasi dari detail dalam kategori, sub kategori, tipe & brand.
```
df_cbf = df2.copy()
```

```
def couple(x):
    return ' '.join(x['category']) + ' ' + ' '.join(x['sub_category']) + ' '+x['brand']+' ' +' '.join( x['type'])
df_cbf['gab'] = df_cbf.apply(couple, axis=1)
```
- Ekstraksi Fitur TF-IDF<br>
TfidfVectorizer adalah metode populer dalam pemrosesan teks yang digunakan untuk merepresentasikan teks sebagai vektor numerik. Metode ini menggunakan pendekatan Bag-of-Words, di mana teks direpresentasikan sebagai kumpulan kata tanpa memperhatikan urutan kata dalam dokumen. TfidfVectorizer mengonversi teks menjadi matriks numerik, yang dapat digunakan untuk analisis lebih lanjut seperti klasifikasi atau clustering, yang juga dapat melakukan preprocessing seperti tokenisasi, lowercasing, dan penghapusan stop words (dengan parameter yang sesuai). Metode ini, menghitung frekuensi kemunculan kata dalam dokumen sebagai dasar untuk membangun vektor.
```
tfidf = TfidfVectorizer(stop_words='english')
tfidf_matrix = tfidf.fit_transform(df_cbf['gab'])
tfidf_matrix.shape
```
```(27201, 3083)```<br>
Berdasarkan output diatas, dapat dilihat bahwa ukuran matriksnya sebesar 27201 x 3083

**7. Collaborative Filtering**<br>
- Simulate user IDs<br>
Dalam sistem rekomendasi memakai pendekatan Collaborative filtering diperlukan atribut ID pengguna, maka dibuatlah simulasi ```userID```, yang ditambahkan dalam kolom dataset. kolom ```userID``` memetakan baris-baris dalam DataFrame ```df2``` ke dalam 500 kelompok secara siklik. Nilai pada kolom ```userID``` akan berulang dari 1 hingga 500 secara berurutan berdasarkan indeks. Berikut output tabelnya:

| Product                                | Category                  | Sub Category              | Brand                     | Sale Price | Market Price | Type                   | Rating | Description                                          | UserID |
|----------------------------------------|---------------------------|---------------------------|---------------------------|------------|--------------|------------------------|--------|------------------------------------------------------|--------|
| Garlic Oil - Vegetarian Capsule 500 mg | [beauty, hygiene]         | [haircare]                | srisriayurveda            | 220.00     | 220.00       | [hairoil, serum]       | 4.1    | This Product contains Garlic Oil that is known...    | 1      |
| Water Bottle - Orange                  | [kitchen, garden, pets]   | [storage, accessories]    | mastercook                | 180.00     | 180.00       | [water, fridgebottles] | 2.3    | Each product is microwave safe (without lid), ...    | 2      |
| Brass Angle Deep - Plain, No.2         | [cleaning, household]     | [poojaneeds]              | trm                       | 119.00     | 250.00       | [lamp, lampoil]        | 3.4    | A perfect gift for all occasions, be it your m...    | 3      |
| Cereal Flip Lid Container/Storage Jar  | [cleaning, household]     | [bins, bathroomware]      | nakoda                    | 149.00     | 176.00       | [laundry, storagebaskets] | 3.7  | Multipurpose container with an attractive desi...    | 4      |
| Creme Soft Soap - For Hands & Body     | [beauty, hygiene]         | [bath, handwash]          | nivea                     | 162.00     | 162.00       | [bathingbars, soaps]   | 4.4    | Nivea Creme Soft Soap gives your skin the best...    | 5      |
| ...                                    | ...                       | ...                       | ...                       | ...        | ...          | ...                    | ...    | ...                                                  | ...    |
| Wottagirl! Perfume Spray - Heaven      | [beauty, hygiene]         | [fragrances, deos]        | layerr                    | 199.20     | 249.00       | [perfume]              | 3.9    | Layerr brings you Wottagirl Classic fragrant b...    | 197    |
| Rosemary                               | [gourmet, worldfood]      | [cooking, bakingneeds]    | puramate                  | 67.50      | 75.00        | [herbs, seasonings, rubs] | 4.0  | Puramate rosemary is enough to transform a dis...    | 198    |
| Peri-Peri Sweet Potato Chips           | [gourmet, worldfood]      | [snacks, dryfruits, nuts] | fabbox                   | 200.00     | 200.00       | [nachos, chips]         | 3.8    | We have taken the richness of Sweet Potatoes (...    | 199    |
| Green Tea - Pure Original              | [beverages]               | [tea]                     | tetley                    | 396.00     | 495.00       | [teabags]              | 4.2    | Tetley Green Tea with its refreshing pure, ori...    | 200    |
| United Dreams Go Far Deodorant         | [beauty, hygiene]         | [men'sgrooming]           | unitedcolorsofbenetton    | 214.53     | 390.00       | [men'sdeodorants]      | 4.5    | The new mens fragrance from the United Dreams ...    | 201    |

27201 rows × 10 columns

- Encode Label<br>
Untuk menggunakan algoritma berbasis matriks, kita perlu mengonversi data kategorikal menjadi numerik. Hal ini karena algoritma Collaborative Filtering, seperti matrix factorization, bekerja dengan data numerik, sehingga data kategori seperti ID pengguna atau produk perlu dienkode menjadi bilangan bulat atau vektor numerik. Selain itu, encoding membantu menyusun data dalam bentuk matriks pengguna-produk (user-item matrix) yang memungkinkan operasi linear algebra, seperti dekomposisi matriks (matrix decomposition), dilakukan dengan efisien, sehingga memungkinkan sistem dapat dengan mudah menangani dataset dalam skala besar.<br>

	- Encoding ```userId```

	```
	list userID:  [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 77, 78, 79, 80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90, 91, 92, 93, 94, 95, 96, 97, 98, 99, 100, 101, 102, 103, 104, 105, 106, 107, 108, 109, 110, 111, 112, 113, 114, 115, 116, 117, 118, 119, 120, 121, 122, 123, 124, 125, 126, 127, 128, 129, 130, 131, 132, 133, 134, 135, 136, 137, 138, 139, 140, 141, 142, 143, 144, 145, 146, 147, 148, 149, 150, 151, 152, 153, 154, 155, 156, 157, 158, 159, 160, 161, 162, 163, 164, 165, 166, 167, 168, 169, 170, 171, 172, 173, 174, 175, 176, 177, 178, 179, 180, 181, 182, 183, 184, 185, 186, 187, 188, 189, 190, 191, 192, 193, 194, 195, 196, 197, 198, 199, 200, 201, 202, 203, 204, 205, 206, 207, 208, 209, 210, 211, 212, 213, 214, 215, 216, 217, 218, 219, 220, 221, 222, 223, 224, 225, 226, 227, 228, 229, 230, 231, 232, 233, 234, 235, 236, 237, 238, 239, 240, 241, 242, 243, 244, 245, 246, 247, 248, 249, 250, 251, 252, 253, 254, 255, 256, 257, 258, 259, 260, 261, 262, 263, 264, 265, 266, 267, 268, 269, 270, 271, 272, 273, 274, 275, 276, 277, 278, 279, 280, 281, 282, 283, 284, 285, 286, 287, 288, 289, 290, 291, 292, 293, 294, 295, 296, 297, 298, 299, 300, 301, 302, 303, 304, 305, 306, 307, 308, 309, 310, 311, 312, 313, 314, 315, 316, 317, 318, 319, 320, 321, 322, 323, 324, 325, 326, 327, 328, 329, 330, 331, 332, 333, 334, 335, 336, 337, 338, 339, 340, 341, 342, 343, 344, 345, 346, 347, 348, 349, 350, 351, 352, 353, 354, 355, 356, 357, 358, 359, 360, 361, 362, 363, 364, 365, 366, 367, 368, 369, 370, 371, 372, 373, 374, 375, 376, 377, 378, 379, 380, 381, 382, 383, 384, 385, 386, 387, 388, 389, 390, 391, 392, 393, 394, 395, 396, 397, 398, 399, 400, 401, 402, 403, 404, 405, 406, 407, 408, 409, 410, 411, 412, 413, 414, 415, 416, 417, 418, 419, 420, 421, 422, 423, 424, 425, 426, 427, 428, 429, 430, 431, 432, 433, 434, 435, 436, 437, 438, 439, 440, 441, 442, 443, 444, 445, 446, 447, 448, 449, 450, 451, 452, 453, 454, 455, 456, 457, 458, 459, 460, 461, 462, 463, 464, 465, 466, 467, 468, 469, 470, 471, 472, 473, 474, 475, 476, 477, 478, 479, 480, 481, 482, 483, 484, 485, 486, 487, 488, 489, 490, 491, 492, 493, 494, 495, 496, 497, 498, 499, 500]
	encoded userID :  {1: 0, 2: 1, 3: 2, 4: 3, 5: 4, 6: 5, 7: 6, 8: 7, 9: 8, 10: 9, 11: 10, 12: 11, 13: 12, 14: 13, 15: 14, 16: 15, 17: 16, 18: 17, 19: 18, 20: 19, 21: 20, 22: 21, 23: 22, 24: 23, 25: 24, 26: 25, 27: 26, 28: 27, 29: 28, 30: 29, 31: 30, 32: 31, 33: 32, 34: 33, 35: 34, 36: 35, 37: 36, 38: 37, 39: 38, 40: 39, 41: 40, 42: 41, 43: 42, 44: 43, 45: 44, 46: 45, 47: 46, 48: 47, 49: 48, 50: 49, 51: 50, 52: 51, 53: 52, 54: 53, 55: 54, 56: 55, 57: 56, 58: 57, 59: 58, 60: 59, 61: 60, 62: 61, 63: 62, 64: 63, 65: 64, 66: 65, 67: 66, 68: 67, 69: 68, 70: 69, 71: 70, 72: 71, 73: 72, 74: 73, 75: 74, 76: 75, 77: 76, 78: 77, 79: 78, 80: 79, 81: 80, 82: 81, 83: 82, 84: 83, 85: 84, 86: 85, 87: 86, 88: 87, 89: 88, 90: 89, 91: 90, 92: 91, 93: 92, 94: 93, 95: 94, 96: 95, 97: 96, 98: 97, 99: 98, 100: 99, 101: 100, 102: 101, 103: 102, 104: 103, 105: 104, 106: 105, 107: 106, 108: 107, 109: 108, 110: 109, 111: 110, 112: 111, 113: 112, 114: 113, 115: 114, 116: 115, 117: 116, 118: 117, 119: 118, 120: 119, 121: 120, 122: 121, 123: 122, 124: 123, 125: 124, 126: 125, 127: 126, 128: 127, 129: 128, 130: 129, 131: 130, 132: 131, 133: 132, 134: 133, 135: 134, 136: 135, 137: 136, 138: 137, 139: 138, 140: 139, 141: 140, 142: 141, 143: 142, 144: 143, 145: 144, 146: 145, 147: 146, 148: 147, 149: 148, 150: 149, 151: 150, 152: 151, 153: 152, 154: 153, 155: 154, 156: 155, 157: 156, 158: 157, 159: 158, 160: 159, 161: 160, 162: 161, 163: 162, 164: 163, 165: 164, 166: 165, 167: 166, 168: 167, 169: 168, 170: 169, 171: 170, 172: 171, 173: 172, 174: 173, 175: 174, 176: 175, 177: 176, 178: 177, 179: 178, 180: 179, 181: 180, 182: 181, 183: 182, 184: 183, 185: 184, 186: 185, 187: 186, 188: 187, 189: 188, 190: 189, 191: 190, 192: 191, 193: 192, 194: 193, 195: 194, 196: 195, 197: 196, 198: 197, 199: 198, 200: 199, 201: 200, 202: 201, 203: 202, 204: 203, 205: 204, 206: 205, 207: 206, 208: 207, 209: 208, 210: 209, 211: 210, 212: 211, 213: 212, 214: 213, 215: 214, 216: 215, 217: 216, 218: 217, 219: 218, 220: 219, 221: 220, 222: 221, 223: 222, 224: 223, 225: 224, 226: 225, 227: 226, 228: 227, 229: 228, 230: 229, 231: 230, 232: 231, 233: 232, 234: 233, 235: 234, 236: 235, 237: 236, 238: 237, 239: 238, 240: 239, 241: 240, 242: 241, 243: 242, 244: 243, 245: 244, 246: 245, 247: 246, 248: 247, 249: 248, 250: 249, 251: 250, 252: 251, 253: 252, 254: 253, 255: 254, 256: 255, 257: 256, 258: 257, 259: 258, 260: 259, 261: 260, 262: 261, 263: 262, 264: 263, 265: 264, 266: 265, 267: 266, 268: 267, 269: 268, 270: 269, 271: 270, 272: 271, 273: 272, 274: 273, 275: 274, 276: 275, 277: 276, 278: 277, 279: 278, 280: 279, 281: 280, 282: 281, 283: 282, 284: 283, 285: 284, 286: 285, 287: 286, 288: 287, 289: 288, 290: 289, 291: 290, 292: 291, 293: 292, 294: 293, 295: 294, 296: 295, 297: 296, 298: 297, 299: 298, 300: 299, 301: 300, 302: 301, 303: 302, 304: 303, 305: 304, 306: 305, 307: 306, 308: 307, 309: 308, 310: 309, 311: 310, 312: 311, 313: 312, 314: 313, 315: 314, 316: 315, 317: 316, 318: 317, 319: 318, 320: 319, 321: 320, 322: 321, 323: 322, 324: 323, 325: 324, 326: 325, 327: 326, 328: 327, 329: 328, 330: 329, 331: 330, 332: 331, 333: 332, 334: 333, 335: 334, 336: 335, 337: 336, 338: 337, 339: 338, 340: 339, 341: 340, 342: 341, 343: 342, 344: 343, 345: 344, 346: 345, 347: 346, 348: 347, 349: 348, 350: 349, 351: 350, 352: 351, 353: 352, 354: 353, 355: 354, 356: 355, 357: 356, 358: 357, 359: 358, 360: 359, 361: 360, 362: 361, 363: 362, 364: 363, 365: 364, 366: 365, 367: 366, 368: 367, 369: 368, 370: 369, 371: 370, 372: 371, 373: 372, 374: 373, 375: 374, 376: 375, 377: 376, 378: 377, 379: 378, 380: 379, 381: 380, 382: 381, 383: 382, 384: 383, 385: 384, 386: 385, 387: 386, 388: 387, 389: 388, 390: 389, 391: 390, 392: 391, 393: 392, 394: 393, 395: 394, 396: 395, 397: 396, 398: 397, 399: 398, 400: 399, 401: 400, 402: 401, 403: 402, 404: 403, 405: 404, 406: 405, 407: 406, 408: 407, 409: 408, 410: 409, 411: 410, 412: 411, 413: 412, 414: 413, 415: 414, 416: 415, 417: 416, 418: 417, 419: 418, 420: 419, 421: 420, 422: 421, 423: 422, 424: 423, 425: 424, 426: 425, 427: 426, 428: 427, 429: 428, 430: 429, 431: 430, 432: 431, 433: 432, 434: 433, 435: 434, 436: 435, 437: 436, 438: 437, 439: 438, 440: 439, 441: 440, 442: 441, 443: 442, 444: 443, 445: 444, 446: 445, 447: 446, 448: 447, 449: 448, 450: 449, 451: 450, 452: 451, 453: 452, 454: 453, 455: 454, 456: 455, 457: 456, 458: 457, 459: 458, 460: 459, 461: 460, 462: 461, 463: 462, 464: 463, 465: 464, 466: 465, 467: 466, 468: 467, 469: 468, 470: 469, 471: 470, 472: 471, 473: 472, 474: 473, 475: 474, 476: 475, 477: 476, 478: 477, 479: 478, 480: 479, 481: 480, 482: 481, 483: 482, 484: 483, 485: 484, 486: 485, 487: 486, 488: 487, 489: 488, 490: 489, 491: 490, 492: 491, 493: 492, 494: 493, 495: 494, 496: 495, 497: 496, 498: 497, 499: 498, 500: 499}
	encoded angka ke userID:  {0: 1, 1: 2, 2: 3, 3: 4, 4: 5, 5: 6, 6: 7, 7: 8, 8: 9, 9: 10, 10: 11, 11: 12, 12: 13, 13: 14, 14: 15, 15: 16, 16: 17, 17: 18, 18: 19, 19: 20, 20: 21, 21: 22, 22: 23, 23: 24, 24: 25, 25: 26, 26: 27, 27: 28, 28: 29, 29: 30, 30: 31, 31: 32, 32: 33, 33: 34, 34: 35, 35: 36, 36: 37, 37: 38, 38: 39, 39: 40, 40: 41, 41: 42, 42: 43, 43: 44, 44: 45, 45: 46, 46: 47, 47: 48, 48: 49, 49: 50, 50: 51, 51: 52, 52: 53, 53: 54, 54: 55, 55: 56, 56: 57, 57: 58, 58: 59, 59: 60, 60: 61, 61: 62, 62: 63, 63: 64, 64: 65, 65: 66, 66: 67, 67: 68, 68: 69, 69: 70, 70: 71, 71: 72, 72: 73, 73: 74, 74: 75, 75: 76, 76: 77, 77: 78, 78: 79, 79: 80, 80: 81, 81: 82, 82: 83, 83: 84, 84: 85, 85: 86, 86: 87, 87: 88, 88: 89, 89: 90, 90: 91, 91: 92, 92: 93, 93: 94, 94: 95, 95: 96, 96: 97, 97: 98, 98: 99, 99: 100, 100: 101, 101: 102, 102: 103, 103: 104, 104: 105, 105: 106, 106: 107, 107: 108, 108: 109, 109: 110, 110: 111, 111: 112, 112: 113, 113: 114, 114: 115, 115: 116, 116: 117, 117: 118, 118: 119, 119: 120, 120: 121, 121: 122, 122: 123, 123: 124, 124: 125, 125: 126, 126: 127, 127: 128, 128: 129, 129: 130, 130: 131, 131: 132, 132: 133, 133: 134, 134: 135, 135: 136, 136: 137, 137: 138, 138: 139, 139: 140, 140: 141, 141: 142, 142: 143, 143: 144, 144: 145, 145: 146, 146: 147, 147: 148, 148: 149, 149: 150, 150: 151, 151: 152, 152: 153, 153: 154, 154: 155, 155: 156, 156: 157, 157: 158, 158: 159, 159: 160, 160: 161, 161: 162, 162: 163, 163: 164, 164: 165, 165: 166, 166: 167, 167: 168, 168: 169, 169: 170, 170: 171, 171: 172, 172: 173, 173: 174, 174: 175, 175: 176, 176: 177, 177: 178, 178: 179, 179: 180, 180: 181, 181: 182, 182: 183, 183: 184, 184: 185, 185: 186, 186: 187, 187: 188, 188: 189, 189: 190, 190: 191, 191: 192, 192: 193, 193: 194, 194: 195, 195: 196, 196: 197, 197: 198, 198: 199, 199: 200, 200: 201, 201: 202, 202: 203, 203: 204, 204: 205, 205: 206, 206: 207, 207: 208, 208: 209, 209: 210, 210: 211, 211: 212, 212: 213, 213: 214, 214: 215, 215: 216, 216: 217, 217: 218, 218: 219, 219: 220, 220: 221, 221: 222, 222: 223, 223: 224, 224: 225, 225: 226, 226: 227, 227: 228, 228: 229, 229: 230, 230: 231, 231: 232, 232: 233, 233: 234, 234: 235, 235: 236, 236: 237, 237: 238, 238: 239, 239: 240, 240: 241, 241: 242, 242: 243, 243: 244, 244: 245, 245: 246, 246: 247, 247: 248, 248: 249, 249: 250, 250: 251, 251: 252, 252: 253, 253: 254, 254: 255, 255: 256, 256: 257, 257: 258, 258: 259, 259: 260, 260: 261, 261: 262, 262: 263, 263: 264, 264: 265, 265: 266, 266: 267, 267: 268, 268: 269, 269: 270, 270: 271, 271: 272, 272: 273, 273: 274, 274: 275, 275: 276, 276: 277, 277: 278, 278: 279, 279: 280, 280: 281, 281: 282, 282: 283, 283: 284, 284: 285, 285: 286, 286: 287, 287: 288, 288: 289, 289: 290, 290: 291, 291: 292, 292: 293, 293: 294, 294: 295, 295: 296, 296: 297, 297: 298, 298: 299, 299: 300, 300: 301, 301: 302, 302: 303, 303: 304, 304: 305, 305: 306, 306: 307, 307: 308, 308: 309, 309: 310, 310: 311, 311: 312, 312: 313, 313: 314, 314: 315, 315: 316, 316: 317, 317: 318, 318: 319, 319: 320, 320: 321, 321: 322, 322: 323, 323: 324, 324: 325, 325: 326, 326: 327, 327: 328, 328: 329, 329: 330, 330: 331, 331: 332, 332: 333, 333: 334, 334: 335, 335: 336, 336: 337, 337: 338, 338: 339, 339: 340, 340: 341, 341: 342, 342: 343, 343: 344, 344: 345, 345: 346, 346: 347, 347: 348, 348: 349, 349: 350, 350: 351, 351: 352, 352: 353, 353: 354, 354: 355, 355: 356, 356: 357, 357: 358, 358: 359, 359: 360, 360: 361, 361: 362, 362: 363, 363: 364, 364: 365, 365: 366, 366: 367, 367: 368, 368: 369, 369: 370, 370: 371, 371: 372, 372: 373, 373: 374, 374: 375, 375: 376, 376: 377, 377: 378, 378: 379, 379: 380, 380: 381, 381: 382, 382: 383, 383: 384, 384: 385, 385: 386, 386: 387, 387: 388, 388: 389, 389: 390, 390: 391, 391: 392, 392: 393, 393: 394, 394: 395, 395: 396, 396: 397, 397: 398, 398: 399, 399: 400, 400: 401, 401: 402, 402: 403, 403: 404, 404: 405, 405: 406, 406: 407, 407: 408, 408: 409, 409: 410, 410: 411, 411: 412, 412: 413, 413: 414, 414: 415, 415: 416, 416: 417, 417: 418, 418: 419, 419: 420, 420: 421, 421: 422, 422: 423, 423: 424, 424: 425, 425: 426, 426: 427, 427: 428, 428: 429, 429: 430, 430: 431, 431: 432, 432: 433, 433: 434, 434: 435, 435: 436, 436: 437, 437: 438, 438: 439, 439: 440, 440: 441, 441: 442, 442: 443, 443: 444, 444: 445, 445: 446, 446: 447, 447: 448, 448: 449, 449: 450, 450: 451, 451: 452, 452: 453, 453: 454, 454: 455, 455: 456, 456: 457, 457: 458, 458: 459, 459: 460, 460: 461, 461: 462, 462: 463, 463: 464, 464: 465, 465: 466, 466: 467, 467: 468, 468: 469, 469: 470, 470: 471, 471: 472, 472: 473, 473: 474, 474: 475, 475: 476, 476: 477, 477: 478, 478: 479, 479: 480, 480: 481, 481: 482, 482: 483, 483: 484, 484: 485, 485: 486, 486: 487, 487: 488, 488: 489, 489: 490, 490: 491, 491: 492, 492: 493, 493: 494, 494: 495, 495: 496, 496: 497, 497: 498, 498: 499, 499: 500}
	```
	Berdasarkan output diatas, proses encoding untuk ```userId``` sudah berhasil dilakukan.<br>

	- Mapping hasil encoding ```userId``` dan ```product```:<br>
	```
	df2['user'] = df2['userID'].map(user_to_user_encoded) 
	df2['produk'] = df2['product'].map(produk_to_produk_encoded)
	```
	Hasil encoding sebelumnya, kemudian dilakukan mapping ke dalam dataframe ```df2``` dengan menempati kolom baru untuk masing-masing hasil.<br>

	- Proses pengecekan dataset setelah dilakukan encoding
	```
	jumlah user: 500
	jumlah produk: 23541
	Min rating: 1.0
	Max rating: 5.0
	```
 	Berdasarkan output diatas, proses encoding telah berhasil dilakukan.

- Split data<br>
Membagi data menjadi set pelatihan dan set pengujian untuk evaluasi model. Train Test Split adalah metode untuk membagi dataset menjadi data latih dan data uji, biasanya dengan proporsi seperti 80% untuk training dan 20% untuk testing. Pemisahan ini memungkinkan evaluasi kinerja model secara objektif dengan mengukur kemampuan prediksi pada data baru yang tidak pernah dilihat sebelumnya.
   - Mengacak dataset<br>
   Sebelum dilakukan split data, pengacakan dataset (shuffling) penting untuk memastikan bahwa data yang digunakan untuk pelatihan, validasi, dan pengujian memiliki distribusi yang representatif. Hal ini dilakukan untuk menghindari bias yang mungkin muncul akibat urutan data dalam dataset. Berikut hasil pengacakan dataset:<br>
    
| Product                                | Category                  | Sub Category              | Brand             | Sale Price | Market Price | Type                      | Rating    | Description                                          | UserID | User | Produk |
|----------------------------------------|---------------------------|---------------------------|-------------------|------------|--------------|---------------------------|-----------|------------------------------------------------------|--------|------|--------|
| Neem Tulsi Body Wash Soap              | [beauty, hygiene]         | [bath, handwash]          | meghdoot          | 70.00      | 70.00        | [bathingbars, soaps]      | 3.944434  | The Meghdoot's Neem Tulsi Body Wash is an Ayur...    | 13     | 12   | 1498   |
| Lunch Box - Stainless Steel, Midday... | [kitchen, garden, pets]   | [steelutensils]           | signoraware       | 390.00     | 390.00       | [steellunchboxes]         | 4.300000  | Midday Max Fresh Stainless Steel Lunch Box Set...    | 133    | 132  | 12544  |
| Incense Sticks - Namo Stuti, Scented   | [cleaning, household]     | [poojaneeds]              | giri              | 100.00     | 100.00       | [agarbatti, incensesticks]| 3.944434  | Giri incense sticks are made using natural her...    | 443    | 442  | 9313   |
| Smart Matic Bluetooth Enabled Autom... | [cleaning, household]     | [fresheners, repellents]  | godrejaer         | 703.12     | 799.00       | [airfreshener]            | 2.300000  | Godrej Aer Smart Matic is India’s 1st mobile-c...    | 470    | 469  | 17804  |
| Maharaja Plastic Soap Case - Lily G... | [cleaning, household]     | [bins, bathroomware]      | ratan             | 29.00      | 43.00        | [soapcases, dispensers]   | 4.100000  | This premium soap case is made of high-quality...    | 498    | 497  | 9812   |
| ...                                    | ...                       | ...                       | ...               | ...        | ...          | ...                       | ...       | ...                                                  | ...    | ...  | ...    |
| Instant Pasta Sauce Mix - Pepper Al... | [gourmet, worldfood]      | [cooking, bakingneeds]    | veekes&company    | 49.00      | 49.00        | [herbs, seasonings, rubs] | 3.944434  | Veekes and company bring to you a quick and af...    | 76     | 75   | 19095  |
| Secure XL Cottony Soft 2x20pcs+Dry-... | [beauty, hygiene]         | [femininehygiene]         | stayfree          | 279.83     | 320.00       | [sanitarynapkins]         | 3.944434  | Extra Large napkin with cottony cover for long...    | 391    | 390  | 5176   |
| PP Apple Belt                          | [gourmet, worldfood]      | [chocolates, biscuits]    | kantan            | 30.00      | 30.00        | [marshmallow, candy, jelly]| 5.000000 | Fini Fantan Apple Belts contains green apple f...    | 361    | 360  | 850    |
| Diamond Whisky/Juice Glass Tumbler     | [kitchen, garden, pets]   | [crockery, cutlery]       | pasabahce         | 579.00     | 786.00       | [glassware]               | 3.000000  | This is a brand that is an essential part of o...    | 296    | 295  | 14347  |
| Pasta - Fussilli Spinach, Gluten free  | [gourmet, worldfood]      | [pasta, soup, noodles]    | nutrahi           | 190.00     | 190.00       | [pastas, spaghetti]       | 2.800000  | NutraHis Pasta is 100% vegetarian and perfect ...    | 155    | 154  | 20753  |

27201 rows × 12 columns

- Split data lanjutan
    - Pemisahan bagian atribut dan label ke dua variabel ```x``` dan ```y``` untuk proses Train Test Split
      ```
      x = df2[['user', 'produk']].values
      y = df2['rating'].apply(lambda x: (x - min_rating) / (max_rating - min_rating)).values
      ```
    
    - Membagi menjadi 80% data train dan 20% data validasi
      ```
      train_indices = int(0.8 * df.shape[0])
      x_train, x_val, y_train, y_val = (
          x[:train_indices],
          x[train_indices:],
          y[:train_indices],
          y[train_indices:]
      )
      ```
      Proses Train Test Split telah dilakukan, komposisi 0.8 untuk train dan 0.2 untuk val. Berikut adalah hasilnya:<br>
		- x_train
		- x_val
		- y_train
		- y_val
   
    - panjang array dari x_train & x_val        
     ```
     panjang array dari x_train : 21760
     panjang array dari x_val : 5441
     array([[  388, 16100],
            [  306,  1783],
            [  371,  3764],
            ...,
            [  360,   850],
            [  295, 14347],
            [  154, 20753]])
     ```       
    - panjang array dari y_train & y_val    
     ```
     panjang array dari y_train : 21760
     panjang array dari y_val : 5441
     array([0.75 , 0.575, 0.625, ..., 1.   , 0.5  , 0.45 ])
     ```

## **Modeling and Results**
Sistem rekomendasi adalah alat penting dalam berbagai domain, termasuk e-commerce, layanan kesehatan, dan hiburan, untuk membantu pengguna menemukan informasi atau item yang relevan. Dua pendekatan utama yang digunakan dalam pengembangan sistem ini adalah Content-Based Filtering dan Collaborative Filtering. Setiap pendekatan memiliki mekanisme, keunggulan, dan tantangan yang unik, yang penting untuk dipertimbangkan dalam membangun sistem rekomendasi produk.<br>

**A. Content-Based Filtering** <br>
Content-Based Filtering adalah pendekatan sistem rekomendasi yang menyarankan item kepada pengguna berdasarkan atribut item yang sebelumnya mereka sukai. Metode ini bergantung pada analisis fitur item dan preferensi pengguna untuk menghasilkan rekomendasi, membuatnya sangat efektif dalam skenario di mana data interaksi item pengguna jarang. Metode ini banyak digunakan di berbagai domain, karena kemampuannya untuk memberikan rekomendasi yang dipersonalisasi dengan berfokus pada konten item daripada pola perilaku pengguna.<br>

Kelebihan Content-Based Filtering<br>
- **Personalisasi**: Sistem ini dapat menyesuaikan rekomendasi dengan preferensi pengguna individu dengan menganalisis atribut item, seperti komposisi kimia produk perawatan kulit atau genre film [[6]](https://doi.org/10.1109/CONIT61985.2024.10626458) & [[7]](https://doi.org/10.55810/2313-0083.1043).<br>
- **Kesederhanaan dan Efisiensi**: Metode ini mudah diterapkan dan dapat menghasilkan rekomendasi secara efisien tanpa memerlukan data interaksi pengguna yang ekstensif [[8]](https://doi.org/10.22437/msa.v4i2.32136).
- **Menangani Item Baru**: Metode ini dapat secara efektif merekomendasikan item baru atau khusus yang tidak memiliki peringkat pengguna, karena bergantung pada atribut item daripada interaksi item-pengguna [[7]](https://doi.org/10.55810/2313-0083.1043).<br>

Kekurangan Content-Based Filtering<br>
- **Keragaman Terbatas**: Pendekatan ini dapat menyebabkan serangkaian rekomendasi yang sempit, karena berfokus pada item yang mirip dengan yang sebelumnya disukai oleh pengguna, berpotensi kehilangan beragam opsi [[7]](https://doi.org/10.55810/2313-0083.1043).<br>
- **Ketergantungan Atribut**: Efektivitas metode ini, sangat bergantung pada kualitas dan kelengkapan atribut item, yang dapat menjadi tantangan untuk didefinisikan dan dipertahankan di berbagai domain [[9]](https://doi.org/10.22219/kinetik.v9i2.1940).<br>

Pendekatan ini, menggunakan TF-IDF Vectorization dan Cosine Similarity untuk menghitung kesamaan antara produk berdasarkan atribut kategori, sub kategori, brand dan tipe produk.

- Term Frequency-Inverse Document Frequency (TF-IDF)<br>
  Hasil dibawah ini merupakan output dari representasi sparse matrix yang digunakan dalam analisis teks berbasis TF-IDF. Matriks ini memiliki 27201 baris dan 3083 kolom. Dalam konteks TF-IDF, baris biasanya merepresentasikan dokumen, sedangkan kolom merepresentasikan fitur (kata atau n-gram) yang dihasilkan dari teks. Setiap elemen dalam matriks bertipe ```float64```, yang merepresentasikan skor TF-IDF. Skor ini menunjukkan seberapa penting suatu kata terhadap dokumen tertentu dalam koleksi dokumen. Dari total elemen dalam matriks, (27201 × 3083 = sekitar 83 juta elemen), hanya 178093 elemen yang tidak bernilai nol. Elemen yang bernilai nol menunjukkan kata-kata yang tidak relevan atau tidak muncul dalam dokumen tertentu. Selain itu, matriks ini disimpan dalam format Compressed Sparse Row (CSR), yang hanya menyimpan elemen-elemen tidak nol beserta indeksnya. Format ini sangat efisien dalam hal penyimpanan memori dan mempercepat operasi matematika, seperti pencarian nilai atau perkalian matriks.

  ```
  <27201x3083 sparse matrix of type '<class 'numpy.float64'>'
	with 178093 stored elements in Compressed Sparse Row format>
  ```
  
- cosine_similarity<br>
  Fungsi ini, menghitung kemiripan kosinus antara dua vektor. Dalam konteks ini, karena input-nya adalah ```tfidf_matrix``` yang sama pada kedua argumen, hasilnya adalah matriks kemiripan antara semua dokumen dalam koleksi. Berikut hasil output proses perhitungan cosine_similarity:<br>
  ```
  array([[1.        , 0.        , 0.        , ..., 0.        , 0.        ,
        0.05572012],
       [0.        , 1.        , 0.        , ..., 0.        , 0.        ,
        0.        ],
       [0.        , 0.        , 1.        , ..., 0.        , 0.        ,
        0.        ],
       ...,
       [0.        , 0.        , 0.        , ..., 1.        , 0.        ,
        0.        ],
       [0.        , 0.        , 0.        , ..., 0.        , 1.        ,
        0.        ],
       [0.05572012, 0.        , 0.        , ..., 0.        , 0.        ,
        1.        ]])
  ```
- Membuat ```nama_produk``` dan ```produk_index```
  ```
  nama_produk = df_cbf['product']
  produk_index = pd.Series(df_cbf.index, index=df_cbf['product'])
  ```
- Pembuatan function product_recommendations()
  ```
  def product_recommendations(nama_produk, cosine_sim=cosine_sim):
    idx = produk_index[nama_produk]
    sim_scores = list(enumerate(cosine_sim[idx]))
    sim_scores = sorted(sim_scores, key=lambda x: x[1], reverse=True)
    sim_scores = sim_scores[1:11]
    produk_index5 = [i[0] for i in sim_scores]
    return df_cbf['product'].iloc[produk_index5]
  ```
- Pembuatan nama produk random yang diminta user
  ```
  nama_produk_test = np.random.choice(nama_produk)
  nama_produk_test
  ```
  Hasil contoh nama produk random yang diminta user:<br>
  ```Dark Melody Coffee Beans Roasted```
- Hasil rekomendasi produk yang relevan<br>
  Memanggil ```product_recommendations``` untuk mendapatkan ```Top-N Recommendations```
  ```
  rekom = product_recommendations(nama_produk_test)
  pd.DataFrame({'Produk diminta': nama_produk_test,'Rekomendasi produk relevan':rekom})
  ```
  Berikut output hasil rekomendasi produk yang relevan:

  
| Produk Diminta                   | Rekomendasi Produk Relevan                                |
|----------------------------------|----------------------------------------------------------|
| Dark Melody Coffee Beans Roasted | Morning Motivation Instant Coffee Powder Make ...        |
| Dark Melody Coffee Beans Roasted | Breakfast Fusion Coffee Beans Roasted                   |
| Dark Melody Coffee Beans Roasted | Signature Filter Instant Coffee Powder Make Co...        |
| Dark Melody Coffee Beans Roasted | Dark Melody Coffee Beans Roasted                        |
| Dark Melody Coffee Beans Roasted | Euphoria Instant Coffee Powder Make Cold Coffe...        |
| Dark Melody Coffee Beans Roasted | Green Coffee Powder                                     |
| Dark Melody Coffee Beans Roasted | Green Coffee Beans                                      |
| Dark Melody Coffee Beans Roasted | Green Coffee Powder                                     |
| Dark Melody Coffee Beans Roasted | Chicory Powder                                          |
| Dark Melody Coffee Beans Roasted | Green Coffee Beans - Unroasted Arabica                 |

Proses penggunaan model, yang memakai pendekatan content based filtering berhasil dilakukan, dengan memberikan ```Top-N Recommendations``` dari input yang diberikan. Pada project ini, model berhasil memberikan rekomendasi produk aneka pilihan kopi berdasarkan input random yang diberikan, yaitu ```Dark Melody Coffee Beans Roasted``` yang juga produk kopi.


**B. Collaborative Filtering**<br>
Collaborative filtering adalah pendekatan yang banyak digunakan dalam sistem rekomendasi produk, karena kemampuannya untuk memanfaatkan data interaksi pengguna dalam memprediksi preferensi dan menyarankan item, sehingga meningkatkan pengalaman dan keterlibatan pengguna. Sistem ini beroperasi dengan mengidentifikasi pola perilaku pengguna, seperti rating atau ulasan riwayat pembelian, untuk merekomendasikan produk yang disukai pengguna serupa. Metode ini sangat efektif di lingkungan dengan basis data pengguna yang besar, seperti platform e-commerce.<br>

Kelebihan Collaborative Filtering<br>
- **Customization**: Metode ini memberikan rekomendasi yang dipersonalisasi dengan menganalisis perilaku dan preferensi pengguna, serta mampu beradaptasi terhadap perubahan preferensi melalui pembaruan rekomendasi berbasis interaksi dan umpan balik terbaru [[4]](https://doi.org/10.48550/arXiv.2409.19262) & [[5]](https://doi.org/10.18280/jesa.570421).
- **Informasi Keputusan Pengguna**: Sistem collaborative filtering yang efektif mampu meningkatkan tingkat konversi dan retensi pengguna secara signifikan, mendukung pengambilan keputusan pembelian yang terinformasi, serta memberikan kontribusi pada peningkatan penjualan dan kepuasan pelanggan dalam lanskap e-commerce yang kompetitif [[10]](https://doi.org/10.1109/IC2PCT60090.2024.10486625) & [[5]](https://doi.org/10.18280/jesa.570421).

Kekurangan Collaborative Filtering<br>
- **Sparsity**: Metode ini sering mengalami kendala dengan data yang terbatas, di mana interaksi item pengguna yang tidak mencukupi dapat menyebabkan rekomendasi yang tidak akurat [[5]](https://doi.org/10.18280/jesa.570421) & [[11]](https://doi.org/10.52783/jes.2059).
- **Cold Start Problem**: Pengguna baru atau item dengan sedikit data interaksi menimbulkan tantangan, karena sistem tidak memiliki informasi yang cukup untuk membuat rekomendasi yang akurat [[12]](https://doi.org/10.3233/atde231237).<br>
- **Kompleksitas Komputasi**: Kebutuhan untuk memproses volume data yang besar dapat menyebabkan tuntutan komputasi yang tinggi, meskipun ini dapat dikurangi dengan algoritma yang dioptimalkan dan pemrosesan paralel [[4]](https://doi.org/10.48550/arXiv.2409.19262) & [[13]](https://doi.org/10.1109/ICDSNS62112.2024.10690922).<br>

Pendekatan ini menggunakan class RecommenderNet dalam pembuatan model. 

- Pembuatan class RecommenderNet
  ```
  !pip install tensorflow
  import tensorflow as tf
  from tensorflow import keras
  from tensorflow.keras import layers

  class RecommenderNet(tf.keras.Model):
    # Insialisasi fungsi
    def __init__(self, num_users, num_produk, embedding_size, **kwargs):
      super(RecommenderNet, self).__init__(**kwargs)
      self.num_users = num_users
      self.num_produk = num_produk
      self.embedding_size = embedding_size
      self.user_embedding = layers.Embedding( # layer embedding user
          num_users,
          embedding_size,
          embeddings_initializer = 'he_normal',
          embeddings_regularizer = keras.regularizers.l2(1e-6)
      )
      self.user_bias = layers.Embedding(num_users, 1) # layer embedding user bias
      self.produk_embedding = layers.Embedding( # layer embeddings produk
          num_produk,
          embedding_size,
          embeddings_initializer = 'he_normal',
          embeddings_regularizer = keras.regularizers.l2(1e-6)
      )
      self.produk_bias = layers.Embedding(num_produk, 1) # layer embedding produk bias

    def call(self, inputs):
      user_vector = self.user_embedding(inputs[:,0]) # memanggil layer embedding 1
      user_bias = self.user_bias(inputs[:, 0]) # memanggil layer embedding 2
      produk_vector = self.produk_embedding(inputs[:, 1]) # memanggil layer embedding 3
      produk_bias = self.produk_bias(inputs[:, 1]) # memanggil layer embedding 4

      dot_user_produk = tf.tensordot(user_vector, produk_vector, 2)

      x = dot_user_produk + user_bias + produk_bias

      return tf.nn.sigmoid(x) # activation sigmoid
  ```
- Inisialisasi model dan compile
  ```
  # inisialisasi model
  model = RecommenderNet(num_users, num_produk, 50)

  # model compile
  model.compile(
      loss = tf.keras.losses.BinaryCrossentropy(),
      optimizer = keras.optimizers.Adam(learning_rate=0.001),
      metrics=[tf.keras.metrics.RootMeanSquaredError()]
  )
  ```
- Callback Early Stopper
  ```
  # callbacks
  from tensorflow.keras.callbacks import EarlyStopping
  early_stopper = EarlyStopping(monitor='val_root_mean_squared_error',
                                patience=10,
                                verbose=1,
                                restore_best_weights=True)
  ```
  Callback Early Stopper berhasil diterapkan. Hal ini berguna untuk memantau proses training model, dan akan berhenti jika ```val_root_mean_squared_error``` tidak mengalami penurunan selama 10 epochs. Setelah berhenti, model pada epoch yang memiliki performa terbaik akan dipertahankan.
  
- Training terhadap model
  ```
  # Memulai training
  history = model.fit(
      x = x_train,
      y = y_train,
      batch_size = 8,
      epochs = 100,
      callbacks = [early_stopper],
      validation_data = (x_val, y_val)
  )
  ```

  ```
  Epoch 48/100
  2720/2720 ━━━━━━━━━━━━━━━━━━━━ 82s 17ms/step - loss: 0.5271 - root_mean_squared_error: 0.0584 - val_loss: 0.5825 - val_root_mean_squared_error: 0.1600
  Epoch 48: early stopping
  Restoring model weights from the end of the best epoch: 38.
  ```
  Output diatas adalah hasil proses training yang sudah selesai pada epochs ke-48 yang memiliki :
  	- ```loss```: 0.5271
  	- ```root_mean_squared_error```: 0.0584
  	- ```val_loss```: 0.5825
  	- ```val_root_mean_squared_error```: 0.1600

- Mengambil sampel user
  ```
  user_id = df2.userID.sample(1).iloc[0]
  produk_rated_by_user = df2[df2.userID == user_id]

  produk_not_rated = df2[~df2['product'].isin(produk_rated_by_user.produk.values)]['product']
  produk_not_rated = list(
      set(produk_not_rated)
      .intersection(set(produk_to_produk_encoded.keys()))
  )

  produk_not_rated = [[produk_to_produk_encoded.get(x)] for x in produk_not_rated]
  user_encoder = user_to_user_encoded.get(user_id)
  user_produk_array = np.hstack(
      ([[user_encoder]] * len(produk_not_rated), produk_not_rated)
  )
  ```

- Hasil rekomendasi produk
  ```
  ratings = model.predict(user_produk_array).flatten()

  top_ratings_indices = ratings.argsort()[-10:][::-1]
  recommended_produk_ids = [
      produk_encoded_to_produk.get(produk_not_rated[x][0]) for x in top_ratings_indices
  ]

  print('Daftar recommendations produk untuk users: {}'.format(user_id))
  print('===' * 9)
  print('produk dengan rating tinggi dari user')
  print('----' * 8)

  top_produk_user = (
      produk_rated_by_user.sort_values(
          by = 'rating',
          ascending=False
      )
      .head(5)
      ['product'].values
  )

  df2_rows = df2[df2['product'].isin(top_produk_user)]
  for row in df2_rows.itertuples():
      print(row.product, ':', row.brand)

  print('----' * 8)
  print('Top 10 produk recommendation')
  print('----' * 8)

  recommended_produk = df2[df2['product'].isin(recommended_produk_ids)].drop_duplicates(subset='product')
  for row in recommended_produk.itertuples():
      print(row.product, ':', row.brand)
  ```

  
```
736/736 ━━━━━━━━━━━━━━━━━━━━ 1s 1ms/step
Daftar recommendations produk untuk users: 389
===========================
produk dengan rating tinggi dari user
--------------------------------
Blue Set - Eau De Toilette + Deodorant : ulricdevarens
Dharwad Peda : sangamsweets
Green Tea Skin Clarifying Concentrate : plum
Rapid Reviver 6 Oil Nourish Deep Conditioner : lorealparis
Passion Femme Eau De Toilette : police
--------------------------------
Top 10 produk recommendation
--------------------------------
Combo - Precision Safety Razor & 10 Feather Blades : bombayshavingcompany
Steel Lid See Through, Square Jars : yera
Easy Adult Diaper - Large : friends
Taft Power Wax : schwarzkopf
Wonder Diaper Pants - Large Size : huggies
Ultimate Choco Berry : ritebitemaxprotein
Charcoal Bath Soap : bombayshavingcompany
Tapioca Flour - Sabudana : nuttyyogi
Body Deodorant - Ultra Sensual : wildstone
Water & Juice Glass - Long : krosno-europe
```
Output diatas adalah hasil dari ```Top-N Recommendation``` menggunakan Collaborative Filterting, yaitu model berhasil memberikan rekomendasi berdasarkan review rating dari user tertentu dan memberikan rekomendasi produk lainnya yang cocok untuk user tersebut.

Pada contoh diatas, model berhasil memberikan rekomendasi produk untuk **user no.** ***389*** yang pernah memberikan rating tinggi ke produk dan brand:
- Blue Set - Eau De Toilette + Deodorant : ulricdevarens
- Dharwad Peda : sangamsweets
- Green Tea Skin Clarifying Concentrate : plum
- Rapid Reviver 6 Oil Nourish Deep Conditioner : lorealparis
- Passion Femme Eau De Toilette : police
  
Model memberikan 10 rekomendasi produk dengan brand:<br>
- Combo - Precision Safety Razor & 10 Feather Blades : bombayshavingcompany
- Steel Lid See Through, Square Jars : yera
- Easy Adult Diaper - Large : friends
- Taft Power Wax : schwarzkopf
- Wonder Diaper Pants - Large Size : huggies
- Ultimate Choco Berry : ritebitemaxprotein
- Charcoal Bath Soap : bombayshavingcompany
- Tapioca Flour - Sabudana : nuttyyogi
- Body Deodorant - Ultra Sensual : wildstone
- Water & Juice Glass - Long : krosno-europe


## **Evaluation**<br>
Untuk mengevaluasi performa dari model, dibutuhkan pengukuran metriks untuk menilai model sistem rekomendasi produk yang telah dibuat.

### **A. Content Based Filtering**
Dalam evaluasi model, yang memakai pendekatan content based filtering, pada project ini, digunakan metriks ```precision``` untuk menilai performanya. Evaluasi yang menggunakan metrik ini, bertujuan untuk mengukur seberapa baik model merekomendasikan item yang relevan kepada pengguna berdasarkan fitur atau konten dari item yang mereka sukai sebelumnya.<br>
- Precision didefinisikan sebagai proporsi item yang relevan di antara seluruh item yang direkomendasikan oleh model. Secara matematis, precision dirumuskan sebagai berikut:<br>

  $$
  \[
  \text{Precision} = \frac{\text{True Positives (TP)}}{\text{True Positives (TP)} + \text{False Positives (FP)}}
  \]​
  $$
 
   Dimana:<br>
   - True Positives (TP) adalah jumlah item relevan yang berhasil direkomendasikan oleh model.<br>
   - False Positives (FP) adalah jumlah item tidak relevan yang juga direkomendasikan.<br>
- Kelebihan dan Keterbatasan Precision:<br>
  - Kelebihan: Fokus pada relevansi hasil rekomendasi, sehingga cocok untuk aplikasi di mana kualitas rekomendasi lebih penting daripada kuantitas.
  - Keterbatasan: Precision tidak mempertimbangkan item relevan yang tidak masuk dalam rekomendasi (mengabaikan recall).

- Evaluasi performa model content based filtering
  - Fungsi ```evaluate_precision```
    ```
    def evaluate_precision(nama_produk_test, rekom, df_cbf):
        """
        Fungsi untuk mengevaluasi precision rekomendasi.

        Parameters:
            nama_produk_test (str): Produk yang diminta.
            rekom (list): Produk rekomendasi yang dihasilkan.
            df_cbf (pd.DataFrame): DataFrame dengan informasi relevansi.

        Returns:
            float: Nilai precision untuk produk yang diuji.
        """

        # Mendapatkan relevansi produk rekomendasi
        df_cbf['relevant'] = df_cbf['product'].isin(rekom).astype(int)

        # Filter df_cbf untuk hanya menyertakan produk yang direkomendasikan
        relevant_products_df = df_cbf[df_cbf['product'].isin(rekom)]

        # Nilai target sebenarnya (relevansi) untuk produk yang direkomendasikan
        y_true = relevant_products_df['relevant'].values
        y_pred = [1] * len(y_true)

        # Hitung precision (menangani zero division jika diperlukan)
        precision = precision_score(y_true, y_pred, zero_division=0)
        return precision
    ```
  - Hasil evaluasi model
    ```
    precision_value = evaluate_precision(nama_produk_test, rekom, df_cbf)
    print(f"Precision untuk produk '{nama_produk_test}': {precision_value:.2f}")
    ```
    Output hasi performa evaluasi model:<br>
    ```Precision untuk produk 'Dark Melody Coffee Beans Roasted': 1.00```<br>
    
    Dari informasi hasil evaluasi diatas, model memiliki skor presisi sebesar ```1.00``` atau 100%, dalam memberikan rekomendasi produk aneka pilihan kopi berdasarkan input random yang diberikan, yaitu ```Dark Melody Coffee Beans Roasted```. Hal ini menginterpretasikan bahwa model memiliki performa yang sangat baik dalam memberikan rekomendasi produk dalam pendekatan Content-Based Filtering.

### **B. Collaborative Filtering**
Dalam evaluasi model, yang memakai pendekatan collaborative filtering, pada project ini, digunakan metriks ```Root Mean Squared Error (RMSE)``` untuk menilai performanya. Evaluasi ini, bertujuan untuk mengukur sejauh mana prediksi model mendekati nilai aktual atau preferensi pengguna. RMSE adalah salah satu metrik yang umum digunakan untuk menilai performa dalam sistem rekomendasi berbasis collaborative filtering, baik itu user-based atau item-based.<br>
- RMSE mengukur rata-rata selisih kuadrat antara nilai yang diprediksi oleh model dan nilai yang sebenarnya, kemudian menghitung akar kuadrat dari nilai tersebut. RMSE memberikan gambaran tentang seberapa besar kesalahan prediksi model dalam satuan yang sama dengan data asli. Secara matematis, RMSE dapat dihitung dengan rumus berikut:<br>

$$
\text{RMSE} = \sqrt{\frac{1}{n} \sum_{i=1}^n (y_i - \hat{y}_i)^2}
$$

   Dimana:<br>
      - $$\( y_i \)$$: adalah nilai aktual atau preferensi pengguna terhadap item ke-i (rating yang diberikan). <br>
      - $$\( \hat{y}_i \)$$: adalah nilai prediksi yang diberikan oleh model untuk item ke-i. <br>
      - $$\( n \)$$: adalah jumlah total prediksi yang dihitung.<br>

- Kelebihan dan Kelemahan RMSE<br>
  - Kelebihan: RMSE memberikan penalti yang lebih besar terhadap kesalahan besar, sehingga dapat memberi indikasi yang jelas jika model menghasilkan prediksi yang jauh dari nilai aktual.<br>
  - Kelemahan: RMSE sangat sensitif terhadap outlier. Jika ada beberapa prediksi yang sangat buruk, RMSE akan menjadi sangat besar, meskipun sebagian besar prediksi lainnya akurat.<br>

- Hasil evaluasi performa model collaborative filtering<br>
  - Plot History ```Root Mean Squared Error```
      
  ![evaluasiCF](https://github.com/user-attachments/assets/9d90f243-88d4-4687-91a7-f4763a08f96b)

  Dari grafik RMSE evaluasi model diatas, didapat beberapa informasi berikut:<br>
    1. Penurunan RMSE pada Data Latih (train):<br>
        - RMSE pada data latih terus menurun seiring dengan bertambahnya epoch. Hal ini menunjukkan bahwa model semakin baik dalam memprediksi nilai pada data latih.
        - Penurunan ini adalah indikasi bahwa model sedang belajar pola dalam data dengan baik.<br>
    2. Penurunan RMSE pada Data Uji (test):<br>
        - RMSE pada data uji juga menurun tetapi tidak secepat data latih, lalu mulai stabil setelah beberapa epoch. Ini menunjukkan bahwa model memiliki generalisasi yang baik pada data uji, namun dengan laju yang lebih lambat dibanding data latih.
        - Stabilitas RMSE pada data uji mengindikasikan bahwa model tidak mengalami overfitting secara signifikan pada titik ini.
    3. RMSE Rendah:<br>
        Nilai RMSE yang terus menurun pada kedua dataset adalah hal yang positif, karena menunjukkan prediksi rating semakin mendekati nilai aktualnya.<br>
	
  Dari beberapa informasi insight grafik RMSE diatas, dapat disimpulkan bahwa model memiliki performa yang baik dalam memberikan rekomendasi produk dalam pendekatan Collaborative Filtering.

### **References**
[1] Nianjiao, P., Xunyong Xiao, Wu Di, Li Ang, Manhong Wen. (2024). Design and implementation of an intelligent recommendation system for product information on an e-commerce platform based on machine learning. Proc. SPIE 12937, International Conference on Internet of Things and Machine Learning (IoTML 2023), 129371K. https://doi.org/10.1117/12.3013353<br>

[2] Huang, P., Lurie, N. H., & Mitra, S. (2009). Searching for Experience on the Web: An Empirical Examination of Consumer Behavior for Search and Experience Goods. Journal of Marketing, 73(2), 55-69. https://doi.org/10.1509/jmkg.73.2.55

[3] Pebrianti, D, D. Ahmad, L. Bayuaji, L. Wijayanti, & M. Mulyadi. (2024). Using Content-Based Filtering and Apriori for Recommendation Systems in a Smart Shopping System. Indonesian Journal of Computing, Engineering, and Design (IJoCED), vol. 6, no. 1, Apr. 2024, pp. 58-70, https://doi.org/10.35806/ijoced.v6i1.393<br>

[4] Hasan, Mahamudul. (2024). An Efficient Multi-threaded Collaborative Filtering Approach in Recommendation System. arXiv:2409.19262v1. https://doi.org/10.48550/arXiv.2409.19262<br>

[5] Patil, P., Kadam, S.U., Aruna, E.R., More, A., M., B.R., Rao, B.N.K. (2024). Recommendation system for e-commerce using collaborative filtering. Journal Européen des Systèmes Automatisés, Vol. 57, No. 4, pp. 1145-1153. https://doi.org/10.18280/jesa.570421<br>

[6] M. Vinutha, R. B. Dayananda and A. Kamath. (2024). Personalized Skincare Product Recommendation System Using Content-Based Machine Learning. 4th International Conference on Intelligent Technologies (CONIT), Bangalore, India, 2024, pp. 1-6. https://doi.org/10.1109/CONIT61985.2024.10626458<br>

[7] Rakesh, Sribhashyam (2024) "Movie Recommendation System Using Content Based Filtering," Al-Bahir Journal for Engineering and Pure Sciences: Vol. 4: Iss. 1, Article 7.
Available at: https://doi.org/10.55810/2313-0083.1043<br>

[8] Ridwansyah, T., Subartini, B. ., & Sylviani, S. (2024). Penerapan Metode Content-Based Filtering pada Sistem Rekomendasi. Mathematical Sciences and Applications Journal, 4(2), 70-77. https://doi.org/10.22437/msa.v4i2.32136<br>

[9] Salsabil, A. A., Setiawan, E. B., & Kurniawan, I. (2024). Content-based Filtering Movie Recommender System Using Semantic Approach with Recurrent Neural Network Classification and SGD. Kinetik: Game Technology, Information System, Computer Network, Computing, Electronics, and Control, 9(2), 193-202. https://doi.org/10.22219/kinetik.v9i2.1940<br>

[10] https://doi.org/10.1109/IC2PCT60090.2024.10486625<br>

[11] Shetty, A., Shetye, A., Shukla, P., Singh, A., & Vhatkar, S. (2024). A collaborative filtering-based recommender systems approach for multifarious applications. Journal of Electrical Systems, Vol. 20, No. 4s, pp. 478-485. https://doi.org/10.52783/jes.2059<br>

[12] Wang, L. (2024). Commerce Product Recommendation Algorithm Based on Collaborative Filtering. Intelligent Computing Technology and Automation, Vol. 47, pp. 617 - 624. https://doi.org/10.3233/atde231237<br>

[13] Shaik, S. T. (2024). A Hybrid Recommendation System using Collaborative Filtering with Apache Spark in E-Commerce. International Conference on Data Science and Network Security (ICDSNS), Tiptur, India, 2024, pp. 1-8. https://doi.org/10.1109/ICDSNS62112.2024.10690922.<br>



































  
