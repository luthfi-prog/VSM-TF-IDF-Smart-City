# IMPLEMENTASI VECTOR SPACE MODEL DENGAN PEMBOBOTAN TF-IDF PADA DOKUMEN SMART CITY

## PENDAHULUAN

### Latar Belakang

Vector Space Model (VSM) dengan pembobotan TF-IDF adalah metode yang efektif dalam sistem temu balik dokumen. Berbeda dengan binary weight yang hanya melihat keberadaan term, TF-IDF mempertimbangkan frekuensi kemunculan term (TF) dan tingkat kepentingan term dalam koleksi dokumen (IDF).

Penelitian ini mengimplementasikan VSM dengan TF-IDF pada 8 dokumen abstrak tentang aplikasi smart city dengan 2 query berbeda untuk menemukan dokumen yang paling relevan.

### Tujuan

Penelitian ini bertujuan untuk mengimplementasikan metode VSM dengan pembobotan TF-IDF, menghitung similarity untuk 2 query yang berbeda, membandingkan hasil ranking dari kedua query, dan menganalisis keefektifan metode TF-IDF.

## METODE TF-IDF

### Term Frequency (TF)

TF mengukur seberapa sering suatu term muncul dalam dokumen.

**Rumus:**
```
TF(t,d) = Jumlah kemunculan term t dalam dokumen d
```

### Inverse Document Frequency (IDF)

IDF mengukur seberapa penting suatu term dalam seluruh koleksi dokumen.

**Rumus:**
```
IDF(t) = log₁₀(N / df)
```

**Dimana:**
- N = Total jumlah dokumen (8)
- df = Document Frequency (jumlah dokumen yang mengandung term t)

### TF-IDF

TF-IDF adalah perkalian antara TF dan IDF.

**Rumus:**
```
TF-IDF(t,d) = TF(t,d) × IDF(t)
```

### Cosine Similarity

**Rumus:**
```
Similarity(Q,D) = (Q · D) / (||Q|| × ||D||)
```

**Dimana:**
- Q · D = Dot Product (Σ TF-IDF_Q × TF-IDF_D)
- ||Q|| = √(Σ TF-IDF_Q²)
- ||D|| = √(Σ TF-IDF_D²)

## DATA PENELITIAN

### Dokumen (8 Dokumen Smart City)

**D1 – IoT in Smart Transportation**

"iot sensors collect traffic data for intelligent transportation management"

**D2 – Smart Energy Optimization**

"smart grid applies data analytics to improve energy distribution efficiency"

**D3 – Public Safety Monitoring**

"computer vision detects incidents in public areas using camera analytics"

**D4 – Urban Air Quality Monitoring**

"air quality sensors track pollution level and support sustainable city planning"

**D5 – Smart Waste Management**

"waste collection system uses sensor data to optimize route planning"

**D6 – Smart Water Management**

"water usage prediction model improves distribution and prevents leakage"

**D7 – Citizen Engagement Platform**

"mobile application enhances citizen engagement using information sharing features"

**D8 – Intelligent Building System**

"building automation uses iot devices to control energy usage efficiently"

### Query (2 Query)

**Q1:** "smart energy analytics"
- Setelah preprocessing: smart, energy, analytics

**Q2:** "sensor data model"
- Setelah preprocessing: sensor, data, model

## IMPLEMENTASI

### Preprocessing & Ekstraksi Term

Setelah preprocessing dari semua dokumen dan query, diperoleh term unik yang relevan. Berikut adalah daftar term yang akan digunakan dalam perhitungan:

**Term unik:** iot, sensors, data, smart, energy, analytics, model, sensor, collection, usage, vision, system, devices

### Tabel Term Frequency (TF)

Menghitung berapa kali setiap term muncul di setiap dokumen:

| Dok | iot | sensors | data | smart | energy | analytics | model | sensor | system | usage | devices |
|-----|-----|---------|------|-------|--------|-----------|-------|--------|--------|-------|---------|
| D1  | 1   | 1       | 1    | 0     | 0      | 0         | 0     | 0      | 0      | 0     | 0       |
| D2  | 0   | 0       | 1    | 1     | 1      | 1         | 0     | 0      | 0      | 0     | 0       |
| D3  | 0   | 0       | 0    | 0     | 0      | 1         | 0     | 0      | 0      | 0     | 0       |
| D4  | 0   | 1       | 0    | 0     | 0      | 0         | 0     | 0      | 0      | 0     | 0       |
| D5  | 0   | 0       | 1    | 0     | 0      | 0         | 0     | 1      | 1      | 0     | 0       |
| D6  | 0   | 0       | 0    | 0     | 0      | 0         | 1     | 0      | 0      | 1     | 0       |
| D7  | 0   | 0       | 0    | 0     | 0      | 0         | 0     | 0      | 0      | 0     | 0       |
| D8  | 1   | 0       | 0    | 0     | 1      | 0         | 0     | 0      | 0      | 1     | 1       |
| Q1  | 0   | 0       | 0    | 1     | 1      | 1         | 0     | 0      | 0      | 0     | 0       |
| Q2  | 0   | 0       | 1    | 0     | 0      | 0         | 1     | 1      | 0      | 0     | 0       |

### Menghitung IDF

**Rumus:** IDF(t) = log₁₀(N / df), dimana N = 8 (total dokumen)

| Term      | df (muncul di berapa dok) | IDF = log₁₀(8/df) |
|-----------|---------------------------|-------------------|
| iot       | 2                         | 0.602             |
| sensors   | 2                         | 0.602             |
| data      | 3                         | 0.426             |
| smart     | 1                         | 0.903             |
| energy    | 2                         | 0.602             |
| analytics | 2                         | 0.602             |
| model     | 1                         | 0.903             |
| sensor    | 1                         | 0.903             |
| system    | 1                         | 0.903             |
| usage     | 2                         | 0.602             |
| devices   | 1                         | 0.903             |

**Interpretasi IDF:**
- IDF tinggi (contoh: smart = 0.903, model = 0.903, sensor = 0.903) → term jarang muncul, lebih diskriminatif
- IDF sedang (contoh: iot = 0.602, energy = 0.602) → term cukup diskriminatif
- IDF rendah (contoh: data = 0.426) → term lebih umum, kurang diskriminatif

### Menghitung TF-IDF

**Rumus:** TF-IDF = TF × IDF

**Tabel TF-IDF untuk semua dokumen:**

| Dok | iot   | sensors | data  | smart | energy | analytics | model | sensor | system | usage | devices |
|-----|-------|---------|-------|-------|--------|-----------|-------|--------|--------|-------|---------|
| D1  | 0.602 | 0.602   | 0.426 | 0.000 | 0.000  | 0.000     | 0.000 | 0.000  | 0.000  | 0.000 | 0.000   |
| D2  | 0.000 | 0.000   | 0.426 | 0.903 | 0.602  | 0.602     | 0.000 | 0.000  | 0.000  | 0.000 | 0.000   |
| D3  | 0.000 | 0.000   | 0.000 | 0.000 | 0.000  | 0.602     | 0.000 | 0.000  | 0.000  | 0.000 | 0.000   |
| D4  | 0.000 | 0.602   | 0.000 | 0.000 | 0.000  | 0.000     | 0.000 | 0.000  | 0.000  | 0.000 | 0.000   |
| D5  | 0.000 | 0.000   | 0.426 | 0.000 | 0.000  | 0.000     | 0.000 | 0.903  | 0.903  | 0.000 | 0.000   |
| D6  | 0.000 | 0.000   | 0.000 | 0.000 | 0.000  | 0.000     | 0.903 | 0.000  | 0.000  | 0.602 | 0.000   |
| D7  | 0.000 | 0.000   | 0.000 | 0.000 | 0.000  | 0.000     | 0.000 | 0.000  | 0.000  | 0.000 | 0.000   |
| D8  | 0.602 | 0.000   | 0.000 | 0.000 | 0.602  | 0.000     | 0.000 | 0.000  | 0.000  | 0.602 | 0.903   |

**TF-IDF untuk Query:**

| Query | iot   | sensors | data  | smart | energy | analytics | model | sensor | system | usage | devices |
|-------|-------|---------|-------|-------|--------|-----------|-------|--------|--------|-------|---------|
| Q1    | 0.000 | 0.000   | 0.000 | 0.903 | 0.602  | 0.602     | 0.000 | 0.000  | 0.000  | 0.000 | 0.000   |
| Q2    | 0.000 | 0.000   | 0.426 | 0.000 | 0.000  | 0.000     | 0.903 | 0.903  | 0.000  | 0.000 | 0.000   |

### Contoh Perhitungan Similarity (Q1 dengan D2)

**Langkah 1: Dot Product (Q1 · D2)**

```
Q1 = [0.000, 0.000, 0.000, 0.903, 0.602, 0.602, 0.000, 0.000, 0.000, 0.000, 0.000]
D2 = [0.000, 0.000, 0.426, 0.903, 0.602, 0.602, 0.000, 0.000, 0.000, 0.000, 0.000]

Q1 · D2 = (0.000×0.000) + (0.000×0.000) + (0.000×0.426) + (0.903×0.903) + (0.602×0.602) + (0.602×0.602) + ...
        = 0 + 0 + 0 + 0.815 + 0.362 + 0.362 + 0 + 0 + 0 + 0 + 0
        = 1.539
```

**Langkah 2: Magnitude Q1**

```
||Q1|| = √(0.000² + 0.000² + 0.000² + 0.903² + 0.602² + 0.602² + 0.000² + 0.000² + 0.000² + 0.000² + 0.000²)
       = √(0 + 0 + 0 + 0.815 + 0.362 + 0.362 + 0 + 0 + 0 + 0 + 0)
       = √1.539
       = 1.241
```

**Langkah 3: Magnitude D2**

```
||D2|| = √(0.000² + 0.000² + 0.426² + 0.903² + 0.602² + 0.602² + 0.000² + 0.000² + 0.000² + 0.000² + 0.000²)
       = √(0 + 0 + 0.182 + 0.815 + 0.362 + 0.362 + 0 + 0 + 0 + 0 + 0)
       = √1.721
       = 1.312
```

**Langkah 4: Cosine Similarity**

```
Similarity(Q1, D2) = 1.539 / (1.241 × 1.312)
                   = 1.539 / 1.628
                   = 0.945
```

### Hasil Perhitungan Similarity

#### Query 1: "smart energy analytics"

| Dokumen | Dot Product | \|\|Q1\|\| | \|\|D\|\| | Similarity | Rank |
|---------|-------------|---------|--------|------------|------|
| D1      | 0.000       | 1.241   | 0.904  | 0.000      | 7    |
| D2      | 1.539       | 1.241   | 1.312  | 0.945      | 1    |
| D3      | 0.362       | 1.241   | 0.602  | 0.485      | 3    |
| D4      | 0.000       | 1.241   | 0.602  | 0.000      | 7    |
| D5      | 0.000       | 1.241   | 1.277  | 0.000      | 7    |
| D6      | 0.000       | 1.241   | 1.085  | 0.000      | 7    |
| D7      | 0.000       | 1.241   | 0.000  | 0.000      | 7    |
| D8      | 0.362       | 1.241   | 1.277  | 0.228      | 4    |

**Ranking Q1:**
1. D2 – Smart Energy Optimization (Similarity 0.945)
2. D3 – Public Safety Monitoring (Similarity 0.485)
3. D8 – Intelligent Building System (Similarity 0.228)

#### Query 2: "sensor data model"

| Dokumen | Dot Product | \|\|Q2\|\| | \|\|D\|\| | Similarity | Rank |
|---------|-------------|---------|--------|------------|------|
| D1      | 0.182       | 1.306   | 0.904  | 0.154      | 4    |
| D2      | 0.182       | 1.306   | 1.312  | 0.106      | 5    |
| D3      | 0.000       | 1.306   | 0.602  | 0.000      | 7    |
| D4      | 0.000       | 1.306   | 0.602  | 0.000      | 7    |
| D5      | 1.201       | 1.306   | 1.277  | 0.720      | 1    |
| D6      | 0.815       | 1.306   | 1.085  | 0.575      | 2    |
| D7      | 0.000       | 1.306   | 0.000  | 0.000      | 7    |
| D8      | 0.000       | 1.306   | 1.277  | 0.000      | 7    |

**Ranking Q2:**
1. D5 – Smart Waste Management (Similarity 0.720)
2. D6 – Smart Water Management (Similarity 0.575)
3. D1 – IoT in Smart Transportation (Similarity 0.154)

## HASIL DAN PEMBAHASAN

### Analisis Query 1: "smart energy analytics"

D2 (Smart Energy Optimization) mendapat Rank 1 dengan similarity 0.945 karena dokumen ini mengandung ketiga term dari query (smart, energy, analytics) dengan sempurna. Term "smart" memiliki IDF tinggi (0.903) yang membuat dokumen ini sangat relevan. TF-IDF untuk ketiga term di D2 sangat tinggi, sehingga menghasilkan dot product yang besar.

D3 (Public Safety Monitoring) mendapat Rank 2 dengan similarity 0.485 karena hanya mengandung term "analytics" dari query. Meskipun hanya satu term yang cocok, term ini memiliki IDF yang cukup tinggi (0.602).

### Analisis Query 2: "sensor data model"

D5 (Smart Waste Management) mendapat Rank 1 dengan similarity 0.720 karena mengandung dua term penting: "sensor" dan "data". Term "sensor" memiliki IDF sangat tinggi (0.903) karena jarang muncul, sehingga memberikan bobot yang signifikan.

D6 (Smart Water Management) mendapat Rank 2 dengan similarity 0.575 karena mengandung term "model" yang memiliki IDF tinggi (0.903). Meskipun hanya satu term yang cocok, nilai IDF yang tinggi membuat dokumen ini cukup relevan.

### Perbandingan Kedua Query

| Aspek                | Query 1 (smart energy analytics)      | Query 2 (sensor data model)           |
|----------------------|---------------------------------------|---------------------------------------|
| Rank 1               | D2 (Smart Energy Optimization)        | D5 (Smart Waste Management)           |
| Faktor Penentu       | Ketiga term cocok sempurna            | Term "sensor" dan "data" diskriminatif|
| Similarity Tertinggi | 0.945                                 | 0.720                                 |
| Term Paling Berpengaruh | "smart" (IDF 0.903)                | "sensor" (IDF 0.903)                  |

Query yang berbeda menghasilkan ranking yang berbeda. TF-IDF berhasil membedakan dokumen berdasarkan term yang penting dan diskriminatif. Term dengan IDF tinggi (seperti "smart", "sensor", "model") lebih berpengaruh dalam menentukan relevansi dokumen.

### Keunggulan TF-IDF

Metode TF-IDF menunjukkan keunggulan dalam mempertimbangkan frekuensi kemunculan term (TF), memberikan bobot lebih tinggi pada term yang jarang dan diskriminatif (IDF tinggi), menghasilkan ranking yang akurat sesuai konteks query, dan membedakan dokumen yang benar-benar relevan dari yang kurang relevan.

## KESIMPULAN

Metode VSM dengan TF-IDF berhasil diimplementasikan untuk 8 dokumen smart city dan 2 query dengan hasil yang akurat. Untuk Query 1 (smart energy analytics), D2 (Smart Energy Optimization) paling relevan dengan similarity 0.945 karena mengandung ketiga term query dengan sempurna. Untuk Query 2 (sensor data model), D5 (Smart Waste Management) paling relevan dengan similarity 0.720 karena mengandung term "sensor" dan "data" yang memiliki IDF tinggi.

TF-IDF terbukti efektif dalam memberikan bobot lebih pada term yang diskriminatif (IDF tinggi), mempertimbangkan frekuensi kemunculan term dalam dokumen (TF), dan menghasilkan ranking yang sesuai dengan konteks pencarian. Query yang berbeda menghasilkan ranking yang berbeda, membuktikan sistem bekerja dengan baik dalam membedakan relevansi dokumen berdasarkan term yang dicari.

### Saran

Untuk penelitian selanjutnya, dapat ditambahkan preprocessing yang lebih lengkap seperti stemming dan stopword removal, membandingkan dengan metode lain seperti BM25 atau LSI, mengimplementasikan dalam aplikasi web untuk penggunaan praktis, dan menambah jumlah dokumen serta variasi query untuk testing yang lebih komprehensif.

## DAFTAR PUSTAKA

Manning, C. D., Raghavan, P., & Schütze, H. (2008). Introduction to Information Retrieval. Cambridge University Press.

Salton, G., & Buckley, C. (1988). Term-weighting approaches in automatic text retrieval. Information Processing & Management, 24(5), 513-523.

Baeza-Yates, R., & Ribeiro-Neto, B. (2011). Modern Information Retrieval: The Concepts and Technology behind Search (2nd ed.). Addison-Wesley Professional.
