# product-recommendation-system
*dalam proses pembuatan (belum selesai)*

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

| Index | Product | Category               | Sub-Category       | Brand            | Sale Price | Market Price | Type                     | Rating | Description                                    |
|-------|---------|------------------------|--------------------|------------------|------------|--------------|--------------------------|--------|------------------------------------------------|
| 0     | Garlic Oil - Vegetarian Capsule 500 mg | Beauty & Hygiene     | Hair Care          | Sri Sri Ayurveda | 220.0      | 220.0        | Hair Oil & Serum         | 4.1    | This Product contains Garlic Oil that is known... |
| 1     | Water Bottle - Orange           | Kitchen, Garden & Pets | Storage & Accessories | Mastercook       | 180.0      | 180.0        | Water & Fridge Bottles   | 2.3    | Each product is microwave safe (without lid), ... |
| 2     | Brass Angle Deep - Plain, No.2  | Cleaning & Household   | Pooja Needs        | Trm              | 119.0      | 250.0        | Lamp & Lamp Oil          | 3.4    | A perfect gift for all occasions, be it your m... |
| 3     | Cereal Flip Lid Container/Storage Jar - Assorted | Cleaning & Household   | Bins & Bathroom Ware | Nakoda           | 149.0      | 176.0        | Laundry, Storage Baskets | 3.7    | Multipurpose container with an attractive desi... |
| 4     | Creme Soft Soap - For Hands & Body | Beauty & Hygiene     | Bath & Hand Wash    | Nivea            | 162.0      | 162.0        | Bathing Bars & Soaps     | 4.4    | Nivea Creme Soft Soap gives your skin the best... |


**2. Struktur dataset**<br>
Dataset terdiri dari 10 kolom dengan 27555 baris. Mayoritas kolom bertipe ```object``` dan sebagian bertipe ```float64``` kecuali kolom *index* bertipe ```int64```. Berikut detail variabel fitur dataset ini:<br>

- ```index```: Nama obat yang digunakan.
- ```product```: Nama produk (dapat digunakan untuk deskripsi dalam Content-Based Filtering).
- ```category```: Kategori utama produk.
- ```sub_category```: Sub-kategori produk.
- ```brand```: Merek produk.
- ```sale_price```: Harga jual produk diplatform e-commerce.
- ```market_price```: Harga produk dipasaran.
- ```type```: Tipe produk (lebih spesifik dibanding kategori/sub-kategori).
- ```rating```: Rating pengguna (berguna untuk evaluasi Collaborative Filtering).
- ```description```: Deskripsi produk (fitur utama untuk Content-Based Filtering).
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
Kolom *rating* memiliki nilai yang hilang yang paling tinggi (8626 nilai kosong, dari total 27555 entri), yang bisa jadi mempengaruhi analisis lebih lanjut.
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
- index mempunyai 27555 nilai yang unik
- product mempunyai 23540 nilai yang unik
- category mempunyai 11 nilai yang unik
- sub_category mempunyai 90 nilai yang unik
- brand mempunyai 2313 nilai yang unik
- sale_price mempunyai 3256 nilai yang unik
- market_price mempunyai 1348 nilai yang unik
- type mempunyai 426 nilai yang unik
- rating mempunyai 40 nilai yang unik
- description mempunyai 21944 nilai yang unik

**6. Visualisasi jumlah item disetiap kategori**

![vis kategori](https://github.com/user-attachments/assets/11780fc7-c6ca-4aa8-bf3a-a3dba16c05af)

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

## **Data Preparation**
**1. Dropping Uneeded Column**<br>
Kita akan menghapus kolom yang tidak relevan (```index```) untuk analisis lebih lanjut.

| Product                                    | Category                | Sub-Category         | Brand             | Sale Price | Market Price | Type                    | Rating | Description                                                                 |
|--------------------------------------------|-------------------------|----------------------|-------------------|------------|--------------|-------------------------|--------|-----------------------------------------------------------------------------|
| Garlic Oil - Vegetarian Capsule 500 mg    | Beauty & Hygiene        | Hair Care            | Sri Sri Ayurveda  | 220.0      | 220.0        | Hair Oil & Serum        | 4.1    | This Product contains Garlic Oil that is known...                          |
| Water Bottle - Orange                      | Kitchen, Garden & Pets  | Storage & Accessories| Mastercook        | 180.0      | 180.0        | Water & Fridge Bottles  | 2.3    | Each product is microwave safe (without lid), ...                          |
| Brass Angle Deep - Plain, No.2             | Cleaning & Household    | Pooja Needs          | Trm               | 119.0      | 250.0        | Lamp & Lamp Oil         | 3.4    | A perfect gift for all occasions, be it your m...                          |
| Cereal Flip Lid Container/Storage Jar - Assorted | Cleaning & Household | Bins & Bathroom Ware | Nakoda            | 149.0      | 176.0        | Laundry, Storage Baskets| 3.7    | Multipurpose container with an attractive desi...                          |
| Creme Soft Soap - For Hands & Body         | Beauty & Hygiene        | Bath & Hand Wash     | Nivea             | 162.0      | 162.0        | Bathing Bars & Soaps    | 4.4    | Nivea Creme Soft Soap gives your skin the best...                          |

**2. Detection and Removal Duplicates**<br>
Sebelum membangun model, kita akan memeriksa duplikasi dalam data dan menghapusnya untuk memastikan data yang digunakan tidak redundan.
```
Total Duplicates : 354
After remove Duplicates : 0
```

**3. Handling Missing Value**<br>
Kita juga perlu menangani nilai yang hilang dalam data, baik dengan mengisi nilai yang hilang atau menghapus baris yang mengandung nilai yang hilang.
- Distribusi kolom rating
  
![vis distribusi rating](https://github.com/user-attachments/assets/4cab67ac-2d0a-4687-8c8c-b1da3a65c2a7)

- Mean rating<br>
```3.943409583179249```
- Median rating<br>
```4.1```
- Nilai kolom ```rating``` yang hilang, diisi dengan nilai mean rating
- Prosentase nilai yang hilang keseluruan<br>
```Total Data Null: 0.05```
- Hasil setelah nilai yang hilang dihapus

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

**4. Changing Certain Value** (menghapus spasi, memisahkan string, menyusun dalam bentuk list)

**5. Changing Certain Value** (merubah jadi huruf kecil, menghilangkan spasi)

**6. Content-Based Filtering** <br>
- Menggabungkan nilai kategori, sub kategori, tipe & brand
- Ekstraksi Fitur TF-IDF

**7. Collaborative Filtering**<br>
- Simulate user IDs

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
Untuk menggunakan algoritma berbasis matriks, kita perlu mengonversi data kategorikal menjadi numerik.
```
list userID:  [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 77, 78, 79, 80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90, 91, 92, 93, 94, 95, 96, 97, 98, 99, 100, 101, 102, 103, 104, 105, 106, 107, 108, 109, 110, 111, 112, 113, 114, 115, 116, 117, 118, 119, 120, 121, 122, 123, 124, 125, 126, 127, 128, 129, 130, 131, 132, 133, 134, 135, 136, 137, 138, 139, 140, 141, 142, 143, 144, 145, 146, 147, 148, 149, 150, 151, 152, 153, 154, 155, 156, 157, 158, 159, 160, 161, 162, 163, 164, 165, 166, 167, 168, 169, 170, 171, 172, 173, 174, 175, 176, 177, 178, 179, 180, 181, 182, 183, 184, 185, 186, 187, 188, 189, 190, 191, 192, 193, 194, 195, 196, 197, 198, 199, 200, 201, 202, 203, 204, 205, 206, 207, 208, 209, 210, 211, 212, 213, 214, 215, 216, 217, 218, 219, 220, 221, 222, 223, 224, 225, 226, 227, 228, 229, 230, 231, 232, 233, 234, 235, 236, 237, 238, 239, 240, 241, 242, 243, 244, 245, 246, 247, 248, 249, 250, 251, 252, 253, 254, 255, 256, 257, 258, 259, 260, 261, 262, 263, 264, 265, 266, 267, 268, 269, 270, 271, 272, 273, 274, 275, 276, 277, 278, 279, 280, 281, 282, 283, 284, 285, 286, 287, 288, 289, 290, 291, 292, 293, 294, 295, 296, 297, 298, 299, 300, 301, 302, 303, 304, 305, 306, 307, 308, 309, 310, 311, 312, 313, 314, 315, 316, 317, 318, 319, 320, 321, 322, 323, 324, 325, 326, 327, 328, 329, 330, 331, 332, 333, 334, 335, 336, 337, 338, 339, 340, 341, 342, 343, 344, 345, 346, 347, 348, 349, 350, 351, 352, 353, 354, 355, 356, 357, 358, 359, 360, 361, 362, 363, 364, 365, 366, 367, 368, 369, 370, 371, 372, 373, 374, 375, 376, 377, 378, 379, 380, 381, 382, 383, 384, 385, 386, 387, 388, 389, 390, 391, 392, 393, 394, 395, 396, 397, 398, 399, 400, 401, 402, 403, 404, 405, 406, 407, 408, 409, 410, 411, 412, 413, 414, 415, 416, 417, 418, 419, 420, 421, 422, 423, 424, 425, 426, 427, 428, 429, 430, 431, 432, 433, 434, 435, 436, 437, 438, 439, 440, 441, 442, 443, 444, 445, 446, 447, 448, 449, 450, 451, 452, 453, 454, 455, 456, 457, 458, 459, 460, 461, 462, 463, 464, 465, 466, 467, 468, 469, 470, 471, 472, 473, 474, 475, 476, 477, 478, 479, 480, 481, 482, 483, 484, 485, 486, 487, 488, 489, 490, 491, 492, 493, 494, 495, 496, 497, 498, 499, 500]
encoded userID :  {1: 0, 2: 1, 3: 2, 4: 3, 5: 4, 6: 5, 7: 6, 8: 7, 9: 8, 10: 9, 11: 10, 12: 11, 13: 12, 14: 13, 15: 14, 16: 15, 17: 16, 18: 17, 19: 18, 20: 19, 21: 20, 22: 21, 23: 22, 24: 23, 25: 24, 26: 25, 27: 26, 28: 27, 29: 28, 30: 29, 31: 30, 32: 31, 33: 32, 34: 33, 35: 34, 36: 35, 37: 36, 38: 37, 39: 38, 40: 39, 41: 40, 42: 41, 43: 42, 44: 43, 45: 44, 46: 45, 47: 46, 48: 47, 49: 48, 50: 49, 51: 50, 52: 51, 53: 52, 54: 53, 55: 54, 56: 55, 57: 56, 58: 57, 59: 58, 60: 59, 61: 60, 62: 61, 63: 62, 64: 63, 65: 64, 66: 65, 67: 66, 68: 67, 69: 68, 70: 69, 71: 70, 72: 71, 73: 72, 74: 73, 75: 74, 76: 75, 77: 76, 78: 77, 79: 78, 80: 79, 81: 80, 82: 81, 83: 82, 84: 83, 85: 84, 86: 85, 87: 86, 88: 87, 89: 88, 90: 89, 91: 90, 92: 91, 93: 92, 94: 93, 95: 94, 96: 95, 97: 96, 98: 97, 99: 98, 100: 99, 101: 100, 102: 101, 103: 102, 104: 103, 105: 104, 106: 105, 107: 106, 108: 107, 109: 108, 110: 109, 111: 110, 112: 111, 113: 112, 114: 113, 115: 114, 116: 115, 117: 116, 118: 117, 119: 118, 120: 119, 121: 120, 122: 121, 123: 122, 124: 123, 125: 124, 126: 125, 127: 126, 128: 127, 129: 128, 130: 129, 131: 130, 132: 131, 133: 132, 134: 133, 135: 134, 136: 135, 137: 136, 138: 137, 139: 138, 140: 139, 141: 140, 142: 141, 143: 142, 144: 143, 145: 144, 146: 145, 147: 146, 148: 147, 149: 148, 150: 149, 151: 150, 152: 151, 153: 152, 154: 153, 155: 154, 156: 155, 157: 156, 158: 157, 159: 158, 160: 159, 161: 160, 162: 161, 163: 162, 164: 163, 165: 164, 166: 165, 167: 166, 168: 167, 169: 168, 170: 169, 171: 170, 172: 171, 173: 172, 174: 173, 175: 174, 176: 175, 177: 176, 178: 177, 179: 178, 180: 179, 181: 180, 182: 181, 183: 182, 184: 183, 185: 184, 186: 185, 187: 186, 188: 187, 189: 188, 190: 189, 191: 190, 192: 191, 193: 192, 194: 193, 195: 194, 196: 195, 197: 196, 198: 197, 199: 198, 200: 199, 201: 200, 202: 201, 203: 202, 204: 203, 205: 204, 206: 205, 207: 206, 208: 207, 209: 208, 210: 209, 211: 210, 212: 211, 213: 212, 214: 213, 215: 214, 216: 215, 217: 216, 218: 217, 219: 218, 220: 219, 221: 220, 222: 221, 223: 222, 224: 223, 225: 224, 226: 225, 227: 226, 228: 227, 229: 228, 230: 229, 231: 230, 232: 231, 233: 232, 234: 233, 235: 234, 236: 235, 237: 236, 238: 237, 239: 238, 240: 239, 241: 240, 242: 241, 243: 242, 244: 243, 245: 244, 246: 245, 247: 246, 248: 247, 249: 248, 250: 249, 251: 250, 252: 251, 253: 252, 254: 253, 255: 254, 256: 255, 257: 256, 258: 257, 259: 258, 260: 259, 261: 260, 262: 261, 263: 262, 264: 263, 265: 264, 266: 265, 267: 266, 268: 267, 269: 268, 270: 269, 271: 270, 272: 271, 273: 272, 274: 273, 275: 274, 276: 275, 277: 276, 278: 277, 279: 278, 280: 279, 281: 280, 282: 281, 283: 282, 284: 283, 285: 284, 286: 285, 287: 286, 288: 287, 289: 288, 290: 289, 291: 290, 292: 291, 293: 292, 294: 293, 295: 294, 296: 295, 297: 296, 298: 297, 299: 298, 300: 299, 301: 300, 302: 301, 303: 302, 304: 303, 305: 304, 306: 305, 307: 306, 308: 307, 309: 308, 310: 309, 311: 310, 312: 311, 313: 312, 314: 313, 315: 314, 316: 315, 317: 316, 318: 317, 319: 318, 320: 319, 321: 320, 322: 321, 323: 322, 324: 323, 325: 324, 326: 325, 327: 326, 328: 327, 329: 328, 330: 329, 331: 330, 332: 331, 333: 332, 334: 333, 335: 334, 336: 335, 337: 336, 338: 337, 339: 338, 340: 339, 341: 340, 342: 341, 343: 342, 344: 343, 345: 344, 346: 345, 347: 346, 348: 347, 349: 348, 350: 349, 351: 350, 352: 351, 353: 352, 354: 353, 355: 354, 356: 355, 357: 356, 358: 357, 359: 358, 360: 359, 361: 360, 362: 361, 363: 362, 364: 363, 365: 364, 366: 365, 367: 366, 368: 367, 369: 368, 370: 369, 371: 370, 372: 371, 373: 372, 374: 373, 375: 374, 376: 375, 377: 376, 378: 377, 379: 378, 380: 379, 381: 380, 382: 381, 383: 382, 384: 383, 385: 384, 386: 385, 387: 386, 388: 387, 389: 388, 390: 389, 391: 390, 392: 391, 393: 392, 394: 393, 395: 394, 396: 395, 397: 396, 398: 397, 399: 398, 400: 399, 401: 400, 402: 401, 403: 402, 404: 403, 405: 404, 406: 405, 407: 406, 408: 407, 409: 408, 410: 409, 411: 410, 412: 411, 413: 412, 414: 413, 415: 414, 416: 415, 417: 416, 418: 417, 419: 418, 420: 419, 421: 420, 422: 421, 423: 422, 424: 423, 425: 424, 426: 425, 427: 426, 428: 427, 429: 428, 430: 429, 431: 430, 432: 431, 433: 432, 434: 433, 435: 434, 436: 435, 437: 436, 438: 437, 439: 438, 440: 439, 441: 440, 442: 441, 443: 442, 444: 443, 445: 444, 446: 445, 447: 446, 448: 447, 449: 448, 450: 449, 451: 450, 452: 451, 453: 452, 454: 453, 455: 454, 456: 455, 457: 456, 458: 457, 459: 458, 460: 459, 461: 460, 462: 461, 463: 462, 464: 463, 465: 464, 466: 465, 467: 466, 468: 467, 469: 468, 470: 469, 471: 470, 472: 471, 473: 472, 474: 473, 475: 474, 476: 475, 477: 476, 478: 477, 479: 478, 480: 479, 481: 480, 482: 481, 483: 482, 484: 483, 485: 484, 486: 485, 487: 486, 488: 487, 489: 488, 490: 489, 491: 490, 492: 491, 493: 492, 494: 493, 495: 494, 496: 495, 497: 496, 498: 497, 499: 498, 500: 499}
encoded angka ke userID:  {0: 1, 1: 2, 2: 3, 3: 4, 4: 5, 5: 6, 6: 7, 7: 8, 8: 9, 9: 10, 10: 11, 11: 12, 12: 13, 13: 14, 14: 15, 15: 16, 16: 17, 17: 18, 18: 19, 19: 20, 20: 21, 21: 22, 22: 23, 23: 24, 24: 25, 25: 26, 26: 27, 27: 28, 28: 29, 29: 30, 30: 31, 31: 32, 32: 33, 33: 34, 34: 35, 35: 36, 36: 37, 37: 38, 38: 39, 39: 40, 40: 41, 41: 42, 42: 43, 43: 44, 44: 45, 45: 46, 46: 47, 47: 48, 48: 49, 49: 50, 50: 51, 51: 52, 52: 53, 53: 54, 54: 55, 55: 56, 56: 57, 57: 58, 58: 59, 59: 60, 60: 61, 61: 62, 62: 63, 63: 64, 64: 65, 65: 66, 66: 67, 67: 68, 68: 69, 69: 70, 70: 71, 71: 72, 72: 73, 73: 74, 74: 75, 75: 76, 76: 77, 77: 78, 78: 79, 79: 80, 80: 81, 81: 82, 82: 83, 83: 84, 84: 85, 85: 86, 86: 87, 87: 88, 88: 89, 89: 90, 90: 91, 91: 92, 92: 93, 93: 94, 94: 95, 95: 96, 96: 97, 97: 98, 98: 99, 99: 100, 100: 101, 101: 102, 102: 103, 103: 104, 104: 105, 105: 106, 106: 107, 107: 108, 108: 109, 109: 110, 110: 111, 111: 112, 112: 113, 113: 114, 114: 115, 115: 116, 116: 117, 117: 118, 118: 119, 119: 120, 120: 121, 121: 122, 122: 123, 123: 124, 124: 125, 125: 126, 126: 127, 127: 128, 128: 129, 129: 130, 130: 131, 131: 132, 132: 133, 133: 134, 134: 135, 135: 136, 136: 137, 137: 138, 138: 139, 139: 140, 140: 141, 141: 142, 142: 143, 143: 144, 144: 145, 145: 146, 146: 147, 147: 148, 148: 149, 149: 150, 150: 151, 151: 152, 152: 153, 153: 154, 154: 155, 155: 156, 156: 157, 157: 158, 158: 159, 159: 160, 160: 161, 161: 162, 162: 163, 163: 164, 164: 165, 165: 166, 166: 167, 167: 168, 168: 169, 169: 170, 170: 171, 171: 172, 172: 173, 173: 174, 174: 175, 175: 176, 176: 177, 177: 178, 178: 179, 179: 180, 180: 181, 181: 182, 182: 183, 183: 184, 184: 185, 185: 186, 186: 187, 187: 188, 188: 189, 189: 190, 190: 191, 191: 192, 192: 193, 193: 194, 194: 195, 195: 196, 196: 197, 197: 198, 198: 199, 199: 200, 200: 201, 201: 202, 202: 203, 203: 204, 204: 205, 205: 206, 206: 207, 207: 208, 208: 209, 209: 210, 210: 211, 211: 212, 212: 213, 213: 214, 214: 215, 215: 216, 216: 217, 217: 218, 218: 219, 219: 220, 220: 221, 221: 222, 222: 223, 223: 224, 224: 225, 225: 226, 226: 227, 227: 228, 228: 229, 229: 230, 230: 231, 231: 232, 232: 233, 233: 234, 234: 235, 235: 236, 236: 237, 237: 238, 238: 239, 239: 240, 240: 241, 241: 242, 242: 243, 243: 244, 244: 245, 245: 246, 246: 247, 247: 248, 248: 249, 249: 250, 250: 251, 251: 252, 252: 253, 253: 254, 254: 255, 255: 256, 256: 257, 257: 258, 258: 259, 259: 260, 260: 261, 261: 262, 262: 263, 263: 264, 264: 265, 265: 266, 266: 267, 267: 268, 268: 269, 269: 270, 270: 271, 271: 272, 272: 273, 273: 274, 274: 275, 275: 276, 276: 277, 277: 278, 278: 279, 279: 280, 280: 281, 281: 282, 282: 283, 283: 284, 284: 285, 285: 286, 286: 287, 287: 288, 288: 289, 289: 290, 290: 291, 291: 292, 292: 293, 293: 294, 294: 295, 295: 296, 296: 297, 297: 298, 298: 299, 299: 300, 300: 301, 301: 302, 302: 303, 303: 304, 304: 305, 305: 306, 306: 307, 307: 308, 308: 309, 309: 310, 310: 311, 311: 312, 312: 313, 313: 314, 314: 315, 315: 316, 316: 317, 317: 318, 318: 319, 319: 320, 320: 321, 321: 322, 322: 323, 323: 324, 324: 325, 325: 326, 326: 327, 327: 328, 328: 329, 329: 330, 330: 331, 331: 332, 332: 333, 333: 334, 334: 335, 335: 336, 336: 337, 337: 338, 338: 339, 339: 340, 340: 341, 341: 342, 342: 343, 343: 344, 344: 345, 345: 346, 346: 347, 347: 348, 348: 349, 349: 350, 350: 351, 351: 352, 352: 353, 353: 354, 354: 355, 355: 356, 356: 357, 357: 358, 358: 359, 359: 360, 360: 361, 361: 362, 362: 363, 363: 364, 364: 365, 365: 366, 366: 367, 367: 368, 368: 369, 369: 370, 370: 371, 371: 372, 372: 373, 373: 374, 374: 375, 375: 376, 376: 377, 377: 378, 378: 379, 379: 380, 380: 381, 381: 382, 382: 383, 383: 384, 384: 385, 385: 386, 386: 387, 387: 388, 388: 389, 389: 390, 390: 391, 391: 392, 392: 393, 393: 394, 394: 395, 395: 396, 396: 397, 397: 398, 398: 399, 399: 400, 400: 401, 401: 402, 402: 403, 403: 404, 404: 405, 405: 406, 406: 407, 407: 408, 408: 409, 409: 410, 410: 411, 411: 412, 412: 413, 413: 414, 414: 415, 415: 416, 416: 417, 417: 418, 418: 419, 419: 420, 420: 421, 421: 422, 422: 423, 423: 424, 424: 425, 425: 426, 426: 427, 427: 428, 428: 429, 429: 430, 430: 431, 431: 432, 432: 433, 433: 434, 434: 435, 435: 436, 436: 437, 437: 438, 438: 439, 439: 440, 440: 441, 441: 442, 442: 443, 443: 444, 444: 445, 445: 446, 446: 447, 447: 448, 448: 449, 449: 450, 450: 451, 451: 452, 452: 453, 453: 454, 454: 455, 455: 456, 456: 457, 457: 458, 458: 459, 459: 460, 460: 461, 461: 462, 462: 463, 463: 464, 464: 465, 465: 466, 466: 467, 467: 468, 468: 469, 469: 470, 470: 471, 471: 472, 472: 473, 473: 474, 474: 475, 475: 476, 476: 477, 477: 478, 478: 479, 479: 480, 480: 481, 481: 482, 482: 483, 483: 484, 484: 485, 485: 486, 486: 487, 487: 488, 488: 489, 489: 490, 490: 491, 491: 492, 492: 493, 493: 494, 494: 495, 495: 496, 496: 497, 497: 498, 498: 499, 499: 500}
```

```
jumlah user: 500
jumlah produk: 23541
Min rating: 1.0
Max rating: 5.0
```

- Split data<br>
Membagi data menjadi set pelatihan dan set pengujian untuk evaluasi model.
   - Mengacak dataset
    
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
    - Membuat variabel x untuk mencocokkan data user dan produk menjadi satu value
    - Membuat variabel y untuk membuat rating dari hasil
    - Membagi menjadi 80% data train dan 20% data validasi
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

## **Modeling**
**Content-Based Filtering** <br>
Menggunakan TF-IDF Vectorization dan Cosine Similarity untuk menghitung kesamaan antara produk berdasarkan atribut kategori, sub kategori, brand dan tipe produk.

- TF-IDF
  ```
  <27201x3083 sparse matrix of type '<class 'numpy.float64'>'
	with 178093 stored elements in Compressed Sparse Row format>
  ```
- cosine_similarity
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
- Hasil rekomendasi produk yang relevan
  
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

**Collaborative Filtering**<br>
Menggunakan class RecommenderNet untuk Collaborative Filtering.

- Inisialisasi model dan compile

- Training terhadap model

- Membuat variabel produk_not_rated sebagai daftar produk untuk direkomendasikan pada pengguna

- Hasil rekomendasi produk
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

## **Evaluation**<br>
Untuk mengevaluasi kedua model rekomendasi ini, kita dapat menggunakan precision dan recall untuk mengukur akurasi prediksi.
### **Content Based Filtering**
```Precision untuk produk 'Dark Melody Coffee Beans Roasted': 1.00```
### **Collaborative Filtering**
![evaluasiCF](https://github.com/user-attachments/assets/9d90f243-88d4-4687-91a7-f4763a08f96b)

### **References**































  
