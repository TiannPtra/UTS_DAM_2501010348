# West Java Tourism Intelligence (UTS Data Acquisition & Management)

Integrasi inventaris potensi wisata Jawa Barat (BPS, 2005), cuaca historis Open-Meteo (2005), dan paket wisata JADESTA.

## Struktur
`data/raw` (mentah, read-only) · `data/interim` · `data/processed` · `docs` (provenance, checklist, bukti) · `notebooks`

## Tujuan penggunaan data
Tugas akademik UTS (non-komersial): mempraktikkan akuisisi (berkas, API, scraping), provenance, etika data, dan integrasi.
Hasil tidak dimaksudkan sebagai dasar kebijakan atau klaim jumlah kunjungan wisatawan.

## Keterbatasan dan potensi bias
- CSV utama hanya tahun 2005: inventaris objek, bukan kunjungan; objek yang tidak tercatat di publikasi tidak ada.
- Koordinat objek tidak tersedia dari sumber yang diizinkan; cuaca diambil pada titik acuan kabupaten/kota (bukan lokasi objek) sehingga mengabaikan variasi mikroiklim/ketinggian.
- Open-Meteo historis berbasis reanalisis (grid), bukan stasiun di lokasi objek.
- JADESTA berisi paket desa wisata yang terdaftar saat scraping (bukan 2005), tidak mewakili seluruh pariwisata; sampel scraping dibatasi.
- Perubahan batas wilayah sejak 2005 (mis. pemekaran) dapat menggeser pemetaan kabupaten/kota.
- Periode berbeda antar sumber: tidak boleh ditafsirkan sebagai kondisi yang sama.

## Keamanan, penyimpanan, penghapusan
- Tidak ada API key/token; Open-Meteo dipakai tanpa key (non-komersial). Tidak ada kredensial pada notebook.
- Raw disimpan read-only; hash MD5 pada docs/provenance.json; verifikasi ulang di BAB 7.
- Data kontak pribadi (telepon/email desa wisata) tidak dikumpulkan.
- Retensi: data dipakai untuk penilaian UTS; setelah semester selesai salinan lokal/Colab dihapus kecuali diminta dosen.

## Atribusi
Weather data by Open-Meteo.com (lisensi CC BY 4.0 sesuai ketentuan yang dirujuk; lihat docs/bukti_lisensi.csv).
Data potensi wisata: BPS Provinsi Jawa Barat. Data paket wisata: JADESTA, Kementerian Pariwisata RI.
