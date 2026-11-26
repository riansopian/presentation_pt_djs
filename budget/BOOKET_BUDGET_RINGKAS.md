# Booket — ringkasan budget terbaru

**Pembaruan:** 26 November 2026 · **Mata uang:** USD · **Zona waktu:** WIB  
**Project:** `booket-512519` · region utama `asia-southeast2` (Jakarta)

Gunakan dokumen ini sebagai ringkasan anggaran terkini. Dokumen budget bertanggal 23 November di folder ini tetap disimpan sebagai arsip dan sudah tidak menggambarkan scope resource terbaru.

## Angka utama

| Periode | Total dasar | Cadangan GCP | Total dengan cadangan |
| --- | ---: | ---: | ---: |
| November 2026 (historis + forecast 26–30 Nov) | US$2.014,62 | US$10,00 | **US$2.024,62** |
| 1 Des 2026–30 Nov 2027 | US$15.226,78 | US$120,00 | **US$15.346,78** |

Forecast 12 bulan rata-rata **US$1.278,90 per bulan** termasuk cadangan. Nilai GCP November dan tahunan adalah estimasi gross pemakaian sebelum free tier, kredit, diskon dan pajak. Billing export tidak tersedia. Angka Brevo dan Confluent memakai asumsi paket/pemakaian; invoice dan telemetry kedua akun vendor belum diverifikasi. Periode 26–30 November masih forecast, bukan aktual.

## Komposisi

| Vendor | November | Proyeksi 12 bulan |
| --- | ---: | ---: |
| GCP gross | US$968,82 | US$2.587,18 |
| Brevo | US$499,00 | US$5.988,00 |
| Confluent Standard | US$546,80 | US$6.651,60 |
| **Total dasar** | **US$2.014,62** | **US$15.226,78** |
| Cadangan GCP | US$10,00 | US$120,00 |
| **Total dengan cadangan** | **US$2.024,62** | **US$15.346,78** |

Cadangan merupakan alokasi perencanaan untuk gap biaya GCP yang belum terukur; bukan biaya aktual dan bukan batas maksimum tagihan.

## Perubahan scope yang memengaruhi budget

Audit mencatat Cloud SQL `booket-id-postgres` dihapus pada **23 November 2026 pukul 22:37 WIB**. Biaya compute, SSD 100 GiB dan data cache 375 GiB tetap masuk histori November sampai waktu penghapusan. Forecast sisa November dan proyeksi 12 bulan mengasumsikan SQL tidak dibuat kembali, sehingga biaya SQL adalah **US$0** pada periode forecast. Biaya retained backup juga belum terukur.

Forecast mempertahankan dua VM, minimum dua instance VPC connector, konfigurasi minimum Cloud Run, storage yang terukur, dan static IP Runner yang masih reserved. Trafik mengikuti pola 19–25 November tanpa pertumbuhan otomatis. Forecast tidak memasukkan lingkungan kedua, HA baru, kampanye bertrafik tinggi, atau resource SQL baru.

## Perbandingan dengan budget 23 November

| Dasar tahunan | Budget lama | Forecast baru |
| --- | ---: | ---: |
| PoC/operasi dengan cadangan | US$28.108,88 | US$15.346,78 |
| Selisih nominal |  | **US$12.762,10 lebih rendah (45,4%)** |

Perbandingan ini **tidak setara scope**. Budget lama memasukkan Cloud SQL 32 GiB, cadangan 20%, dan masa transisi 14 hari. Forecast baru mengasumsikan Cloud SQL tetap dihapus dan hanya memasukkan cadangan US$10/bulan. Selisih nominal tersebut tidak dapat disebut penghematan dari konfigurasi yang sama. Biaya menyediakan database kembali tidak dihitung dalam forecast.

Budget PoC tiga bulan lama (**US$7.200**) dan opsi lamanya (**US$8.400/US$9.800**) tidak berlaku sebagai pagu terkini. Dokumen 26 November tidak menyediakan estimasi tiga bulan yang setara. Pagu PoC harus dihitung kembali setelah kebutuhan database dan lingkungan PoC dimasukkan.

## Asumsi forecast

- Periode forecast operasi: **1 Desember 2026–30 November 2027** (8.760 jam).
- GCP memakai metrik resource, inventaris, audit log, dan tarif publik. Billing export tidak ditemukan; angka gross bukan invoice aktual.
- Brevo diasumsikan US$499/bulan sepanjang periode.
- Confluent Standard diasumsikan satu eCKU 24/7, storage logis 20 GB dengan faktor replikasi tiga, dan transfer 10 GB ingress + 30 GB egress/bulan.
- Dua VM berjalan terus; VPC connector minimum dua; minimum instance Cloud Run dipertahankan.
- Tanpa free tier/kredit/diskon spekulatif, pertumbuhan trafik, pemulihan SQL, atau penambahan environment.
- Belum termasuk pajak, biaya network/backup/monitoring yang belum terukur, overage Brevo, connector/Flink Confluent berbayar, atau biaya layanan tambahan.

## Sumber dan rekonsiliasi

Sumber rincian terbaru: `budget-projection-and-forcasting.md` (26 November 2026). Rekonsiliasikan GCP dengan billing export atau invoice setelah November selesai; konfirmasi invoice/telemetry Brevo dan Confluent. Jika SQL atau environment PoC akan dibuat, tambahkan biaya provision, storage, backup, HA, dan migrasinya sebelum menetapkan pagu.
