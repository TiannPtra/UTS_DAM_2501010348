Nama : Kristian Putra Santosa

Nim : 2501010348

Prodi : Informatika - MDA

Kelas: E

# 🧭 WEST JAVA TOURISM INTELLIGENCE 

### Integrasi Data Potensi Wisata, Historical Weather, dan Informasi Paket Wisata

**UTS --- Data Acquisition & Management**\
**Program Studi Informatika · INSTIKI**

`<br/>`{=html}

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Processing-150458?style=for-the-badge&logo=pandas)
![Status](https://img.shields.io/badge/Project-Academic%20Research-16A34A?style=for-the-badge)

*Turning tourism data into meaningful regional insights.*
:::

------------------------------------------------------------------------

## 👤 Identitas Penyusun

  Identitas        Keterangan
  ---------------- -------------------------------
  Nama             **\[ISI NAMA LENGKAP\]**
  NIM              **\[ISI NIM\]**
  Program Studi    Informatika
  Institusi        INSTIKI
  Mata Kuliah      Data Acquisition & Management
  Dosen Pengampu   **\[ISI NAMA DOSEN\]**
  Tahun Akademik   **\[ISI TAHUN AKADEMIK\]**

> **Catatan:** Lengkapi bagian identitas bertanda `[ISI ...]` sebelum
> README ini dikumpulkan atau dipublikasikan.

------------------------------------------------------------------------

## 🌏 Tentang Proyek

**West Java Tourism Intelligence** merupakan proyek akuisisi dan
pengelolaan data yang berfokus pada inventaris potensi pariwisata di
Provinsi Jawa Barat. Proyek ini menggabungkan data statistik pariwisata,
data cuaca historis, serta upaya pengumpulan informasi paket wisata dari
sumber daring.

Proyek dirancang untuk mempraktikkan tiga pendekatan akuisisi data:

1.  **File-based acquisition** --- membaca dataset CSV dari publikasi
    Badan Pusat Statistik (BPS).
2.  **API acquisition** --- mengambil data cuaca historis melalui
    Open-Meteo Archive API.
3.  **Web scraping** --- menguji pengambilan informasi paket wisata dari
    JADESTA dengan pemeriksaan `robots.txt` dan ketentuan penggunaan
    terlebih dahulu.

Hasil integrasi utama menghubungkan inventaris objek wisata BPS tahun
2005 dengan ringkasan cuaca historis Open-Meteo pada tingkat
kabupaten/kota. Informasi JADESTA diperlakukan sebagai tabel terpisah
dan hanya dihubungkan jika pemetaan wilayah dapat divalidasi.

## 🎯 Tujuan

-   Mengakuisisi data pariwisata dari sumber yang dapat ditelusuri.
-   Memahami proses pengambilan data dari CSV, API, dan website.
-   Menyimpan data mentah (*raw data*) tanpa mengubah sumber aslinya.
-   Melakukan identifikasi, pembersihan ringan, dan pemeriksaan kualitas
    data.
-   Mendokumentasikan asal data (*data provenance*), lisensi, hash MD5,
    dan etika penggunaan.
-   Mengintegrasikan data dengan kunci yang dapat dipertanggungjawabkan.
-   Menyajikan analisis deskriptif mengenai sebaran potensi wisata dan
    konteks cuaca wilayah.

------------------------------------------------------------------------

## 📚 Sumber Dataset --- Bagian Paling Penting

Semua sumber utama proyek dicantumkan di bawah agar asal data dapat
diperiksa dan direproduksi.

### 1. Dataset utama --- BPS Provinsi Jawa Barat

  -----------------------------------------------------------------------
  Informasi                           Keterangan
  ----------------------------------- -----------------------------------
  Nama publikasi                      *Kebudayaan dan Pariwisata Jawa
                                      Barat dalam Angka 2005*

  Instansi                            Badan Pusat Statistik Provinsi Jawa
                                      Barat

  Metode akuisisi                     File CSV

  Periode data                        2005

  Cakupan                             23 kabupaten/kota di Jawa Barat

  Ukuran dataset yang dicatat         198 baris × 7 kolom
  notebook                            

  Format                              CSV
  -----------------------------------------------------------------------

**Sumber publikasi resmi:**\
https://jabar.bps.go.id/id/publication/2006/05/09/225528b03bf64b54704968de/kebudayaan-dan-pariwisata-jawa-barat-dalam-angka-2005.html

Kolom dataset utama:

-   `tahun`
-   `kabupaten_kota`
-   `lokasi`
-   `jenis_potensi`
-   `nama_objek_wisata`
-   `halaman_sumber`
-   `tabel_sumber`

**Catatan interpretasi:** dataset ini merupakan inventaris/pencatatan
objek wisata pada tahun 2005, bukan data jumlah kunjungan wisatawan.
Tanda `-` pada kolom `lokasi` diperlakukan sebagai informasi yang tidak
tersedia. Kandidat duplikasi ditandai untuk ditinjau, bukan otomatis
dihapus.

**Lisensi dan penggunaan:** periksa ketentuan publikasi BPS yang berlaku
dan catat hasil verifikasi pada `docs/bukti_lisensi.csv`. Jangan
menyatakan izin atau lisensi tertentu sebelum ada bukti yang dapat
dirujuk.

### 2. Data cuaca historis --- Open-Meteo

  Informasi              Keterangan
  ---------------------- --------------------------------------------------
  Penyedia               Open-Meteo
  Metode akuisisi        REST API
  Endpoint               Historical Weather / Archive API
  Periode yang diminta   1 Januari--31 Desember 2005
  Resolusi               Harian
  Zona waktu             `Asia/Jakarta`
  Cakupan lokasi         Titik acuan kabupaten/kota yang lolos verifikasi

**Dokumentasi API:**\
https://open-meteo.com/en/docs/historical-weather-api

**Endpoint:**\
https://archive-api.open-meteo.com/v1/archive

Variabel cuaca yang diminta:

-   `temperature_2m_mean` --- suhu rata-rata (°C)
-   `temperature_2m_max` --- suhu maksimum (°C)
-   `precipitation_sum` --- jumlah presipitasi (mm)
-   `relative_humidity_2m_mean` --- kelembapan relatif rata-rata (%)
-   `wind_speed_10m_max` --- kecepatan angin maksimum pada 10 m (km/jam)
-   `weather_code` --- kode kondisi cuaca

Atribusi yang digunakan notebook: **Weather data by Open-Meteo.com**,
dengan lisensi CC BY 4.0 sesuai keterangan sumber.\
https://creativecommons.org/licenses/by/4.0/

**Catatan metodologis:** CSV BPS tidak menyediakan koordinat objek
wisata. Karena itu, notebook mencari dan memverifikasi titik acuan
kabupaten/kota melalui layanan geocoding, bukan mengarang koordinat
objek. Data cuaca merupakan konteks cuaca pada titik acuan wilayah dan
tidak boleh dianggap sebagai pengukuran tepat di setiap objek wisata.

### 3. Sumber web scraping --- JADESTA

  Informasi               Keterangan
  ----------------------- ------------------------------------------------
  Nama                    JADESTA --- Jejaring Desa Wisata
  Pengelola               Kementerian Pariwisata
  Metode yang dirancang   Web scraping halaman paket wisata
  Target wilayah          Provinsi Jawa Barat
  Halaman daftar          https://jadesta.kemenpar.go.id/paket/jenis/105

Atribut yang ditargetkan jika pengambilan data diizinkan dan tersedia:

-   Nama paket wisata
-   Nama desa wisata
-   Kabupaten/kota dan provinsi
-   Harga
-   Deskripsi
-   Fasilitas
-   URL detail dan URL sumber
-   Tanggal pengambilan data

**PENTING --- status akses:** notebook menerapkan gerbang keselamatan:
scraping hanya dijalankan apabila pemeriksaan `robots.txt` mengizinkan
dan ketentuan penggunaan (ToS) telah dibaca serta dikonfirmasi secara
manual. Jika status robots tidak jelas, akses ditolak, atau ToS belum
terkonfirmasi, proses harus dihentikan dan didokumentasikan. Dengan
demikian, JADESTA tidak boleh diklaim menghasilkan data apabila `df_jds`
kosong atau status scraping menunjukkan dihentikan.

**Halaman robots.txt:**\
https://jadesta.kemenpar.go.id/robots.txt

Tidak ditemukannya tampilan robots.txt saja bukan bukti bahwa scraping
otomatis diperbolehkan. Catat kode HTTP dan isi respons aktual pada
`docs/bukti_robots.csv`, lalu verifikasi ketentuan penggunaan dengan
pengelola bila diperlukan.

------------------------------------------------------------------------

## 🧪 Metodologi dan Alur Pengolahan

``` text
BPS CSV (2005) ───────────────┐
                              │
Open-Meteo Archive API ───────┼──> Validasi & pembersihan
                              │          │
JADESTA (bersyarat izin) ─────┘          ▼
                                  Integrasi terkontrol
                                          │
                         ┌────────────────┴───────────────┐
                         ▼                                ▼
                Dataset terintegrasi              Tabel JADESTA
                BPS × cuaca 2005                  terpisah / relasi
                         │                        wilayah tervalidasi
                         ▼
                 Analisis deskriptif
                         │
                         ▼
             Dokumentasi & audit provenance
```

### Tahapan notebook

  -----------------------------------------------------------------------
  Bab                                 Proses
  ----------------------------------- -----------------------------------
  BAB 0                               Persiapan proyek, library,
                                      konfigurasi request, folder,
                                      helper, dan provenance

  BAB 1                               Akuisisi dan pembacaan dataset
                                      utama BPS

  BAB 2                               Identifikasi dataset, pemeriksaan
                                      kualitas, dan estimasi storage

  BAB 3                               Lisensi, pemindaian PII, dan Data
                                      Ethics Checklist

  BAB 4                               Akuisisi API Open-Meteo untuk cuaca
                                      historis 2005

  BAB 5                               Pemeriksaan robots/ToS dan scraping
                                      JADESTA secara bersyarat

  BAB 6                               Integrasi data dan analisis
                                      deskriptif

  BAB 7                               Audit akhir, MD5, provenance, dan
                                      validasi ketentuan UTS
  -----------------------------------------------------------------------

### Strategi integrasi

-   Data BPS dan cuaca tahunan diintegrasikan menggunakan
    `kabupaten_kota`.
-   Merge utama menggunakan `validate="many_to_one"` karena banyak objek
    wisata dapat berada pada satu kabupaten/kota, sedangkan tabel cuaca
    tahunan diringkas menjadi satu baris per wilayah.
-   Jumlah baris dan keunikan `id_objek` diperiksa sebelum dan sesudah
    merge.
-   JADESTA disimpan sebagai tabel terpisah. Relasi wilayah hanya dibuat
    ketika nama kabupaten/kota cocok secara terverifikasi.
-   Kandidat kecocokan nama desa dengan kolom lokasi BPS hanya menjadi
    bahan verifikasi manual, bukan relasi yang dianggap pasti.

------------------------------------------------------------------------

## 🗂️ Struktur Direktori

Struktur proyek yang dihasilkan notebook:

``` text
WEST_JAVA_TOURISM_INTELLIGENCE/
│
├── data/
│   ├── raw/
│   │   ├── dataset_utama/
│   │   ├── openmeteo/
│   │   └── jadesta/
│   │
│   ├── interim/
│   └── processed/
│
├── docs/
│   ├── provenance.json
│   ├── data_dictionary.csv
│   ├── data_ethics_checklist.csv
│   ├── bukti_robots.csv
│   ├── bukti_lisensi.csv
│   ├── pii_scan.csv
│   ├── storage_estimation.csv
│   └── scraping_quality_report.csv
│
├── notebooks/
│   └── 01_west_java_tourism_uts.ipynb
│
├── README.md
└── requirements.txt
```

**Prinsip penyimpanan:**

-   `data/raw/` menyimpan respons/berkas sumber apa adanya dan tidak
    ditimpa oleh proses cleaning.
-   `data/interim/` menyimpan salinan untuk pemeriksaan dan pembersihan.
-   `data/processed/` menyimpan hasil olahan dan integrasi.
-   `docs/` menyimpan bukti sumber, metadata, kualitas, lisensi, etika,
    dan provenance.

------------------------------------------------------------------------

## 🛠️ Teknologi yang Digunakan

-   **Python** --- bahasa pemrograman utama.
-   **Pandas & NumPy** --- manipulasi dan pemeriksaan data.
-   **Requests** --- HTTP request untuk API dan halaman web.
-   **BeautifulSoup4 & lxml** --- parsing HTML ketika scraping
    diizinkan.
-   **Matplotlib** --- visualisasi analisis deskriptif.
-   **Jupyter Notebook / Google Colab** --- lingkungan pengerjaan.

Instalasi dependensi:

``` bash
pip install requests beautifulsoup4 lxml pandas numpy matplotlib
```

------------------------------------------------------------------------

## 🔍 Data Quality, Provenance & Etika

Proyek ini menerapkan beberapa kontrol berikut:

-   Menyimpan URL sumber dan waktu akses dalam format UTC.
-   Menghitung hash MD5 untuk memeriksa integritas berkas mentah.
-   Mendokumentasikan lisensi atau status verifikasi lisensi; tidak
    mengarang izin.
-   Mengidentifikasi missing value dan kandidat duplikasi.
-   Tidak menghapus kandidat duplikasi secara otomatis tanpa
    pemeriksaan.
-   Menggunakan timeout request 30 detik, User-Agent yang
    mengidentifikasi proyek, jeda minimal 2 detik, dan retry terbatas.
-   Memeriksa `robots.txt` dan ketentuan penggunaan sebelum scraping.
-   Tidak melakukan bypass login, CAPTCHA, pembatasan akses, atau
    larangan robots.
-   Tidak mengumpulkan kontak pribadi.
-   Mencatat keterbatasan dan butir checklist yang belum selesai secara
    transparan.

------------------------------------------------------------------------

## ⚠️ Batasan Penelitian

1.  Dataset BPS merepresentasikan inventaris objek wisata tahun 2005,
    bukan jumlah kunjungan.
2.  Data cuaca merupakan data historis pada titik acuan kabupaten/kota,
    bukan koordinat persis setiap objek wisata.
3.  Tidak semua lokasi objek wisata memiliki informasi lokasi yang
    lengkap.
4.  Data JADESTA berasal dari periode pengambilan saat ini dan tidak
    setara periode dengan inventaris BPS 2005.
5.  Pengambilan data JADESTA bersyarat pada hasil pemeriksaan robots.txt
    dan ketentuan penggunaan.
6.  Hasil analisis bersifat deskriptif; hubungan antara cuaca dan jumlah
    objek wisata tidak membuktikan hubungan sebab-akibat.
7.  Isi dan jumlah data akhir mengikuti hasil eksekusi notebook, bukan
    angka yang diasumsikan sebelumnya.

------------------------------------------------------------------------

## 🚀 Cara Menjalankan

1.  Unduh atau clone repository proyek.
2.  Siapkan dataset CSV BPS sesuai sumber yang dicantumkan.
3.  Buka `notebooks/01_west_java_tourism_uts.ipynb` menggunakan Jupyter
    Notebook atau Google Colab.
4.  Isi email kampus pada konfigurasi User-Agent.
5.  Jalankan notebook dari BAB 0 secara berurutan.
6.  Periksa hasil geocoding, kualitas API, provenance, dan file
    dokumentasi.
7.  Pada BAB 5, baca dan verifikasi ketentuan JADESTA. Jangan
    mengaktifkan scraping jika izin belum jelas.
8.  Jalankan BAB 6 dan BAB 7 untuk integrasi, analisis, serta audit
    akhir.
9.  Periksa kembali `docs/data_ethics_checklist.csv` dan
    `docs/provenance.json` sebelum pengumpulan.

------------------------------------------------------------------------

## 📌 Luaran Proyek

Luaran yang diharapkan dari eksekusi notebook:

-   Data mentah BPS, respons API Open-Meteo, dan bukti pemeriksaan
    JADESTA.
-   Dataset BPS yang telah diperiksa dan diperkaya dengan konteks cuaca.
-   Agregasi cuaca harian, bulanan, dan tahunan sesuai hasil proses.
-   Tabel relasi wilayah JADESTA jika data berhasil diperoleh secara
    sah.
-   Data dictionary, provenance, hash MD5, bukti robots, bukti lisensi,
    dan checklist etika.
-   Visualisasi dan ringkasan analisis deskriptif.
-   Arsip ZIP proyek untuk pengumpulan.

------------------------------------------------------------------------

## 📖 Referensi Resmi

1.  Badan Pusat Statistik Provinsi Jawa Barat. *Kebudayaan dan
    Pariwisata Jawa Barat dalam Angka 2005*.\
    https://jabar.bps.go.id/id/publication/2006/05/09/225528b03bf64b54704968de/kebudayaan-dan-pariwisata-jawa-barat-dalam-angka-2005.html

2.  Open-Meteo. *Historical Weather API Documentation*.\
    https://open-meteo.com/en/docs/historical-weather-api

3.  Open-Meteo. *Historical Weather API Endpoint*.\
    https://archive-api.open-meteo.com/v1/archive

4.  JADESTA --- Jejaring Desa Wisata. *Paket Wisata*.\
    https://jadesta.kemenpar.go.id/paket/jenis/105

5.  JADESTA. *Robots Exclusion Protocol*.\
    https://jadesta.kemenpar.go.id/robots.txt

6.  Creative Commons. *Attribution 4.0 International (CC BY 4.0)*.\
    https://creativecommons.org/licenses/by/4.0/

7.  Internet Engineering Task Force. *RFC 9309 --- Robots Exclusion
    Protocol*.\
    https://www.rfc-editor.org/rfc/rfc9309

------------------------------------------------------------------------


**WEST JAVA TOURISM INTELLIGENCE**\
*Data • Discovery • Documentation*

Dibuat untuk keperluan akademik --- UTS Data Acquisition & Management\
Program Studi Informatika · INSTIKI
:::
