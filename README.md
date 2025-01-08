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

![bottom kategori](https://github.com/user-attachments/assets/874ec2b8-67f5-45f6-8af8-563be44f1004)

## **Data Preparation**
**1. Detection and Removal Duplicates**<br>
Sebelum membangun model, kita akan memeriksa duplikasi dalam data dan menghapusnya untuk memastikan data yang digunakan tidak redundan.
```
Total Duplicates : 354
After remove Duplicates : 0
```

**2. Dropping Uneeded Column**<br>
Kita akan menghapus kolom yang tidak relevan (```index```) untuk analisis lebih lanjut.
# Product Information Table

| Product                                    | Category                | Sub-Category         | Brand             | Sale Price | Market Price | Type                    | Rating | Description                                                                 |
|--------------------------------------------|-------------------------|----------------------|-------------------|------------|--------------|-------------------------|--------|-----------------------------------------------------------------------------|
| Garlic Oil - Vegetarian Capsule 500 mg    | Beauty & Hygiene        | Hair Care            | Sri Sri Ayurveda  | 220.0      | 220.0        | Hair Oil & Serum        | 4.1    | This Product contains Garlic Oil that is known...                          |
| Water Bottle - Orange                      | Kitchen, Garden & Pets  | Storage & Accessories| Mastercook        | 180.0      | 180.0        | Water & Fridge Bottles  | 2.3    | Each product is microwave safe (without lid), ...                          |
| Brass Angle Deep - Plain, No.2             | Cleaning & Household    | Pooja Needs          | Trm               | 119.0      | 250.0        | Lamp & Lamp Oil         | 3.4    | A perfect gift for all occasions, be it your m...                          |
| Cereal Flip Lid Container/Storage Jar - Assorted | Cleaning & Household | Bins & Bathroom Ware | Nakoda            | 149.0      | 176.0        | Laundry, Storage Baskets| 3.7    | Multipurpose container with an attractive desi...                          |
| Creme Soft Soap - For Hands & Body         | Beauty & Hygiene        | Bath & Hand Wash     | Nivea             | 162.0      | 162.0        | Bathing Bars & Soaps    | 4.4    | Nivea Creme Soft Soap gives your skin the best...                          |

**3. Handling Missing Value**<br>
Kita juga perlu menangani nilai yang hilang dalam data, baik dengan mengisi nilai yang hilang atau menghapus baris yang mengandung nilai yang hilang.

- Distribusi kolom rating
![vis distribusi rating](https://github.com/user-attachments/assets/4cab67ac-2d0a-4687-8c8c-b1da3a65c2a7)

- Mean rating
```3.943409583179249```
- Median rating
```4.1```
- Nilai rating yang hilang, diisi dengan nilai mean rating
- Prosentase nilai yang hilang keseluruan
```
Total Data Null
0.05
```
- Hasil setelah nilai yang hilang dihapus

| Product                                   | Category                | Sub-Category               | Brand                        | Sale Price | Market Price | Type                    | Rating | Description                                                               |
|-------------------------------------------|-------------------------|----------------------------|------------------------------|------------|--------------|-------------------------|--------|---------------------------------------------------------------------------|
| Garlic Oil - Vegetarian Capsule 500 mg    | Beauty & Hygiene        | Hair Care                  | Sri Sri Ayurveda            | 220.00     | 220.0        | Hair Oil & Serum        | 4.1    | This Product contains Garlic Oil that is known...                         |
| Water Bottle - Orange                     | Kitchen, Garden & Pets  | Storage & Accessories      | Mastercook                  | 180.00     | 180.0        | Water & Fridge Bottles  | 2.3    | Each product is microwave safe (without lid), ...                         |
| Brass Angle Deep - Plain, No.2            | Cleaning & Household    | Pooja Needs                | Trm                         | 119.00     | 250.0        | Lamp & Lamp Oil         | 3.4    | A perfect gift for all occasions, be it your m...                         |
| Cereal Flip Lid Container/Storage Jar ... | Cleaning & Household    | Bins & Bathroom Ware       | Nakoda                      | 149.00     | 176.0        | Laundry, Storage Baskets| 3.7    | Multipurpose container with an attractive desi...                         |
| Creme Soft Soap - For Hands & Body        | Beauty & Hygiene        | Bath & Hand Wash           | Nivea                       | 162.00     | 162.0        | Bathing Bars & Soaps    | 4.4    | Nivea Creme Soft Soap gives your skin the best...                         |
| ...                                       | ...                     | ...                        | ...                         | ...        | ...          | ...                     | ...    | ...                                                                       |
| Wottagirl! Perfume Spray - Heaven, Classic| Beauty & Hygiene        | Fragrances & Deos          | Layerr                      | 199.20     | 249.0        | Perfume                 | 3.9    | Layerr brings you Wottagirl Classic fragrant b...                         |
| Rosemary                                  | Gourmet & World Food    | Cooking & Baking Needs     | Puramate                    | 67.50      | 75.0         | Herbs, Seasonings & Rubs| 4.0    | Puramate rosemary is enough to transform a dis...                         |
| Peri-Peri Sweet Potato Chips              | Gourmet & World Food    | Snacks, Dry Fruits, Nuts   | FabBox                      | 200.00     | 200.0        | Nachos & Chips          | 3.8    | We have taken the richness of Sweet Potatoes (...                         |
| Green Tea - Pure Original                 | Beverages               | Tea                        | Tetley                      | 396.00     | 495.0        | Tea Bags                | 4.2    | Tetley Green Tea with its refreshing pure, ori...                         |
| United Dreams Go Far Deodorant            | Beauty & Hygiene        | Men's Grooming             | United Colors Of Benetton   | 214.53     | 390.0        | Men's Deodorants        | 4.5    | The new mens fragrance from the United Dreams ...                         |

**Total Products**: 27,439 rows





























  
