# Analisis Kekeringan Meteorologis (SPI-1) Provinsi Jambi

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Thesis-blue)

## Deskripsi Proyek

Repositori ini berisi implementasi kode Python untuk menghitung dan memetakan **Indeks Kekeringan Meteorologis** menggunakan metode **SPI-1 (*Standardized Precipitation Index - 1 Bulan*)**.

Studi kasus dilakukan di **Provinsi Jambi** dengan menggunakan data curah hujan satelit **CHIRPS v3.0** yang telah dikoreksi bias dan di-*downscale* ke resolusi 1 km. Analisis ini bertujuan untuk mengidentifikasi tingkat keparahan kekeringan bulanan dari tahun 2009 hingga 2020.

## Metodologi

### 1. Perhitungan SPI
Nilai SPI dihitung dengan menyesuaikan data curah hujan bulanan terhadap **Distribusi Gamma** (*Gamma Distribution*).
* Transformasi probabilitas kumulatif Gamma ke distribusi normal standar (Z-score).
* Dilakukan *pixel-by-pixel* untuk seluruh wilayah kajian.

### 2. Klasifikasi Kekeringan
Nilai SPI diklasifikasikan menjadi 5 kelas tingkat kekeringan berdasarkan standar BMKG/WMO:

| Nilai SPI | Kelas Kekeringan | Label |
| :--- | :--- | :--- |
| $\geq$ -1.00 | **Tidak Ada Kekeringan** | 1 (Biru) |
| -0.99 s.d 0.99 | **Mendekati Normal** | 2 (Hijau Muda) |
| -1.00 s.d -1.49 | **Agak Kering** | 3 (Kuning) |
| -1.50 s.d -1.99 | **Kering** | 4 (Oranye) |
| $\leq$ -2.00 | **Sangat Kering** | 5 (Merah) |

*> Catatan: Dalam script ini, nilai SPI positif (Basah) disederhanakan menjadi satu kelas "Tidak Ada Kekeringan" karena fokus studi adalah bencana kekeringan.*

## Alur Kerja Script

1.  **Load Data:** Membaca data raster CHIRPS (format GeoTIFF) dan Shapefile batas administrasi.
2.  **Preprocessing:** Reproyeksi data ke WGS84 dan *masking* area di luar Provinsi Jambi.
3.  **Core Calculation:** Menghitung fitting distribusi Gamma dan nilai SPI untuk setiap piksel.
4.  **Classification:** Mengubah nilai kontinu SPI menjadi data diskrit (Kelas 1-5).
5.  **Export:**
    * Raster SPI (`.tif` Float32)
    * Raster Kelas (`.tif` Int16)
    * Vektor Polygon (`.shp`) untuk analisis lanjut di GIS.

## Struktur Direktori Data

Script ini dirancang untuk berjalan di **Google Colab** dengan struktur folder Google Drive sebagai berikut:

```text
/My Drive/Colab Notebooks/Skripsi/
├── Batas Adm Jambi/            # Shapefile Batas Provinsi
├── CHIRPS v3/
│   └── bias_corrected_regression/
│       └── downscaled_1km_metric/  # Input: Raster CHIRPS 1km
└── SPI-Output/                 # Output: Folder hasil (Otomatis dibuat)

```

## Prasyarat Instalasi

Script membutuhkan pustaka geospasial Python tingkat lanjut:

```bash
pip install rioxarray xarray geopandas rasterio scipy leafmap

```

## Cara Penggunaan

1. Pastikan data CHIRPS 1km sudah tersedia di Google Drive.
2. Buka script di Google Colab.
3. Jalankan script. Proses akan dilakukan secara *batch* per periode tahun (misal: 2009-2011, 2012-2013, dst) untuk menghemat memori RAM.
4. Hasil peta kekeringan akan tersimpan di folder `SPI-Output`.

## Contoh Visualisasi

Script akan menghasilkan plot peta klasifikasi kekeringan secara otomatis untuk bulan-bulan tertentu (misal: Agustus) guna memantau puncak musim kemarau.

## Penulis

**Jariyan Arifudin** Mahasiswa Geografi Lingkungan

Universitas Gadjah Mada (UGM)

## Lisensi & Sitasi

Kode ini didistribusikan di bawah **MIT License**.
Jika Anda menggunakan metode ini untuk penelitian, silakan sitasi repositori ini:

> Arifudin, J. (2026). *Analisis Kekeringan Meteorologis (SPI-1) Provinsi Jambi*. GitHub Repository.
