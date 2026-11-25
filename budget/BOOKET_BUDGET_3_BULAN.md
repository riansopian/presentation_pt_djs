# Proyeksi Budget Booket Tiga Bulan

**Tanggal penyusunan:** 23 November 2026 (WIB).  
**Project:** `booket-512519`, terutama region Jakarta `asia-southeast2`.  
**Dasar:** konfigurasi live, metrik 16–22 November 2026 WIB, tarif publik yang dicek 23 November 2026, serta informasi layanan eksternal dari pengguna.  
**Mata uang:** USD. Semua angka sebelum pajak. Nilai ditampilkan sampai dua desimal; tarif satuan mempertahankan presisi yang diperlukan.

## Periode dan keputusan anggaran

Periode rancangan PoC adalah **1 Desember 2026–28 Februari 2027**, yaitu tiga bulan kalender penuh berikutnya, total **90 hari atau 2.160 jam**. Sisa 23–30 November 2026 berada di luar proyeksi. Jadwal ini merupakan asumsi perencanaan, belum jadwal pelaksanaan yang disetujui.

Usulan anggaran PoC adalah **US$7.140,74**, dibulatkan menjadi **US$7.200,00 sebelum pajak**. Angka tersebut sudah memasukkan Brevo, Confluent Standard, cadangan 20%, satu renewal domain setahun penuh, dan dua minggu transisi konfigurasi existing ke target PoC.

| Skenario | 3 bulan sebelum cadangan | Kas dengan 1 renewal domain dan cadangan 20% |
| --- | --- | --- |
| Existing + Kafka | US$6.958,73 | US$8.364,11 |
| PoC dengan transisi | US$5.939,25 | US$7.140,74 |
| Uji beban | US$8.127,12 | US$9.766,19 |

Kolom biaya tiga bulan memakai alokasi domain 3/12 tahun. Kolom kas menggantinya dengan satu renewal penuh US$15,15, kemudian menerapkan cadangan 20%. Ini cadangan kas konservatif; tanggal renewal belum diketahui dan pembayaran domain mungkin sudah ditanggung sebelumnya. Domain tidak dijumlahkan dua kali.

## Rincian per bulan sebelum cadangan

| Bulan | Existing + Kafka | PoC dengan transisi | Uji beban |
| --- | --- | --- | --- |
| Desember 2026 | US$2.376,05 | US$2.149,49 | US$2.775,95 |
| Januari 2027 | US$2.376,05 | US$1.962,92 | US$2.775,95 |
| Februari 2027 | US$2.206,63 | US$1.826,84 | US$2.575,22 |

Semua kolom di atas sudah memasukkan alokasi domain US$1,26/bulan. Pada skenario PoC, 1–14 Desember memakai estimasi konfigurasi existing + Kafka dan 15 Desember seterusnya memakai target PoC. Biaya berbasis pemakaian/bulanan diprorata untuk transisi sebagai pendekatan anggaran. Brevo dan Confluent tetap aktif selama periode.

Target PoC apabila konfigurasi sudah berlaku sejak 1 Desember memerlukan kas sekitar **US$6.916,85** termasuk cadangan dan satu renewal domain. Transisi menambahkan **US$186,58 sebelum cadangan**, sehingga total kas meningkat **US$223,89** menjadi angka usulan di atas.

## Konfigurasi target PoC

| Komponen | Existing | Target PoC yang dianggarkan |
| --- | --- | --- |
| Cloud SQL | N2 8 vCPU, 64 GiB, cache 375 GiB | N2 4 vCPU, 32 GiB, cache off setelah uji |
| Disk database | SSD 100 GiB | SSD tetap 100 GiB, tidak diasumsikan shrink |
| Backup database | Scheduled backup/PITR disabled | Scheduled backup, allowance used storage 20 GiB |
| Core worker | Request based, sebagian min 0 | Order, outbox dan waiting room: min 1, instance billing |
| Cloud Run lain | Konfigurasi existing | Tetap masuk anggaran, consumer lain divalidasi |
| VM dan disk | 2 VM, 2 disk | Dipertahankan |
| VPC connector dan unused IP | Masih dialokasikan | Usulan dilepas setelah dependensi terverifikasi |
| Confluent Kafka | Akun belum diperiksa | Standard 1 eCKU aktif 24/7, public TLS/SASL |
| Brevo | US$499/bulan menurut pengguna | US$499/bulan |
| Cloudflare dan domain | Free 2 sites, 1 domain .id | Free 2 sites, reserve 1 renewal domain |

Cloud Run target lebih mahal daripada konfigurasi idle existing karena tiga worker diberi alokasi CPU sepanjang waktu untuk menguji pemrosesan background. Penghematan utama berasal dari database dan cache, bukan asumsi seluruh proses dapat scale to zero. Penentuan CPU kontinu untuk consumer di service lain masih perlu validasi dan dapat menambah biaya.

## Layanan eksternal selama PoC

- **Brevo:** US$499 × 3 = **US$1.497** sebelum cadangan dan pajak. Overage email, kontak, SMS/WhatsApp dan add-on belum tersedia.
- **Confluent Standard:** compute 1 eCKU × US$0,75 × 2.160 jam = **US$1.620**; storage allowance **US$14,40**; transfer allowance **US$7,80**. Total vendor **US$1.642,20** sebelum cadangan/pajak. GCP outbound ke Kafka **US$5,70** selama tiga bulan ditambahkan pada Network GCP.
- **Domain:** alokasi tiga bulan US$3,79, tetapi cadangan kas menyisihkan US$15,15 agar satu renewal penuh tersedia jika jatuh tempo.
- **Cloudflare:** Free pada skenario utama. Uji beban menyediakan US$5/account/bulan, sehingga tiga bulan US$15 sebelum overage.

## Skenario uji beban

Skenario ketiga memakai aktivitas Cloud Run **100x sampel**, sekitar 2,74 juta panggilan container per bulan standar. Jumlah ini tidak sama dengan jumlah pengunjung atau order dan tidak membuktikan concurrency maupun kapasitas penjualan.

Kafka memakai **2 eCKU kontinu**, storage logical rata-rata 100 GiB, ingress 100 GiB/bulan dan egress 300 GiB/bulan. Log allowance 100 GiB/bulan dan transfer non-Kafka 200 GiB/bulan. Kondisi beban tinggi dianggarkan selama seluruh tiga bulan sebagai envelope konservatif, bukan ramalan traffic. Biaya kas tiga bulan **US$9.766,19** atau pembulatan **US$9.800,00 sebelum pajak**. Hanya gunakan envelope ini bila kampanye uji beban tersebut dijalankan.

## Batas penggunaan anggaran

Anggaran mengasumsikan resource existing dipakai ulang. Ini bukan biaya membangun lingkungan kedua secara paralel. Bila PoC harus terisolasi dari lingkungan existing, resource duplikat perlu dihitung kembali. SQL 4 vCPU/32 GiB dan cache off merupakan usulan bersyarat hasil pengujian, bukan perubahan yang sudah dilakukan.

Jika target resize gagal lolos, gunakan skenario existing + Kafka sebagai fallback **US$8.364,11**, dibulatkan **US$8.400,00 sebelum pajak**. Jangan memakai angka target PoC sambil mempertahankan seluruh kapasitas database existing tanpa menghitung selisihnya.

## Batas cakupan dan ketidakpastian

- Estimasi ini **bukan tagihan aktual**. Dataset BigQuery tidak ditemukan pada project, sehingga billing export tidak tersedia di scope yang diperiksa. Kredit promo, CUD, kontrak diskon, pajak dan biaya konversi kartu belum direkonsiliasi.
- Cloud Asset API belum aktif. Pemeriksaan menggunakan API produk langsung, bukan inventaris seluruh produk GCP yang dijamin lengkap. Firebase Hosting mengembalikan 403 sehingga belum diverifikasi. Project GCP lain dan akun vendor lain belum dicakup.
- Connector memakai **minimum dua instance**; jumlah instance aktual belum diukur. Jika sudah naik ke 10 instance, compute connector dapat mencapai sekitar US$82,22/bulan, bukan US$16,44.
- Tidak adanya time series bukan bukti biaya historis nol. Tiga Cloud Run tanpa sampel pemakaian diberi asumsi nol untuk aktivitas variabel pada run rate ini, dengan keterbatasan tersebut. Storage dan telemetry memakai allowance ketika metrik atau tagihan belum lengkap.
- Pemeriksaan Cloud Build mencakup lokasi default yang ditanya; region lain dan biaya external CI belum dipastikan. Proses di dalam dua VM belum diperiksa. Valkey, PostgreSQL tambahan, atau Kafka yang mungkin berjalan di VM tidak diasumsikan sebagai layanan managed terpisah.
- Kafka Confluent adalah **anggaran tambahan/usulan** sesuai permintaan, belum terbukti sebagai subscription existing. Brevo sesuai harga yang diberikan, belum termasuk overage, add-on, SMS, WhatsApp atau dedicated IP jika ditagih terpisah.
- Belum mencakup fee payment gateway, biaya pengembangan dan migrasi, tenaga operasional, support premium, managed Kafka connectors, Flink, ksqlDB, governance berbayar, serta biaya layanan lain yang belum teridentifikasi. Tidak adanya angka untuk item ini tidak berarti gratis.
- Cadangan 20% adalah cadangan perencanaan. Cadangan tidak menjamin seluruh biaya yang belum diketahui pasti tertutup. Proyeksi kapasitas perlu diuji sebelum digunakan untuk production.

## Rujukan

Detail resource, tarif, asumsi Kafka, dan sumber publik ada pada [Budget existing](BOOKET_BUDGET_EXISTING.md). Lihat juga [Proposal PoC](BOOKET_PROPOSAL_POC.md) dan [Proyeksi satu tahun](BOOKET_BUDGET_1_TAHUN.md).
