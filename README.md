# UTS_DAM_2501010348
[README.md](https://github.com/user-attachments/files/34959617/README.md)
# UTS Data Acquisition and Management (DAM): Industri Kreatif di Indonesia

Proyek Ujian Tengah Semester mata kuliah **Data Acquisition and Management** dengan tema **ekonomi kreatif di Indonesia**. Proyek ini menggabungkan tiga sumber data yang diperoleh dengan tiga cara akuisisi berbeda, lalu membersihkan, menggabungkan, menganalisis, dan mendokumentasikannya dengan Python di Google Colab.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/USERNAME/NAMA-REPO/blob/main/UTS_DAM_Industri_Kreatif.ipynb)

> Ganti `USERNAME/NAMA-REPO` pada tautan di atas dengan milik Anda setelah repositori dibuat.

---

## Ringkasan

| Sumber | Peran | Cara akuisisi | Cakupan waktu |
|---|---|---|---|
| **BPS**: *Tenaga Kerja Ekonomi Kreatif 2011-2016* (PDF, Lampiran 1.2) | Dataset utama | Ekstraksi tabel PDF dengan `pdfplumber` | 2011-2016 (historis) |
| **OpenStreetMap** via Overpass API | Fasilitas kreatif (museum, galeri, bioskop, teater, pusat seni, tempat musik, kerajinan) | REST API (`requests`) | Snapshot saat diambil |
| **Wikipedia**: *Daftar museum di Indonesia* | Daftar museum | Web scraping (`requests` + `BeautifulSoup`) | Snapshot saat diambil |

> **Catatan penting tentang waktu data.** Data fasilitas dari OpenStreetMap dan Wikipedia adalah kondisi **saat data diambil**, bukan jumlah fasilitas pada 2011-2016. Karena itu data tersebut hanya dipakai sebagai konteks, dan tidak disimpulkan sebagai penyebab atau pengaruh terhadap tenaga kerja historis.

## Pertanyaan yang dijawab
1. Bagaimana mengubah tabel di PDF BPS menjadi dataset tabular yang rapi dan tervalidasi?
2. Bagaimana sebaran dan perkembangan tenaga kerja ekonomi kreatif antar provinsi pada 2011-2016?
3. Bagaimana data fasilitas kreatif dapat diperoleh secara legal dan etis, lalu digabungkan dengan data BPS melalui kunci provinsi?
4. Apa yang dapat dan tidak dapat disimpulkan dari penggabungan data historis dengan data snapshot?

## Alur kerja

```
PDF BPS ──pdfplumber──▶ tabel lebar ──melt──▶ tabel panjang (34 provinsi × 6 tahun = 204 baris)
Overpass API ──requests──▶ JSON mentah ──▶ tabel fasilitas OSM
Wikipedia ──requests + BeautifulSoup──▶ HTML mentah ──▶ tabel museum
        │
        ▼
  profiling ▶ cleaning + normalisasi provinsi ▶ pd.merge (validate) ▶ analisis + visualisasi ▶ dokumentasi provenance
```

Isi notebook:

| Bab | Isi |
|---|---|
| 1 | Latar belakang, rumusan masalah, tujuan, manfaat |
| 2 | Deskripsi dataset utama dan dataset tambahan |
| 3 | Ekstraksi tabel dari PDF BPS dan validasi terhadap baris Total |
| 4 | Dataset profiling (baris, kolom, tipe data, missing values, duplikasi, ukuran file) |
| 5 | Lisensi, privasi, etika, dan provenance |
| 6 | Akuisisi API: penanganan status HTTP, timeout, retry, penyimpanan data mentah |
| 7 | Web scraping: `robots.txt`, User-Agent, timeout, jeda permintaan |
| 8 | Data cleaning dan normalisasi nama provinsi |
| 9 | Penggabungan dengan `pd.merge` dan parameter `validate` |
| 10 | Analisis deskriptif dan visualisasi (Matplotlib, Seaborn) |
| 11 | Dokumentasi sumber data, ekspor Excel/CSV/JSON, manifest, dan ZIP |
| 12 | Kesimpulan dan rekomendasi |
| Lampiran | Kerangka presentasi dan antisipasi pertanyaan dosen |

## Cara menjalankan (Google Colab)

1. Buka notebook `UTS_DAM_Industri_Kreatif.ipynb` di Google Colab (tombol di atas, atau **File ▸ Upload notebook**).
2. Siapkan PDF BPS *Tenaga Kerja Ekonomi Kreatif 2011-2016* (nama file yang diharapkan: `BPS0000-tenaga_kerja_ekonomi_kreatif_2011_2016.pdf`).
3. Pada sel **Setup**, ganti `EMAIL_KONTAK` dengan email Anda. Nilai ini dimasukkan ke header `User-Agent` agar pengelola situs dapat menghubungi Anda bila ada masalah.
4. Jalankan sel dari atas ke bawah (**Runtime ▸ Run all** atau satu per satu). Saat diminta, unggah file PDF.
5. Di akhir, file `uts_dam_output.zip` akan terunduh otomatis.

Pustaka yang dipakai: `pandas`, `numpy`, `requests`, `pdfplumber`, `beautifulsoup4`, `lxml`, `openpyxl`, `matplotlib`, `seaborn`, `scipy`. Semua dipasang atau sudah tersedia di Colab.

## Struktur keluaran

```
uts_dam/
├── data/
│   ├── raw/      # data mentah: PDF, teks halaman, overpass_*.json, HTML Wikipedia, robots.txt, tabel mentah
│   ├── clean/    # bps_ekraf_long_clean.csv, osm_fasilitas_clean.csv, wikipedia_museum_clean.csv
│   └── final/    # data_gabungan_long.csv, data_gabungan_provinsi.csv, UTS_DAM_data_lengkap.xlsx, dll.
├── docs/         # dokumentasi_sumber_data.json/.xlsx, manifest_file.csv (ukuran + SHA-256)
└── figures/      # grafik hasil analisis (.png)
```

Repositori ini juga menyertakan `bps_ekraf_tabel_204_baris_hasil_ekstraksi_PDF.csv`, yaitu tabel 204 baris hasil ekstraksi dan normalisasi dari PDF BPS.

## Ekstraksi tabel dari PDF

PDF sumber adalah buku utuh (174 halaman). Tabel yang dibutuhkan ada di **Lampiran 1.2** (halaman PDF ke-108). Tantangan dan penanganannya:

| Tantangan | Penanganan |
|---|---|
| Watermark diagonal menyisipkan huruf nyasar ke angka (mis. `326.743i`) | Karakter ber-font watermark dibuang sebelum teks dibaca |
| Nama provinsi terpotong ke 2-3 baris (mis. *Nusa / Tenggara / Barat*) | Fragmen nama dicocokkan ke baris angka berdasarkan posisi vertikal (dynamic programming) |
| Angka berformat Indonesia (`1.029.764`, `1,25`) dan tanda `-` | Fungsi `ke_angka()`; `-` menjadi nilai kosong, bukan 0 |
| PDF bukan file data, rawan salah baca | Jumlah 34 provinsi dibandingkan dengan baris **Total** pada setiap tahun |

Hasil validasi pada PDF yang dipakai: jumlah 34 provinsi **sama persis** dengan baris Total untuk keenam tahun (2011-2016).

## Temuan utama (dari data BPS)

- Tenaga kerja ekonomi kreatif naik dari **13.447.184 (2011)** menjadi **16.909.690 (2016)**, sekitar **+25,7%** (rata-rata sekitar 4,7% per tahun).
- Sangat terkonsentrasi di **Pulau Jawa** (sekitar 71% dari total 2016). Tiga provinsi teratas pada 2016: **Jawa Barat, Jawa Tengah, Jawa Timur**.
- Pertumbuhan 2011-2016 tertinggi: Maluku Utara, Kalimantan Barat, dan Kepulauan Bangka Belitung (dari basis angka yang kecil). **DKI Jakarta** justru turun sekitar 2,5%.
- **Kalimantan Utara** tidak memiliki data 2011-2014 (tercatat `-`), sehingga pertumbuhannya tidak dihitung.

Temuan dari data OSM dan Wikipedia bergantung pada hasil akuisisi saat notebook dijalankan; ringkasannya dicetak otomatis di akhir notebook.

## Etika, lisensi, dan privasi

| Sumber | Lisensi / ketentuan | Catatan |
|---|---|---|
| BPS | Hak cipta BPS; penggandaan untuk tujuan komersial memerlukan izin tertulis | Digunakan untuk keperluan akademik non-komersial dengan sitasi. Periksa ketentuan terbaru di bps.go.id |
| OpenStreetMap | [ODbL 1.0](https://opendatacommons.org/licenses/odbl/) | Wajib atribusi **© OpenStreetMap contributors** |
| Wikipedia | [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) | Wajib atribusi dan berbagi-serupa |

Praktik akuisisi yang diterapkan:
- `User-Agent` yang jelas beserta kontak.
- `timeout`, *retry* dengan *exponential backoff*, dan jeda antar-permintaan.
- `robots.txt` Wikipedia diperiksa sebelum scraping; hanya **satu halaman** yang diambil (tanpa crawling).
- Tidak ada data pribadi: BPS berupa agregat provinsi, OSM/Wikipedia berupa nama tempat publik; kontak personal tidak diambil.
- Tidak ada data yang dibuat atau digandakan manual. Bila sumber tidak mengembalikan data, notebook menampilkan pesan kendala dan melewati tahap yang bergantung padanya.
- Data mentah disimpan terpisah dan tidak diubah; semua perubahan dilakukan pada salinan.

> **Saran untuk repositori:** jangan meng-commit file PDF BPS (ukuran sekitar 8 MB dan berhak cipta). Cantumkan saja tautan sumbernya, dan biarkan pengguna mengunggah PDF sendiri saat menjalankan notebook. Tambahkan `data/raw/*.pdf` ke `.gitignore`.

## Keterbatasan

- Data OSM dan Wikipedia bersifat **crowdsourced**: kelengkapannya tidak merata antar daerah.
- Query Overpass dibatasi jumlah record per kategori (`LIMIT_PER_KATEGORI`), sehingga hasilnya adalah **sampel**, bukan total fasilitas se-Indonesia.
- Banyak record OSM tidak memiliki tag provinsi. Provinsi dilengkapi lewat reverse geocoding Nominatim (opsional); yang tetap gagal ditandai `tidak tersedia`.
- Struktur halaman Wikipedia dapat berubah. Parser mencoba pola tabel dan daftar berpoin, dan menampilkan diagnosis bila tidak ada yang cocok.
- Data BPS berasal dari survei sampel (Sakernas), bukan sensus.
- Korelasi antara tenaga kerja 2016 dan jumlah fasilitas saat ini bersifat **deskriptif**, bukan sebab-akibat.

## Pengembangan lanjutan

- Gunakan WebAPI BPS bila tersedia, agar tidak bergantung pada ekstraksi PDF.
- Akuisisi OSM per provinsi (query per area administratif) agar sampel lebih merata.
- Simpan snapshot bertanggal secara berkala untuk membangun deret waktu fasilitas sendiri.
- Normalisasi per kapita (mis. terhadap jumlah penduduk) agar perbandingan antar provinsi lebih adil.

## Lisensi

- **Kode** (notebook): tentukan lisensi sendiri, misalnya MIT, dengan menambahkan file `LICENSE`.
- **Data**: mengikuti lisensi masing-masing sumber seperti pada tabel di atas.

## Sumber dan atribusi

- Badan Pusat Statistik. *Tenaga Kerja Ekonomi Kreatif 2011-2016*. Jakarta: BPS (No. Katalog 2301034, ISBN 978-602-438-197-4). Data dasar: Sakernas.
- © OpenStreetMap contributors. Data melalui Overpass API.
- Wikipedia bahasa Indonesia, *Daftar museum di Indonesia*, tersedia di bawah CC BY-SA 4.0.
