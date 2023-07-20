# Project Overview

Salah satu akibat dari perkembangan teknologi yang semakin cepat adalah dihasilkannya berbagai macam data yang semakin hari semakin bertambah. Berbagai macam penelitian pun mencoba mendapatkan manfaat dan wawasan dari data tersebut.

Menghadapi berbagai macam data tersebut sistem rekomendasi pun diusulkan. Tujuan dari sistem rekomendasi adalah menghasilkan rekomendasi yang mungkin user tertarik berdasarkan preferensi user tersebut [1].

Menemukan buku yang relevan di internet merupakan tantangan besar bagi pengguna internet. Sistem rekomendasi telah hadir untuk melakukan pencarian yang efektif berdasarkan preferensi pengguna. Sebagian besar sistem rekomendasi yang ada menggunakan *content based filtering* dan *collaborative filtering* berdasarkan rating dari user [2].

# Business Understanding

Proyek ini bertujuan untuk mengembangkan sistem rekomendasi film dengan memanfaatkan informasi yang terdapat dalam dataset, seperti genre, sutradara, pemeran, dan diskripsi film, untuk memberikan rekomendasi judul film yang disukai penonton. Aplikasi ini dapat diaplikasikan pada penyedia layanan putar film online, ketika seseorang ingin menonton film yang ia suka, maka sistem aplikasi akan merekomendasikan beberapa film lain dengan genre yang sama. Jadi penonton akan mendapatkan lebih banyak referensi film yang ia sukai. Ketika pengguna merasa puas dengan rekomendasi yang diberikan oleh aplikasi, mereka lebih cenderung untuk tetap menggunakan layanan ini secara berulang dan tidak beralih ke platform kompetitor, dengan kata lain dapat meningkatkan retensi pengguna.

## Problem Statements
* Bagaimana cara mendapatkan rekomendasi film berdasarkan deskripsi film?

## Goals
* Mengetahui bagaimana mendapatkan model yang bisa digunakan untuk merekomendasikan film berdasarkan genre film sebagai inputan.

# Data Understanding

Data yang digunakan pada proyek kali ini bersumber dari situs *kaggle* dapat diunduh pada [tautan ini](https://www.kaggle.com/datasets/shivamb/netflix-shows). Terdapat 8807 data unik dengan 12 kolom.

Fitur-fitur dataset film adalah sebagai berikut:

* ***show_id***: ID unik untuk setiap *Movie / Tv Show*.
* ***type***: Klasifikasi jenis film apakah *Movie* atau *TV Show*.
* ***title***: Judul dari *Movie / TV Show*.
* ***director***: Sutradara film.
* ***cast***: Aktor-aktor yang terlibat dalam film.
* ***country***: Negara dimana *movie* atau *TV Show* diproduksi.
* ***date_added***: Tanggal penambahan film di Netflix.
* ***release_year***: Tahun rilis dari *Movie* atau *TV Show*.
* ***rating***: Rating TV untuk *Movie* atau *TV Show*.
* ***duration***: Total durasi - dalam menit atau *season*.
* ***listed_in***: Genre film.
* ***description***: Ringkasan diskripsi film.

Untuk mempermudah analisa, yang pertama dilakukan adalah mengganti fitur *listed_in* menjadi *Genre* karena fitur ini memang berisi genre dari judul film. tabel dibawah ini adalah contoh ringkasan film pada dataset.

Tabel 1. Ringkasan genre pada *dataset*.
|    | title                            | Genre                                                         |
|----|----------------------------------|---------------------------------------------------------------|
|  1 | Dick Johnson Is Dead             | Documentaries                                                 |
|  2 | Blood & Water                    | International TV Shows, TV Dramas, TV Mysteries               |
|  3 | Ganglands                        | Crime TV Shows, International TV Shows, TV Action & Adventure |
|  4 | Jailbirds New Orleans            | Docuseries, Reality TV                                        |
|  5 | Kota Factory                     | International TV Shows, Romantic TV Shows, TV Comedies        |
|  6 | Midnight Mass                    | TV Dramas, TV Horror, TV Mysteries                            |
|  7 | My Little Pony: A New Generation | Children & Family Movies                                      |
|  8 | Sankofa                          | Dramas, Independent Movies, International Movies              |
|  9 | The Great British Baking Show    | British TV Shows, Reality TV                                  |
| 10 | The Starling                     | Comedies, Dramas                                              |

Adapun genre pada dataset ini memiliki 33 macam genre. Genre tersebut adalah

1. ***Crime TV Shows***: Serial TV yang berfokus pada kejahatan, investigasi, dan kehidupan dalam dunia kriminal.
2. ***TV Dramas***: Serial TV dengan fokus pada cerita naratif yang kompleks dan penuh emosi.
3. ***Children & Family Movies***: Film yang ditujukan untuk penonton anak-anak dan keluarga, biasanya mengandung pesan moral atau pembelajaran.
4. ***Dramas***: Film dengan fokus pada konflik emosional dan pertentangan antara karakter.
5. ***British TV Shows***: Serial TV produksi Inggris dengan berbagai genre dan tema.
6. ***Comedies***: Film atau serial TV yang bertujuan untuk menghibur penonton dengan unsur humor dan kelucuan.
7. ***Thrillers***: Film yang mengejar ketegangan dan kegembiraan dengan plot yang menegangkan dan misterius.
8. ***Horror Movies***: Film dengan tujuan untuk menimbulkan ketakutan dan rasa ngeri pada penonton.
9. ***Action & Adventure***: Film dengan aksi dan petualangan yang menghibur.
10. ***International TV Shows***: Serial TV dari berbagai negara di luar Amerika Serikat.
11. ***Documentaries***: Film atau serial TV yang bertujuan untuk menggambarkan fakta dan realitas tentang kejadian, orang, atau topik tertentu.
12. ***International Movies***: Film dari berbagai negara di luar Amerika Serikat.
13. ***Sci-Fi & Fantasy***: Film dengan elemen fiksi ilmiah dan fantasi, seringkali berlatar di dunia yang tidak nyata atau masa depan.
14. ***Classic Movies***: Film-film klasik dari era lampau yang dianggap sebagai karya berpengaruh dalam sejarah perfilman.
15. ***TV Shows***: Program televisi dengan berbagai genre dan tema.
16. ***Stand-Up Comedy***: Pertunjukan tunggal komedi di mana seorang komedian tampil di depan penonton langsung.
17. ***Movies***: Film dengan berbagai genre dan tema.
18. ***Stand-Up Comedy & Talk Shows***: Program komedi tunggal dengan tambahan elemen wawancara atau obrolan dengan tamu.
19. ***Anime Features***: Film animasi dari Jepang.
20. ***Anime Series***: Serial animasi dari Jepang.
21. ***Reality TV***: Program televisi yang menampilkan kehidupan nyata orang atau situasi dengan elemen drama atau hiburan.
22. ***Romantic TV Shows***: Serial TV dengan fokus pada cerita cinta dan hubungan romantis.
23. ***Cult Movies***: Film yang telah mendapatkan basis penggemar yang setia dan kultus.
24. ***Independent Movies***: Film produksi independen yang biasanya memiliki kreativitas artistik dan visi yang lebih bebas.
25. ***Docuseries***: Serial dokumenter yang berfokus pada topik atau kehidupan nyata.
26. ***Classic & Cult TV***: Serial TV klasik dan kultus yang dianggap sebagai karya berpengaruh dalam sejarah televisi.
27. ***Kids' TV***: Program televisi untuk anak-anak dengan berbagai tema dan hiburan.
28. ***Music & Musicals***: Film dengan fokus pada musik dan tari.
29. ***Romantic Movies***: Film dengan fokus pada kisah cinta dan hubungan romantis.
30. ***LGBTQ Movies***: Film dengan fokus pada karakter dan cerita yang berhubungan dengan komunitas LGBTQ+.
31. ***TV Action & Adventure***: Serial TV dengan fokus pada aksi dan petualangan.
32. ***TV Comedies***: Serial TV dengan fokus pada elemen komedi dan humor.
33. ***TV Horror***: Serial TV dengan fokus pada genre horor dan ketegangan.

Dalam proyek ini fitur *descriptopion* yang akan digunakan sebagai variabel untuk mendapatkan rekomendasi *Genre*, akan tetapi jika dirasa rekomendasi yang diberikan sistem kurang akurat, maka ada beberapa fitur yang bisa dijadikan variabel tambahan untuk memperbaiki akurasi dari rekomendasi. Fitur yang pertama adalah fitur *director*. Sutradara film memiliki keterkaitan langsung terhadap judul film yang diproduksi, ditangan sutradaralah alur cerita disajikan ke dalam suatu film. Gambar dibawah ini menunjukkan 10 sutradara yang paling banyak menyutradarai film di *Netflix*. 

![Top 10 sutradara terbanyak](https://github.com/rozinurhuda/Netflix-recommendations/assets/11625567/1142bd48-f18e-4d05-bcfb-a15cf186b434)

Gambar 1. Diagram Top 10 Sutradara dengan Film Terbanyak

Dari Tabel sutradara Raul Campos, Jan Suter merupakan sutradara yang paling banyak menyutradarai film, yaitu 18 film.

Fitur Kedua yaitu *cast* atau pemeran dalam film. Fitur *cast* atau pemeran film adalah elemen penting dalam pembuatan sistem rekomendasi karena dapat memberikan dampak yang signifikan pada preferensi penonton. Dengan menyertakan informasi tentang pemeran film dalam sistem rekomendasi, aplikasi dapat lebih memahami selera dan minat pengguna terkait dengan aktor atau aktris tertentu. Pengguna sering kali memiliki preferensi pribadi terhadap pemeran favorit mereka, dan ini dapat mempengaruhi keputusan mereka untuk menonton film atau acara tertentu. Gambar dibawah ini akan menujukkan aktor dan aktris yang banyak memerankan film di Netflix.

![aktor aktris netfilx](https://github.com/rozinurhuda/Netflix-recommendations/assets/11625567/26e5844e-37dc-4a5f-b14e-7f4fbfabe45e)

Gambar 2. Diagram Top 10 Aktor atau Aktris Pemeran Film di Netflix

Gambar diatas menunjukkan aktor dan aktris Vatsal Dubey, Julie Tejwani, Rupa Bhimani, Jigna Bhardwaj, Rajesh Kava, Mousam, Swapnil merupakan aktor/aktris terbanyak dalam memerankan film di Netflix dengan memerankan 13 buah film, diikuti oleh Samuel West diperingkat kedua dengan jumlah 10 film yang telah dimainkan.

Fitur ketiga adalah *Genre*. Dengan menerapkan fitur *genre* dalam sistem rekomendasi, aplikasi dapat menciptakan pengalaman yang lebih personal, meningkatkan engagement, dan membantu pengguna menemukan konten hiburan yang paling sesuai dengan minat mereka. 

![Top 10 genre terpopuler](https://github.com/rozinurhuda/Netflix-recommendations/assets/11625567/8926b7bc-fe09-4c27-aad4-ec2ef5ce9b51)

Gambar 3. Diagram Top 10 Genre terbanyak di Netflix

Dari gambar diatas menujukkan bahwa genre *Dramas* merupakan genre terbanyak dalam Netflix dengan 1585 film, disusul oleh *comedies* dengan 1184 film dan ketiga adalah genre *action & adventure* dengan 848 film.

# Data Preparation

Sebelum data diproses oleh model, penting untuk mengetahui apakah ada data kosong pada dataset, agar hasil keluaran yang dihasilkan benar representasi data seluruh data pada dataset, olehnya itu langkah selanjutnya adalah memeriksa data yang memiliki nilai *null*.

Tabel 2. Jumlah data *null* pada setiap fitur.

| Fitur        | Banyak data null |
|--------------|------------------|
| show_id      | 0                |
| type         | 0                |
| title        | 0                |
| director     | 2634             |
| cast         | 825              |
| country      | 831              |
| date_added   | 10               |
| release_year | 0                |
| rating       | 4                |
| duration     | 3                |
| listed_in    | 0                |
| description  | 0                |

Tabel diatas menunjukkan bahwa fitur *director*, *cast*, dan *country* memiliki nilai *null* yang sangat banyak untuk direkayasa, untuk itu fitur-fitur tersebut akan di-*drop* atau dihilangkan dari dataset. Untuk jumlah nilai *null* yang kecil pada fitur *country*, *date_added*, *rating*, dan *duration* kita dapat mengisinya menggunakan mode(nilai paling umum) dan rata-rata.

Langkah selanjutnya adalah mengganti nama fitur *listed_in* dengan fitur *Genre* untuk memudahkan pembacaan. Terlihat dari dataset diatas fitur *Genre* memiliki beberapa genre yang tertulis, untuk memudahkan pembuatan model, maka dipilih salah satu dari genre tersebut.

# Modeling and Result

Fitur *description* adalah data teks, data teks memerlukan persiapan khusus sebelum digunakan untuk pemodelan. Teks harus diurai terlebih dahulu dengan teknik tokenisasi. Kemudian, ia perlu dikodekan menjadi bilangan numerik agar dapat digunakan sebagai masukan pada algoritma *machine learning*. Proses ini disebut ekstraksi atau rekayasa fitur(*feature engineering*).

Terdapat beberapa metode rekayasa fitur untuk representasi teks. Metode ini diklasifikasikan ke dalam empat kategori, antara lain:

* *Basic vectorization approaches* (contoh: *One-Hot Encoding, Bag of Words, Bag of N-Grams*, dan *TF-IDF*).
* *Distributed representations* (contoh: *Words Embeddings*).
* *Universal text representation* (menggunakan *pre-trained embedding*).
* *Handcrafted features* (mengandalkan *domain-specific knowledge*).

Metode rekayasa fitur untuk representasi teks yang digunakan pada aplikasi ini adalah TF-IDF.

## TF-IDF

Teknik penilaian TF-IDF merupakan kepanjangan dari *Term Frequency-Inverse Document Frequency*. TF-IDF adalah ukuran statistik yang menggambarkan pentingnya suatu istilah terhadap sebuah dokumen dalam sebuah kumpulan atau korpus [3]. Sedangkan IDF (*Inverse Document Frequency*) mengukur pentingnya istilah di seluruh korpus. Dalam komputasi TF, semua istilah diberikan bobot kepentingan (*weight*) yang sama. Meskipun sering muncul, *stop word* seperti *is, are, am*, dsb, tidaklah penting. Untuk mengatasi kasus seperti ini, IDF mempertimbangkan term yang sangat umum di seluruh dokumen dan menimbang istilah-istilah yang jarang.

Langkah awal proses TF akan bekerja pada dataset dengan memecahkan kata pada kolom *description*. Setelah semua kata didapatkan, TF akan menghitung berapa kali kata tersebut muncul dalam kolom *description*. Setelah itu TF akan membagi setiap jumlah kemunculan suatu kata kemudian membagi dengan banyak kata untuk mendapatkan frekuensi pada setiap kata.

Langkah selanjutnya adalah melakukan proses IDF, proses IDF merupakan proses yang penting karena ingin didapatkan kata-kata yang jarang muncul, kemudian memberikannya dengan nilai yang tinggi, jika tidak dilakukan proses IDF, maka kata-kata yang sering muncul seperti imbuhan the akan memiliki nilai yang tinggi pada kalimat.

Setelah dilakukan proses TF-IDF. Untuk mengetahui nilai setiap kata pada *description*, selanjutnya dipasangkan dengan kolom *title*.

Untuk mendapatkan rekomendasi judul film, akan digunakan metode Content Based Filtering meskipun metode ini memiliki kekurangan yaitu sangat bergantung dengan preferensi pengguna sebelumnya.

## Content Based Filtering

Ide dari sistem rekomendasi berbasis konten (*content-based filtering*) adalah merekomendasikan item yang mirip dengan item yang disukai pengguna di masa lalu. *Content-based filtering* mempelajari profil minat pengguna berdasarkan data dari objek yang telah dinilai pengguna. Algoritma ini bekerja dengan menyarankan item serupa yang pernah disukai di masa lalu atau sedang dilihat di masa kini kepada pengguna. Semakin banyak informasi yang diberikan pengguna, semakin baik akurasi sistem rekomendasi.

Metode ini akan digunakan pada rekomendasi buku ini dengan asumsi bahwa pengguna pernah menyukai atau melihat suatu buku. Untuk merekomendasikan buku lain maka diambil genre buku yang dipilih oleh *user* kemudian mencocokkannya dengan buku lain yang genrenya mirip dengannya. Untuk mengetahui derajat kesamaan antar buku, maka akan digunakan metode *Cosine Similarity*.

## Cosine Similarity

Metode *cosine similarity* merupakan metode untuk menghitung kesamaan antara dua buah objek yang dinyatakan dalam dua buah *vector* dengan menggunakan *keywords* (kata kunci) dari sebuah dokumen sebagai ukuran. Semakin mirip kedua objek tersebut, maka nilai akan mendekati 1, jika berbeda akan mendekat 0. Lankah selanjutnya adalah mencari nilai *cosine similarity* pada nilai *TF-IDF* yang didapatkan.

Langkah selanjutnya adalah memasangkan judul dengan judul yang lain agar didapatkan judul yang mirip berdasarkan nilai *cosine similarity* nya. Untuk pengujian ini kita memasukan inputan dengan Judul film "*Midnight Mass*". Hasil dari rekomendasinya akan ditunjukkan dalam tabel dibawah ini.

Tabel 3. Hasil rekomendasi film yang mirip dengan "*Midnight Mass*"

|  ID  | title                 | Genre                    |
|------|-----------------------|--------------------------|
| 1899 | Turkish Dance School  | Comedies                 |
| 2502 | Escaping Tel Aviv     | Action & Adventure       |
| 4158 | Nathicharami          | Dramas                   |
| 4720 | Chillar Party         | Children & Family Movies |
| 4864 | Spivak                | Comedies                 |
| 6688 | Emma' (Mother)        | Dramas                   |
| 7380 | Mahabharat            | Action & Adventure       |
| 7533 | My Daddy is in Heaven | Dramas                   |
| 7827 | Ram Teri Ganga Maili  | Classic Movies           |
| 7841 | Recall                | Dramas                   |
```

Dari tabel diatas terdapat 5 jenis genre yaitu *Comedies*, *Action & Adventure*, *Dramas*, *Children & Family Movies* dan *Classic Movies*. Kita tidak tahu apa sebenarnya genre dari film *Midnight Mass* itu. Untuk mengetahuinya dapat dilihat dari tabel dibawah ini.

Tabel 4. Genre film *Midnight Mass*

| ID | title         | Genre     |
|----|---------------|-----------|
|  5 | Midnight Mass | TV Dramas |

Ternyata film *Midnight Mass* adalah ber-genre *dramas*. Hanya ada 40% rekomendasi judul film yang sama dengan sampel film yang diinputkan. Ini merupakan hasil yang kurang baik untuk sebuah rekomendasi. Untuk itu perlu ditambahkan matriks baru dalam proses *cosine similarity* nya. Kolom-kolom yang ditambahkan sebagai matriks adalah *Genre*, *Cast* dan *Director* untuk meningkatkan hasil rekomendasi. Untuk pengujian kedua ini kita memasukan judul film yang berbeda yaitu *Django Unchained*. Hasil rekomendasinya akan ditunjukkan dalam tabel dibawah ini. 

Tabel 5. Hasil rekomendasi film yang mirip dengan *Django Unchained*

|  ID  | title                               | Genre              |
|------|-------------------------------------|--------------------|
|  332 | Deep Blue Sea                       | Action & Adventure |
| 3893 | The Hateful Eight: Extended Version | TV Shows           |
| 5203 | The Hateful Eight                   | Action & Adventure |
| 6244 | Barely Lethal                       | Action & Adventure |
| 7077 | Inglourious Basterds                | Action & Adventure |
| 7714 | Patriot Games                       | Action & Adventure |
| 7912 | S.W.A.T.                            | Action & Adventure |
| 8445 | The Other Guys                      | Action & Adventure |
| 8766 | XXx                                 | Action & Adventure |
| 8767 | XXX: State of the Union             | Action & Adventure |

Tabel diatas menunjukkan bahwa hampir semua rekomendasi menujukkan genre *Action & Adventure*. Namun kita perlu mengetahui apakah film tersebut juga memiliki genre yang sama dengan melihat tabel di bawah ini.

Tabel 6. Genre film *Django Unchained*

| ID  | title            | Genre              |
|-----|------------------|--------------------|
| 392 | Django Unchained | Action & Adventure |

# Evaluation

Sistem rekomendasi dianggap sebagai jenis sistem pengambilan informasi tertentu oleh peneliti. Presisi telah digunakan oleh banyak peneliti untuk evaluasi sistem rekomendasi. Presisi diartikan sebagai rasio rekomendasi yang relevan dengan jumlah total item yang direkomendasikan.

$$ presisi = {Rekomendasi judul yang relevan \over Banyak Buku yang direkomendasikan} $$

Judul film yang diuji adalah *Django Unchained* dengan genre *Action & Adventure*. Ketika genre film pada hasil rekomendasi relevan dengan genre inputannya, maka akan diberi nilai 1, namun jika genre hasil rekomendasi tidak relevan dengan genre inputannya, maka akan diberi nilai 0, maka berdasarkan tabel 5 diatas, dapat dihitung nilai presisi sebagai berikut.

$$ presisi = {1+0+1+1+1+1+1+1+1+1 \over 10} \times 100 \text {% = 90%}  $$

# Kesimpulan

Model yang dihasilkan dengan hanya berdasar pada diskripsi film saja kurang mampu memenuhi hasil rekomendasi, hanya 40% keakuratnya. Namun dengan memambah matriks kolom *Genre*, *Cast* dan *Director* nilai presisi meningkat menjadi 90%.

Untuk mendapatkan hasil yang maksimal dapat menggunakan metode lain seperti *Collaborative Filtering*.

# Daftar Pustaka

[1] Chun-Mei Li, Yi-han Mai, Pi Wey, Qi yan, Jiang jie teng, Dong shuo,"*Personalized Recommendation Algorithm for books and its implementation*", Journal of Physics: Conference Series : IOP Publishing, 2020.[Online serial].Available: https://iopscience.iop.org/article/10.1088/1742-6596/1738/1/012053/pdf. [Accessed July. 17, 2023].

[2] Sarma Dhiman,Mittra Tanni ,Hossain Mohammad Shahadat, "*Personalized Book Recommendation System using Machine Learning Algorithm*", _(IJACSA) *International Journal of Advanced Computer Science and Applications*, Vol. 12,No.1_2021.[Online serial].Available: https://pdfs.semanticscholar.org/d53f/32726dd28cd19a7b583712d33198561b7e09.pdf. [Accessed July. 17, 2023].

[3] Rajaraman, A.; Ullman, J. D. (2011). "Data Mining" (PDF). Mining of Massive Datasets. hlm. 1–17. doi:10.1017/CBO9781139058452.002. ISBN 978-1-1390-5845-2.
