# 📊 Analisis Efisiensi Biaya Iklan Paid Ads

## 📌 Overview Project
Project ini bertujuan untuk menganalisis efisiensi biaya iklan **Paid Ads** berdasarkan data pengguna dari berbagai channel pemasaran. Analisis difokuskan pada metrik **ROAS** (Return on Ad Spend), **CAC** (Customer Acquisition Cost), dan **biaya terbuang**, yaitu biaya iklan pada pengguna yang tidak melakukan pembelian.

Data yang digunakan mencakup periode 1 Januari sampai 1 Maret 2025 dan berisi informasi seperti channel, source, device, aktivitas pengguna (sesi, page views, add to cart, purchase), revenue, serta biaya iklan.

Melalui analisis ini diharapkan dapat ditemukan bagian dari Paid Ads yang kurang efisien, sehingga dapat mendukung keputusan pengalokasian anggaran pemasaran yang lebih tepat.

## ❗ Problem Statement
Data keseluruhan kinerja Google Ads dari Agustus 2025 hingga Juli 2026 menunjukkan kondisi platform iklan yang menantang, yang ditandai dengan peningkatan biaya dan penurunan efisiensi. Marketing Efficiency Ratio (MER) meningkat +3,73%, namun Return on Ad Spend (ROAS) dan Conversion Rate (CVR) justru menurun (masing-masing -3,54% dan -7,02%).

> Sumber: [Triple Whale – Google Ads Benchmarks by Industry](https://www.triplewhale.com/blog/google-ads-benchmarks)

Dengan menganalisis data pemasaran, kita dapat mengidentifikasi di mana biaya iklan tidak menghasilkan pembelian. Insight dari analisis ini dapat membantu:

- Tim marketing dalam mengalokasikan anggaran iklan ke source dan device yang lebih efisien
- Tim produk dan teknis dalam menelusuri hambatan pada proses pembelian
- Manajemen dalam memantau efisiensi Paid Ads secara berkala

## ❓ Business Question
Beberapa pertanyaan yang ingin dijawab dalam analisis ini antara lain:

1. Seberapa efisien Paid Ads secara keseluruhan?
2. Bagaimana tren ROAS Paid Ads dari Januari ke Februari?
3. Device mana yang memiliki ROAS paling rendah di Februari?
4. Pada device tersebut, source mana yang tidak menghasilkan pembelian?
5. Berapa biaya iklan di device tersebut yang tidak menghasilkan pembelian?

## 📊 Dataset
- **Nama file:** `data_marketing_growth_analytics.csv`
- **Sumber:** [Google Drive](https://drive.google.com/file/d/16YPDZhN2sMJHJ5zdzvWKRuyknJokaRZY)
- **Format:** CSV
- **Jumlah baris:** 800
- **Jumlah kolom:** 16

| Kolom | Deskripsi |
|---|---|
| user_id | Identifikasi unik untuk setiap pengguna |
| first_visit_date | Tanggal kunjungan pertama pengguna |
| channel | Saluran pemasaran asal pengguna (Referral, Paid Ads, Organic Search, Social Media, Email) |
| source | Sumber trafik atau platform spesifik (instagram, facebook, tiktok, direct, google, email) |
| device | Jenis perangkat yang digunakan (Mobile, Desktop, Tablet) |
| sessions | Jumlah sesi/kunjungan |
| page_views | Jumlah total halaman yang dilihat |
| bounce | Langsung meninggalkan situs atau tidak (1 = Bounce, 0 = Tidak) |
| add_to_cart | Menambahkan produk ke keranjang (1 = Ya, 0 = Tidak) |
| purchase | Melakukan pembelian (1 = Ya, 0 = Tidak) |
| revenue | Total pendapatan dari pembelian pengguna |
| ad_cost | Biaya iklan untuk mendapatkan pengguna tersebut |
| clicks | Jumlah klik pada iklan/tautan |
| impressions | Jumlah kemunculan iklan/tautan |
| repeat_purchase | Pembelian berulang (1 = Ya, 0 = Tidak) |
| retention_month | Durasi (bulan) pengguna tetap aktif |

## 🧹 Data Preparation
1. **Data Exploration:** tidak ditemukan inkonsistensi, missing value, maupun duplicate value.
2. **Tipe data:** kolom `first_visit_date` masih bertipe string, sehingga diubah menjadi `datetime`.
3. **Outlier:** ditemukan outlier pada tujuh kolom (bounce, purchase, revenue, ad_cost, impressions, repeat_purchase, retention_month). Outlier dianggap wajar karena mayoritas user tidak membeli sehingga nilai 0 sangat dominan, dan pembeli terdeteksi sebagai outlier oleh metode IQR. Karena itu tidak dilakukan penanganan outlier.
4. **Feature Engineering:** dibuat kolom `Bulan` dari `first_visit_date` (1 = Januari, 2 = Februari, 3 = Maret).

## 📈 KPI Utama

| Total Biaya Iklan | Total Revenue | ROAS | CAC | Biaya Terbuang |
|:---:|:---:|:---:|:---:|:---:|
| $10.948.907 | $41.003.905 | 3,75 | $72.993 | 77,9% |

> ROAS = revenue ÷ biaya iklan. Biaya terbuang = biaya iklan pada user tanpa pembelian. ROAS dihitung dari revenue, bukan profit. Khusus channel **Paid Ads**, ROAS tercatat sebesar **1,20** (revenue $13.177.258 dari biaya $10.948.907). Maret dikecualikan dari analisis tren karena hanya memiliki 1 hari data.

## 🔍 Insight dari Data

Secara keseluruhan, marketing menghasilkan revenue $41.003.905 dari biaya iklan $10.948.907, namun **Paid Ads** sebagai satu-satunya channel yang menanggung biaya iklan hanya mencatat **ROAS 1,20** dengan CAC sekitar $214.684, sehingga efisiensinya tergolong tipis. **ROAS Paid Ads turun 29,2% di Februari** (dari 1,40 menjadi 0,99) bukan karena biaya naik (biaya justru turun 4,9%), melainkan karena **revenue turun 32,9%** akibat pembeli berkurang dari 28 menjadi 22. Penurunan terjadi pada **Desktop (ROAS 0,92)** dan Mobile (0,98), dan pada Desktop, **source email dan tiktok memiliki ROAS 0,00** karena 7 user yang memasukkan produk ke keranjang tidak ada yang menyelesaikan pembelian. Akibatnya, **85,77% biaya Desktop di Februari ($1.476.056) tidak menghasilkan pembelian**, dengan sekitar 44% berasal dari email dan tiktok.

## 💡 Recommendation

Disarankan untuk **memeriksa proses checkout dan pembayaran di desktop** bagi user email dan tiktok, misalnya dengan mencoba alur checkout dan menelusuri catatan error pembayaran selama Februari, dengan target minimal 50% user yang memasukkan produk ke keranjang menyelesaikan pembelian dalam 1 bulan setelah perbaikan. Selain itu, dapat dipertimbangkan untuk **mengurangi anggaran email dan tiktok di Desktop sebesar 50% selama satu bulan** dengan target ROAS Desktop naik dari 0,92 menjadi minimal 1,0, dan apabila ROAS masih di bawah 1 selama dua bulan berturut-turut, penghentian anggaran dapat dipertimbangkan. Terakhir, disarankan **mengalihkan anggaran secara bertahap ke source Desktop yang masih efisien** (google, instagram, dan direct dengan ROAS Februari 2,75, 2,09, dan 1,57), maksimal 25% tambahan biaya per source setiap bulan, dengan target ROAS Paid Ads naik dari 0,99 menjadi minimal 1,20 dalam dua bulan.

## 📊 Dashboard
Dashboard interaktif dibuat di Tableau dengan filter **Channel**, **Bulan**, **Device**, dan **Source**, terdiri dari:

- 5 KPI (Total Biaya Iklan, Total Revenue, ROAS, CAC, Biaya Terbuang)
- Trend ROAS per bulan
- Biaya dan revenue setiap channel
- Nilai ROAS berdasarkan device
- Nilai ROAS berdasarkan source
- Funnel marketing (impressions hingga repeat purchase)
- Proporsi biaya terbuang (membeli vs tidak membeli)

> 📷 Tambahkan screenshot dashboard di sini:
> [`![Dashboard](images/dashboard.png)`](https://github.com/ahmad2129/Analisis-Efisiensi-Biaya-Iklan-Paid-Ads/blob/main/dashboard/Dashboard%20Mini%20Project%20Marketing%20Analysis.png)

## 📊 Presentasi Project
Untuk melihat penjelasan lengkap dari project ini, silakan lihat slide presentasi berikut:

🔗 [[Link PPT](#) <!-- ganti dengan link presentasi Anda -->](https://github.com/ahmad2129/Analisis-Efisiensi-Biaya-Iklan-Paid-Ads/blob/main/report/Presentasi_Mini_Project_Marketing_Growth_Analytics_Ahmad_Malik_Ibrahim.pdf)

## 🛠 Tools yang Digunakan
- **Python** (Data Exploration, Data Cleaning & Feature Engineering)
- **Tableau** (Visualisasi dan Dashboard)

## 👤 Author
**Ahmad Malik Ibrahim**
Lulusan kedokteran dengan minat pada statistik, analisis, dan matematika, yang sedang menargetkan karir sebagai data analis.
