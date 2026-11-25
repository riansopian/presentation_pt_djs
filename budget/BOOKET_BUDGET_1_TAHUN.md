# Proyeksi Budget Booket Satu Tahun

**Tanggal penyusunan:** 23 November 2026 (WIB).  
**Project:** `booket-512519`, terutama region Jakarta `asia-southeast2`.  
**Dasar:** konfigurasi live, metrik 16–22 November 2026 WIB, tarif publik yang dicek 23 November 2026, serta informasi layanan eksternal dari pengguna.  
**Mata uang:** USD. Semua angka sebelum pajak. Nilai ditampilkan sampai dua desimal; tarif satuan mempertahankan presisi yang diperlukan.

## Periode dan ringkasan

Periode **1 Desember 2026–30 November 2027**, 12 bulan kalender penuh atau **8.760 jam**. Proyeksi ini mempertahankan tarif pada snapshot, tanpa inflasi otomatis, diskon komitmen, promo, atau kenaikan harga vendor. Ini proyeksi operasional, bukan komitmen berlangganan satu tahun.

| Skenario | Biaya 12 bulan sebelum cadangan | Anggaran dengan cadangan 20% |
| --- | --- | --- |
| Existing + Kafka | US$28.117,27 | US$33.740,73 |
| PoC dengan transisi | US$23.424,06 | US$28.108,88 |
| Uji beban | US$32.843,06 | US$39.411,67 |

Untuk target PoC yang diteruskan menjadi konfigurasi operasi dan lolos validasi, rencana satu tahun adalah **US$28.108,88**, dibulatkan **US$28.200,00 sebelum pajak**. Anggaran memasukkan dua minggu transisi di Desember. Opsi mempertahankan konfigurasi existing plus Kafka memerlukan sekitar **US$33.800,00 sebelum pajak** termasuk cadangan.

## Kalender bulanan sebelum cadangan

| Bulan | Jam kalender | Existing + Kafka | PoC dengan transisi | Uji beban |
| --- | --- | --- | --- | --- |
| Desember 2026 | 744 | US$2.376,05 | US$2.149,49 | US$2.775,95 |
| Januari 2027 | 744 | US$2.376,05 | US$1.962,92 | US$2.775,95 |
| Februari 2027 | 672 | US$2.206,63 | US$1.826,84 | US$2.575,22 |
| Maret 2027 | 744 | US$2.376,05 | US$1.962,92 | US$2.775,95 |
| April 2027 | 720 | US$2.319,58 | US$1.917,56 | US$2.709,04 |
| Mei 2027 | 744 | US$2.376,05 | US$1.962,92 | US$2.775,95 |
| Juni 2027 | 720 | US$2.319,58 | US$1.917,56 | US$2.709,04 |
| Juli 2027 | 744 | US$2.376,05 | US$1.962,92 | US$2.775,95 |
| Agustus 2027 | 744 | US$2.376,05 | US$1.962,92 | US$2.775,95 |
| September 2027 | 720 | US$2.319,58 | US$1.917,56 | US$2.709,04 |
| Oktober 2027 | 744 | US$2.376,05 | US$1.962,92 | US$2.775,95 |
| November 2027 | 720 | US$2.319,58 | US$1.917,56 | US$2.709,04 |

Semua angka sudah memasukkan alokasi domain US$15,15/12 per bulan. Total setahun memasukkan satu renewal domain US$15,15, bukan dua belas renewal. Cadangan dihitung 20% dari total tahunan. Untuk kas bulanan, pindahkan pembayaran domain ke bulan renewal sebenarnya tanpa menambah total setahun.

Komponen per jam berubah mengikuti 28, 30, atau 31 hari. Subscription Brevo, allowance storage dan transfer tetap berbasis bulan. Selama 14 hari pertama Desember, skenario PoC memakai pendekatan biaya existing lalu beralih ke target PoC. Tambahan transisi setahun adalah **US$223,89 termasuk cadangan** terhadap target yang berlaku sejak hari pertama.

## Komponen biaya setahun pada target stabil

| Komponen | USD 12 bulan target stabil |
| --- | --- |
| Cloud SQL | US$6.043,73 |
| Compute Engine dan disk | US$1.319,71 |
| Cloud Run | US$2.868,30 |
| Network dan VPC | US$156,00 |
| Storage observability dan operasi | US$187,80 |
| Confluent Standard usulan | US$6.658,80 |
| Cloudflare 2 sites | US$0,00 |
| Brevo email dan CRM | US$5.988,00 |
| Domain .id | US$15,15 |
| Total target stabil sebelum cadangan | US$23.237,49 |
| Tambahan transisi 14 hari | US$186,58 |
| Total rencana sebelum cadangan | US$23.424,06 |

Tabel komponen memakai target stabil sebagai titik awal; tambahan transisi ditunjukkan sekali pada baris terpisah sehingga dapat direkonsiliasi dengan kalender bulanan. Perbedaan kecil penjumlahan angka yang ditampilkan dapat berasal dari pembulatan ke dua desimal.

## Driver yang paling memengaruhi anggaran

- **Brevo:** **US$5.988/tahun** sebelum cadangan/pajak, sesuai harga US$499/bulan yang diberikan. Tidak mengasumsikan email/contact tier tertentu.
- **Confluent Standard 1 eCKU:** compute US$6.570/tahun, storage allowance US$57,60 dan transfer allowance US$31,20. Total vendor **US$6.658,80/tahun** sebelum cadangan/pajak; GCP outbound ke Kafka US$22,80/tahun berada di Network.
- **Cloud SQL:** N2 4 vCPU/32 GiB, SSD tetap 100 GiB, cache off, dan backup used-storage allowance 20 GiB menghasilkan sekitar US$503,64/bulan standar. Kelayakan kapasitas harus diuji; rata-rata RAM existing sekitar 24,57 GiB belum membuktikan batas puncak pada event.
- **Worker:** tiga service inti dihitung instance billing min 1 sepanjang tahun. Jika consumer service lain juga memerlukan CPU kontinu atau jumlah worker bertambah, proyeksi diperbarui.
- **Traffic dan observability:** allowance utama menggunakan 20 GiB/bulan non-Kafka egress, log 10 GiB, reserve Monitoring/Trace US$10 dan operasi/CI US$5. Pola peak event perlu budget terpisah.

## Penggunaan skenario uji beban

Skenario uji beban bukan target pertumbuhan tahunan otomatis. Angkanya adalah envelope bila aktivitas API 100x sampel dan Kafka 2 eCKU dipertahankan **setiap bulan**. Gunakan kolom itu hanya untuk bulan yang benar-benar memerlukan beban tersebut, lalu campurkan dengan bulan target stabil. Uji kapasitas dan throughput tetap diperlukan.

Perkiraan penghematan antara existing + Kafka dan PoC dengan transisi selama setahun adalah **US$5.631,85 termasuk cadangan**, sekitar **16,7%**. Ini penghematan dua konfigurasi usulan dengan subscription eksternal yang sama; bukan bukti penurunan invoice yang sudah terjadi.

## Pengendalian dan review

Lakukan review bulanan per SKU GCP, biaya account Confluent, email/contact usage Brevo, dan quota Workers. Tinjau ulang sebelum peluncuran event atau perubahan retention, eCKU, jumlah worker, atau paket vendor. Anggaran alert yang diusulkan adalah 50%, 80%, 100% actual dan 100% forecast per bulan pada scope biaya GCP; vendor eksternal dipantau terpisah. Alert bukan hard cap dan belum dibuat.

Halaman harga Google Observability sudah mencantumkan biaya metric alerting policies mulai **1 September 2027**. Jumlah policy dan metric reference belum diinventarisasi; biaya tersebut perlu dipetakan ke reserve Monitoring/Trace, bukan dianggap selalu gratis. [Sumber tarif Observability](https://cloud.google.com/products/observability/pricing).

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

Lihat [Budget existing dan sumber tarif](BOOKET_BUDGET_EXISTING.md), [Budget tiga bulan](BOOKET_BUDGET_3_BULAN.md), serta [Proposal PoC](BOOKET_PROPOSAL_POC.md).
