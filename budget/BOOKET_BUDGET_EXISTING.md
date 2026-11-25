# Budget Existing Cloud Booket

**Tanggal penyusunan:** 23 November 2026 (WIB).  
**Project:** `booket-512519`, terutama region Jakarta `asia-southeast2`.  
**Dasar:** konfigurasi live, metrik 16–22 November 2026 WIB, tarif publik yang dicek 23 November 2026, serta informasi layanan eksternal dari pengguna.  
**Mata uang:** USD. Semua angka sebelum pajak. Nilai ditampilkan sampai dua desimal; tarif satuan mempertahankan presisi yang diperlukan.

## Ringkasan anggaran

Run rate konfigurasi GCP sekarang, Cloudflare Free, Brevo dan alokasi domain adalah **US$1.786,31/bulan** sebelum cadangan dan sebelum tambahan Kafka Confluent. Anggaran tersebut memakai pemakaian rendah pada sampel tujuh hari, sehingga tidak mewakili peak event.

Sesuai permintaan, tambahan Confluent Cloud Standard dan allowance GCP egress ke Kafka dimasukkan terpisah. Total kombinasi menjadi **US$2.343,11/bulan**, atau **US$2.811,73/bulan dengan cadangan 20%**.

| Komponen | USD per bulan | Status |
| --- | --- | --- |
| Cloud SQL | US$1.059,03 | Konfigurasi/metrik dan allowance |
| Compute Engine dan disk | US$109,98 | Konfigurasi/metrik dan allowance |
| Cloud Run | US$73,13 | Konfigurasi/metrik dan allowance |
| Network dan VPC | US$35,57 | Konfigurasi/metrik dan allowance |
| Storage observability dan operasi | US$10,23 | Konfigurasi/metrik dan allowance |
| Confluent Standard usulan | US$554,90 | Usulan tambahan |
| Cloudflare 2 sites | US$0,00 | Konfigurasi/metrik dan allowance |
| Brevo email dan CRM | US$499,00 | Konfigurasi/metrik dan allowance |

| Rekap | USD per bulan |
| --- | --- |
| Subtotal seluruh layanan | US$2.341,84 |
| Alokasi domain .id | US$1,26 |
| Total sebelum cadangan | US$2.343,11 |
| Cadangan 20% | US$468,62 |
| Total anggaran bulanan | US$2.811,73 |

Angka bulanan standar memakai 730 jam. Proyeksi kalender pada file tiga bulan dan satu tahun memakai hari sebenarnya setiap bulan. Domain pada tabel ini dialokasikan 1/12 per bulan, bukan pembayaran bulanan kepada registrar.

## Inventaris Cloud Run

Ditemukan **17 service**. Seluruh konfigurasi saat pemeriksaan memakai request-based billing (`cpuIdle=true`). Minimum instance di tingkat service dan revision sama-sama diperiksa. Max service dan max revision ditampilkan terpisah karena beberapa nilainya berbeda.

| Service | vCPU / GiB | Min | Max service / revision | Request 7 hari | Estimasi USD/bulan |
| --- | --- | --- | --- | --- | --- |
| booket-api-gateway | 1.0 / 0.5 | 0 | 10 / 10 | Tidak ada sampel | US$0,00 |
| booket-id-web | 1.0 / 0.5 | 0 | 5 / 5 | 2.184 | US$0,34 |
| dukcapil-mock | 0.2 / 0.125 | 1 | 28 / 2 | 1 | US$3,00 |
| keycloak | 2.0 / 2.0 | 0 | 3 / 1 | 1.773 | US$0,56 |
| service-middleware-go | 1.0 / 0.5 | 1 | 5 / 5 | 684 | US$13,81 |
| service-middleware-rust | 1.0 / 0.5 | 1 | 10 / 3 | 8 | US$13,80 |
| service-notification | 1.0 / 0.5 | 1 | 5 / 5 | 3 | US$13,81 |
| service-order | 1.0 / 0.5 | 0 | 5 / 5 | 135 | US$0,03 |
| service-outbox | 1.0 / 0.5 | 1 | 1 / 5 | 4 | US$13,80 |
| service-product | 1.0 / 0.5 | 0 | 5 / 5 | 496 | US$0,09 |
| service-testing-cr | 1.0 / 0.5 | 0 | 10 / 10 | Tidak ada sampel | US$0,00 |
| service-ticket | 1.0 / 0.5 | 1 | 5 / 5 | 509 | US$13,81 |
| service-user | 1.0 / 0.5 | 0 | 5 / 5 | 165 | US$0,04 |
| service-waiting-room | 1.0 / 0.5 | 0 | 5 / 5 | 325 | US$0,04 |
| simple-api | 1.0 / 0.5 | 0 | 3 / 1 | Tidak ada sampel | US$0,00 |
| simple-web | 1.0 / 0.25 | 0 | 3 / 0 | 4 | US$0,00 |
| web | 1.0 / 0.5 | 0 | 5 / 5 | 4 | US$0,00 |

Enam service memiliki minimum satu instance: `dukcapil-mock`, `service-middleware-go`, `service-middleware-rust`, `service-notification`, `service-outbox`, dan `service-ticket`. Total kapasitas minimum adalah **5,2 vCPU dan 2,625 GiB**. Minimum instance tetap menghasilkan biaya idle walaupun request rendah.

Total sampel **6.295 panggilan container dalam tujuh hari**. Ini bukan jumlah pengguna unik atau pesanan. Satu alur pengguna dapat menghasilkan beberapa request antarlayanan.

Biaya Cloud Run memakai CPU allocation time, memory allocation time, request count dan konfigurasi minimum instance. Memori aktif diestimasi setelah mengurangi alokasi minimum idle selama jendela sampel, dibatasi minimum nol. Formula request-based adalah `CPU aktif × tarif aktif + CPU idle minimum × tarif idle + GiB-detik aktif dan idle × tarif memori + request × tarif`. Startup CPU boost tercermin dalam CPU allocation time yang terukur. Free tier billing account tidak diasumsikan tersedia.

## Database dan mesin virtual

| Resource | Konfigurasi live | USD/bulan |
| --- | --- | --- |
| Cloud SQL CPU | Enterprise Plus N2, 8 vCPU × 730 jam × US$0,0698 | US$407,63 |
| Cloud SQL RAM | 64 GiB × 730 jam × US$0,0118 | US$551,30 |
| Cloud SQL SSD | 100 GiB dialokasikan × US$0,221 | US$22,10 |
| Cloud SQL data cache | 375 GiB × US$0,208 | US$78,00 |
| VM booket-id-api-gateway | e2-custom-medium-6144, 6 GiB, 1 fractional vCPU untuk billing | US$40,56 |
| VM booket-id-datastore | e2-standard-2, 2 vCPU, 8 GiB | US$65,78 |
| Disk VM gateway | 20 GiB pd-standard × US$0,052 | US$1,04 |
| Disk VM datastore | 20 GiB pd-balanced × US$0,13 | US$2,60 |

Cloud SQL `booket-id-postgres` menggunakan **PostgreSQL 18**, zonal di Jakarta, SSD 100 GiB dan data cache enabled. Backup terjadwal dan PITR disabled pada snapshot. Anggaran existing tidak menambahkan backup baru; backup lama yang mungkin masih ditagih belum direkonsiliasi.

Metrik SQL selama sampel: CPU rata-rata **0,94%**, maksimum **1,07%**; memori rata-rata **38,39%**, maksimum **38,52%** dari 64 GiB. Metrik disk maksimum sekitar 257 MiB. Quota cache terukur 375 GiB dan cache terpakai sekitar 7,5 GiB. Ukuran data kecil tidak otomatis membuktikan working set RAM dapat diperkecil ke 16 GiB.

## Network storage dan observability

- Dua external IPv4 in-use, satu per VM: **US$7,30/bulan**. Satu alamat `booket-id-gitlab-runner` masih RESERVED dan tidak dipakai VM: **US$8,03/bulan** dengan tarif tabel regional Jakarta.
- Connector `cloud-run-internal-con` READY memakai e2-micro, minimum 2 dan maksimum 10 instance. Anggaran minimum **US$16,44/bulan**. Latest Cloud Run yang memiliki jaringan menggunakan Direct VPC egress, tetapi dependensi lain belum dipastikan.
- Tiga bucket: `booket-id` di ASIA, `bucket-id-public` di US, dan `cdn.booket.id` di Jakarta. Dua bucket terakhir memiliki total metrik sekitar 15,5 MiB. `booket-id` tidak muncul pada sampel metrik yang dibaca. Model memakai allowance total **1 GiB × US$0,03**, bukan menyatakan ukuran aktual 1 GiB.
- Lima repository Artifact Registry: `booket-id-backend`, `booket-id-web`, `booket-id-maven`, `booket-id-npm`, dan `service-user`. Ukuran yang dilaporkan pada dua repo adalah 1.218.787.937 byte. Model memakai allowance **2 GiB × US$0,10** secara gross.
- Log terukur sekitar 0,37 GiB/tujuh hari. Allowance existing 2 GiB/bulan berada di bawah free allotment 50 GiB/project/bulan. Reserve Monitoring/Trace US$5 dan CI/CD/storage operations US$5 tetap dicantumkan karena data billing belum lengkap.
- Allowance transfer non-Kafka **10 GiB × US$0,19 = US$1,90/bulan**. Tambahan GCP egress ke Confluent untuk Kafka ingress 10 GiB adalah **US$1,90/bulan** lagi. Keduanya berbeda dari fee ingress/egress Confluent.
- Ada empat topic Pub/Sub milik Artifact Analysis dan tidak ditemukan subscription. Tidak ditemukan GKE cluster, managed Redis di Jakarta, Cloud Run jobs, Cloud Tasks queue Jakarta, managed API Gateway, atau snapshot disk pada scope yang diperiksa. API enabled sendiri tidak dianggap sebagai biaya.

## Cloudflare domain dan Brevo

Cloudflare memakai **Free**, CDN/cache standar serta Workers/Pages untuk **dua site**. Budget existing US$0 mengikuti informasi pengguna. Static asset Pages tidak ditagih sebagai dynamic Function. Kuota dynamic Workers/Pages Functions dibagi pada account; Free memiliki batas harian 100.000 request. Angka US$0 mengasumsikan masih di dalam kuota, bukan jaminan bebas limit. Tidak memasukkan Cache Reserve, R2, KV, Durable Objects atau paket zone Pro tanpa bukti pemakaian.

Untuk opsi naik paket, Workers Paid memiliki minimum **US$5/account/bulan**, bukan US$5 per site dan bukan upgrade Cloudflare Pro per zone. Paket ini mencakup 10 juta dynamic request dan 30 juta CPU-ms per bulan; overage berbeda dari biaya langganan. PoC utama tetap Free, opsi uji beban menyediakan Paid.

Satu domain `.id` diberi allowance renewal **US$15,15/tahun**. Registrar, invoice dan tanggal jatuh tempo belum diberikan. Brevo dihitung sesuai informasi pengguna **US$499/bulan**, tanpa mengasumsikan kuota email, kontak atau seat tertentu.

## Tambahan anggaran Kafka Confluent

“Lower professional plan” ditafsirkan sebagai **Confluent Cloud Standard**, tier untuk workload production dengan public networking. Ini bukan nama paket resmi “Professional”. Basic lebih murah untuk development/testing, tetapi tidak dipakai sebagai basis proposal ini. Region target **GCP Jakarta** didukung menurut dokumentasi vendor. Akun, cluster aktif, tarif region, dan kontrak Confluent belum diverifikasi.

| Driver Kafka | Asumsi | USD/bulan |
| --- | --- | --- |
| Capacity | 1 eCKU × US$0,75/jam × 730 jam, aktif 100% | US$547,50 |
| Storage | 20 GiB logical rata-rata × 3 replikasi × US$0,08 | US$4,80 |
| Ingress dan egress | 10 + 30 GiB × US$0,05 × allowance multiplier 1,3 | US$2,60 |
| Subtotal Confluent | Base rate publik dan asumsi regional, belum quote Jakarta | US$554,90 |
| GCP outbound ke Kafka | 10 GiB × US$0,19, sudah di kelompok Network | US$1,90 |
| Tambahan total untuk Kafka | Confluent + GCP outbound | US$556,80 |

Rate US$0,75/eCKU-jam dan US$0,08/GiB-bulan memakai public base list. Multiplier 1,3 khusus transfer adalah **asumsi anggaran**, bukan tarif Jakarta yang sudah dikonfirmasi. Semua tarif regional harus dicocokkan dengan Cost Estimator atau quote akun. Angka mulai sekitar US$385/bulan di halaman marketing menggunakan asumsi cluster aktif sekitar 70% waktu; model ini memakai 24/7. Storage dihitung sesudah replikasi 3x. Standard 1 eCKU mengacu SLA 99,9%; klaim 99,99% memerlukan sedikitnya 2 eCKU menurut ketentuan vendor.

## Batas cakupan dan ketidakpastian

- Estimasi ini **bukan tagihan aktual**. Dataset BigQuery tidak ditemukan pada project, sehingga billing export tidak tersedia di scope yang diperiksa. Kredit promo, CUD, kontrak diskon, pajak dan biaya konversi kartu belum direkonsiliasi.
- Cloud Asset API belum aktif. Pemeriksaan menggunakan API produk langsung, bukan inventaris seluruh produk GCP yang dijamin lengkap. Firebase Hosting mengembalikan 403 sehingga belum diverifikasi. Project GCP lain dan akun vendor lain belum dicakup.
- Connector memakai **minimum dua instance**; jumlah instance aktual belum diukur. Jika sudah naik ke 10 instance, compute connector dapat mencapai sekitar US$82,22/bulan, bukan US$16,44.
- Tidak adanya time series bukan bukti biaya historis nol. Tiga Cloud Run tanpa sampel pemakaian diberi asumsi nol untuk aktivitas variabel pada run rate ini, dengan keterbatasan tersebut. Storage dan telemetry memakai allowance ketika metrik atau tagihan belum lengkap.
- Pemeriksaan Cloud Build mencakup lokasi default yang ditanya; region lain dan biaya external CI belum dipastikan. Proses di dalam dua VM belum diperiksa. Valkey, PostgreSQL tambahan, atau Kafka yang mungkin berjalan di VM tidak diasumsikan sebagai layanan managed terpisah.
- Kafka Confluent adalah **anggaran tambahan/usulan** sesuai permintaan, belum terbukti sebagai subscription existing. Brevo sesuai harga yang diberikan, belum termasuk overage, add-on, SMS, WhatsApp atau dedicated IP jika ditagih terpisah.
- Belum mencakup fee payment gateway, biaya pengembangan dan migrasi, tenaga operasional, support premium, managed Kafka connectors, Flink, ksqlDB, governance berbayar, serta biaya layanan lain yang belum teridentifikasi. Tidak adanya angka untuk item ini tidak berarti gratis.
- Cadangan 20% adalah cadangan perencanaan. Cadangan tidak menjamin seluruh biaya yang belum diketahui pasti tertutup. Proyeksi kapasitas perlu diuji sebelum digunakan untuk production.

## Dokumen terkait

- [Budget tiga bulan](BOOKET_BUDGET_3_BULAN.md)
- [Budget satu tahun](BOOKET_BUDGET_1_TAHUN.md)
- [Proposal PoC](BOOKET_PROPOSAL_POC.md)

## Sumber dan cara memperbarui

- Konfigurasi diperiksa melalui `gcloud run`, `compute`, `sql`, `storage`, `artifacts`, `pubsub`, `vpc-access`, dan REST API Cloud Monitoring. Pemeriksaan bersifat read only.
- [Cloud SQL pricing](https://cloud.google.com/sql/pricing), pilih Jakarta dan Enterprise Plus N2. [Spesifikasi N2 dan data cache](https://docs.cloud.google.com/sql/docs/postgres/machine-series-overview).
- [Cloud Run pricing](https://cloud.google.com/run/pricing), pilih Jakarta. [Compute E2 pricing](https://cloud.google.com/products/compute/pricing/general-purpose). [Persistent Disk pricing](https://cloud.google.com/compute/disks-image-pricing).
- [Network dan external IPv4 pricing](https://cloud.google.com/vpc/network-pricing). [Direct VPC versus connector](https://docs.cloud.google.com/run/docs/configuring/connecting-vpc).
- [Cloud Storage](https://cloud.google.com/storage/pricing), [Artifact Registry](https://cloud.google.com/artifact-registry/pricing), dan [Observability pricing](https://cloud.google.com/products/observability/pricing).
- [Cloudflare Workers pricing](https://developers.cloudflare.com/workers/platform/pricing/), [Pages Functions pricing](https://developers.cloudflare.com/pages/functions/pricing/), dan [cache standar versus Cache Reserve](https://developers.cloudflare.com/cache/advanced-configuration/cache-reserve/).
- [Confluent pricing](https://www.confluent.io/pricing/), [tipe cluster dan minimum eCKU](https://docs.confluent.io/cloud/current/clusters/cluster-types.html), [dimensi billing dan replikasi](https://docs.confluent.io/cloud/current/billing/dimensions/overview.html), [ketersediaan GCP Jakarta](https://docs.confluent.io/cloud/current/get-started/regions.html).
- Brevo **US$499/bulan**, Cloudflare Free untuk dua site, dan kepemilikan satu domain `.id` berasal dari pengguna. Kontrak dan dashboard vendor belum diperiksa. Domain memakai allowance US$15,15/tahun; [harga publik registrar](https://www.domainesia.com/domain-id/) hanya acuan, bukan bukti registrar yang digunakan.

Sebelum menjadi plafon pengadaan, cocokkan laporan billing GCP per SKU dan kontrak vendor. Perbarui eCKU dan region Confluent, traffic, biaya Brevo di atas kuota, serta invoice dan tanggal renewal domain. Tidak ada resource, paket langganan, atau budget alert yang diubah melalui penyusunan ini.
